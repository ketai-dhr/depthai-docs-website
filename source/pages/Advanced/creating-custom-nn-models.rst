在设备上运行您自己的CV模型
==========================

正如在 :ref:`设备上编程` 中提到的，您可以使用您最喜欢的NN库创建 **自定义CV模型** ，将其转换并编译为 :code:`.blob` 并在设备上运行。本教程将介绍如何做到这一点。

如果您对 **训练和部署自己的AI模型** 感兴趣，请参阅 `自定义训练 <https://docs.oakchina.cn/en/latest/pages/training/>`__ 。

**示例：**

- `Frame concatenation <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-custom-models/generate_model#concatenate-frames>`__ - using `PyTorch <https://pytorch.org/>`__
- `Laplacian edge detection <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-custom-models/generate_model#blur-frames>`__ - using :ref:`Kornia`
- `Frame blurring <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-custom-models/generate_model#corner-detection>`__ - using :ref:`Kornia`
- `Tutorial on running custom models on OAK <https://rahulrav.com/blog/depthai_camera.html>`__ by Rahul Ravikumar
- `Harris corner detection in PyTorch <https://github.com/kunaltyagi/pytorch_harris/>`__ by Kunal Tyagi

使用PyTorch创建自定义模型
***************************

**TL;DR** 如果您对实现代码感兴趣，请 `点击此处 <https://gitee.com/oakchina/depthai-experiments/blob/master/gen2-custom-models/generate_model/pytorch_concat.py>`__ 。

#. **创建PyTorchNN模块**

    我们首先需要创建一个扩展PyTorch的 `nn.Module <https://pytorch.org/docs/stable/generated/torch.nn.Module.html>`__ 的Python类。然后我们可以将我们的NN逻辑放入 :code:`forward` 创建的类的函数中。在帧连接的例子中，我们可以使用 `torch.cat <https://pytorch.org/docs/master/generated/torch.cat.html#torch-cat>`__ 函数连接多个帧：

    .. code-block:: python

        class CatImgs(nn.Module):
            def forward(self, img1, img2, img3):
                return torch.cat((img1, img2, img3), 3)
    
    如需更复杂的模块，请参阅Kunal Tyagi的 `Harris corner detection in PyTorch <https://github.com/kunaltyagi/pytorch_harris/>`__ 。

    **请记住** ，VPU仅支持 `FP16 <https://en.wikipedia.org/wiki/Half-precision_floating-point_format#Half_precision_examples>`__ ，这意味着最大值为 65504。当乘以几个值时，如果您没有正确使用归一化或除法，您可能会快速溢出。


#. **将NN模块导出到onnx**

    由于OpenVINO不直接支持PyTorch，我们首先需要将模型导出为 `onnx <https://onnx.ai/>`__ 格式，然后再导出为 OpenVINO。`PyTorch集成了对onnx的支持 <https://pytorch.org/docs/stable/onnx.html>`__ ，因此导出到onnx非常简单：

    .. code-block:: python

        # For 300x300 frames
        X = torch.ones((1, 3, 300, 300), dtype=torch.float32)
        torch.onnx.export(
            CatImgs(),
            (X, X, X), # Dummy input for shape
            "path/to/model.onnx",
            opset_version=12,
            do_constant_folding=True,
        )
    
    这会将连接模型导出为onnx格式。我们可以使用 `Netron应用程序 <https://netron.app/>`__ 可视化创建的模型：

    .. image:: /_static/images/tutorials/custom_model/concat_model.png

#. **简化onnx模型**

    将模型导出到onnx时，PyTorch效率不高。它创建了大量不必要的操作/层，从而增加了网络的大小(这可能导致FPS降低)。这就是我们推荐使用 `onnx-simplifier <https://github.com/daquexian/onnx-simplifier>`__ 的原因，这是一个简单的python包，可以删除不必要的操作/层。

    .. code-block:: python

        import onnx
        from onnxsim import simplify

        onnx_model = onnx.load("path/to/model.onnx")
        model_simpified, check = simplify(onnx_model)
        onnx.save(model_simpified, "path/to/simplified/model.onnx")

    以下是使用onnx-simplifier进行简化的重要性的示例。左边是直接从PyTorch导出的模糊模型(来自Kornia)，右边是具有 **相同功能** 的简化网络：

    .. image:: /_static/images/tutorials/custom_model/blur_comparison.png

#. **转换为OpenVINO/blob**

    现在我们有了(简化的)onnx模型，我们可以将其转换为OpenVINO，然后再转换为 :code:`.blob` 格式。有关转换模型的其他信息，请参阅 :ref:`将自定义模型转换为MyriadX blob` 。

    这通常首先使用 `OpenVINO的模型优化器 <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html>`__ 将onnx转换为 IR 格式(.bin/.xml)，然后使用编译工具编译为 :code:`.blob` 。 但我们也可以使用blobconverter将onnx直接转换为.blob。

    Blobconverter只需同时执行这两个步骤 - 无需安装OpenVINO。你可以像这样编译你的onnx模型：

    .. code-block:: python

        import blobconverter

        blobconverter.from_onnx(
            model="/path/to/model.onnx",
            output_dir="/path/to/output/model.blob",
            data_type="FP16",
            shaves=6,
            use_cache=False,
            optimizer_params=[]
        )

#. **在管道中使用.blob**

    您现在可以将 :code:`.blob` 模型与 `NeuralNetwork <https://docs.oakchina.com/projects/api/en/latest/components/nodes/neural_network/>`__ 节点一起使用。浏览 `depthai-experiments/custom-models <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-custom-models>`__ 以运行使用这些自定义模型的演示应用程序。

Kornia
********

`Kornia <https://kornia.readthedocs.io/en/latest/>`__ , “State-of-the-art and curated Computer Vision algorithms for AI.”, 有 **一组在PyTorch中实现的通用计算机视觉算法 ** 。这允许用户执行类似的操作：

.. code-block:: python

    import kornia

    class Model(nn.Module):
        def forward(self, image):
            return kornia.filters.gaussian_blur2d(image, (9, 9), (2.5, 2.5))

使用与 :ref:`PyTorch创建自定义模型 <使用PyTorch创建自定义模型>` 相同的步骤来实现 `帧模糊 <https://gitee.com/oakchina/depthai-experiments/blob/master/gen2-custom-models/generate_model/kornia_blur.py>`__ ，如下所示:

.. image:: /_static/images/tutorials/custom_model/blur.jpeg

.. note::

    在我们的测试中，我们发现 **一些算法** 既不支持OpenVINO框架，也不支持VPU。我们已经为 `Sobel过滤器 <https://kornia.readthedocs.io/en/latest/filters.html?highlight=sobel#kornia.filters.Sobel>`__ 提交了一个 `问题 <https://github.com/openvinotoolkit/openvino/issues/7557>`__ 。

部署参考的API示例
===========================

`官方参考链接 <https://docs.oakchina.cn/projects/api/components/nodes/neural_network.html>`__

.. include::  /pages/includes/footer-short.rst