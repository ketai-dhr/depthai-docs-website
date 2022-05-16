设置POE设备静态IP
==========================

此脚本允许您设置静态或动态IP，或清除OAK-POE设备上的引导加载程序配置。

演示
#############

示例脚本输出：

.. code-block:: bash

    Found device with name: 192.168.1.136
    -------------------------------------
    "1" to set a static IPv4 address
    "2" to set a dynamic IPv4 address
    "3" to clear the config
    1
    -------------------------------------
    Enter IPv4: 192.168.1.200
    Enter IPv4 Mask: 255.255.255.0
    Enter IPv4 Gateway: 255.255.255.255
    Flashing static IPv4 192.168.1.200, mask 255.255.255.0, gateway 255.255.255.255 to the POE device. Enter 'y' to confirm. y
    Flashing successful.

如果 10 秒后再次运行相同的示例，您将看到 IP 更改为192.168.1.200：

.. code-block:: bash

    Found device with name: 192.168.1.200
    -------------------------------------
    "1" to set a static IPv4 address
    "2" to set a dynamic IPv4 address
    "3" to clear the config

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/bootloader/poe_set_ip.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/poe_set_ip.py>`_ 上找到。

.. code-block:: python

    import depthai as dai

    (found, info) = dai.DeviceBootloader.getFirstAvailableDevice()

    def check_str(s: str):
        spl = s.split(".")
        if len(spl) != 4:
            raise ValueError(f"Entered value {s} doesn't contain 3 dots. Value has to be in the following format: '255.255.255.255'")
        for num in spl:
            if 255 < int(num):
                raise ValueError("Entered values can't be above 255!")
        return s

    if found:
        print(f'Found device with name: {info.desc.name}')
        print('-------------------------------------')
        print('"1" to set a static IPv4 address')
        print('"2" to set a dynamic IPv4 address')
        print('"3" to clear the config')
        key = input('Enter the number: ').strip()
        print('-------------------------------------')
        if int(key) < 1 or 3 < int(key):
            raise ValueError("Entered value should either be '1', '2' or '3'!")

        with dai.DeviceBootloader(info) as bl:
            if key in ['1', '2']:
                ipv4 = check_str(input("Enter IPv4: ").strip())
                mask = check_str(input("Enter IPv4 Mask: ").strip())
                gateway = check_str(input("Enter IPv4 Gateway: ").strip())
                mode = 'static' if key == '1' else 'dynamic'
                val = input(f"Flashing {mode} IPv4 {ipv4}, mask {mask}, gateway {gateway} to the POE device. Enter 'y' to confirm. ").strip()
                if val != 'y':
                    raise Exception("Flashing aborted.")

                conf = dai.DeviceBootloader.Config()
                if key == '1': conf.setStaticIPv4(ipv4, mask, gateway)
                elif key == '2': conf.setDynamicIPv4(ipv4, mask, gateway)
                (success, error) = bl.flashConfig(conf)
            elif key == '3':
                (success, error) = bl.flashConfigClear()

            if not success:
                print(f"Flashing failed: {error}")
            else:
                print(f"Flashing successful.")

.. include::  /pages/includes/footer-short.rst