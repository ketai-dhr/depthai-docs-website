Bootloader Config
=====================

此示例允许您读取/刷新/清除设备上的引导加载程序。您可以通过cmd参数指定 :code:`.json` 引导加载程序配置文件，该文件将刷入设备。

演示
**************

示例脚本输出

.. code-block:: bash

    ~/depthai-python/examples$ python3 Bootloader/bootloader_config.py flash
    Found device with name: 14442C10D1789ACD00-ma2480
    Successfully flashed bootloader configuration

    ~/depthai-python/examples$ python3 Bootloader/bootloader_config.py read
    Found device with name: 14442C10D1789ACD00-ma2480
    Current flashed configuration
    {'appMem': -1, 'network': {'ipv4': 0, 'ipv4Dns': 0, 'ipv4DnsAlt': 0, 'ipv4Gateway': 0, 'ipv4Mask': 0, 'ipv6': [0, 0, 0, 0], 'ipv6Dns': [0, 0, 0, 0], 'ipv6DnsAlt': [0, 0, 0, 0], 'ipv6Gateway': [0, 0, 0, 0], 'ipv6Prefix': 0, 'mac': [0, 0, 0, 0, 0, 0], 'staticIpv4': False, 'staticIpv6': False, 'timeoutMs': 30000}, 'usb': {'maxUsbSpeed': 3, 'pid': 63036, 'timeoutMs': 3000, 'vid': 999}}

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/bootloader/bootloader_config.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/bootloader_config.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import depthai as dai
    import sys
    import json

    usage = False
    read = True
    clear = False
    path = ''
    if len(sys.argv) >= 2:
        op = sys.argv[1]
        if op == 'read':
            read = True
        elif op == 'flash':
            read = False
            if len(sys.argv) >= 3:
                path = sys.argv[2]
        elif op == 'clear':
            clear = True
            read = False
        else:
            usage = True
    else:
        usage = True

    if usage:
        print(f'Usage: {sys.argv[0]} [read/flash/clear] [flash: path/to/config/json]')
        exit(-1)

    (res, info) = dai.DeviceBootloader.getFirstAvailableDevice()

    if res:
        print(f'Found device with name: {info.desc.name}');
        with dai.DeviceBootloader(info) as bl:
            if read:
                print('Current flashed configuration')
                print(json.dumps(bl.readConfigData()))
            else:
                success = None
                error = None
                if clear:
                    (success, error) = bl.flashConfigClear()
                else:
                    if path == '':
                        (success, error) = bl.flashConfig(dai.DeviceBootloader.Config())
                    else:
                        (success, error) = bl.flashConfigFile(path)
                if success:
                    print('Successfully flashed bootloader configuration')
                else:
                    print(f'Error flashing bootloader configuration: {error}')
    else:
        print('No devices found')