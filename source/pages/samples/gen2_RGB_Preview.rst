Gen2 彩色相机Preview流
==============================

此示例显示了如何建立一个管道，该管道输出RGB摄像头的preview流，如何通过XLink连接以将其实时传输到主机，以及如何使用OpenCV在主机上显示RGB帧。

演示
*******************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=204422388&bvid=BV1zh411k7Xc&cid=304662050&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
*********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
**************************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/01_rgb_preview.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/01_rgb_preview.py>`_ 上找到。

.. code-block:: python

    import cv2
    import depthai as dai
    import numpy as np

    # 定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机流
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(300, 300)
    cam_rgb.setBoardSocket(dai.CameraBoardSocket.RGB)
    cam_rgb.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)
    cam_rgb.setInterleaved(False)
    cam_rgb.setColorOrder(dai.ColorCameraProperties.ColorOrder.RGB)

    # 创建输出流
    xout_rgb = pipeline.createXLinkOut()
    xout_rgb.setStreamName("rgb")
    cam_rgb.preview.link(xout_rgb.input)

    # 管道已创建，现在将设备连接管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取rgb帧
        q_rgb = device.getOutputQueue(name="rgb", maxSize=4, blocking=False)

        while True:
            in_rgb = q_rgb.get()  # 阻止呼叫，将等待直到新数据到达

            # 使用OpenCV将图像显示出来
            cv2.imshow("bgr", in_rgb.getCvFrame())

            # 按'q'退出程序
            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst