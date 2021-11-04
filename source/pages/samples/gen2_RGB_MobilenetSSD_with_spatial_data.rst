Gen2 在RGB相机上运行MobilenetSSD神经网络并获取深度信息
============================================================

本示例说明如何在RGB输入帧上运行MobileNetv2SSD，以及如何显示RGB预览，检测，深度图和空间信息（X，Y，Z）。除了具有空间数据外。X，Y，Z坐标相对于深度图的中心。

setConfidenceThreshold-置信度阈值，高于该阈值将检测到对象

演示
**************


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

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/SpatialDetection/spatial_mobilenet.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/SpatialDetection/spatial_mobilenet.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np
    import time

    '''
    空间检测网络演示。
    在RGB相机上进行推断，并检索相对于深度图中心的空间位置坐标：x，y，z。
    '''

    # MobilenetSSD标签
    labelMap = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow",
                "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]

    syncNN = True

    # 首先获取模型文件
    nnBlobPath = str((Path(__file__).parent / Path('models/mobilenet-ssd_openvino_2021.2_6shave.blob')).resolve().absolute())
    if len(sys.argv) > 1:
        nnBlobPath = sys.argv[1]

    if not Path(nnBlobPath).exists():
        import sys
        raise FileNotFoundError(f'Required file/s not found, please run "{sys.executable} install_requirements.py"')

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 定义来源-彩色相机
    colorCam = pipeline.createColorCamera()
    spatialDetectionNetwork = pipeline.createMobileNetSpatialDetectionNetwork()
    monoLeft = pipeline.createMonoCamera()
    monoRight = pipeline.createMonoCamera()
    stereo = pipeline.createStereoDepth()

    xoutRgb = pipeline.createXLinkOut()
    xoutNN = pipeline.createXLinkOut()
    xoutBoundingBoxDepthMapping = pipeline.createXLinkOut()
    xoutDepth = pipeline.createXLinkOut()

    xoutRgb.setStreamName("rgb")
    xoutNN.setStreamName("detections")
    xoutBoundingBoxDepthMapping.setStreamName("boundingBoxDepthMapping")
    xoutDepth.setStreamName("depth")


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

    spatialDetectionNetwork.setBlobPath(nnBlobPath)
    spatialDetectionNetwork.setConfidenceThreshold(0.5)
    spatialDetectionNetwork.input.setBlocking(False)
    spatialDetectionNetwork.setBoundingBoxScaleFactor(0.5)
    spatialDetectionNetwork.setDepthLowerThreshold(100)
    spatialDetectionNetwork.setDepthUpperThreshold(5000)

    # 创建输出

    monoLeft.out.link(stereo.left)
    monoRight.out.link(stereo.right)

    colorCam.preview.link(spatialDetectionNetwork.input)
    if syncNN:
        spatialDetectionNetwork.passthrough.link(xoutRgb.input)
    else:
        colorCam.preview.link(xoutRgb.input)

    spatialDetectionNetwork.out.link(xoutNN.input)
    spatialDetectionNetwork.boundingBoxMapping.link(xoutBoundingBoxDepthMapping.input)

    stereo.depth.link(spatialDetectionNetwork.inputDepth)
    spatialDetectionNetwork.passthroughDepth.link(xoutDepth.input)

    # 连接并启动管道
    with dai.Device(pipeline) as device:

        # 输出队列将用于从上面定义的输出中获取rgb帧和nn数据
        previewQueue = device.getOutputQueue(name="rgb", maxSize=4, blocking=False)
        detectionNNQueue = device.getOutputQueue(name="detections", maxSize=4, blocking=False)
        xoutBoundingBoxDepthMapping = device.getOutputQueue(name="boundingBoxDepthMapping", maxSize=4, blocking=False)
        depthQueue = device.getOutputQueue(name="depth", maxSize=4, blocking=False)

        frame = None
        detections = []

        startTime = time.monotonic()
        counter = 0
        fps = 0
        color = (255, 255, 255)

        while True:
            inPreview = previewQueue.get()
            inNN = detectionNNQueue.get()
            depth = depthQueue.get()

            counter+=1
            current_time = time.monotonic()
            if (current_time - startTime) > 1 :
                fps = counter / (current_time - startTime)
                counter = 0
                startTime = current_time

            frame = inPreview.getCvFrame()
            depthFrame = depth.getFrame()

            depthFrameColor = cv2.normalize(depthFrame, None, 255, 0, cv2.NORM_INF, cv2.CV_8UC1)
            depthFrameColor = cv2.equalizeHist(depthFrameColor)
            depthFrameColor = cv2.applyColorMap(depthFrameColor, cv2.COLORMAP_HOT)
            detections = inNN.detections
            if len(detections) != 0:
                boundingBoxMapping = xoutBoundingBoxDepthMapping.get()
                roiDatas = boundingBoxMapping.getConfigData()

                for roiData in roiDatas:
                    roi = roiData.roi
                    roi = roi.denormalize(depthFrameColor.shape[1], depthFrameColor.shape[0])
                    topLeft = roi.topLeft()
                    bottomRight = roi.bottomRight()
                    xmin = int(topLeft.x)
                    ymin = int(topLeft.y)
                    xmax = int(bottomRight.x)
                    ymax = int(bottomRight.y)

                    cv2.rectangle(depthFrameColor, (xmin, ymin), (xmax, ymax), color, cv2.FONT_HERSHEY_SCRIPT_SIMPLEX)


            # 如果框架可用，请在其上绘制边框并显示框架
            height = frame.shape[0]
            width  = frame.shape[1]
            for detection in detections:
                # 归一化边界框
                x1 = int(detection.xmin * width)
                x2 = int(detection.xmax * width)
                y1 = int(detection.ymin * height)
                y2 = int(detection.ymax * height)
                try:
                    label = labelMap[detection.label]
                except:
                    label = detection.label
                cv2.putText(frame, str(label), (x1 + 10, y1 + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, "{:.2f}".format(detection.confidence*100), (x1 + 10, y1 + 35), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"X: {int(detection.spatialCoordinates.x)} mm", (x1 + 10, y1 + 50), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"Y: {int(detection.spatialCoordinates.y)} mm", (x1 + 10, y1 + 65), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                cv2.putText(frame, f"Z: {int(detection.spatialCoordinates.z)} mm", (x1 + 10, y1 + 80), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)

                cv2.rectangle(frame, (x1, y1), (x2, y2), color, cv2.FONT_HERSHEY_SIMPLEX)

            cv2.putText(frame, "NN fps: {:.2f}".format(fps), (2, frame.shape[0] - 4), cv2.FONT_HERSHEY_TRIPLEX, 0.4, color)
            cv2.imshow("depth", depthFrameColor)
            cv2.imshow("rgb", frame)

            if cv2.waitKey(1) == ord('q'):
                break


.. include::  /pages/includes/footer-short.rst