Edge Detector
=============

此示例对 3 个不同的输入执行边缘检测：左、右和 RGB 相机。使用 HW 加速 sobel 滤波器 3x3。Sobel 滤波器参数可以通过按键 1 和 2 进行更改。

Demo
*********

.. image:: /_static/images/samples/edge_detections.png

设置
*****************

.. warning::
    说明：此处安装的是第二代depthai库

请运行以下命令来安装所需的依赖项

.. code-block:: bash

    python3 -m pip install -U pip
    python3 -m pip install opencv-python
    python3 -m pip install -U --force-reinstall depthai

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
****************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/EdgeDetector/edge_detector.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/EdgeDetector/edge_detector.py>`_ 上找到。

流程图
#############

.. image:: /_static/images/samples/flow_chart/edge_detection.png

.. code-block:: python

    #!/usr/bin/env python3

    import cv2
    import depthai as dai
    import numpy as np

    # Create pipeline
    pipeline = dai.Pipeline()

    # Define sources and outputs
    camRgb = pipeline.create(dai.node.ColorCamera)
    monoLeft = pipeline.create(dai.node.MonoCamera)
    monoRight = pipeline.create(dai.node.MonoCamera)

    edgeDetectorLeft = pipeline.create(dai.node.EdgeDetector)
    edgeDetectorRight = pipeline.create(dai.node.EdgeDetector)
    edgeDetectorRgb = pipeline.create(dai.node.EdgeDetector)

    xoutEdgeLeft = pipeline.create(dai.node.XLinkOut)
    xoutEdgeRight = pipeline.create(dai.node.XLinkOut)
    xoutEdgeRgb = pipeline.create(dai.node.XLinkOut)
    xinEdgeCfg = pipeline.create(dai.node.XLinkIn)

    edgeLeftStr = "edge left"
    edgeRightStr = "edge right"
    edgeRgbStr = "edge rgb"
    edgeCfgStr = "edge cfg"

    xoutEdgeLeft.setStreamName(edgeLeftStr)
    xoutEdgeRight.setStreamName(edgeRightStr)
    xoutEdgeRgb.setStreamName(edgeRgbStr)
    xinEdgeCfg.setStreamName(edgeCfgStr)

    # Properties
    camRgb.setBoardSocket(dai.CameraBoardSocket.RGB)
    camRgb.setResolution(dai.ColorCameraProperties.SensorResolution.THE_1080_P)

    monoLeft.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    monoLeft.setBoardSocket(dai.CameraBoardSocket.LEFT)
    monoRight.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    monoRight.setBoardSocket(dai.CameraBoardSocket.RIGHT)

    edgeDetectorRgb.setMaxOutputFrameSize(camRgb.getVideoWidth() * camRgb.getVideoHeight())

    # Linking
    monoLeft.out.link(edgeDetectorLeft.inputImage)
    monoRight.out.link(edgeDetectorRight.inputImage)
    camRgb.video.link(edgeDetectorRgb.inputImage)

    edgeDetectorLeft.outputImage.link(xoutEdgeLeft.input)
    edgeDetectorRight.outputImage.link(xoutEdgeRight.input)
    edgeDetectorRgb.outputImage.link(xoutEdgeRgb.input)

    xinEdgeCfg.out.link(edgeDetectorLeft.inputConfig)
    xinEdgeCfg.out.link(edgeDetectorRight.inputConfig)
    xinEdgeCfg.out.link(edgeDetectorRgb.inputConfig)

    # Connect to device and start pipeline
    with dai.Device(pipeline) as device:

        # Output/input queues
        edgeLeftQueue = device.getOutputQueue(edgeLeftStr, 8, False)
        edgeRightQueue = device.getOutputQueue(edgeRightStr, 8, False)
        edgeRgbQueue = device.getOutputQueue(edgeRgbStr, 8, False)
        edgeCfgQueue = device.getInputQueue(edgeCfgStr)

        print("Switch between sobel filter kernels using keys '1' and '2'")

        while(True):
            edgeLeft = edgeLeftQueue.get()
            edgeRight = edgeRightQueue.get()
            edgeRgb = edgeRgbQueue.get()

            edgeLeftFrame = edgeLeft.getFrame()
            edgeRightFrame = edgeRight.getFrame()
            edgeRgbFrame = edgeRgb.getFrame()

            # Show the frame
            cv2.imshow(edgeLeftStr, edgeLeftFrame)
            cv2.imshow(edgeRightStr, edgeRightFrame)
            cv2.imshow(edgeRgbStr, edgeRgbFrame)

            key = cv2.waitKey(1)
            if key == ord('q'):
                break

            if key == ord('1'):
                print("Switching sobel filter kernel.")
                cfg = dai.EdgeDetectorConfig()
                sobelHorizontalKernel = [[1, 0, -1], [2, 0, -2], [1, 0, -1]]
                sobelVerticalKernel = [[1, 2, 1], [0, 0, 0], [-1, -2, -1]]
                cfg.setSobelFilterKernels(sobelHorizontalKernel, sobelVerticalKernel)
                edgeCfgQueue.send(cfg)

            if key == ord('2'):
                print("Switching sobel filter kernel.")
                cfg = dai.EdgeDetectorConfig()
                sobelHorizontalKernel = [[3, 0, -3], [10, 0, -10], [3, 0, -3]]
                sobelVerticalKernel = [[3, 10, 3], [0, 0, 0], [-3, -10, -3]]
                cfg.setSobelFilterKernels(sobelHorizontalKernel, sobelVerticalKernel)
                edgeCfgQueue.send(cfg)


.. include::  /pages/includes/footer-short.rst