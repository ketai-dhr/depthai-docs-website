IoT系列入门必看
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
OAK LUX-ESP32 设备入门

.. note::
    设备上内嵌 ESP32 并不意味着用户可以在没有 USB-C 连接的情况下开发设备。


代码示例
#############

代码示例在我们的 `esp32-spi-message-demo <https://github.com/luxonis/esp32-spi-message-demo>`__
github 存储库。

概观
########

.. code-block::

                  DepthAI device                        ┌─────┐
  ┌───────────────────────────────────────────────┐     │  ◎  │
  │                                               │     │     │
  │                ┌───────────────────ESP32───┐  │  BT |     |
  │                │                           │--│◄───►|     |
  │                │   (Your ESP32 firmware)   │  │     └─────┘
  │                │                           │  │         ┌──────────┐
  │                │---------------------------│  │         │          │
  │                │     depthai-spi-api       │--│◄───────►├──────────┤
  │                └───────▲───────────────────┘  │  WiFi   │  Server  │
  │                        │                      │         ├──────────┤
  │                        │SPI                   │         │          │
  │      Right             │                      │         └──────────┘
  │       ┌───┐   ┌───┬────▼───┬─MyriadX(VPU)──┐  │
  │       │ ◯ │--►|   │        │               │  │            Host
  │ ┌───┐ └───┘   │   │ SpiOut │     ┌─────────┤  │        ┌───────────┐
  │ │ ◯ │--------►|   └────────┘     │         │  │        │           │
  │ └───┘ ┌───┐   │                  │ XLinkIn │  │  XLink │           │
  │Color  │ ◯ |--►| (Your pipeline)  │ XLinkOut│--│◄──────►│           │
  │       └───┘   │                  │         │  │        └────┬─┬────┘
  │        Left   └──────────────────┴─────────┘  │             │ │
  │                                               │           ──┴─┴──
  └───────────────────────────────────────────────┘

概述解释：

- `MyriadX <https://www.intel.com/content/www/us/en/products/details/processors/movidius-vpu/movidius-myriad-x.html>`__ 是 DepthAI 上的 VPU，您可以在其中运行您的管道
- MyriadX 连接到主机（例如 PC）
- MyriadX 可以通过 SPI (使用 `SPIOut <https://docs.luxonis.com/projects/api/en/latest/components/nodes/spi_out/>`__  节点)向 ESP32发送 `信息 <https://docs.luxonis.com/projects/api/en/latest/components/messages/>`__
- ESP32可以使用 `depthai-spi-api <https://github.com/luxonis/depthai-spi-api>`__ 库(这是 `ESP-IDF <https://github.com/espressif/esp-idf>`__ 组件)接收这些消息。
- 在 ESP32上，你可以运行消息的后期处理，并有选择地将结果发送到服务器(如果连接到 WiFi 网络)或蓝牙设备(如智能手机)

局限性
###########

目前，用例的瓶颈是 SPI 吞吐量，大约为 **200kbps**。
这意味着您不能通过 SPI 实时传输帧。这是 SPI 驱动程序当前的限制，但是我们正计划对此进行研究，并将 SPI 的吞吐量提高到 **~8mbps** (目前还没有 ETA)。

目前我们只支持 `ESP-IDF <https://www.espressif.com/en/products/sdks/esp-idf>`__ 作为开发框架，将来我们可能会支持 `Arduino <https://github.com/espressif/arduino-esp32>`__ 。

ESP32的通用用例
##############################

**TL;DR:**

- 发送元数据结果(空间坐标、神经网络结果、小轨迹等)
- OTA 更新
- 捕获和发送(图像的一部分)
- 系统信息记录
- 将数据发送到云(比如. AWS/Azure/GCP 或任何其他物联网平台)
- 将数据发送到蓝牙设备(例如智能手机)

**更详细的解释:**

- 您可以在 VPU 上运行空间对象检测和/或对象跟踪管道，并通过 SPI 向 ESP32发送带有空间数据的 tracklet。在 ESP32上，您可以运行一些简单的过滤和/或 NN 结果解码，然后将最终结果发送到云
- ESP32还可以闪存 DepthAI 引导加载程序和/或管道，这意味着支持 OTA (通过无线方式)更新。
- 我们有一个例子，通过 SPI 从 VPU 到 ESP32发送一个 `完整的图像 <https://github.com/luxonis/esp32-spi-message-demo/tree/main/jpeg_demo>`__ 或 `一个图像的一部分 <https://github.com/luxonis/esp32-spi-message-demo/tree/main/image_part>`__ 
- 可以将 VPU 上的管道配置为将系统信息(使用 `SystemLogger <https://docs.luxonis.com/projects/api/en/latest/components/nodes/system_logger/>`__ 节点)发送到 ESP32，再由 ESP32将其转发到日志平台
- ESP32 也有蓝牙功能，所以你可以将数据从 ESP32转发到智能手机

如何开始开发
#######################################

#. 安装 ESP-IDF,  `说明在这里 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#installation-step-by-step>`__.
#. 在设置 `环境变量 <https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#step-4-set-up-the-environment-variables>`__ 之后，您可以使用 :code:`idf.py build` 构建来自 `esp32-spi-message-demo <https://github.com/luxonis/esp32-spi-message-demo>`__ 存储库的任何演示。
#. 构建完成后，您可以使用 :code:`idf.py -p PORT flash monitor` (使用 ESP32 端口替换 :code:`PORT` , 例如 :code:`/dev/ttyUSB0`) 刷入您的 ESP32 。您可能需要使用 :code:`sudo chmod 777 PORT` 更改端口的权限，以便 idf.py 可以访问它。
#. 在刷入 ESP32 之后，您可以启动管道。如果您使用了一个 ESP32 demo 代码，那么您应该从 `gen2-spi demos <https://github.com/luxonis/depthai-experiments/tree/master/gen2-spi>`__ 演示中运行相应的 python 脚本。


DepthAI应用程序包（\ **DAP**\ ）
##############################

.. note:: 

   `DepthAI应用程序包使用指南 <https://www.oakchina.cn/2021/12/20/depthai-dap/>`__

以下部分将展示一个示例：
刷入引导加载程序（仅需要一次）并将创建的管道“myExamplePipeline”刷入设备（该示例是用
Python 编写的，类似的步骤适用于 C++）

1. `Flashing bootloader <https://github.com/luxonis/depthai-python/blob/main/examples/bootloader/flash_bootloader.py>`__ 
 .. code:: python

      import depthai as dai
      (found, info) = dai.DeviceBootloader.getFirstAvailableDevice()
      if not found:
         print("No device found to flash. Exiting.")
         exit(-1)
      bootloader = dai.DeviceBootloader(info,allowFlashingBootloader=True)
      blType = bootloader.getType()
      progress = lambda p : print(f'Flashing progress: {p*100:.1f}%')
      (res, message) = bootloader.flashBootloader(dai.DeviceBootloader.Memory.FLASH, blType, progress)
      if res:
         print("Flashing successful. Took", time.monotonic() - startTime, "seconds")
      else:
         print("Flashing failed:", message)

2. 刷入 创建的管道

   .. code:: python

      import depthai as dai
      # ...
      # Create Pipeline 'myExamplePipeline'
      # ...
      (f, bl) = dai.DeviceBootloader.getFirstAvailableDevice()
      bootloader = dai.DeviceBootloader(bl)
      progress = lambda p : print(f'Flashing progress: {p*100:.1f}%')
      bootloader.flash(progress, myExamplePipeline)

   ..

   .. warning::

      经测试，2.15.2.0以上的depthai也可以，其他版本可自行测试。

.. include::  /pages/includes/footer-short.rst