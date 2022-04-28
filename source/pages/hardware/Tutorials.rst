产品系列特定使用教程
====================

USB系列入门必看
~~~~~~~~~~~~~~~~~~~~~

请一定要注意接线问题，然后你可以看 :ref:`OAK快速上手 <安装depthai>` 这部分内容了。

PoE系列入门必看
~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

    `POE设备入门指南 <https://www.oakchina.cn/2021/12/16/oak-poe-user-guide/>`__ 

本教程适用的设备可在 `这里 <https://www.oakchina.cn/product-category/oak-poe/>`__ 查看。

PoE 允许使用单根 Cat5e（或更高）以太网电缆为设备供电并为其提供长达 100 米（328 英尺）的 1,000 Mbps (1 Gbps) 全双工连接。

.. image:: /_static/images/tutorials/poe/poe.gif
    :alt: POE
    :align: center

*连接 PoE 设备后，以太网连接 LED（如上所示）应亮起并开始偶尔闪烁。*

分步教程
#####################

#. 您将需要一个 PoE 交换机或注入器 **来为PoE设备供电**. `单击此处获取完整教程 <https://docs.luxonis.com/projects/hardware/en/latest/pages/powering_poe_devices.html>`__. 为设备供电后，LED 应开始闪烁，如上面的 GIF 所示。
#. 将您的计算机连接到与PoE 设备相同的 `LAN <https://en.wikipedia.org/wiki/Local_area_network>`__ 。
#. 确保你有 **depthai version 2.7.0.0** 或更新版本。您可以使用以下命令更新您的depthai python包 :code:`python3 -m pip install depthai>=2.7.0.0`
#. 现在，您可以像使用 USB-C 电缆连接 DepthAI 设备一样运行任何 `代码示例 <https://docs.luxonis.com/projects/api/en/latest/tutorials/code_samples/>`__ / `depthai 实验 <https://github.com/luxonis/depthai-experiments>`__ / `depthai_demo <https://github.com/luxonis/depthai>`__ ！

.. image:: /_static/images/tutorials/poe/poe-working.jpeg
    :alt: POE-working
    :align: center

*在这些步骤之后，depthai_demo 正在 OAK-D-POE 上工作！*

POE版本的OAK如何运作
########################

当您的程序尝试创建设备 (:code:`with dai.Device(pipeline) as device:`) 时，depthai将搜索通过 USB 端口或 LAN 连接的可用设备。它在同一网络（例如 LAN）上搜索 PoE 设备并通过 TCP 协议进行通信。这样 PoE 设备的工作方式与 USB 设备相同。与 USB-C 连接一样，您可以指定 Mx ID 以指定您想要连接到哪个 DepthAI PoE 设备(`了解更多信息请点击此处 <https://docs.luxonis.com/projects/api/en/latest/tutorials/multiple/>`__)。

PoE 故障排除
###################

- **DHCP和静态IP**
    默认情况下，PoE 设备会尝试从 DHCP 获取 IP 地址。如果网络上没有 DHCP 服务器，设备将回退到静态IP :code:`169.254.1.222`.  在这种静态回退情况下，您的计算机需要在相同的范围内。这可以通过在您的计算机上设置静态IP来实现(例如使用静态IP: :code:`169.254.1.10` 和网络掩码: :code:`255.255.0.0`).

    Windows：

    .. image:: /_static/images/tutorials/poe/pc_static_ip.png

    Linux：
    
    您可以使用以下命令修改主机以太网静态IP:

    .. code-block:: bash

        sudo ifconfig eth0 169.254.1.10 netmask 255.255.0.0 up

- **端口和防火墙**
    UDP设备发现在端口上处理 :code:`11491`, TCP XLink 连接在端口上处理 :code:`11490`.
    
    默认情况下，在Ubuntu上防火墙是禁用的，因此您应该没有任何问题。您可以通过执行以下命令来检查这一点：

    .. code-block:: bash

      > sudo ufw status
      Status: inactive

    如果您启用了防火墙，则可能需要允许这两个端口：

    .. code-block:: bash

      sudo ufw allow 11490/tcp
      sudo ufw allow 11491/udp
    
    如果以上操作防火墙命令不起作用，可以尝试下面的方案：

    .. code-block:: bash

        sudo iptables -F
    
    .. warning::

        此命令为当时生效重启即恢复原样。且此方案为暂时解决方案。

- **VPN连接**
    VPN 连接也可能会中断与 PoE 设备的连接（因为您的计算机可能仅在远程网络中搜索该设备，因此无法在本地网络上发现它），因此我们建议在使用 PoE 时关闭 VPN设备或以其他方式确保您的本地路由设置为当 VPN 连接处于活动状态时本地设备可用/发现。

- **通过2个接口(WiFi/以太网)连接到同一个LAN**
    我们已经看到，在一些罕见的情况下，当您的主机连接到同一个局域网时，设备发现可能会发现两次相同的 POE 设备，因此它会打印该设备的 IP 地址两次。在极少数情况下，这可能会导致初始化错误（我们在使用多个设备时看到过这种情况）； `运行时错误：启动后无法找到设备，错误消息： X_LINK_DEVICE_NOT_FOUND。`  我们会尽快修复这个错误。
    **解决方法：断开其中一个接口；所以断开(from the)WiFi应该可以解决这个问题。**

- **供电不足**
    如果您的PoE设备不工作，或者在极少数情况下，它工作了一段时间后突然停止工作，则您的PoE交换机可能存在问题。例如，当每个端口的功率预算似乎足够时，但由于其他端口上的设备的需求而超出了交换机的整体功率预算。
	有必要根据整体功率预算检查 PoE 交换机/注入器的规格。

测试
*****************

您可以在 :ref:`此处下载 <测试设备连接>` 测试程序。

闪存静态IP
##########################

您可以刷写OAK-POE设备的静态/动态IP，:ref:`演示在这里 <设置POE设备静态IP>`。您还可以指定DNS和MAC地址，但这不包含在此演示中。

手动指定设备IP
##########################

如果您想访问 :code:`ping` 一个设备，但是发现失败了(例如，设备本身不在同一个LAN中), 您可以手动指定POE设备的IP地址。

.. code-block:: python

  import cv2
  import depthai as dai

  pipeline = dai.Pipeline()

  camRgb = pipeline.createColorCamera()

  xoutRgb = pipeline.createXLinkOut()
  xoutRgb.setStreamName("rgb")
  camRgb.preview.link(xoutRgb.input)

  device_info = dai.DeviceInfo()
  device_info.state = dai.XLinkDeviceState.X_LINK_BOOTLOADER
  device_info.desc.protocol = dai.XLinkProtocol.X_LINK_TCP_IP
  device_info.desc.name = "169.254.1.222"

  with dai.Device(pipeline, device_info) as device:
      qRgb = device.getOutputQueue(name="rgb", maxSize=4, blocking=False)
      while True:
          cv2.imshow("rgb", qRgb.get().getCvFrame())
          if cv2.waitKey(1) == ord('q'):
              break


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

      经测试，目前 depthai 2.12.0.0 版本支持上述操作。
      其他版本可自行测试。

    
All-in-One系列入门必看
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~
`点击查看 <https://www.oakchina.cn/2021/09/10/oak-d-cm4-user-guide/>`__

相机校准标定
~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~~

.. note::

  所有非模块化 OAK 设备在发货前都经过校准。不需要（重新）校准任何这些，但对于仅PCBA的模型，如 OAK-D-IoT-75、OAK-D-IoT-40、OAK-D-CM4、OAK-D-CM3、OAK -D-PCBA 如果深度质量因安装 PCBA（进入外壳）而降低，则可能需要这样做。

  任何平台都可以进行校准。

.. note::

  `保姆级校准文档，新手必看！！！ <https://www.oakchina.cn/2021/12/04/oak-calibration/>`__ 

对于 DepthAI 的模块化相机版本 (:ref:`BW1098FFC - USB3 模块化相机版` and :ref:`BW1094 - RaspberryPi Hat`)
，在将摄像头安装到适用于您的应用程序的基线/配置中之后，有必要进行立体相机校准。


