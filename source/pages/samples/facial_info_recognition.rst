示例-面部信息识别
================================

本示例演示了如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行两阶段推理。
首先，在图像上检测到脸部，然后将裁剪的脸部框架发送到年龄性别识别网络与表情识别网络。

.. figure:: /_static/images/samples/facial_info_recognition1.png
    :alt: facial_info_recognition
    :align: center

此示例使用 3 个模型来识别图像上的面部信息。

-  ``face-detection-retail-0004`` 是检测定位人脸的主要网络
-  ``age-gender-recognition-retail-0013`` 会根据第一个网络的结果为基础识别年龄与性别
-  ``emotions-recognition-retail-0003`` 会根据第一个网络的结果为基础识别面部表情

示例源代码：
#####################################

面部信息识别的源代码已上传至 GitHub 的
`代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/facial_info_recognition>`_ 中,
国内用户可以在 Gitee 的
`代码仓库 <https://gitee.com/OAKChina/depthai-examples/tree/master/facial_info_recognition>`_ 获得

请参考 README 运行程序.

.. include::  /pages/includes/footer-short.rst