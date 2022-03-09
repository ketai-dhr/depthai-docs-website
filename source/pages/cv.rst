计算机视觉
============

我们的平台支持计算机视觉(CV)功能在设备本身上执行。虽然你不能运行 OpenCV，但是你可以使用它支持的许多功能。使用DepthAI管道构建器，你可以:

- 使用 `ImageManip <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/image_manip/>`__ 节点实现图像裁剪、旋转、扭曲/去扭曲、镜像、翻转、变换透视等。
- 使用 `EdgeDetector <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/edge_detector/>`__ 节点实现边缘检测(Sobel过滤器)。
- 使用 `FeatureTracker <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/feature_tracker/>`__ 节点实现图像特征点检测及跟踪功能。
- 使用 `ObjectTracker <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/object_tracker/>`__ 节点实现对象跟踪(卡尔曼滤波器、匈牙利算法)。支持Yolo和MobileNet对象检测器，做到开箱即用。
- 使用 `StereoDepth <https://docs.oakchina.cn/projects/api/en/latest/components/nodes/stereo_depth/>`__ 节点实现立体深度推理(Census Tranform, Cost Matching and Aggregation)。

如果你想使用其他 CV 函数，请参阅 :ref:`在设备上运行您自己的CV模型` 文档，了解如何在设备的硬件加速模块上有效地实现和运行 CV 函数。

.. image:: /_static/images/cv.gif

**一些其他的CV示例：**

- `Lossless zooming <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-lossless-zooming>`__
- `Host-side WLS depth filtering <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-wls-filter>`__
- `PointCloud visualization <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-rgbd-projection>`__

.. toctree::
   :hidden:

   tutorials/creating-custom-nn-models.rst

.. include::  /pages/includes/footer-short.rst