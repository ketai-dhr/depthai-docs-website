Gen2 彩色相机编码
=============================

本示例说明如何将h.265格式的depthai视频编码器配置为以30FPS编码8MP / 4K / 2160p（3840x2160）的RGB摄像机输入（编码器可能的最大编码分辨率，更高的帧速率为较低的分辨率，例如60FPS时的1440p），并通过XLINK将编码后的视频传输到主机，并将其作为视频文件保存到磁盘上。

按Ctrl + C将停止录制，然后使用ffmpeg将其转换为mp4以使其可播放。请注意，必须成功安装并运行ffmpeg才能成功转换为mp4。

请注意，此示例将编码的视频保存到主机存储中。因此，如果让它们继续运行，则可以在主机上填满存储。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=886901788&bvid=BV15K4y1J74X&cid=304662892&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
*******************

请运行以下命令以安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参考 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
***********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/VideoEncoder/rgb_encoding.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/VideoEncoder/rgb_encoding.py>`_ 上找到。

.. code-block:: python 

    import depthai as dai

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 定义彩色相机流
    cam = pipeline.createColorCamera()
    cam.setBoardSocket(dai.CameraBoardSocket.RGB)
    cam.setResolution(dai.ColorCameraProperties.SensorResolution.THE_4_K)

    # 创建一个编码器，使用这些帧并使用H.265编码对其进行编码
    videoEncoder = pipeline.createVideoEncoder()
    videoEncoder.setDefaultProfilePreset(3840, 2160, 30, dai.VideoEncoderProperties.Profile.H265_MAIN)
    cam.video.link(videoEncoder.input)

    # 创建输出
    videoOut = pipeline.createXLinkOut()
    videoOut.setStreamName('h265')
    videoEncoder.bitstream.link(videoOut.input)

    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取编码数据
        q = device.getOutputQueue(name="h265", maxSize=30, blocking=True)

        # .h265文件是原始流文件（尚无法播放）
        with open('video.h265','wb') as videoFile:
            print("Press Ctrl+C to stop encoding...")
            try:
                while True:
                    h264Packet = q.get()  # 阻止请求，将等待直到新数据到达
                    h264Packet.getData().tofile(videoFile)  # 将数据包数据添加到打开的文件中
            except KeyboardInterrupt:
                # 按Ctrl + C退出程序
                pass

        print("To view the encoded data, convert the stream file (.h265) into a video file (.mp4) using a command below:")
        print("ffmpeg -framerate 30 -i video.h265 -c copy video.mp4")

.. include::  /pages/includes/footer-short.rst