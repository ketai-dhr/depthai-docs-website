示例-车辆车牌识别
===================

演示
----

.. figure:: /_static/images/samples/security_barrier_camera_demo.png
   :alt: demo
   :align: center


此示例展示了用于构建模型管道的基本架构，该模型管道支持在设备上放置不同模型以及使用
python 中的 DepthAI 库同时并行或顺序串行运行。

本示例展示了车辆和车牌检测网络，以及在检测结果之上应用的车辆属性识别和车牌识别网络。您可以在示例中使用以下一组预先训练的模型：

-  ``vehicle-license-plate-detection-barrier-0106``
   是查找车辆和车牌的主要检测网络

-  ``vehicle-attributes-recognition-barrier-0039``
   会根据第一个网络的结果执行，并报告常规车辆属性，例如车辆类型（\ ``car``
   / ``van`` / ``bus`` /``truck``\ ）和颜色

-  ``license-plate-recognition-barrier-0007``
   在第一个网络的结果之上执行，并报告每个识别的牌照的字符串

示例源代码
----------

车辆车牌识别示例的源代码在 GitHub 的
`代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/security_barrier_camera>`__ 中,
国内用户可以在 Gitee 的
`代码仓库 <https://gitee.com/OAKChina/depthai-examples/tree/master/security_barrier_camera>`_ 获得


请参考 README 运行程序.

示例原理说明：
--------------

1. 通过相机/视频读取图像

.. figure:: /_static/images/samples/security_barrier_camera_car_1.bmp
   :alt: car\_1
   :align: center


2. 运行 ``vehicle-license-plate-detection-barrier-0106``
   模型，检测（正面）车辆和（中文）车牌

    这是基于 MobileNetV2 + SSD 的检测器

.. figure:: /_static/images/samples/vehicle-license-plate-detection-barrier-0106.png
   :alt: vehicle-license-plate-detection-barrier-0106
   :align: center


3. 运行 ``vehicle-attributes-recognition-barrier-0039``
   模型，根据第一个模型的结果检测车辆属性

    该模型提出了一种用于交通分析场景的车辆属性分类算法

.. figure:: /_static/images/samples/vehicle-attributes-recognition-barrier-0039.png
   :alt: vehicle-attributes-recognition-barrier-0039
   :align: center


4. 运行 ``license-plate-recognition-barrier-0007``
   模型，根据第一个模型的结果识别中国车牌

    这同样是基于 MobileNetV2 + SSD 的检测器

.. figure:: /_static/images/samples/license-plate-recognition-barrier-0007.png
   :alt: license-plate-recognition-barrier-0007
   :align: center


5. 最终效果

.. figure:: /_static/images/samples/security_barrier_camera_demo.png
   :alt: demo
   :align: center


应用程序流程图
--------------

.. figure:: /_static/images/samples/security_barrier_camera_drawio.png
   :alt: drawio
   :align: center

.. include::  /pages/includes/footer-short.rst