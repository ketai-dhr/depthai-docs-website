Gen2 设备队列事件
==============================

此示例演示如何使用 getQueueEvent 函数，以便在选定流中的一个数据包到达时得到通知

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=971880194&bvid=BV1Vp4y1H766&cid=304669815&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/host_side/device_queue_event.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/host_side/device_queue_event.py>`_ 上找到。

.. code-block:: python

    import cv2
    import depthai as dai
    import numpy as np

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色和黑白相机流
    cam_rgb = pipeline.createColorCamera()
    cam_mono = pipeline.createMonoCamera()
    # 分别为他们创建输出流
    xout_rgb = pipeline.createXLinkOut()
    xout_mono = pipeline.createXLinkOut()

    # 设置属性
    xout_rgb.setStreamName("rgb")
    xout_mono.setStreamName("mono")
    # 设置彩色摄像机为5 fps
    cam_rgb.setFps(5)
    cam_rgb.setInterleaved(True)
    cam_rgb.setPreviewSize(300, 300)

    # 连接
    cam_rgb.preview.link(xout_rgb.input)
    cam_mono.out.link(xout_mono.input)


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 清除队列事件
        device.getQueueEvents()

        while True:
            # 阻塞，直到消息到达任何指定的队列
            queueName = device.getQueueEvent(("rgb", "mono"))

            # 尝试从队列中获取事件指定名称的消息
            # 注意：事件数量不一定与队列中的消息数量匹配
            # 因为可以将队列设置为非阻塞（覆盖）行为
            message = device.getOutputQueue(queueName).tryGet()

            # 分别处理
            if queueName == "rgb" and type(message) == dai.ImgFrame :
                frame_rgb = message.getData().reshape(message.getHeight(), message.getWidth(), 3)
                frame_rgb = np.ascontiguousarray(frame_rgb)
                cv2.imshow("rgb", frame_rgb)
            elif queueName == "mono" and type(message) == dai.ImgFrame :
                cv2.imshow("mono", message.getData().reshape((message.getHeight(), message.getWidth())))

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst