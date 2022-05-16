Bootloader Version
==========================

此示例显示了基本的引导加载程序交互，检索设备上运行的引导加载程序的版本。

演示
**************

示例脚本输出

.. code-block:: bash

    ~/depthai-python/examples$ python3 bootloader_version.py
    Found device with name: 14442C10D1789ACD00-ma2480
    Version: 0.0.15

设置
##############

请运行 `安装依赖脚本 <https://gitee.com/oakchina/depthai-python/blob/main/examples/install_requirements.py>`__ 以下载所有必需的依赖项。请注意，此脚本必须在git上下文中运行，因此您必须先下载 `depthai-python <https://gitee.com/oakchina/depthai-python>`__ 存储库，然后再运行该脚本：

.. code-block:: bash

    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python/examples
    python3 install_requirements.py

源代码
##############

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/bootloader/bootloader_version.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/bootloader/bootloader_version.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import depthai as dai

    (res, info) = dai.DeviceBootloader.getFirstAvailableDevice()

    if res == True:
        print(f'Found device with name: {info.desc.name}')
        bl = dai.DeviceBootloader(info)
        print(f'Version: {bl.getVersion()}')
    else:
        print('No devices found')