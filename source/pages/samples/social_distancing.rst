示例-社交距离
===========================

演示
####

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//www.youtube.com/embed/-Ut9TemGZ8I" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

示例源代码
##########

社交距离示例的源代码在github的 `代码仓库 <https://github.com/OAKChina/depthai-examples/tree/master/social-distancing>`_ 中.

请参考README运行程序.

示例原理说明
#################################

1.捕获图片
*********************************

.. image:: /_static/images/samples/social-distancing1.png
  :alt: captured
  :align: center

Depthai 从板载的相机获取图像

2.检测人和人到相机的距离
**********************************

.. image:: /_static/images/samples/social-distancing2.png
  :alt: captured
  :align: center

Depthai 从板载的相机取图并运行 person-detection-retail-0013 模型进行行人检测, 并将检测到的目标信息和深度信息打包到一个 detection 中送给主机端。

.. image:: /_static/images/samples/social-distancing3.png
  :alt: captured
  :align: center

depthai 提供的深度信息包括三个维度：

- X 水平坐标
- y 垂直坐标
- z 距离坐标

有了这些信息我们就可以创建相机到物体之间 3D 向量.

3.将位置信息映射到2D空间
**********************************

.. image:: /_static/images/samples/social-distancing4.png
  :alt: captured
  :align: center

相对于想象三维空间中的位置我们更容易想象人在二维空间中的位置，因为只需要考虑 x 和 y 坐标。所以为了调试目的我们需要将三维空间中的位置映射到二维空间。

.. image:: /_static/images/samples/social-distancing5.png
  :alt: captured
  :align: center

这是应用程序输出的”鸟瞰”图, 显示人在二维空间投影中的位置。

这种视角下我们可以使用简单的 2D 几何进行距离计算，并且更容易理解相机如何确定我们在空间中的位置。但是，在本应用程序使用了 3D 空间中的距离，因为它没有任何缺点，并且它可以提供更准确的结果并处理一些边缘情况。(比如计算站在梯子下面和梯子上的人的距离)。

4.计算距离
************************************

.. image:: /_static/images/samples/social-distancing6.png
  :alt: captured
  :align: center

知道了每个人的位置后, 我们就可以计算他们之间的距离并检查他们是否离得过近了。应用程式中使用的公式是规则的 3D 欧式距离，公式如下:

.. image:: /_static/images/samples/social-distancing7.png
  :alt: captured
  :align: center

最后，如果任意两个人之间的距离小于我们指定的阈值，我们会将两个人都视为危险地靠近对方，并显示警告消息。

.. image:: /_static/images/samples/social-distancing8.png
  :alt: captured
  :align: center

.. image:: /_static/images/samples/social-distancing9.png
  :alt: captured
  :align: center


.. include::  /pages/includes/footer-short.rst
