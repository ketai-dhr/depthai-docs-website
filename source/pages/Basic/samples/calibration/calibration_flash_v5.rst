Calibration Flash v5
=============================

此示例显示如何将版本5的校准数据(第1代校准数据)闪存到设备。

其他示例：

- :ref:`Calibration Flash <Calibration Flash>`
- :ref:`Calibration Reader <Calibration Reader>`
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

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/calibration/calibration_flash_v5.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/calibration_flash_v5.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    from pathlib import Path
    import cv2
    import depthai as dai
    import argparse

    boardConfigFile = str((Path(__file__).parent / Path('../models/BW1098OBC.json')).resolve().absolute())
    calibBinaryFile = str((Path(__file__).parent / Path('../models/depthai_v5.calib')).resolve().absolute())
    calibBackUpFile = str((Path(__file__).parent / Path('depthai_calib_backup.json')).resolve().absolute())

    parser = argparse.ArgumentParser()
    parser.add_argument('boardConfigFile', nargs='?', help="Path to board config file", default=boardConfigFile)
    parser.add_argument('calibBinaryFile', nargs='?', help="Path to version 5 .calib file", default=calibBinaryFile)
    args = parser.parse_args()

    # Connect device
    with dai.Device() as device:

        deviceCalib = device.readCalibration()
        deviceCalib.eepromToJsonFile(calibBackUpFile)
        print("Calibration Data on the device is backed up at:")
        print(calibBackUpFile)
        calibData = dai.CalibrationHandler(args.calibBinaryFile, args.boardConfigFile)

        status = device.flashCalibration(calibData)
        if status:
            print('Calibration Flash Successful')
        else:
            print('Calibration Flash Failed!!!')