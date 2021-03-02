Gen2 最大编码限制
===========================

这个例子展示了如何设置编码器节点来同时编码 RGB 摄像机和两个灰度摄像机(deptai/OAK-D) ，将所有编码器参数设置为最大质量和 FPS。RGB 被设置为4K (3840x2160) ，灰度被设置为1280x720，每个都是30fps。每个编码的视频流通过 XLINK 传输并保存到各自的文件中。

按 Ctrl + c 将停止程序，然后使用 ffmpeg 将其转换为 mp4，使其可播放。请注意，ffmpeg 需要安装并运行才能成功转换为 mp4。

注意，此示例将编码视频保存到主机存储中。因此，如果您让它们一直运行，您可以填满您的主机上的存储空间。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=459445238&bvid=BV1M5411K7r2&cid=304672231&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
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

也可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/13_encoding_max_limit.py>`_ 上找到。

.. code-block:: python

    import depthai as dai

    pipeline = dai.Pipeline()

    # 创建节点流
    colorCam = pipeline.createColorCamera()
    colorCam.setResolution(dai.ColorCameraProperties.SensorResolution.THE_4_K)
    monoCam = pipeline.createMonoCamera()
    monoCam2 = pipeline.createMonoCamera()
    ve1 = pipeline.createVideoEncoder()
    ve2 = pipeline.createVideoEncoder()
    ve3 = pipeline.createVideoEncoder()

    ve1Out = pipeline.createXLinkOut()
    ve2Out = pipeline.createXLinkOut()
    ve3Out = pipeline.createXLinkOut()

    # 设置流属性
    monoCam.setBoardSocket(dai.CameraBoardSocket.LEFT)
    monoCam2.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    ve1Out.setStreamName('ve1Out')
    ve2Out.setStreamName('ve2Out')
    ve3Out.setStreamName('ve3Out')

    # 设置为26fps将触发错误
    ve1.setDefaultProfilePreset(1280, 720, 25, dai.VideoEncoderProperties.Profile.H264_MAIN)
    ve2.setDefaultProfilePreset(3840, 2160, 25, dai.VideoEncoderProperties.Profile.H265_MAIN)
    ve3.setDefaultProfilePreset(1280, 720, 25, dai.VideoEncoderProperties.Profile.H264_MAIN)

    # 链接节点
    monoCam.out.link(ve1.input)
    colorCam.video.link(ve2.input)
    monoCam2.out.link(ve3.input)

    ve1.bitstream.link(ve1Out.input)
    ve2.bitstream.link(ve2Out.input)
    ve3.bitstream.link(ve3Out.input)


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as dev:

        # 获取数据输出队列
        outQ1 = dev.getOutputQueue('ve1Out', maxSize=30, blocking=True)
        outQ2 = dev.getOutputQueue('ve2Out', maxSize=30, blocking=True)
        outQ3 = dev.getOutputQueue('ve3Out', maxSize=30, blocking=True)

        # 启动管道
        dev.startPipeline()

        # 处理循环
        with open('mono1.h264', 'wb') as file_mono1_h264, open('color.h265', 'wb') as file_color_h265, open('mono2.h264', 'wb') as file_mono2_h264:
            print("Press Ctrl+C to stop encoding...")
            while True:
                try:
                    # 清空每个队列
                    while outQ1.has():
                        outQ1.get().getData().tofile(file_mono1_h264)

                    while outQ2.has():
                        outQ2.get().getData().tofile(file_color_h265)

                    while outQ3.has():
                        outQ3.get().getData().tofile(file_mono2_h264)
                except KeyboardInterrupt:
                    break

        print("To view the encoded data, convert the stream file (.h264/.h265) into a video file (.mp4), using commands below:")
        cmd = "ffmpeg -framerate 25 -i {} -c copy {}"
        print(cmd.format("mono1.h264", "mono1.mp4"))
        print(cmd.format("mono2.h264", "mono2.mp4"))
        print(cmd.format("color.h265", "color.mp4"))

.. include::  /pages/includes/footer-short.rst