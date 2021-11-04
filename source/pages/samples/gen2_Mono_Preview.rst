Gen2 灰度相机Preview流
==============================

这个例子展示了如何设置一个输出左右灰度相机图像的管道，通过 XLink 连接以实时传输这些图像到主机，并使用 OpenCV 显示这两个图像。

展示
*********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=756946784&bvid=BV1Sr4y1A7zT&cid=304668980&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
**********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
**************************

以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/MonoCamera/mono_preview.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/MonoCamera/mono_preview.py>`_ 上找到。

.. code-block:: python

    import cv2
    import depthai as dai
    import numpy as np

    # 定义管道
    pipeline = dai.Pipeline()

    # 创建左右两个灰度相机流
    cam_left = pipeline.createMonoCamera()
    cam_left.setBoardSocket(dai.CameraBoardSocket.LEFT)
    cam_left.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    cam_right = pipeline.createMonoCamera()
    cam_right.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    cam_right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    # 创建输出流
    xout_left = pipeline.createXLinkOut()
    xout_left.setStreamName('left')
    cam_left.out.link(xout_left.input)
    xout_right = pipeline.createXLinkOut()
    xout_right.setStreamName('right')
    cam_right.out.link(xout_right.input)

    # 管道已创建，现在将设备连接管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取灰度帧
        q_left = device.getOutputQueue(name="left", maxSize=4, blocking=False)
        q_right = device.getOutputQueue(name="right", maxSize=4, blocking=False)

        frame_left = None
        frame_right = None

        while True:
            # 代替使用get的tryGet方法，它将返回可用数据，否则返回None
            in_left = q_left.tryGet()
            in_right = q_right.tryGet()

            if in_left is not None:
                # 如果左摄像机的数据不为空，则将一维数据转换为一帧
                frame_left = in_left.getData().reshape((in_left.getHeight(), in_left.getWidth())).astype(np.uint8)
                frame_left = np.ascontiguousarray(frame_left)

            if in_right is not None:
                # 如果右摄像机的数据不为空，则将一维数据转换为一帧
                frame_right = in_right.getData().reshape((in_right.getHeight(), in_right.getWidth())).astype(np.uint8)
                frame_right = np.ascontiguousarray(frame_right)

            # 使用OpenCV将图像显示出来
            if frame_left is not None:
                cv2.imshow("left", frame_left)
            if frame_right is not None:
                cv2.imshow("right", frame_right)

            # 按'q'退出程序
            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst