示例-手掌检测
================================

此示例展示了用于构建模型管道的基本架构，该模型管道支持在不同设备上放置模型以及使用 python 中的 DepthAI 库同时并行或顺序串行。

此示例使用 1 个模型来检测图像上的手掌。

.. figure:: /_static/images/samples/palm_detection.gif
    :alt: palm_detection_demo
    :align: center

示例源代码：
#####################################

手掌检测的源代码已上传至 GitHub 的
`代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/palm_detection>`_ 中,
国内用户可以在 Gitee 的
`代码仓库 <https://gitee.com/OAKChina/depthai-examples/tree/master/palm_detection>`_ 获得

请参考 README 运行程序.

原理说明：
#####################################

手掌检测模型
************

手部检测是一项非常复杂的任务：
    此模型必须能够处理各种手部尺寸，相对于图像帧具有较大的比例范围(~20 倍)，并且能够检测遮挡和自遮挡的手部。

    虽然脸部具有高对比度的图案，例如在眼睛和嘴部区域，但手中缺乏这样的特征使得仅从其视觉特征来可靠地检测它们变得相对困难。
    相反，提供额外的上下文，如手臂、身体或人的特征，有助于准确的手部定位。

模型作者使用不同的方法策略来应对上述挑战。
    首先，训练手掌检测器而不是手检测器，因为估计手掌和拳头等刚性物体的包围盒比用关节手指检测手要简单得多。

    此外，由于手掌是较小的对象，非最大值抑制算法即使在双手自遮挡的情况下(如握手)也能很好地工作。

    此外，可以使用方形边界框( ML 术语中的锚点)来建模手掌，忽略其他纵横比，从而将锚点的数量减少 3-5 倍。

利用上述技术，手掌检测的平均准确率达到 95.7% 。

运行手掌检测模型
************************************

    运行 palm_detection.blob 模型检测并框出图像中的手掌。

.. figure:: /_static/images/samples/palm_detection.png
    :alt: palm_detection
    :align: center


.. include::  /pages/includes/footer-short.rst