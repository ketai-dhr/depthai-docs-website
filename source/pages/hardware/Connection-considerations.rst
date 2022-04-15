硬件连接注意事项
==========================

USB 系列
~~~~~~~~~~~~~~~~~~~~~~~~

如果您的 OAK 配赠了 USB 数据线，我们建议使用该数据线将 OAK 相机连接到主机。

.. warning::
  确保使用 **USB3 数据线!** 如果没有，请在程序中 :ref:`强制使用 USB2 通信` 。

.. image:: /_static/images/tutorials/usb3.png

**USB3 数据线** 在 USB-C 数据线的 USB-A 连接器内侧 **呈蓝色** 。

请确保设备直接连接到您的主机（可以是 PC 或 Raspberry Pi 或其他有能力的设备）到 USB 端口，或通过有源 USB 集线器。

在 Ubuntu 上，您可以通过运行下面命令检查是否检测到新的 USB 设备


.. code-block:: bash

  $ lsusb | grep MyriadX
  Bus 003 Device 002: ID 03e7:2485 Intel Movidius MyriadX

.. note::
  如果您运行的是 Ubuntu 以外的其他操作系统，或者您认为出现问题，我们提供了详细的操作系统特定安装指南
  `点击这里 <https://docs.luxonis.com/projects/api/en/latest/install/#supported-platforms>`__。

强制使用 USB2 通信
-------------------

如果您没有使用（可以正常工作的）USB3 数据线，或者您的主机不支持 USB3，可以强制使用 USB2 数据线进行通信。如果您使用的是较长的 USB 数据线（2 米+），则建议使用 USB2 通信。

对于 API 用法，请在创建设备时将 **usb2Mode=True** 设置为：

.. code-block:: python

  # 强制使用 USB2 连接
  with dai.Device(pipeline, usb2Mode=True) as device:

如果你使用的是 `depthai_demo <https://github.com/luxonis/depthai/blob/main/depthai_demo.py>`__ 程序，则可以使用 -usbs 参数指定 USB 速度：

.. code-block:: python

  python3 depthai_demo.py -usbs usb2

OAK-D
-------------------

.. image:: /_static/images/hardware/oak-d.jpg

**OAK-D** 通过 **USB TYPE-C** 或 **5V**、 **5.5m x 2.5mm** 桶形插孔（BARREL_JACK）供电。 **USB3 5Gbps** 速度是流式传输来自设备的视频或数据的标准配置。对于相机和 **OAK-SoM** 
，总功耗通常低于 USB 3 的 900ma 规格，但建议使用 **1.5A** 或更高的 TYPE-C 功率。

PoE 系列
~~~~~~~~~~~~~~~~~~~~~

PoE 允许使用一根 Cat5e（或更高）以太网电缆为设备供电，并在长达 100 米（328 英尺）的范围内以 1，000 Mbps （1 Gbps） 的全双工速度为其提供连接。

.. image:: https://user-images.githubusercontent.com/18037362/125928421-daed2432-73fb-4c5b-843e-037c7383a871.gif

*连接 PoE 设备后，以太网连接 LED（如上所示）应打开并开始偶尔闪烁。*

IoT 系列
~~~~~~~~~~~~~~~~~~~~

OAK-D-IOT-40
--------------------

.. image:: /_static/images/hardware/Iot-40.jpg

**OAK-D-IoT-40** 接受来自 **5V** 桶形插孔（BARREL_JACK）的电源输入，也可以接受来自 **USB TYPE-C** 的电源输入。
其中 **MICRO USB** 用于 **ESP32** 数据传输。

All-in-One 系列
~~~~~~~~~~~~~~~~~~~~~~~~~

OAK-D-CM4
---------------------------

FFC 系列
~~~~~~~~~~~~~~~~~~~~~~~~

SoM 系列
~~~~~~~~~~~~~~~~~~~~~~~~~~~~