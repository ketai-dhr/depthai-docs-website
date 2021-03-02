Gen2 保存灰度相机全分辨率图片
===============================

本示例说明如何将正确的将灰度相机的1280x720p .png图片保存到磁盘。

演示
******************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=929478905&bvid=BV1HK4y1D7Zp&cid=304661874&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
*********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

    python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`python API 安装指南 <Python API安装详解>`

源代码
********************

也可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/07_mono_full_resolution_saver.py>`_ 上找到。

.. code-block:: python

    import time
    from pathlib import Path

    import cv2
    import depthai as dai
    import numpy as np

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建灰度相机流
    cam_right = pipeline.createMonoCamera()
    cam_right.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    cam_right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    # 创建输出
    xout_right = pipeline.createXLinkOut()
    xout_right.setStreamName("right")
    cam_right.out.link(xout_right.input)

    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取灰度帧
        q_right = device.getOutputQueue(name="right", maxSize=4, blocking=False)

        # 开始存储示例之前，请确保目标路径存在
        Path('07_data').mkdir(parents=True, exist_ok=True)

        while True:
            in_right = q_right.get()  # 阻止呼叫，将等待直到新数据到达
            # 数据最初表示为1维数组，需要将其转换为HxW形式
            shape = (in_right.getHeight(), in_right.getWidth())
            frame_right = in_right.getData().reshape(shape).astype(np.uint8)
            frame_right = np.ascontiguousarray(frame_right)
            # 图像已转换并使用OpenCV的imshow方法显示
            cv2.imshow("right", frame_right)
            # 显示框架后，将其作为PNG图像存储在目标目录中
            cv2.imwrite(f"07_data/{int(time.time() * 10000)}.png", frame_right)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst