Gen2 彩色相机编码&灰度相机深度流&运行MobilenetSSD模型
=============================================================================

这个例子显示了如何配置 depthai 视频编码器在 h. 265格式编码 RGB 摄像机输入在30 FPS 的全高清分辨率，并传输编码的视频通过 XLINK 到主机，保存到磁盘作为视频文件。同时，一个 mobileetv2ssd 网络在右灰度摄像机的帧上运行，同时该应用程序还显示两个灰度摄像机产生的深度图。请注意，在这种情况下使用了差异，因为它以更直观的方式着色。

按 Ctrl + c 将停止程序，然后使用 ffmpeg 将其转换为 mp4，使其可播放。请注意，ffmpeg 需要安装并运行才能成功转换为 mp4。

注意，此示例将编码视频保存到主机存储中。因此，如果您让它们一直运行，您可以填满您的主机上的存储空间。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=714429058&bvid=BV1FX4y1V7Z7&cid=304662666&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
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

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/mixed/rgb_encoding_mono_mobilenet_depth.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/mixed/rgb_encoding_mono_mobilenet_depth.py>`_ 上找到。

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

    left = pipeline.createMonoCamera()
    left.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    left.setBoardSocket(dai.CameraBoardSocket.LEFT)

    right = pipeline.createMonoCamera()
    right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    right.setBoardSocket(dai.CameraBoardSocket.RIGHT)

    depth = pipeline.createStereoDepth()
    depth.setConfidenceThreshold(200)
    # 注意：默认情况下，校准后的流是水平镜像的
    depth.setOutputRectified(True)
    depth.setRectifyEdgeFillColor(0) # 黑色，以更好地看到切口
    left.out.link(depth.left)
    right.out.link(depth.right)

    detection_nn = pipeline.createNeuralNetwork()
    detection_nn.setBlobPath(mobilenet_path)

    xout_depth = pipeline.createXLinkOut()
    xout_depth.setStreamName("depth")
    depth.disparity.link(xout_depth.input)

    xout_right = pipeline.createXLinkOut()
    xout_right.setStreamName("rect_right")
    depth.rectifiedRight.link(xout_right.input)

    manip = pipeline.createImageManip()
    manip.initialConfig.setResize(300, 300)
    # NN模型需要BGR输入。默认情况下，ImageManip输出类型将与输入相同（在这种情况下为灰色）
    manip.initialConfig.setFrameType(dai.RawImgFrame.Type.BGR888p)
    depth.rectifiedRight.link(manip.inputImage)
    manip.out.link(detection_nn.input)

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

        q_right = device.getOutputQueue(name="rect_right", maxSize=8, blocking=False)
        q_manip = device.getOutputQueue(name="manip", maxSize=8, blocking=False)
        q_depth = device.getOutputQueue(name="depth", maxSize=8, blocking=False)
        q_nn = device.getOutputQueue(name="nn", maxSize=8, blocking=False)
        q_rgb_enc = device.getOutputQueue(name="h265", maxSize=30, blocking=True)

        frame_right = None
        frame_manip = None
        frame_depth = None
        bboxes = []
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
            in_depth = q_depth.tryGet()

            while q_rgb_enc.has():
                q_rgb_enc.get().getData().tofile(videoFile)

            if in_right is not None:
                shape = (in_right.getHeight(), in_right.getWidth())
                frame_right = in_right.getData().reshape(shape).astype(np.uint8)
                frame_right = np.ascontiguousarray(frame_right)

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
                bboxes = bboxes[:, 3:7]

            if in_depth is not None:
                frame_depth = in_depth.getData().reshape((in_depth.getHeight(), in_depth.getWidth())).astype(np.uint8)
                frame_depth = np.ascontiguousarray(frame_depth)
                frame_depth = cv2.applyColorMap(frame_depth, cv2.COLORMAP_JET)

            if frame_right is not None:
                for raw_bbox, label in zip(bboxes, labels):
                    bbox = frame_norm(frame_right, raw_bbox)
                    cv2.rectangle(frame_right, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame_right, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("rectif_right", frame_right)

            if frame_manip is not None:
                for raw_bbox, label in zip(bboxes, labels):
                    bbox = frame_norm(frame_manip, raw_bbox)
                    cv2.rectangle(frame_manip, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame_manip, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("manip", frame_manip)

            if frame_depth is not None:
                for raw_bbox, label in zip(bboxes, labels):
                    bbox = frame_norm(frame_depth, raw_bbox)
                    cv2.rectangle(frame_depth, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (0, 0, 255), 2)
                    cv2.putText(frame_depth, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, (0, 0, 255))
                cv2.imshow("depth", frame_depth)

            if cv2.waitKey(1) == ord('q'):
                break

        videoFile.close()

        print("To view the encoded data, convert the stream file (.h265) into a video file (.mp4) using a command below:")
        print("ffmpeg -framerate 30 -i video.h265 -c copy video.mp4")

.. include::  /pages/includes/footer-short.rst