对于 :ref:`BW1097 - RaspberryPi 计算模块` 和 :ref:`BW1098OBC <bw1098obc>` ，这些设备是预先校准的 - 但是您可能需要在安装过程中重新校准以获得更好的质量（例如，将板安装到某物后），或者是如果校准质量在使用/处理过程中开始减弱时。

以下是显示 :ref:`BW1097 - RaspberryPi 计算模块` (重新)校准的快速视频。
简而言之，校准使用相交点来确定棋盘格的方向和距离。 
因此，通过在平面上清晰地打印或显示所提供的棋盘格图像，可以获得最高的准确性。

校准棋盘的平整度非常重要。
不要使用曲面显示器，或有任何 "波浪" 的校准目标。
因此，如果你打印棋盘，请确保将棋盘贴在一个已知的平面上，没有任何波纹。
也就是说，使用笔记本电脑与平面显示器通常是最简单的技术。

观看下面的视频将为您提供校准自己的 DepthAI 所需的步骤。有关校准选项的更多信息/详细信息，请参阅以下步骤，并且 :code:`./calibrate.py --help` 还将打印出所有校准选项。


.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="https://player.bilibili.com/player.html?aid=337171125&bvid=BV1bR4y1s7Vb&cid=457012180&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

#. 检出 `depthai <https://gitee.com/oakchina/depthai.git>`__ GitHub 存储库。
    .. warning::

      已经检出 `depthai <https://gitee.com/oakchina/depthai.git>`__ 了吗? **跳过此步骤**

    .. code-block:: bash

      git clone https://gitee.com/oakchina/depthai.git
      cd depthai
      python3 install_requirements.py
    
    .. note::

      如果你的设备是 OAK-D-LITE，执行下面的命令
      
    .. code-block:: bash

      git clone https://gitee.com/oakchina/depthai.git
      cd depthai
      git checkout oak-d-lite
      python3 install_requirements.py

#. 打印棋盘格校准图像。

    将校准棋盘格打印在平坦的表面上，或者将棋盘格显示在平坦的显示器上(不要弯曲！)。请注意，如果您确实要打印校准目标，请确保将其固定在平坦的表面上并且没有褶皱或“波浪”。

    通常，使用监视器显示校准目标更加容易/快捷。

    .. image:: /_static/images/calibration/charuco_297x210_8x11_20_DICT_4X4.png
      :alt: 打印此棋盘格校准图像

    整个图像应放在一张纸上(按比例缩放)。如果在监视器上显示，请以白色背景全屏显示图像。

#. 启动校准脚本。

    用有效的条目替换占位符参数值:

    .. code-block:: bash

      python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd [BOARD] -db

    参数参考:

    - :code:`-s SQUARE_SIZE_IN_CM`, :code:`--square_size_cm SQUARE_SIZE_IN_CM`: 以厘米为单位测量印刷棋盘格的平方大小。
    - :code:`-db`, :code:`--defaultBoard` : 指定我们正在使用的depthai存储库中提供的校准版的标志。如果您使用其他板，请将 :code:`-ms MARKER_SIZE_IN_CM`, :code:`--markerSizeCm MARKER_SIZE_IN_CM` 参数改为charuco板子上标记的尺寸，以厘米为单位。
    - :code:`-brd BOARD`, :code:`--board BOARD`: BW1097，BW1098OBC-资源/面板/中的面板类型(不区分大小写)。或自定义.json板配置的路径。与[-fv -b -w]互斥，可以手动指定视场，基线和相机方向（已交换或未交换）。

    用直尺或游标卡尺从校准目标中检索正方形的大小，然后输入该数字(以厘米为单位)代替[SQUARE_SIZE_IN_CM]。

    例如，如果正方形尺寸为 2.35 厘米，则 :ref:`BW1098OBC <bw1098obc>` 的参数如下所示:

    .. code-block:: bash

      python3 calibrate.py -s 2.35 -brd bw1098obc -db -ih

    并请注意，在校准时对显示进行镜像通常很有用(这样，运动方向就不会向后看)。看到自己时，我们习惯于向后看自己(因为这是我们在镜子中看到的)，因此，请使用以下 :code:`-ih` 选项:

    .. code-block:: bash

      python3 calibrate.py -s 2.35 -brd bw1098obc -ih

    因此，当我们在内部运行校准时，几乎总是使用该 :code:`-ih` 选项，因此我们会将其包括在以下所有示例命令中:

    - **BW1098OBC (USB3 板载摄像头版)):**

      .. code-block:: bash

        python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd bw1098obc -ih

    - **BW1097 (RPi 计算模块版):**

      .. code-block:: bash

        python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd bw1097 -ih


