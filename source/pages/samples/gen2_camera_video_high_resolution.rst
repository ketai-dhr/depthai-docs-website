Gen2 摄像头视频高分辨率
==========================

本示例说明如何以低延迟使用高分辨率视频。与 :ret:`Gen2 彩色相机Preview流 <Gen2 彩色相机Preview流>`_ 相比，此演示输出NV12帧，而彩色相机流示例帧是BGR，不适合较大的分辨率(例如2000*1000)。彩色相机流示例更适合于NN或可视化目的。

演示
*****************

设置
********************

.. warning::
    说明：此处安装的是第二代depthai库

请运行以下命令来安装所需的依赖项

.. code-block:: bash

    python3 -m pip install -U pip
    python3 -m pip install opencv-python
    python3 -m pip install -U --force-reinstall depthai

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
*********************

也可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/28_camera_video_example.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import cv2
    import depthai as dai
    import numpy as np

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 定义彩色相机
    colorCam = pipeline.createColorCamera()
    colorCam.setBoardSocket(dai.CameraBoardSocket.RGB)
    colorCam.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)
    colorCam.setVideoSize(1920, 1080)

    # 创建输出流
    xoutVideo = pipeline.createXLinkOut()
    xoutVideo.setStreamName("video")
    xoutVideo.input.setBlocking(False)
    xoutVideo.input.setQueueSize(1)

    colorCam.video.link(xoutVideo.input)

    # 连接并启动管道
    with dai.Device(pipeline) as device:

        video = device.getOutputQueue(name="video", maxSize=1, blocking=False)

        while True:
            # 获取预览和视频帧
            videoIn = video.get()

            # 从NV12编码的视频帧中获取BGR帧以使用opencv进行显示
            # 在较慢的主机上可视化框架可能会产生开销
            cv2.imshow("video", videoIn.getCvFrame())

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst