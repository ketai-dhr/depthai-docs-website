Gen2 视频上的对象跟踪器
=================================

本示例说明如何在视频输入帧上运行MobileNetv2SSD，以及如何对人员执行对象跟踪。

演示
*****************

设置
********************

.. warning::
    说明：此处安装的是第二代depthai库

请运行以下命令来安装所需的依赖项

.. code-block:: bash

    python3 -m pip install -U pip
    python3 -m pip install opencv-python
    python3 -m pip install -U --force-reinstall depthai

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个示例还需要 mobileenetsdd blob ( :code:`person-detection-0201_openvino_2021.3_7shave.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/person-detection-0201_openvino_2021.3_7shave.blob>`_ 下载它。

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/ObjectTracker/object_tracker_video.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/ObjectTracker/object_tracker_video.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    from pathlib import Path
    import cv2
    import depthai as dai
    import numpy as np
    import time
    import argparse

    labelMap = ["person", ""]

    nnPathDefault = str((Path(__file__).parent / Path('models/person-detection-0201_openvino_2021.3_7shave.blob')).resolve().absolute())
    videoPathDefault = str((Path(__file__).parent / Path('models/construction_vest.mp4')).resolve().absolute())
    parser = argparse.ArgumentParser()
    parser.add_argument('-nnPath', help="Path to mobilenet detection network blob", default=nnPathDefault)
    parser.add_argument('-v', '--videoPath', help="Path to video frame", default=videoPathDefault)

    args = parser.parse_args()

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建神经网络输入
    xinFrame = pipeline.createXLinkIn()
    xinFrame.setStreamName("inFrame")
    xinFrame.setMaxDataSize(1920*1080*3)

    detectionNetwork = pipeline.createMobileNetDetectionNetwork()
    objectTracker = pipeline.createObjectTracker()
    trackerOut = pipeline.createXLinkOut()

    xlinkOut = pipeline.createXLinkOut()

    xlinkOut.setStreamName("trackerFrame")
    trackerOut.setStreamName("tracklets")

    # 创建一个节点，将灰度帧转换为nn可接受的形式
    manip = pipeline.createImageManip()
    manip.initialConfig.setResizeThumbnail(384, 384)
    # manip.initialConfig.setResize(384, 384)
    # manip.initialConfig.setKeepAspectRatio(False) #squash the image to not lose FOV
    # NN模型需要BGR输入。默认情况下，ImageManip输出类型将与输入相同（在这种情况下为灰色）
    manip.initialConfig.setFrameType(dai.ImgFrame.Type.BGR888p)
    xinFrame.out.link(manip.inputImage)
    manip.inputImage.setBlocking(True)

    manipOut = pipeline.createXLinkOut()
    manipOut.setStreamName("manip")
    manip.out.link(manipOut.input)

    nnOut = pipeline.createXLinkOut()
    nnOut.setStreamName("nn")
    detectionNetwork.out.link(nnOut.input)


    # 设置节点配置
    detectionNetwork.setBlobPath(args.nnPath)
    detectionNetwork.setConfidenceThreshold(0.5)

    manip.out.link(detectionNetwork.input)
    detectionNetwork.input.setBlocking(True)
    objectTracker.passthroughTrackerFrame.link(xlinkOut.input)


    objectTracker.setDetectionLabelsToTrack([0])  # 只跟踪人
    # 可能的跟踪类型：ZERO_TERM_COLOR_HISTOGRAM，ZERO_TERM_IMAGELESS
    objectTracker.setTrackerType(dai.TrackerType.ZERO_TERM_COLOR_HISTOGRAM)
    # 跟踪新对象时采用最小的ID，可能的选项：SMALLEST_ID，UNIQUE_ID
    objectTracker.setTrackerIdAssigmentPolicy(dai.TrackerIdAssigmentPolicy.SMALLEST_ID)

    xinFrame.out.link(objectTracker.inputTrackerFrame)
    objectTracker.inputTrackerFrame.setBlocking(True)
    detectionNetwork.passthrough.link(objectTracker.inputDetectionFrame)
    objectTracker.inputDetectionFrame.setBlocking(True)
    detectionNetwork.out.link(objectTracker.inputDetections)
    objectTracker.inputDetections.setBlocking(True)
    objectTracker.out.link(trackerOut.input)


    # 连接并启动管道
    with dai.Device(pipeline) as device:

        qIn = device.getInputQueue(name="inFrame")
        trackerFrameQ = device.getOutputQueue("trackerFrame", 4)
        tracklets = device.getOutputQueue("tracklets", 4)
        qManip = device.getOutputQueue("manip", maxSize=4)
        qDet = device.getOutputQueue("nn", maxSize=4)

        startTime = time.monotonic()
        counter = 0
        fps = 0
        detections = []
        frame = None

        def to_planar(arr: np.ndarray, shape: tuple) -> np.ndarray:
            return cv2.resize(arr, shape).transpose(2, 0, 1).flatten()

        #nn数据（作为边界框的位置）在<0..1>范围内-需要使用框架宽度/高度对其进行归一化
        def frameNorm(frame, bbox):
            normVals = np.full(len(bbox), frame.shape[0])
            normVals[::2] = frame.shape[1]
            return (np.clip(np.array(bbox), 0, 1) * normVals).astype(int)

        def displayFrame(name, frame):
            for detection in detections:
                bbox = frameNorm(frame, (detection.xmin, detection.ymin, detection.xmax, detection.ymax))
                cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                cv2.putText(frame, labelMap[detection.label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.putText(frame, f"{int(detection.confidence * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
            cv2.imshow(name, frame)

        cap = cv2.VideoCapture(args.videoPath)
        baseTs = time.monotonic()
        simulatedFps = 30
        inputFrameShape = (1280, 720)

        while cap.isOpened():
            read_correctly, frame = cap.read()
            if not read_correctly:
                break

            img = dai.ImgFrame()
            img.setType(dai.ImgFrame.Type.BGR888p)
            img.setData(to_planar(frame, inputFrameShape))
            img.setTimestamp(baseTs)
            baseTs += 1/simulatedFps

            img.setWidth(inputFrameShape[0])
            img.setHeight(inputFrameShape[1])
            qIn.send(img)

            trackFrame = trackerFrameQ.tryGet()
            if trackFrame is None:
                continue

            track = tracklets.get()
            manip = qManip.get()
            inDet = qDet.get()

            counter+=1
            current_time = time.monotonic()
            if (current_time - startTime) > 1 :
                fps = counter / (current_time - startTime)
                counter = 0
                startTime = current_time

            detections = inDet.detections
            manipFrame = manip.getCvFrame()
            displayFrame("nn", manipFrame)

            color = (255, 0, 0)
            trackerFrame = trackFrame.getCvFrame()
            trackletsData = track.tracklets
            for t in trackletsData:
                roi = t.roi.denormalize(trackerFrame.shape[1], trackerFrame.shape[0])
                x1 = int(roi.topLeft().x)
                y1 = int(roi.topLeft().y)
                x2 = int(roi.bottomRight().x)
                y2 = int(roi.bottomRight().y)

                try:
                    label = labelMap[t.label]
                except:
                    label = t.label

                cv2.putText(trackerFrame, str(label), (x1 + 10, y1 + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(trackerFrame, f"ID: {[t.id]}", (x1 + 10, y1 + 35), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(trackerFrame, t.status.name, (x1 + 10, y1 + 50), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.rectangle(trackerFrame, (x1, y1), (x2, y2), color, cv2.FONT_HERSHEY_SIMPLEX)

            cv2.putText(trackerFrame, "Fps: {:.2f}".format(fps), (2, trackerFrame.shape[0] - 4), cv2.FONT_HERSHEY_TRIPLEX, 0.4, color)

            cv2.imshow("tracker", trackerFrame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst