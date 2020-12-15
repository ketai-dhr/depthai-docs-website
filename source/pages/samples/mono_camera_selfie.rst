单声道相机自拍器
=========================

此示例需要运行 `TK library <https://docs.oracle.com/cd/E88353_01/html/E37842/libtk-3.html>`__ (用于打开文件对话框)

<<<<<<< HEAD
还需要人脸检测模型，请查看 :ref:`这个教程 <教程-本地OpenVINO模型转换>` 学习如何编译一个。

运行此示例需要 **双目相机** , 它可以是 :ref:`BW1097 - RaspberryPi 计算模块` , :ref:`BW1098OBC - USB3 板载相机` 或使用 :ref:`DepthAI Mono 相机` 自定义设置。
=======
It also requires face detection model, see :ref:`this tutorial <本地 OpenVINO 模型转换>` to see how to compile one

**Stereo camera pair is required** to run this example, it can either be :ref:`BW1097 - RaspberryPi 计算模块`, :ref:`BW1098OBC - 带机载摄像头的 USB3` or any custom setup using :ref:`DepthAI 单目相机`
>>>>>>> 更新 faq 翻译

演示
####

**捕获过程**

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//www.youtube.com/embed/tMWKcIM74CA" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

**捕获图片**

.. image:: /_static/images/samples/face_mono_selfie.png
  :alt: captured

源代码
###########

.. code-block:: python

  import cv2
  import depthai

  device = depthai.Device('', False)

  pipeline = device.create_pipeline(config={
      'streams': ['left', 'right', 'metaout'],
      'ai': {
          "blob_file": "/path/to/face-detection-retail-0004.blob",
          "blob_file_config": "/path/to/face-detection-retail-0004.json",
      },
      'camera': {'mono': {'resolution_h': 720, 'fps': 30}},
  })

  if pipeline is None:
      raise RuntimeError('Pipeline creation failed!')

  detections = []
  face_frame_left = None
  face_frame_right = None

  while True:
      nnet_packets, data_packets = pipeline.get_available_nnet_and_data_packets()

      for nnet_packet in nnet_packets:
          detections = list(nnet_packet.getDetectedObjects())

      for packet in data_packets:
          if packet.stream_name == 'left' or packet.stream_name == 'right':
              frame = packet.getData()

              img_h = frame.shape[0]
              img_w = frame.shape[1]

              for detection in detections:
                  left = int(detection.x_min * img_w)
                  top = int(detection.y_min * img_h)
                  right = int(detection.x_max * img_w)
                  bottom = int(detection.y_max * img_h)

                  face_frame = frame[top:bottom, left:right]
                  if face_frame.size == 0:
                      continue
                  cv2.imshow(f'face-{packet.stream_name}', face_frame)
                  if packet.stream_name == 'left':
                      face_frame_left = face_frame
                  else:
                      face_frame_right = face_frame

      key = cv2.waitKey(1)
      if key == ord('q'):
          break
      if key == ord(' ') and face_frame_left is not None and face_frame_right is not None:
          from tkinter import Tk, messagebox
          from tkinter.filedialog import asksaveasfilename
          Tk().withdraw()
          filename = asksaveasfilename(defaultextension=".png", filetypes=(("Image files", "*.png"),("All Files", "*.*")))
          joined_frame = cv2.hconcat([face_frame_left, face_frame_right])
          cv2.imwrite(filename, joined_frame)
          messagebox.showinfo("Success", "Image saved successfully!")
          Tk().destroy()

  del pipeline
  del device

说明
###########

.. warning::

  **New to the DepthAI?**

  DepthAI 的基础知识在 :ref:`示例-访问DepthAI相机所需的最少代码` and :ref:`Hello World` 中有解释。


我们的网络返回它检测到的面的边界框（将它们存储在 :code:`detections` 数组中）。
因此，在此示例中，我们必须做两件事： **裁剪框架** 以仅包含脸部并将其 **保存** 到用户指定的位置。

表演作物
*******************

**裁剪框架** 要求我们修改 :ref:`示例-访问DepthAI相机所需的最少代码` 的代码, 这样我们就不会产生矩形的两个点，而是需要全部四个点：其中两个点决定裁剪的开始（ :code:`top` 开始Y轴裁剪， :code:`left` 开始X轴裁剪），另外两个点作为裁剪的结束（ :code:`bottom` 结束Y轴裁剪， :code:`right` 结束X轴裁剪）。

.. code-block:: python

  left = int(detection.x_min * img_w)
  top = int(detection.y_min * img_h)
  right = int(detection.x_max * img_w)
  bottom = int(detection.y_max * img_h)

现在，由于我们的帧是 :code:`HWC` f格式（高度、宽度、通道），我们首先裁剪Y轴（高度），然后裁剪X轴（宽度）。所以裁剪代码是这样的:

.. code-block:: python

  face_frame = frame[top:bottom, left:right]

现在，还有一件事要做。因为有时网络可能会产生这样的边界框，当被裁剪后会产生一个空框，我们必须保证自己不受这种情况的影响，因为如果在空框的情况下调用 :code:`cv2.imshow` 会抛出一个错误。

.. code-block:: python

  if face_frame.size == 0:
      continue
  cv2.imshow('face', face_frame)

稍后，由于我们有两台相机同时工作，我们将把显示的画面分配给左面或右面画面变量，这将有助于我们以后保存图像。

.. code-block:: python

  if packet.stream_name == 'left':
      face_frame_left = face_frame
  else:
      face_frame_right = face_frame

保存帧
*****************

为了 **保存图片** 我们需要两步:

- 将左右两台相机的脸部画面合并为一帧。
- 将准备好的框架保存到磁盘中。

值得庆幸的是，OpenCV已经把这一切都解决了，所以对于每一个点，我们只用一行代码就可以了。 调用 :code:`cv2.hconcat` 进行帧合并，调用 :code:`cv2.imwrite` 存储图像。

其余的代码，利用 :code:`tkinter` 包, 是可选的，如果你不需要用户交互来保存帧，可以删除。

在这个例子中，我们使用 :code:`tkinter` 来保存两个对话框：

- 获取目标文件路径(存储为 :code:`filepath` ),允许我们调用 :code:`cv2.imwrite` ，因为它需要路径作为第一个参数。
- 确认文件已成功保存。

.. code-block:: python

    key = cv2.waitKey(1)
    if key == ord('q'):
        break
    if key == ord(' ') and face_frame_left is not None and face_frame_right is not None:
        from tkinter import Tk, messagebox
        from tkinter.filedialog import asksaveasfilename
        Tk().withdraw()
        filename = asksaveasfilename(defaultextension=".png", filetypes=(("Image files", "*.png"),("All Files", "*.*")))
        joined_frame = cv2.hconcat([face_frame_left, face_frame_right])
        cv2.imwrite(filename, joined_frame)
        messagebox.showinfo("Success", "Image saved successfully!")
        Tk().destroy()



.. include::  /pages/includes/footer-short.rst

