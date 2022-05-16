Calibration Reader
=========================

此示例显示如何通过XLink读取存储在设备上的校准数据。

其他示例：

- :ref:`Calibration Flash <Calibration Flash>`
- :ref:`Calibration Flash v5 <Calibration Flash v5>`
- :ref:`Calibration Load <Calibration Load>`

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/calibration/calibration_reader.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/calibration_reader.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import depthai as dai
    import numpy as np
    import sys
    from pathlib import Path

    # Connect Device
    with dai.Device() as device:
        calibFile = str((Path(__file__).parent / Path(f"calib_{device.getMxId()}.json")).resolve().absolute())
        if len(sys.argv) > 1:
            calibFile = sys.argv[1]

        calibData = device.readCalibration()
        calibData.eepromToJsonFile(calibFile)

        M_rgb, width, height = calibData.getDefaultIntrinsics(dai.CameraBoardSocket.RGB)
        print("RGB Camera Default intrinsics...")
        print(M_rgb)
        print(width)
        print(height)

        M_rgb, width, height = calibData.getDefaultIntrinsics(dai.CameraBoardSocket.RGB)

        M_left = np.array(calibData.getCameraIntrinsics(dai.CameraBoardSocket.LEFT, 1280, 720))
        print("LEFT Camera resized intrinsics...")
        print(M_left)

        D_left = np.array(calibData.getDistortionCoefficients(dai.CameraBoardSocket.LEFT))
        print("LEFT Distortion Coefficients...")
        [print(name+": "+value) for (name, value) in zip(["k1","k2","p1","p2","k3","k4","k5","k6","s1","s2","s3","s4","τx","τy"],[str(data) for data in D_left])]

        M_right = np.array(calibData.getCameraIntrinsics(dai.CameraBoardSocket.RIGHT, 1280, 720))
        print("RIGHT Camera resized intrinsics...")
        print(M_right)

        D_right = np.array(calibData.getDistortionCoefficients(dai.CameraBoardSocket.RIGHT))
        print("RIGHT Distortion Coefficients...")
        [print(name+": "+value) for (name, value) in zip(["k1","k2","p1","p2","k3","k4","k5","k6","s1","s2","s3","s4","τx","τy"],[str(data) for data in D_right])]

        print(f"RGB FOV {calibData.getFov(dai.CameraBoardSocket.RGB)}, Mono FOV {calibData.getFov(dai.CameraBoardSocket.LEFT)}")

        R1 = np.array(calibData.getStereoLeftRectificationRotation())
        R2 = np.array(calibData.getStereoRightRectificationRotation())
        M_right = np.array(calibData.getCameraIntrinsics(calibData.getStereoRightCameraId(), 1280, 720))

        H_left = np.matmul(np.matmul(M_right, R1), np.linalg.inv(M_left))
        print("LEFT Camera stereo rectification matrix...")
        print(H_left)

        H_right = np.matmul(np.matmul(M_right, R1), np.linalg.inv(M_right))
        print("RIGHT Camera stereo rectification matrix...")
        print(H_right)

        lr_extrinsics = np.array(calibData.getCameraExtrinsics(dai.CameraBoardSocket.LEFT, dai.CameraBoardSocket.RIGHT))
        print("Transformation matrix of where left Camera is W.R.T right Camera's optical center")
        print(lr_extrinsics)

        l_rgb_extrinsics = np.array(calibData.getCameraExtrinsics(dai.CameraBoardSocket.LEFT, dai.CameraBoardSocket.RGB))
        print("Transformation matrix of where left Camera is W.R.T RGB Camera's optical center")
        print(l_rgb_extrinsics)