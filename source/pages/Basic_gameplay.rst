Hello Word
=================================

请查看此处 `链接 <https://docs.oakchina.cn/projects/api/tutorials/hello_world.html>`__。

Gen2 API入门
================================

Python
##############################

请查看此处 `链接 <https://docs.oakchina.cn/projects/api/references/python.html>`__。

C++
################################

请查看此处 `链接 <https://docs.oakchina.cn/projects/api/references/cpp.html#>`__。

depthai API 示例
=====================

代码示例用于自动化测试。它们也是DepthAI API的一个很好的起点，因为不同的节点功能通过代码呈现。

立体神经推理
#######################

在这种模式下，神经推理（物体检测，标志检测等）上运行的左 **和** 右摄像机，以产生立体声推理结果。与融合立体深度的单眼神经推理不同 - 没有最大视差搜索限制 - 所以最小距离纯粹受 (a) 立体相机本身的水​​平视野 (HFOV) 和 (b) 超焦距中的较大者限制的相机。

全局快门同步立体对的超焦距为 19.6cm。因此，距离小于 19.6 厘米的物体会显得失焦。这有效地用于该操作模式的最小距离，因为在大多数情况下(除用于与非常宽的立体声基线 `OAK-FFC-3P-OG <https://docs.luxonis.com/projects/hardware/en/latest/pages/BW1098FFC.html>`__),
这 **有效** 最小距离大于较高 **实际** 最小距离为的结果立体相机视野。 例如，当距离 `OAK-D <https://docs.luxonis.com/projects/hardware/en/latest/pages/BW1098OAK.html>`__ 小于 `5.25cm
<https://www.google.com/search?ei=GapBX-y3BsuxtQa3-YaQBw&q=%3Dtan%28%2890-71%2F2%29*pi%2F180%29*7.5%2F2&oq=%3Dtan%28%2890-71%2F2%29*pi%2F180%29*7.5%2F2&gs_lcp=CgZwc3ktYWIQAzoECAAQR1DZkwxYmaAMYPilDGgAcAF4AIABS4gB1AKSAQE1mAEAoAEBqgEHZ3dzLXdpesABAQ&sclient=psy-ab&ved=0ahUKEwisqPat-6_rAhXLWM0KHbe8AXIQ4dUDCAw&uact=5>`__
(下图中标记为 **M**)时, 物体将完全脱离两个灰度相机的视野，但比灰度相机的超焦距更近 (即 19.6cm，标记为 **Y**),
所以实际的最小深度就是这个超焦距。

.. image:: https://user-images.githubusercontent.com/59799831/132251763-02d7a767-a057-43e9-8704-e8cb1ec5f497.jpeg
  :alt: Minimum perceiving distance

因此，要计算此操作模式的最小距离，请使用以下公式：

.. code-block:: python

  min_distance = max(tan((90 - HFOV/2) * pi/2) * base_line_dist/2, 19.6)

该公式实现了 HFOV 施加的最小距离的最大值，以及 19.6cm，即超焦距施加的最小距离。

Demo
------------

.. image:: https://user-images.githubusercontent.com/59799831/132098832-70a2d0b9-1a30-4994-8dad-dc880a803fb3.gif
  :target: https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-triangulation
  :alt: Triangulation Demo

有关更多信息，请查看 执行立体神经接口的 `gen2-triangulation 演示 <https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-triangulation>`__ 。

如何最大化FOV
#######################

默认情况下, 当您使用 :ref:`ColorCamera <Gen2 彩色相机Preview流>` 的 :code:`preview` 输出时, DepthAI 将裁剪帧以获得所需的纵横比。例如，如果您使用的是 Mobilenet-SSD 模型，则需要:code:`300x300` 分辨率的图像。 OAK会将1080P帧裁剪为 :code:`1080x1080` 分辨率的图像，然后将它们调整为 :code:`300x300` 的分辨率。 这意味着您将丢失图像的某些部分。

如果您想最大化图像的 FOV，您可以：

#. 更改纵横比（拉伸图像）
#. 对图像使用Letterboxing

更改纵横比
------------

使用 :code:`camRgb.setPreviewKeepAspectRatio(False)`.  这意味着不会保留纵横比并且图像将被"拉伸"。 对于某些现成的 NN 模型，这可能会出现问题，因此可能需要对其进行模型微调。
`此处是用法示例 <https://gitee.com/oakchina/depthai-python/blob/main/examples/MobileNet/rgb_mobilenet_4k.py>`__.

