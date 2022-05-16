示例-口罩检测
================================

此示例展示了用于构建模型管道的基本架构，该模型管道支持在不同设备上放置模型以及使用python中的DepthAI库同时并行或顺序串行。

此示例使用2个模型构建了一个管道，该管道能够检测图像上的人脸及面部是否佩戴口罩。

演示
##########################

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="/_static/images/samples/face_mask.mp4" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

示例源代码：
#####################################

口罩检测的源代码已上传至 GitHub 的
`代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/face_mask>`_ 中,
国内用户可以在 Gitee 的
`代码仓库 <https://gitee.com/OAKChina/depthai-examples/tree/master/face_mask>`_ 获得


请参考README运行程序.

代码原理说明：
#####################################

1. 运行人脸检测模型
************************************

   运行face-detection-retail-0004.blob模型检测图像中的人脸，并截取面部图像。

   |Screenshot from 2021-01-14 13-44-58|

2. 运行口罩检测模型
************************************

   运行sbd_mask4.blob模型检测传入的面部图像中是否佩戴口罩。

   未佩戴口罩

   |Screenshot from 2021-01-18 09-44-13|

   已佩戴口罩

   |Screenshot from 2021-01-18 09-44-37|

.. |Screenshot from 2021-01-14 13-44-58| image:: /_static/images/samples/mask1.png
   :width: 3.86458in
   :height: 2.83333in
.. |Screenshot from 2021-01-18 09-44-13| image:: /_static/images/samples/mask2.png
   :width: 2.97222in
   :height: 3.23403in
.. |Screenshot from 2021-01-18 09-44-37| image:: /_static/images/samples/mask3.png
   :width: 2.98889in
   :height: 3.26944in

.. include::  /pages/includes/footer-short.rst