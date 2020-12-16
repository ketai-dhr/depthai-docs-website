在一个主机上使用多个 DepthAI
================================

学习如何使用DepthAI :code:`-dev` 选项来发现连接到你系统的DepthAI，并单独使用它们。

.. image:: /_static/images/tutorials/multiple/setup.jpg
  :alt: face

画面左侧的是Luxonis `uAI (BW1093) <https://shop.luxonis.com/products/bw1093>`__ 它实际上已插入 `Raspberry Pi计算模块版本 (BW1097) <https://shop.luxonis.com/products/depthai-rpi-compute-module-edition>`__ 中。

因此，在这种情况下，一切都在BW1097背面的（单个）Raspberry Pi 3B+上运行。

依赖
############

您已经在系统上设置了Python API (如果您具有Raspberry Pi计算模块，则它是预先设置的).
如果您尚未在系统上安装DepthAI Python API，请参见此处 :ref:`here <Python API>` 。

发现DepthAI-USB端口映射
#################################

目前，通过选择插入DepthAI的USB端口来完成对DepthAI多设备的支持。

如果你想要把某个DepthAI设备和运行的特定代码（如神经模型)相关联, 建议一次插入一个设备，然后使用以下命令确定哪个设备在哪个端口上:

.. code-block:: bash

  python3 depthai_demo.py -dev list

一个系统接两个DepthAI的结果示例：

.. code-block::

  ...
  XLink initialized.
  Detected 2 device(s):
    2-ma2480     on USB port: 1
    1.1-ma2480   on USB port: 2.1

选择要使用的特定DepthAI设备。
###############################################

从上面检测到的设备中，使用下面的命令选择您想在代码中使用的设备。 例如，你想使用上面的第一个设备（USB端口1上的设备），使用以下命令:

.. code-block:: bash

  python3 depthai_demo.py -dev 1

然后类似地，要在其他第二台设备上再次运行相同的脚本，请使用：

.. code-block::

  python3 depthai_demo.py -dev 2.1

而且，您可以将这些脚本用作自己修改版本的基础，以便可以在不同的DepthAI/uAI模型上运行不同的神经模型。

主机负担的摘要和概述
########################################

现在，您可以根据需要使用任意数量的DepthAI设备。

由于DepthAI完成了所有繁重的工作，因此您通常可以使用非常多的DepthAI设备而对主机造成非常小的负担。

并且值得注意的是，你可以随时只请求 :code:`metaout`
`详见这里 <https://github.com/luxonis/depthai-python-extras/blob/232d1e7529e0278b75192d0870a969b6c0e2d1ae/depthai.py#L104>`__ 来禁用视频流。

所以如果说你用元数据来驱动机器人或者用代码做决策，而不需要视频，你可以这样做来大大减轻主机的负担–因为所有的神经推理工作都是在主机之前的DepthAI上完成的–几乎所有的主机负担都只是来自于显示视频。

所以在禁用视频的情况下，主机每秒只需要处理几千字节的元数据。

.. include::  /pages/includes/footer-short.rst