Letterboxing
--------------

对图像进行 `Letterboxing <https://en.wikipedia.org/wiki/Letterboxing_%28filming%29>`__ 。此方法将减小图像的大小并在图像上方和下方填充"黑条"，因此纵横比得以保留。 您可以通过将 :code:`ImageManip` 和 :code:`manip.setResizeThumbnail(x,y)` (对于Mobilenet :code:`x=300,y=300`) 一起使用来实现此目的。
使用这种方法的缺点是您的实际图像会更小，因此可能无法保留某些特征，这可能意味着 NN 精度可能会降低。
`此处是用法示例 <https://gitee.com/oakchina/depthai-python/blob/main/examples/ObjectTracker/object_tracker_video.py>`__.

.. image:: /_static/images/tutorials/fov.jpeg


在一个主机上使用多个 DepthAI
#######################

学习如何使用 DepthAI :code:`-dev` 选项来发现连接到你系统的DepthAI，并单独使用它们。

.. image:: /_static/images/tutorials/multiple/setup.jpg
  :alt: face

画面左侧的是 Luxonis `uAI (BW1093) <https://shop.luxonis.com/products/bw1093>`__ 它实际上已插入 `Raspberry Pi计算模块版本 (BW1097) <https://shop.luxonis.com/products/depthai-rpi-compute-module-edition>`__ 中。

因此，在这种情况下，一切都在 BW1097 背面的(单个)Raspberry Pi 3B+上运行。

依赖
------------

您已经在系统上设置了 Python API (如果您具有 Raspberry Pi计算模块，则它是预先设置的).
如果您尚未在系统上安装DepthAI Python API，请参见此处 :ref:`here <Python API安装详解>` 。

发现DepthAI-USB端口映射
--------------------

目前，通过选择插入 DepthAI 的 USB 端口来完成对 DepthAI 多设备的支持。

如果你想要把某个 DepthAI 设备和运行的特定代码（如神经模型)相关联, 建议一次插入一个设备，然后使用以下命令确定哪个设备在哪个端口上:

.. code-block:: bash

  python3 depthai_demo.py -dev list

一个系统接两个 DepthAI 的结果示例：

.. code-block::

  ...
  XLink initialized.
  Detected 2 device(s):
    2-ma2480     on USB port: 1
    1.1-ma2480   on USB port: 2.1

选择要使用的特定 DepthAI 设备。
-----------------------

从上面检测到的设备中，使用下面的命令选择您想在代码中使用的设备。 例如，你想使用上面的第一个设备(USB 端口 1 上的设备)，使用以下命令:

.. code-block:: bash

  python3 depthai_demo.py -dev 1

然后类似地，要在其他第二台设备上再次运行相同的脚本，请使用：

.. code-block::

  python3 depthai_demo.py -dev 2.1

而且，您可以将这些脚本用作自己修改版本的基础，以便可以在不同的 DepthAI/uAI 模型上运行不同的神经模型。

主机负担的摘要和概述
-----------------------

现在，您可以根据需要使用任意数量的 DepthAI 设备。

由于 DepthAI 完成了所有繁重的工作，因此您通常可以使用非常多的 DepthAI 设备而对主机造成非常小的负担。

并且值得注意的是，你可以随时只请求 :code:`metaout`
`详见这里 <https://github.com/luxonis/depthai-python-extras/blob/232d1e7529e0278b75192d0870a969b6c0e2d1ae/depthai.py#L104>`__ 来禁用视频流。

所以如果说你用元数据来驱动机器人或者用代码做决策，而不需要视频，你可以这样做来大大减轻主机的负担–因为所有的神经推理工作都是在主机之前的 DepthAI 上完成的–几乎所有的主机负担都只是来自于显示视频。

所以在禁用视频的情况下，主机每秒只需要处理几千字节的元数据。

详细API示例请查看 `此处 <https://docs.oakchina.cn/projects/api/tutorials/code_samples.html>`。

API函数node示例
====================

node 是填充 pipeline 时的构建块。每个 node 都为 OAK 提供一个特定的功能，一组可配置的属性和输入/输出。在管道上创建一个节点后，你还可以根据需要对其进行配置，并将其链接到其他 node。

详细 node 示例请查看 `此处 <https://docs.oakchina.cn/projects/api/components/nodes.html>`__。


.. include::  /pages/includes/footer-long.rst