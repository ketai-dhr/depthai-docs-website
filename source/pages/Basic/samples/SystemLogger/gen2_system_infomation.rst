Gen2 系统信息
=====================

本示例说明如何从板上获取系统信息（内存使用情况，CPU使用情况和温度）。

演示
**********************

脚本输出示例

.. code-block:: bash

    Drr used / total - 0.13 / 414.80 MiB
    Cmx used / total - 2.24 / 2.50 MiB
    LeonCss heap used / total - 4.17 / 46.41 MiB
    LeonMss heap used / total - 2.87 / 27.58 MiB
    Chip temperature - average: 38.59, css: 39.81, mss: 37.71, upa0: 38.65, upa1: 38.18
    Cpu usage - Leon OS: 7.08%, Leon RT: 1.48 %
    ----------------------------------------
    Drr used / total - 0.13 / 414.80 MiB
    Cmx used / total - 2.24 / 2.50 MiB
    LeonCss heap used / total - 4.17 / 46.41 MiB
    LeonMss heap used / total - 2.87 / 27.58 MiB
    Chip temperature - average: 38.59, css: 39.58, mss: 37.94, upa0: 38.18, upa1: 38.65
    Cpu usage - Leon OS: 1.55%, Leon RT: 0.30 %
    ----------------------------------------
    Drr used / total - 0.13 / 414.80 MiB
    Cmx used / total - 2.24 / 2.50 MiB
    LeonCss heap used / total - 4.17 / 46.41 MiB
    LeonMss heap used / total - 2.87 / 27.58 MiB
    Chip temperature - average: 38.94, css: 40.04, mss: 38.18, upa0: 39.35, upa1: 38.18
    Cpu usage - Leon OS: 0.56%, Leon RT: 0.06 %
    ----------------------------------------
    Drr used / total - 0.13 / 414.80 MiB
    Cmx used / total - 2.24 / 2.50 MiB
    LeonCss heap used / total - 4.17 / 46.41 MiB
    LeonMss heap used / total - 2.87 / 27.58 MiB
    Chip temperature - average: 39.46, css: 40.28, mss: 38.88, upa0: 39.81, upa1: 38.88
    Cpu usage - Leon OS: 0.51%, Leon RT: 0.06 %
    ----------------------------------------

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

    python3 -m pip install -U pip
    python3 -m pip install opencv-python
    python3 -m pip install -U --force-reinstall depthai

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/SystemLogger/system_information.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/SystemLogger/system_information.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    import cv2
    import depthai as dai


    def print_sys_info(info):
        m = 1024 * 1024 # MiB
        print(f"Drr used / total - {info.ddrMemoryUsage.used / m:.2f} / {info.ddrMemoryUsage.total / m:.2f} MiB")
        print(f"Cmx used / total - {info.cmxMemoryUsage.used / m:.2f} / {info.cmxMemoryUsage.total / m:.2f} MiB")
        print(f"LeonCss heap used / total - {info.leonCssMemoryUsage.used / m:.2f} / {info.leonCssMemoryUsage.total / m:.2f} MiB")
        print(f"LeonMss heap used / total - {info.leonMssMemoryUsage.used / m:.2f} / {info.leonMssMemoryUsage.total / m:.2f} MiB")
        t = info.chipTemperature
        print(f"Chip temperature - average: {t.average:.2f}, css: {t.css:.2f}, mss: {t.mss:.2f}, upa0: {t.upa:.2f}, upa1: {t.dss:.2f}")
        print(f"Cpu usage - Leon OS: {info.leonCssCpuUsage.average * 100:.2f}%, Leon RT: {info.leonMssCpuUsage.average * 100:.2f} %")
        print("----------------------------------------")


    # 开始定义管道
    pipeline = dai.Pipeline()

    sys_logger = pipeline.createSystemLogger()
    sys_logger.setRate(1)  # 1 Hz

    # 创建输出
    linkOut = pipeline.createXLinkOut()
    linkOut.setStreamName("sysinfo")
    sys_logger.out.link(linkOut.input)

    # 连接管道并启动
    with dai.Device(pipeline) as device:

        # 输出队列将用于获取系统信息
        q_sysinfo = device.getOutputQueue(name="sysinfo", maxSize=4, blocking=False)

        while True:
            info = q_sysinfo.get()  # Blocking call, will wait until a new data has arrived
            print_sys_info(info)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst