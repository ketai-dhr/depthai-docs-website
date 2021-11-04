Gen2 保存彩色相机全分辨率图片
=============================

本示例将尽最大努力以最快的速度从RGB传感器保存全分辨率3840x2160 .png文件。它作为将高分辨率记录到磁盘上以实现高分辨率地面真实数据的示例。我们最近还添加了以下选项，以保存从ISP读取的10位打包的isp-由ISP处理的YUV420p未压缩帧和原始的BayerRG（R_Gr_Gb_B）。有关此功能的拉取请求，请参见此处。

请注意，此示例将全分辨率.png图片保存到您的主机存储中。因此，如果让它们继续运行，则可以在主机上填满存储。

演示
***************************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=204429166&bvid=BV1Yh411k74u&cid=304661388&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
*************************

请运行以下命令以安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
***********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/VideoEncoder/rgb_full_resolution_saver.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/VideoEncoder/rgb_full_resolution_saver.py>`_ 上找到。

.. code-block:: python

    import time
    from pathlib import Path

    import cv2
    import depthai as dai

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setResolution(dai.ColorCameraProperties.SensorResolution.THE_4_K)

    # 创建RGB输出
    xout_rgb = pipeline.createXLinkOut()
    xout_rgb.setStreamName("rgb")
    cam_rgb.video.link(xout_rgb.input)

    # 创建编码器以生成JPEG图像
    video_enc = pipeline.createVideoEncoder()
    video_enc.setDefaultProfilePreset(cam_rgb.getVideoSize(), cam_rgb.getFps(), dai.VideoEncoderProperties.Profile.MJPEG)
    cam_rgb.video.link(video_enc.input)

    # 创建JPEG输出
    xout_jpeg = pipeline.createXLinkOut()
    xout_jpeg.setStreamName("jpeg")
    video_enc.bitstream.link(xout_jpeg.input)


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取rgb帧
        q_rgb = device.getOutputQueue(name="rgb", maxSize=30, blocking=False)
        q_jpeg = device.getOutputQueue(name="jpeg", maxSize=30, blocking=True)

        # 开始存储示例之前，请确保目标路径存在
        Path('06_data').mkdir(parents=True, exist_ok=True)

        while True:
            in_rgb = q_rgb.tryGet()  # 非阻塞呼叫，将返回已到达的新数据，否则返回None

            if in_rgb is not None:
                # 数据最初表示为1维数组，需要将其转换为HxW形式
                shape = (in_rgb.getHeight() * 3 // 2, in_rgb.getWidth())
                frame_rgb = cv2.cvtColor(in_rgb.getData().reshape(shape), cv2.COLOR_YUV2BGR_NV12)
                # 图像已转换并使用OpenCV的imshow方法显示
                cv2.imshow("rgb", frame_rgb)

            for enc_frame in q_jpeg.tryGetAll():
                with open(f"06_data/{int(time.time() * 10000)}.jpeg", "wb") as f:
                    f.write(bytearray(enc_frame.getData()))

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst