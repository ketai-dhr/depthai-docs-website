Gen2 彩色相机编码&灰度相机&运行MobilenetSSD模型
========================================================

这个例子显示了如何配置 depthai 视频编码器在 h. 265格式编码 RGB 摄像机输入在30 FPS 的全高清分辨率，并传输编码的视频通过 XLINK 到主机，保存到磁盘作为视频文件。同时，一个 mobileetv2ssd 网络在右灰度相机的帧上运行

按 Ctrl + c 将停止程序，然后使用 ffmpeg 将其转换为 mp4，使其可播放。请注意，ffmpeg 需要安装并运行才能成功转换为 mp4。

注意，此示例将编码视频保存到主机存储中。因此，如果您让它们一直运行，您可以填满您的主机上的存储空间。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=501895731&bvid=BV1jN41197um&cid=304662423&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
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

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/mixed/rgb_encoding_mono_mobilenet.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/mixed/rgb_encoding_mono_mobilenet.py>`_ 上找到。

.. code-block:: python

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np

    # 首先获取模型
    mobilenet_path = str((Path(__file__).parent / Path('models/mobilenet.blob')).resolve().absolute())
    if len(sys.argv) > 1:
        mobilenet_path = sys.argv[1]


    pipeline = dai.Pipeline()

    cam = pipeline.createColorCamera()
    cam.setBoardSocket(dai.CameraBoardSocket.RGB)
    cam.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)

    # 创建视频编码流
    videoEncoder = pipeline.createVideoEncoder()
    videoEncoder.setDefaultProfilePreset(1920, 1080, 30, dai.VideoEncoderProperties.Profile.H265_MAIN)
    cam.video.link(videoEncoder.input)

    videoOut = pipeline.createXLinkOut()
    videoOut.setStreamName('h265')
    videoEncoder.bitstream.link(videoOut.input)

    cam_right = pipeline.createMonoCamera()
    cam_right.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    cam_right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    detection_nn = pipeline.createNeuralNetwork()
    detection_nn.setBlobPath(mobilenet_path)

    manip = pipeline.createImageManip()
    manip.initialConfig.setResize(300, 300)
    # NN模型需要BGR输入。默认情况下，ImageManip输出类型将与输入相同（在这种情况下为灰色）
    manip.initialConfig.setFrameType(dai.RawImgFrame.Type.BGR888p)
    cam_right.out.link(manip.inputImage)
    manip.out.link(detection_nn.input)

    xout_right = pipeline.createXLinkOut()
    xout_right.setStreamName("right")
    cam_right.out.link(xout_right.input)

    xout_manip = pipeline.createXLinkOut()
    xout_manip.setStreamName("manip")
    manip.out.link(xout_manip.input)

    xout_nn = pipeline.createXLinkOut()
    xout_nn.setStreamName("nn")
    detection_nn.out.link(xout_nn.input)

    # MobilenetSSD标签文本
    texts = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow",
             "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        queue_size = 8
        q_right = device.getOutputQueue("right", queue_size)
        q_manip = device.getOutputQueue("manip", queue_size)
        q_nn = device.getOutputQueue("nn", queue_size)
        q_rgb_enc = device.getOutputQueue('h265', maxSize=30, blocking=True)

        frame = None
        frame_manip = None
        bboxes = []
        confidences = []
        labels = []

        def frame_norm(frame, bbox):
            norm_vals = np.full(len(bbox), frame.shape[0])
            norm_vals[::2] = frame.shape[1]
            return (np.clip(np.array(bbox), 0, 1) * norm_vals).astype(int)

        videoFile = open('video.h265','wb')

        while True:
            in_right = q_right.tryGet()
            in_manip = q_manip.tryGet()
            in_nn = q_nn.tryGet()

            while q_rgb_enc.has():
                q_rgb_enc.get().getData().tofile(videoFile)

            if in_right is not None:
                shape = (in_right.getHeight(), in_right.getWidth())
                frame = in_right.getData().reshape(shape).astype(np.uint8)
                frame = np.ascontiguousarray(frame)

            if in_manip is not None:
                shape = (3, in_manip.getHeight(), in_manip.getWidth())
                frame_manip = in_manip.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
                frame_manip = np.ascontiguousarray(frame_manip)

            if in_nn is not None:
                bboxes = np.array(in_nn.getFirstLayerFp16())
                bboxes = bboxes.reshape((bboxes.size // 7, 7))
                bboxes = bboxes[bboxes[:, 2] > 0.5]
                # 剪切bbox和标签
                labels = bboxes[:, 1].astype(int)
                confidences = bboxes[:, 2]
                bboxes = bboxes[:, 3:7]

            if frame is not None:
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(frame, raw_bbox)
                    cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("right", frame)

            if frame_manip is not None:
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(frame_manip, raw_bbox)
                    cv2.rectangle(frame_manip, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame_manip, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(frame_manip, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("manip", frame_manip)

            if cv2.waitKey(1) == ord('q'):
                break

        videoFile.close()

        print("To view the encoded data, convert the stream file (.h265) into a video file (.mp4) using a command below:")
        print("ffmpeg -framerate 30 -i video.h265 -c copy video.mp4")

.. include::  /pages/includes/footer-short.rst