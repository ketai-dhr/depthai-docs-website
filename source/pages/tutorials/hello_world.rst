Hello World
===========

学习如何使用DepthAI Python API 来显示彩色视频流

.. _hello_world_dependencies:

依赖关系
############

首先把开发环境设置好。本教程使用:

- Python 3.6 (Ubuntu) or Python 3.7 (Raspbian)。
- 安装或升级 DepthAI :ref:`Python API`
- 需要用到 :code:`cv2` 和 :code:`numpy` Python 模块。


代码概述
#############

:code:`depthai` Python 模块可以让您访问主板上的4k 60Hz彩色相机。
我们将把这个相机模组的视频串流到你桌面上。
你可以在GitHub上找到 `本教程的完整源代码 <https://github.com/luxonis/depthai-tutorials/tree/master/1-hello-world>`__。

文件设置
##########

在你的计算机上设置以下文件结构:

.. code-block:: bash

  cd ~
  mkdir -p depthai-tutorials-practice/1-hello-world
  touch depthai-tutorials-practice/1-hello-world/hello-world.py
  cd depthai-tutorials-practice/1-hello-world

父目录名称里面的 :code:`-practice` 后缀是干什么的? 我们的教程可以在 `depthai-tutorials <https://github.com/luxonis/depthai-tutorials>`__ 代码库中获得。
在我们加上 :code:`-practice` 后，你就可以把你个人的代码和我们完整教程的代码区分开来（前提是你下载了这些教程）。


安装 pip 依赖
########################

要显示DepthAI彩色视频流，我们需要导入少量的软件包。
下载并安装本教程所需的包:

.. code-block:: bash

  python3 -m pip install numpy opencv-python depthai --user


测试你的环境
#####################

让我们验证一下是否能够加载所有的依赖项。 在你的代码编辑器中打开你之前 :ref:`创建 <文件设置>` 的 :code:`hello-world.py` 文件。 
复制并粘贴以下内容到 :code:`hello-world.py` 中：


.. code-block:: python

  import numpy as np # numpy - manipulate the packet data returned by depthai
  import cv2 # opencv - display the video stream
  import depthai # access the camera and its data packets

尝试运行脚本并确保其执行无误：

.. code-block:: bash

  python3 hello-world.py

如果你看到了如下的错误:

.. code-block::

  ModuleNotFoundError: No module named 'depthai'

...请按照 :ref:`我们的故障排除部分的这些步骤 <ImportError: 没有名为 'depthai' 的模块>` 来操作。

初始化DepthAI设备
#############################

启动DepthAI设备:

.. code-block:: python

  device = depthai.Device('', False)

尝试运行脚本。您应该可以看到类似以下的输出:

.. code-block::

  No calibration file. Using Calibration Defaults.
  XLink initialized.
  Sending device firmware "cmd_file": /home/pi/Desktop/depthai/depthai.cmd
  Successfully connected to device.
  Loading config file
  Attempting to open stream config_d2h
  watchdog started 6000
  Successfully opened stream config_d2h with ID #0!

如果你看到错误, 请 :ref:`重置你的DepthAI设备 <'depthai：初始化 xlink 时出错'错误，DepthAI 无法运行>` , 然后重试。

创建DepthAI管道
###########################

现在，我们将使用 :code:`previewout` 流创建数据管道。 这个流包含来自彩色相机的数据。
在 :code:`ai` 部分使用的模型是一个MobileNetSSD，有20个不同的类，详见
`这里 <https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json>`__ 。

.. code-block:: python

  # 使用'previewout'流创建管道，建立与设备的第一个连接。
  pipeline = device.create_pipeline(config={
      'streams': ['previewout', 'metaout'],
      'ai': {
          'blob_file': "/path/to/mobilenet-ssd.blob",
          'blob_file_config': "/path/to/mobilenet-ssd.json"
      }
  })

  if pipeline is None:
      raise RuntimeError('Pipeline creation failed!')

显示视频流
########################

DepthAI管道会生成一个数据包流。每个 :code:`previewout` 数据包都包含一个代表图像帧的3D数组。
我们将图像帧的形状改变为和 :code:`cv2` 兼容的格式，并显示出来。

.. code-block:: python

  detections = []

  while True:
      # 从设备中检索数据包。
      # 一个数据包包含视频帧数据。
      nnet_packets, data_packets = pipeline.get_available_nnet_and_data_packets()

      for nnet_packet in nnet_packets:
          detections = list(nnet_packet.getDetectedObjects())

      for packet in data_packets:
          # 默认情况下，DepthAI会添加其他流（尤其是 "meta_2dh"）。只处理 "previewout"。
          if packet.stream_name == 'previewout':
              data = packet.getData()
              # 预览输出图像的格式是CHW（通道，高度，宽度），但OpenCV需要HWC，所以我们
              # 把 (3, 300, 300) 改成 (300, 300, 3)
              data0 = data[0,:,:]
              data1 = data[1,:,:]
              data2 = data[2,:,:]
              frame = cv2.merge([data0, data1, data2])

              img_h = frame.shape[0]
              img_w = frame.shape[1]

              for detection in detections:
                  pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
                  pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

              cv2.imshow('previewout', frame)

      if cv2.waitKey(1) == ord('q'):
          break

  # 退出循环后，应删除管道对象。否则设备将继续工作。
  # 如果你要在退出循环后添加代码，这是必须执行的一步。
  del pipeline
  del device

运行脚本。 请在视频流上（不是您的终端）上按 :code:`Q` 键退出:

.. code-block:: bash

  python3 hello-world.py

你已经上手了！ 你可以在GitHub上找到 `本教程的完整代码 <https://github.com/luxonis/depthai-tutorials/blob/master/1-hello-world/hello_world.py>`__ 。

.. include::  /pages/includes/footer-short.rst
