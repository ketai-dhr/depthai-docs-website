Gen2 彩色相机和灰度相机编码
===============================

本示例说明如何设置编码器节点，以同时对RGB摄像机和两个灰度摄像机（DepthAI / OAK-D）进行编码。RGB均设置为1920x1080，灰度均设置为1280x720，所有这些均以30FPS为单位。每个编码的视频流都通过XLINK传输并保存到相应的文件中。

按Ctrl + C将停止程序，然后使用ffmpeg将其转换为mp4以使其可播放。请注意，必须成功安装并运行ffmpeg才能成功转换为mp4。

请注意，此示例将编码的视频保存到主机存储中。因此，如果让它们继续运行，则可以在主机上填满存储。

演示
******************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=289407365&bvid=BV1uf4y147Vr&cid=304667747&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
****************

请运行以下命令以安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

    python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai == 0.0.2.1 + c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy == 1.19.5 opencv-python = = 4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
***********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/05_rgb_mono_encoding.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/05_rgb_mono_encoding.py>`_ 上找到。

.. code-block:: python

    import depthai as dai

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色和灰度相机流
    colorCam = pipeline.createColorCamera()
    monoCam = pipeline.createMonoCamera()
    monoCam.setBoardSocket(dai.CameraBoardSocket.LEFT)
    monoCam2 = pipeline.createMonoCamera()
    monoCam2.setBoardSocket(dai.CameraBoardSocket.RIGHT)

    # 创建一个编码器，每个摄像机一个，使用帧并使用H.264 H.265编码对其进行编码
    ve1 = pipeline.createVideoEncoder()
    ve1.setDefaultProfilePreset(1280, 720, 30, dai.VideoEncoderProperties.Profile.H264_MAIN)
    monoCam.out.link(ve1.input)

    ve2 = pipeline.createVideoEncoder()
    ve2.setDefaultProfilePreset(1920, 1080, 30, dai.VideoEncoderProperties.Profile.H265_MAIN)
    colorCam.video.link(ve2.input)

    ve3 = pipeline.createVideoEncoder()
    ve3.setDefaultProfilePreset(1280, 720, 30, dai.VideoEncoderProperties.Profile.H264_MAIN)
    monoCam2.out.link(ve3.input)

    # 创建输出
    ve1Out = pipeline.createXLinkOut()
    ve1Out.setStreamName('ve1Out')
    ve1.bitstream.link(ve1Out.input)

    ve2Out = pipeline.createXLinkOut()
    ve2Out.setStreamName('ve2Out')
    ve2.bitstream.link(ve2Out.input)

    ve3Out = pipeline.createXLinkOut()
    ve3Out.setStreamName('ve3Out')
    ve3.bitstream.link(ve3Out.input)



    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as dev:
        # 启动管道
        dev.startPipeline()

        # 输出队列将用于从上面定义的输出中获取编码数据
        outQ1 = dev.getOutputQueue(name='ve1Out', maxSize=30, blocking=True)
        outQ2 = dev.getOutputQueue(name='ve2Out', maxSize=30, blocking=True)
        outQ3 = dev.getOutputQueue(name='ve3Out', maxSize=30, blocking=True)

        # .h264 .h265文件是原始流文件（尚无法播放）
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
                    # 按Ctrl + C退出程序
                    break

        print("To view the encoded data, convert the stream file (.h264/.h265) into a video file (.mp4), using commands below:")
        cmd = "ffmpeg -framerate 30 -i {} -c copy {}"
        print(cmd.format("mono1.h264", "mono1.mp4"))
        print(cmd.format("mono2.h264", "mono2.mp4"))
        print(cmd.format("color.h265", "color.mp4"))

.. include::  /pages/includes/footer-short.rst