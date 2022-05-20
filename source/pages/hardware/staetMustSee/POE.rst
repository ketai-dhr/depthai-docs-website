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

.. include::  /pages/hardware/Standalone_mode.rst

.. include::  /pages/includes/footer-short.rst