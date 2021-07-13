示例-跌倒检测
=============

本示例演示了如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行两阶段推理。
首先，在图像上检测到人体姿势, 生成骨骼图，然后把骨骼图片传入骨骼状态识别模型。

示例源代码：
############

手掌检测的源代码已上传至 `GitHub 的代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/fall_detection_v2>`_ 中,
国内用户可以在 `Gitee 的代码仓库 <https://gitee.com/OAKChina/depthai-examples/tree/master/fall_detection_v2>`_ 获得

请参考 README 运行程序.


Demo
--------

|demo|

依据 *MoveNet* 结果生成骨骼图

========== === =========
|movenet|  ==>  |normal|
========== === =========


把骨骼图片传入全连接层，全连接层对骨骼进行判断，输出骨骼状态（\ *fall*
or *normal*\ ）

并使用依据骨骼图节点画出的矩形框的宽高比进行辅助判断

======== ========
*fall*   *normal*
======== ========
|fall|    |normal|
======== ========

.. |demo| image:: /_static/images/samples/fall_detection/demo.gif
.. |movenet| image:: /_static/images/samples/fall_detection/movenet.png
.. |normal| image:: /_static/images/samples/fall_detection/normal.png
.. |fall| image:: /_static/images/samples/fall_detection/fall.png

.. include::  /pages/includes/footer-short.rst