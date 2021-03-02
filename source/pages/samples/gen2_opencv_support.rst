Gen2 对OpenCV的支持
===============================

此示例显示了API，该API公开了numpy和OpenCV兼容的图像类型，以便于使用。它使用ColorCamera节点检索BGR交错的“预览”和NV12编码的“视频”帧。两者均使用函数getFrame和getCvFrame进行显示。

演示
***********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=246978620&bvid=BV1tv411h7dL&cid=304668643&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个示例还需要 mobileenetsdd blob ( :code:`mobilenet.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`_ 下载它。


源代码
*********************

也可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/24_opencv_support.py>`_ 上找到。

.. code-block:: python

    import cv2
    import depthai as dai
    import numpy as np

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机流
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(300, 300)
    cam_rgb.setBoardSocket(dai.CameraBoardSocket.RGB)
    cam_rgb.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)
    cam_rgb.setInterleaved(True)
    cam_rgb.setColorOrder(dai.ColorCameraProperties.ColorOrder.BGR)

    # 创建输出
    xout_video = pipeline.createXLinkOut()
    xout_video.setStreamName("video")
    xout_preview = pipeline.createXLinkOut()
    xout_preview.setStreamName("preview")

    cam_rgb.preview.link(xout_preview.input)
    cam_rgb.video.link(xout_video.input)

    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        while True:
            # 获取预览和视频帧
            preview = device.getOutputQueue('preview').get()
            video = device.getOutputQueue('video').get()

            # 按原样显示“预览”图像（格式正确，未复制）
            cv2.imshow("preview", preview.getFrame())
            # 从NV12编码的视频帧中获取BGR帧以使用opencv进行显示
            cv2.imshow("video", video.getCvFrame())

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst