Gen2 RGB相机上的空间对象跟踪器
=================================

本示例说明如何在RGB输入帧上运行MobileNetv2SSD，以及如何对人员执行空间对象跟踪。

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

这个示例还需要 mobileenetsdd blob ( :code:`mobilenet.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`_ 下载它。

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/29_2_spatial_object_tracker.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/29_2_spatial_object_tracker.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    from pathlib import Path
    import cv2
    import depthai as dai
    import numpy as np
    import time
    import argparse

    labelMap = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow",
                "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]

    nnPathDefault = str((Path(__file__).parent / Path('models/mobilenet-ssd_openvino_2021.2_6shave.blob')).resolve().absolute())
    parser = argparse.ArgumentParser()
    parser.add_argument('nnPath', nargs='?', help="Path to mobilenet detection network blob", default=nnPathDefault)
    parser.add_argument('-ff', '--full_frame', action="store_true", help="Perform tracking on full RGB frame", default=False)

    args = parser.parse_args()


    fullFrameTracking = args.full_frame

    # 开始定义管道
    pipeline = dai.Pipeline()

    colorCam = pipeline.createColorCamera()
    spatialDetectionNetwork = pipeline.createMobileNetSpatialDetectionNetwork()
    monoLeft = pipeline.createMonoCamera()
    monoRight = pipeline.createMonoCamera()
    stereo = pipeline.createStereoDepth()
    objectTracker = pipeline.createObjectTracker()

    xoutRgb = pipeline.createXLinkOut()
    trackerOut = pipeline.createXLinkOut()

    xoutRgb.setStreamName("preview")
    trackerOut.setStreamName("tracklets")

    colorCam.setPreviewSize(300, 300)
    colorCam.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)
    colorCam.setInterleaved(False)
    colorCam.setColorOrder(dai.ColorCameraProperties.ColorOrder.BGR)

    monoLeft.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    monoLeft.setBoardSocket(dai.CameraBoardSocket.LEFT)
    monoRight.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    monoRight.setBoardSocket(dai.CameraBoardSocket.RIGHT)

    # 设置节点配置
    stereo.setConfidenceThreshold(255)

    spatialDetectionNetwork.setBlobPath(args.nnPath)
    spatialDetectionNetwork.setConfidenceThreshold(0.5)
    spatialDetectionNetwork.input.setBlocking(False)
    spatialDetectionNetwork.setBoundingBoxScaleFactor(0.5)
    spatialDetectionNetwork.setDepthLowerThreshold(100)
    spatialDetectionNetwork.setDepthUpperThreshold(5000)

    # 创建输出流

    monoLeft.out.link(stereo.left)
    monoRight.out.link(stereo.right)

    # 连接插件 CAM . NN . XLINK
    colorCam.preview.link(spatialDetectionNetwork.input)
    objectTracker.passthroughTrackerFrame.link(xoutRgb.input)


    objectTracker.setDetectionLabelsToTrack([15])  # 只追踪人
    # 可能的跟踪类型：ZERO_TERM_COLOR_HISTOGRAM，ZERO_TERM_IMAGELESS
    objectTracker.setTrackerType(dai.TrackerType.ZERO_TERM_COLOR_HISTOGRAM)
    # 跟踪新对象时采用最小的ID，可能的选项：SMALLEST_ID，UNIQUE_ID
    objectTracker.setTrackerIdAssigmentPolicy(dai.TrackerIdAssigmentPolicy.SMALLEST_ID)

    objectTracker.out.link(trackerOut.input)
    if fullFrameTracking:
        colorCam.setPreviewKeepAspectRatio(False)
        colorCam.video.link(objectTracker.inputTrackerFrame)
        objectTracker.inputTrackerFrame.setBlocking(False)
        # 如果全帧速度太慢，请不要阻塞管道
        objectTracker.inputTrackerFrame.setQueueSize(2)
    else:
        spatialDetectionNetwork.passthrough.link(objectTracker.inputTrackerFrame)

    spatialDetectionNetwork.passthrough.link(objectTracker.inputDetectionFrame)
    spatialDetectionNetwork.out.link(objectTracker.inputDetections)

    stereo.depth.link(spatialDetectionNetwork.inputDepth)


    # 连接并启动管道
    with dai.Device(pipeline) as device:

        preview = device.getOutputQueue("preview", 4, False)
        tracklets = device.getOutputQueue("tracklets", 4, False)

        startTime = time.monotonic()
        counter = 0
        fps = 0
        frame = None

        while(True):
            imgFrame = preview.get()
            track = tracklets.get()

            counter+=1
            current_time = time.monotonic()
            if (current_time - startTime) > 1 :
                fps = counter / (current_time - startTime)
                counter = 0
                startTime = current_time

            color = (255, 0, 0)
            frame = imgFrame.getCvFrame()
            trackletsData = track.tracklets
            for t in trackletsData:
                roi = t.roi.denormalize(frame.shape[1], frame.shape[0])
                x1 = int(roi.topLeft().x)
                y1 = int(roi.topLeft().y)
                x2 = int(roi.bottomRight().x)
                y2 = int(roi.bottomRight().y)

                try:
                    label = labelMap[t.label]
                except:
                    label = t.label

                cv2.putText(frame, str(label), (x1 + 10, y1 + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"ID: {[t.id]}", (x1 + 10, y1 + 35), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, t.status.name, (x1 + 10, y1 + 50), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.rectangle(frame, (x1, y1), (x2, y2), color, cv2.FONT_HERSHEY_SIMPLEX)

                cv2.putText(frame, f"X: {int(t.spatialCoordinates.x)} mm", (x1 + 10, y1 + 65), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"Y: {int(t.spatialCoordinates.y)} mm", (x1 + 10, y1 + 80), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"Z: {int(t.spatialCoordinates.z)} mm", (x1 + 10, y1 + 95), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)

            cv2.putText(frame, "NN fps: {:.2f}".format(fps), (2, frame.shape[0] - 4), cv2.FONT_HERSHEY_TRIPLEX, 0.4, color)

            cv2.imshow("tracker", frame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst