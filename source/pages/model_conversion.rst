将模型转换为MyriadX blob
===========================

要允许 DepthAI 使用您的自定义训练模型，您需要将它们转换为 MyriadX blob 文件格式 - 以便它们针对 MyriadX VPU 处理器上的最佳推理进行优化。

为了获得 blob 文件，必须执行两个转换步骤:

- 使用 **模型优化器** 生成 **OpenVINO IR 表示** (其中 IR 代表中间表示)
- 使用 **模型编译器** 将 IR 表示转换为 **MyriadX blob**

下图 (来自 `OpenCV 课程网站 <https://courses.opencv.org/>`__) 显示了这些步骤

.. image:: /_static/images/model_compile.png
  :alt: Model Compile Steps

请在下面找到有关如何使用不同方法执行这些步骤的说明。

本地编译
*****************

如果要进行模型转换和编译，可以按照:

- `OpenVINO 官方说明 <https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit.html>`__
- `OpenVINO Python notebooks <https://github.com/openvinotoolkit/openvino_notebooks>`__
- :ref:`本地 OpenVINO 模型转换`
- `自定义模型转换&编译注意事项 <https://github.com/luxonis/depthai/blob/main/README.md#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__

使用 Google Colab
******************

您还可以使用 Google Colab notebook 训练和转换模型。 您可以查看我们的 :ref:`自定义训练` 教程, 其中每个教程还包含直接在 notebook 中执行的转换和编译步骤。

带有编译步骤的示例笔记本在 `这里 <https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Easy_Object_Detection_With_Custom_Data_Demo_Training.ipynb#scrollTo=_PlfZAR1OCK2>`__ 。

使用在线转换器
**********************

您还可以访问我们的 `MyriadX Blob converter <http://blobconverter.luxonis.com/>`__
这个在线模型转换器允许指定不同的OpenVINO目标版本并支持 **TensorFlow, Caffe, OpenVINO IR 和 OpenVINO Model Zoo** 的转换。

.. image:: /_static/images/blobconverter_web.png
  :alt: BlobConverter Web

使用 blobconverter 库
***************************

为了能够让用户自动使用我们的 blobconverter 工具，我们发布了一个 `blobconverter PyPi 库 <https://pypi.org/project/blobconverter/>`__,
它允许直接从命令行和 Python 脚本编译 MyriadX blob。

安装和使用说明可以在 `这里 <https://github.com/luxonis/blobconverter/tree/master/cli>`__ 找到。

故障排除
###############

将模型转换为 OpenVINO 格式或将其编译为 :code:`.blob`, 你可能会遇到一个问题。 这通常意味着 **不支持两层之间的连接** 或 **不支持该层**.

对于 **NN 模型的可视化** 我们建议使用 `Netron app <https://netron.app/>`__.

.. image:: /_static/images/tutorials/netron.jpeg
  :alt: Netron


支持的层
****************

将您的模型转换为 OpenVINO 的 IR 格式 (:code:`.bin` 和 :code:`.xml`)时, 您必须检查 OpenVINO 是否支持所使用的层。 下面是支持层和它们的局限性 `Caffee <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#caffe_supported_layers>`__, `MXNet <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#mxnet_supported_symbols>`__, `TensorFlow <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#tensorflow_supported_operations>`__, `TensorFlow 2 Keras <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#tensorflow_2_keras_supported_operations>`__, `Kaldi <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#kaldi_supported_layers>`__, and `ONNX <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_prepare_model_Supported_Frameworks_Layers.html#onnx_supported_operators>`__.

不支持的图层类型"layer_type"
***********************************

当使用 `compile_tool <https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html>`__ 从 IR (.xml/.bin) 编译成.blob时，你可能会得到这样的错误:

.. code-block:: bash

  Failed to compile layer "Resize_230": unsupported layer type "Interpolate"

这意味着 VPU(Intels Myriad X)不支持层类型。 您可以在 `此处 <https://docs.openvinotoolkit.org/latest/openvino_docs_IE_DG_supported_plugins_Supported_Devices.html#supported_layers>`__ 找到VPU支持的OpenVINO层,
在 **Supported Layers** header, 标题下的第三列(**VPU**)中。 有关更多信息，请参阅 `英特尔官方故障排除文档 <https://docs.openvinotoolkit.org/latest/openvino_docs_IE_DG_supported_plugins_VPU.html#troubleshooting>`__ 。


数据类型不正确
********************

如果编译器返回类似 **"check error: input #0 has type S32, but one of [FP16] is expected"**,
则意味着您使用了不正确的数据类型。 在上面的例子中，一个 INT32 层直接连接到 FP16。
这些层之间应该有一个转换， 我们可以通过 在这两个层之间使用 OpenVINOs `Convert <https://docs.openvinotoolkit.org/latest/openvino_docs_ops_type_Convert_1.html>`__
层来实现。 您可以通过编辑模型 :code:`.xml` 并添加 `Convert <https://docs.openvinotoolkit.org/latest/openvino_docs_ops_type_Convert_1.html>`__
层来做到这一点。 您可以在此 `discord thread <https://discord.com/channels/790680891252932659/799407361986658354/854501905799184414>`__ 上找到更多信息。



.. include::  /pages/includes/footer-short.rst
