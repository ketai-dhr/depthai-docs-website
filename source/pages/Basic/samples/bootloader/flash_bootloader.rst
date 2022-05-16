Flash Bootloader
=======================

此脚本会将引导加载程序闪存到连接的DepthAI设备。引导加载程序只能刷入具有板载闪存的设备。

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/bootloader/flash_bootloader.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/flash_bootloader.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import depthai as dai
    import sys
    import time

    blType = dai.DeviceBootloader.Type.AUTO
    if len(sys.argv) > 1:
        if sys.argv[1] == 'usb':
            blType = dai.DeviceBootloader.Type.USB
        elif sys.argv[1] == 'network':
            blType = dai.DeviceBootloader.Type.NETWORK
        else:
            print("Specify either 'usb' or 'network' bootloader type")
            exit()

    (found, info) = dai.DeviceBootloader.getFirstAvailableDevice()
    if not found:
        print("No device found to flash. Exiting.")
        exit(-1)

    hasBootloader = (info.state == dai.XLinkDeviceState.X_LINK_BOOTLOADER)
    if hasBootloader:
        print("Warning! Flashing bootloader can potentially soft brick your device and should be done with caution.")
        print("Do not unplug your device while the bootloader is flashing.")
        print("Type 'y' and press enter to proceed, otherwise exits: ")
        if input() != 'y':
            print("Prompt declined, exiting...")
            exit(-1)

    # Open DeviceBootloader and allow flashing bootloader
    print(f"Booting latest bootloader first, will take a tad longer...")
    with dai.DeviceBootloader(info, allowFlashingBootloader=True) as bl:
        currentBlType = bl.getType()

        if blType == dai.DeviceBootloader.Type.AUTO:
            blType = currentBlType

        # Check if bootloader type is the same, if already booted by bootloader (not in USB recovery mode)
        if currentBlType != blType and hasBootloader:
            print(f"Are you sure you want to flash '{blType.name}' bootloader over current '{currentBlType.name}' bootloader?")
            print(f"Type 'y' and press enter to proceed, otherwise exits: ")
            if input() != 'y':
                print("Prompt declined, exiting...")
                exit(-1)

        # Create a progress callback lambda
        progress = lambda p : print(f'Flashing progress: {p*100:.1f}%')

        print(f"Flashing {blType.name} bootloader...")
        startTime = time.monotonic()
        (res, message) = bl.flashBootloader(dai.DeviceBootloader.Memory.FLASH, blType, progress)
        if res:
            print("Flashing successful. Took", time.monotonic() - startTime, "seconds")
        else:
            print("Flashing failed:", message)