Calibration Load
=======================

此示例显示如何在管道中加载和使用版本6的校准数据(第2代校准数据)。

其他示例：

- :ref:`Calibration Flash <Calibration Flash>`
- :ref:`Calibration Flash v5 <Calibration Flash v5>`
- :ref:`Calibration Reader <Calibration Reader>`

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/calibration/calibration_load.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/calibration_load.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    from pathlib import Path
    import cv2
    import depthai as dai
    import argparse
    import numpy as np
    import cv2

    calibJsonFile = str((Path(__file__).parent / Path('../models/depthai_calib.json')).resolve().absolute())

    parser = argparse.ArgumentParser()
    parser.add_argument('calibJsonFile', nargs='?', help="Path to V6 calibration file in json", default=calibJsonFile)
    args = parser.parse_args()

    calibData = dai.CalibrationHandler(args.calibJsonFile)

    # Create pipeline
    pipeline = dai.Pipeline()
    pipeline.setCalibrationData(calibData)

    # Define sources and output
    monoLeft = pipeline.create(dai.node.MonoCamera)
    monoRight = pipeline.create(dai.node.MonoCamera)
    stereo = pipeline.create(dai.node.StereoDepth)
    xoutDepth = pipeline.create(dai.node.XLinkOut)
    xoutDepth.setStreamName("depth")

    # MonoCamera
    monoLeft.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)
    monoLeft.setBoardSocket(dai.CameraBoardSocket.LEFT)
    # monoLeft.setFps(5.0)
    monoRight.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)
    monoRight.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    # monoRight.setFps(5.0)

    # Linking
    monoLeft.out.link(stereo.left)
    monoRight.out.link(stereo.right)
    stereo.depth.link(xoutDepth.input)

    # Connect to device and start pipeline
    with dai.Device(pipeline) as device:

        depthQueue = device.getOutputQueue(name="depth", maxSize=4, blocking=False)

        while True:
            # blocking call, will wait until a new data has arrived
            inDepth = depthQueue.get()
            frame = inDepth.getFrame()

            # frame is ready to be shown
            cv2.imshow("depth", frame)

            if cv2.waitKey(1) == ord('q'):
                break