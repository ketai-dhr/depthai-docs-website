Standalone 模式
##########################

**Standalone / Hostless / On-The-Edge 模式** 意味着 OAK
没有连接到主机。这可以通过 `闪存
bootloader <https://docs.oakchina.cn/projects/api/samples/bootloader/flash_bootloader.html#flash-bootloader>`__
然后 `闪存 pipeline <#闪存-pipeline>`__ 和 assets （ NN 模型）到 OAK
的闪存。

只有在具有板载闪存的 OAK 中才能使用 Standalone 模式，目前支持的有 `OAK
IOT <https://www.oakchina.cn/product-category/oak-iot/>`__ 和 `OAK
POE <https://www.oakchina.cn/product-category/oak-poe/>`__ 型号。

将 demo 转换为 standalone 模式
------------------------------

由于主机和设备之间不会有任何通信，因此首先需要删除所有
`XLinkOut <https://docs.oakchina.cn/projects/api/components/nodes/xlink_out.html#xlinkout>`__
和
`XLinkIn <https://docs.oakchina.cn/projects/api/components/nodes/xlink_in.html#xlinkin>`__
节点。这意味着设备只能通过 SPI
(`SPIOut <https://docs.oakchina.cn/projects/api/components/nodes/spi_in.html#spiout>`__/`SPIIn <https://docs.oakchina.cn/projects/api/components/nodes/spi_in.html#spiin>`__)
或
`Script <https://docs.oakchina.cn/projects/api/components/nodes/script.html#script>`__
节点 (GPIO/UART 或网络协议 HTTP/TCP/UDP…)与 “外界” 通信。

接下来你还可以删除主机端代码，通常是这样的:

.. code:: python

   with dai.Device(pipeline) as device:
       videoQ = device.getOutputQueue("video")
       faceDetQ = device.getOutputQueue("face_det")
       nnQ = device.getOutputQueue("nn")

       while True:
           frame = videoQ.get().getCvFrame()
           # ...

删除所有主机端代码之后，您将只剩下
`Pipeline <https://docs.oakchina.cn/projects/api/components/pipeline.html#pipeline>`__
定义 (带有节点 / 链接)。由于设备不再与主机通信，您需要通过 SPI
或脚本节点 “路由” 程序的输出，如上所述。

闪存 bootloader
---------------

执行下面的代码(或\ `此处 <https://docs.oakchina.cn/projects/api/samples/bootloader/flash_bootloader.html#source-code>`__\ 代码
)，将
`Bootloader <https://docs.oakchina.cn/projects/api/components/bootloader.html#bootloader>`__
闪存到设备上。Bootloader 与 depthai 一起打包，因此如果您有最新的 depthai
版本，您将闪存最新的 bootloader 版本。这个步骤只需要一次。

.. code:: python

   import depthai as dai
   (f, bl) = dai.DeviceBootloader.getFirstAvailableDevice()
   bootloader = dai.DeviceBootloader(bl)
   progress = lambda p : print(f'Flashing progress: {p*100:.1f}%')
   bootloader.flashBootloader(progress)

闪存 pipeline
-------------

当你有独立的
`Pipeline <https://docs.oakchina.cn/projects/api/components/pipeline.html#pipeline>`__
定义并且\ `Bootloader <https://docs.oakchina.cn/projects/api/components/bootloader.html#bootloader>`__
已经闪存在设备上，你可以开始闪存 pipeline
了。你可以用下面的代码片段来闪存 pipeline：

.. code:: python

   import depthai as dai

   pipeline = dai.Pipeline()

   # Define standalone pipeline; add nodes and link them
   # cam = pipeline.create(dai.node.ColorCamera)
   # script = pipeline.create(dai.node.Script)
   # ...

   # Flash the pipeline
   (f, bl) = dai.DeviceBootloader.getFirstAvailableDevice()
   bootloader = dai.DeviceBootloader(bl)
   progress = lambda p : print(f'Flashing progress: {p*100:.1f}%')
   bootloader.flash(progress, pipeline)

在成功地闪存 pipeline
之后，当你打开设备电源时，它将自动启动。如果你想更换 pipeline
，只需再次闪存即可。

清除闪存
--------

由于 pipeline
会在设备供电时启动这可能导致不必要的发热。如果您想要清除闪存的 pipeline
，请使用下面的代码片段。

.. warning::
    下面的代码还不能工作。我们将在未来的版本中添加 ``flashClear`` 函数。

.. code:: python

   import depthai as dai
   (f, bl) = dai.DeviceBootloader.getFirstAvailableDevice()
   if not f:
       print('No devices found, exiting...')
       exit(-1)

   with dai.DeviceBootloader(bl) as bootloader:
       bootloader.flashClear()

Factory reset
-------------

如果您设备已经
`软变砖 <https://zh.wikipedia.org/zh-cn/%E7%A0%96_(%E7%94%B5%E5%AD%90%E8%AE%BE%E5%A4%87)#%E8%BD%AF%E5%8F%98%E7%A0%96>`__\ ，或者只是想清除所有内容（闪存的
pipeline / assets 和 bootloader
配置），我们建议运行下面的出厂重置脚本。它还将闪存最新的 bootloader 。

.. code:: python

   import depthai as dai
   import tempfile

   blBinary = dai.DeviceBootloader.getEmbeddedBootloaderBinary(dai.DeviceBootloader.Type.NETWORK)
   blBinary = blBinary + ([0xFF] * ((8 * 1024 * 1024 + 512) - len(blBinary)))

   with tempfile.NamedTemporaryFile() as tmpBlFw:
       tmpBlFw.write(bytes(blBinary))

       (f, device_info) = dai.DeviceBootloader.getFirstAvailableDevice()
       if not f:
           print('No devices found, exiting...')
           exit(-1)

       with dai.DeviceBootloader(device_info, allowFlashingBootloader=True) as bootloader:
           progress = lambda p : print(f'Factory reset progress: {p*100:.1f}%')
           # Override SBR table, to prevent booting flashed application
           [success, msg] = bootloader.flashBootloader(progress, tmpBlFw.name)
           if success:
               print('Successfully overwritten SBR table. Device should now be reacheable through PoE')
           else:
               print(f"Couldn't overwrite SBR table to unbrick the device. Error: {msg}")

如果不能访问 (不在同一局域网中) **OAK POE** ，可以在指定 IP 上重置。

.. code:: python

   import depthai as dai
   import tempfile

   blBinary = dai.DeviceBootloader.getEmbeddedBootloaderBinary(dai.DeviceBootloader.Type.NETWORK)
   blBinary = blBinary + ([0xFF] * ((8 * 1024 * 1024 + 512) - len(blBinary)))

   with tempfile.NamedTemporaryFile() as tmpBlFw:
       tmpBlFw.write(bytes(blBinary))

       device_info = dai.DeviceInfo()
       device_info.state = dai.XLinkDeviceState.X_LINK_BOOTLOADER
       device_info.desc.protocol = dai.XLinkProtocol.X_LINK_TCP_IP
       device_info.desc.name = "192.168.34.110" # Set IP here

       with dai.DeviceBootloader(device_info, allowFlashingBootloader=True) as bootloader:
           progress = lambda p : print(f'Factory reset progress: {p*100:.1f}%')
           # Override SBR table, to prevent booting flashed application
           [success, msg] = bootloader.flashBootloader(progress, tmpBlFw.name)
           if success:
               print('Successfully overwritten SBR table. Device should now be reacheable through PoE')
           else:
               print(f"Couldn't overwrite SBR table to unbrick the device. Error: {msg}")
               
.. include::  ../includes/footer-short.rst