模块化相机校准
##############################

使用 `此处 <https://gitee.com/oakchina/depthai/tree/main/resources/boards>`__ 的电路板 :code:`*.json` 文件之一来定义立体摄像机之间, 左摄像机与彩色摄像机之间的基线，替换下面括号中的项目。

- 向左/向右交换 (即，摄像机朝向，设置为 :code:`true` 或 :code:`false`)
- 该 :code:`BASELINE` 灰度左/右摄影机之间厘米
- 灰度相机和彩色相机 :code:`RGBLEFT` 之间的距离间隔 :code:`Left` ，以厘米为单位。

.. code-block::

  {
      "board_config":
      {
          "name": "ACME01",
          "revision": "V1.2",
          "swap_left_and_right_cameras": [true | false],
          "left_fov_deg": 71.86,
          "rgb_fov_deg": 68.7938,
          "left_to_right_distance_cm": [BASELINE],
          "left_to_rgb_distance_cm": [RGBLEFT]
      }
  }

因此，例如，如果您将 BW1098FFC 的立体基准设置为 2.5cm，并且彩色摄像机恰好位于两个灰度摄像机之间，如下所示，请进一步使用以下 JSON:

.. image:: /_static/images/products/mono-cameras-min-dist.png
  :alt: Color Camera

.. code-block:: json

  {
      "board_config":
      {
          "name": "ACME01",
          "revision": "V1.2",
          "swap_left_and_right_cameras": true,
          "left_fov_deg": 71.86,
          "rgb_fov_deg": 68.7938,
          "left_to_right_distance_cm": 2.5,
          "left_to_rgb_distance_cm": 5.0
      }
  }

请注意，在照相机的此方向上， :code:`"swap_left_and_right_cameras"` 设置为true。

然后，使用以下电路板名称运行校准:

.. code-block:: bash

  python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd ACME01 -ih

运行 :code:`python3 calibrate.py --help` (或 :code:`-h`) 获取参数和用法示例的完整列表。

放置棋盘格并捕获图像。
###################################

显示左右视频流，每个视频流包含一个多边形叠加层。

举起印刷好的棋盘格(或笔记本电脑，屏幕上显示图像)，以便整个棋盘格显示在两个视频流中。

匹配叠加多边形的方向，然后按[SPACEBAR]捕获图像。棋盘格图案不需要与多边形完全匹配，但是将多边形用作相对于摄影机的角度和位置的准则很重要。有 13 个必需的多边形位置。

在捕获了所有多边形位置的图像之后，校准图像处理步骤将开始。如果成功，将在创建校准文件 :code:`depthai/resources/depthai.calib`.
默认情况下，此文件是通过中的 :code:`calib_fpath` 变量加载的 :code:`consts/resource_paths.py`.

它还将在 :code:`depthai/resources/` 文件夹下创建网格文件命名为 :code:`left_mesh.calib` 和 :code:`right_mesh.calib` ， 可用于克服双目相机模块的扭曲与变形。

测试深度
####################

我们将查看深度流以确保正确校准摄像机:

#. 开始一个终端会话。
#. 访问本地的 `depthai <https://gitee.com/oakchina/depthai>`__ 副本。

  .. code-block:: bash

    cd [depthai repo]

3. 运行测试脚本。

  .. code-block:: bash

    python3 depthai_demo.py

  该脚本将启动一个窗口，启动摄像头，并显示深度视频流:

  .. image:: /_static/images/calibration/calibration-depth.png
    :alt: Depth projection

  在上面的屏幕截图中，手靠近相机。


.. include::  /pages/includes/footer-short.rst
