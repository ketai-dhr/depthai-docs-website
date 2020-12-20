示例-访问 DepthAI 相机所需的最少代码
=====================================

演示
####

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//www.youtube.com/embed/puI57TaFCUM" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

源代码
###########

.. code-block:: python

  import cv2
  import depthai

  device = depthai.Device('', False)

  p = device.create_pipeline(config={
      "streams": ["metaout", "previewout"],
      "ai": {
          "blob_file": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.blob",
          "blob_file_config": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.json"
      }
  })

  if p is None:
      raise RuntimeError("Error initializing pipelne")

  detections = []

  while True:
      nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

      for nnet_packet in nnet_packets:
          detections = list(nnet_packet.getDetectedObjects())

      for packet in data_packets:
          if packet.stream_name == 'previewout':
              data = packet.getData()
              data0 = data[0, :, :]
              data1 = data[1, :, :]
              data2 = data[2, :, :]
              frame = cv2.merge([data0, data1, data2])

              img_h = frame.shape[0]
              img_w = frame.shape[1]

              for detection in detections:
                  pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
                  pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

                  cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

              cv2.imshow('previewout', frame)

      if cv2.waitKey(1) == ord('q'):
          break

  del p
  del device

说明
###########

该代码分为三个阶段: **初始化**, **处理结果** 和 **反初始化**。

**初始化** 在这里完成，因为它正在初始化设备并确保已创建管道。

.. code-block:: python

  device = depthai.Device('', False)

  p = device.create_pipeline(config={
      "streams": ["metaout", "previewout"],
      "ai": {
          "blob_file": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.blob",
          "blob_file_config": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.json"
      }
  })

  if p is None:
      raise RuntimeError("Error initializing pipelne")

**反初始化** 基本上只是两行代码，尽管没有必要将其包括在内，但绝对建议。

.. code-block:: python

  del p
  del device

现在，结果处理包含两个阶段-解析 nnet 结果和显示框架。

解析神经网络结果
******************************

在下面，您将了解解析神经网络结果的部分。

.. code-block:: python

  detections = []

  while True:
      nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

      for nnet_packet in nnet_packets:
          detections = list(nnet_packet.getDetectedObjects())

我们之前在 :code:`blob_file_config` 字段中指定的神经网络配置允许 DepthAI 以正确的格式准备结果并删除不正确的条目（例如，置信度低于阈值的条目）。

该数组中的每个对象都是一个 Detection 实例，以后我们可以在代码中轻松使用它

显示框架
*********************

.. code-block:: python

  for packet in data_packets:
      if packet.stream_name == 'previewout':
          data = packet.getData()
          data0 = data[0, :, :]
          data1 = data[1, :, :]
          data2 = data[2, :, :]
          frame = cv2.merge([data0, data1, data2])

          img_h = frame.shape[0]
          img_w = frame.shape[1]

          for detection in detections:
              pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
              pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

              cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

          cv2.imshow('previewout', frame)

  if cv2.waitKey(1) == ord('q'):
      break

此阶段也分为三个阶段-准备框架，扩展框架和添加控制信号。

**准备框架** 基本上意味着我们正在将框架转换为 OpenCV可用形式。

首先，我们需要确保 :code:`previewout` 对流中的数据包进行操作，因此它是4K彩色摄像机的帧。

接下来，我们从数据包中获取数据，并将其从 :code:`CHW` DepthAI使用的（Channel，Height，Width）形式转换为
:code:`HWC` OpenCV使用的（Height，Width，Channel）形式。

.. code-block:: python

  for packet in data_packets:
      if packet.stream_name == 'previewout':
          data = packet.getData()  # e.x. shape (3, 300, 300)
          data0 = data[0, :, :]
          data1 = data[1, :, :]
          data2 = data[2, :, :]
          frame = cv2.merge([data0, data1, data2])  # e.x. shape (300, 300, 3)

**对框架进行扩展** 意味着可以更改所显示内容的任何过程。在此示例中，我在检测到的项目周围添加了红色矩形。您还可以在此处添加文本显示，延迟信息-基本上可以满足您的业务逻辑要求。

由于边界框的位置是从神经网络返回的范围 :code:`(0, 1)`,
中的浮点数，它指定了点相对于它的宽度/高度的位置，所以我们需要将它转化为图像上的实际点(你可以看到我们在做e.x. :code:`int(detection.x_min * img_w)` )。

接下来，使用 :code:`cv2.rectangle`,我们在 :code:`frame` 上打印实际的矩形。最后，当框架准备好后，我们使用 :code:`cv2.imshow` 函数显示它。

.. code-block:: python

  img_h = frame.shape[0]
  img_w = frame.shape[1]

  for detection in detections:
      pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
      pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

      cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

  cv2.imshow('previewout', frame)

**添加控制信号** 是最后一部分，在这里你可以给显示的图像添加交互性。 我们只增加一个命令–当你按下 :code:`q` 按钮时终止程序。

.. code-block:: python

  if cv2.waitKey(1) == ord('q'):
      break


.. include::  /pages/includes/footer-short.rst
