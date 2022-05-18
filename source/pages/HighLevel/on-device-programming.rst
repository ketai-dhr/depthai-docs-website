设备上编程
===============

虽然由于本机工具的封闭性而无法进行常规（固件）设备上的开发，但我们仍然公开了几种运行自定义代码的替代方法：

1. Scripting-使用带有python3.9版本的 `脚本节点 <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/script/>`__ 。
#. 创建您自己的神经网络模型，来运行更多计算繁重特征的任务。
#. 创建自定义OpenCL内核

使用脚本节点
*************

使用 `脚本节点 <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/script/>`__ 允许您在设备本身上运行自定义python脚本，这使用户在构建管道时具有更大的灵活性。

脚本节点 **在串联使用多个神经网络模型时也非常有用** ，您只需要在将图像提供给第二个前处理第一个神经网络的输出。这里的 **例子** 是 `人脸年龄和性别识别 <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-age-gender>`__ 示例--第一个NN模型将检测人脸，检测结果传递到脚本节点。该节点会创建 `ImageManip配置节点 <https://docs.oakchina.cn/projects/api/en/latest/components/messages/image_manip_config/>`__ 以裁剪原始帧并仅向 `人脸年龄/性别识别模型 <https://docs.openvino.ai/latest/omz_models_model_age_gender_recognition_retail_0013.html>`__ 提供裁剪的人脸帧。

对于运行计算量大的方法(例如图像过滤器)，由于性能原因，您可能希望避免使用脚本节点，而是使用下面描述的2个选项之一。

创建自定义NN模型
******************

您可以使用自己喜欢的 NN 库创建自定义模型，将模型转换为 OpenVINO，然后将其编译为 :code:`.blob` 。有关此主题的更多信息，请参阅 :ref:`将模型转换为MyriadX blob` 文档。

创建自定义OpenCL内核
**********************

创建自定义 NN 模型有一些限制，例如OpenVINO/VPU不支持的 :ref:`层 <支持的层>`。为避免这些限制，您可以考虑创建自定义 OpenCL 内核并为 VPU 编译它。该内核将在 VPU 上的 SHAVE 内核上运行。考虑到此选项对用户不是很友好。我们计划创建一个关于如何开发这些并在 OAK 相机上运行它们的教程。

- `关于如何 <https://docs.openvino.ai/2021.1/openvino_docs_IE_DG_Extensibility_DG_VPU_Kernel.html>`__ 通过OpenVINO使用OpenCL实现自定义层的教程。
- `OpenCL中的自定义内核实现 <https://github.com/openvinotoolkit/openvino/tree/2021.4.2/inference-engine/src/vpu/custom_kernels>`__ 。

.. include::  /pages/includes/footer-short.rst