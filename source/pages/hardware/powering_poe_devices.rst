为PoE设备供电
##########################

我们目前在售的有两款以太网供电(PoE)的设备：

    - `OAK-D-PoE <https://www.oakchina.cn/product/oak-1-poe/>`__
    - `OAK-1-PoE <https://www.oakchina.cn/product/oak-d-poe/>`__

另外，我们还有多款OAK POE设备正在研发中：

    `此处可以看到所有OAK POE设备 <https://www.oakchina.cn/product-category/oak-poe/>`__ 

如果您不熟悉以太网供电(POE),那么本教程非常适合您。

POE非常方便, 它允许使用单根Cat5e(或更高级别)以太网电缆为设备供电并在最远100米(328英尺)处以1,000Mbps(1Gbps)全双工提供连接。

在本教程中，我们将介绍推荐与Luxonis PoE设备一起使用的硬件。

使用PoE设备（例如Luxonis的OAK PoE型号）的推荐方法是使用PoE以太网交换机。或者，如果您已经安装了非 PoE 交换机，则可以将 PoE 注入器与此交换机结合使用。但是建议使用 PoE 以太网交换机，因为它们可以让您更好地控制，安装更简洁，并且可以通过PoE交换机获得诸如电源使用和远程设备断电等控制的方法。

一般规格
************

- PoE标准: **802.3af**
- 数据传输速率: **1 Gbps (1000BASE-T)**
- 设备功耗: **平均~4W**, **最大6.25W**

PoE交换机或注入器
********************

使用DepthAI OAK PoE型号的建议方法是使用PoE交换机。我们喜欢UniFi交换机*，因此我们在下面推荐这些，但任何802.3af（或更高版本，例如 802.3at、802.3bt 等）都可以为OAK PoE型号供电和工作。

PoE交换机有各种尺寸、功率容量和端口号。OAK PoE设备使用最低功率的 PoE标准 802.3af。任何802.3af PoE交换机每个端口都可以提供12.95W的功率，这实际上大大高于OAK PoE型号所需的最大功率（低于 6.25W）。因此，OAK PoE设备可与所有符合标准的PoE交换机配合使用，包括：

- **802.3af** - 12.95W per port
- **802.3at** - 25.5W per port
- **802.3bt/3** - 51W per port
- **802.3bt/4** - 71W per port

以下是已在OAK PoE型号上进行测试的UniFi交换机的快速列表，从最小的交换机开始，到较大的型号：

- US-8-60W (`这里 <https://store.ui.com/collections/unifi-network-switching/products/unifi-switch-8-60w?gclid=Cj0KCQjw8vqGBhC_ARIsADMSd1CqyJdc4DEDE4mjlpSgxuKxGE2QrmUY4N7CRlCBatOrsjd7V8RG17kaAi4PEALw_wcB>`__). 此交换机具有4个802.3af端口，因此可以为多达4个OAK PoE设备供电。
- US-8-150W (`这里 <https://store.ui.com/collections/unifi-network-switching/products/unifi-switch-8-150w?gclid=Cj0KCQjw8vqGBhC_ARIsADMSd1DhfxWteXVfMS3Lk3y1N3jaxIItdLimljE1Y-AGa_2aQuF96h6bTFIaAqa_EALw_wcB>`__).  该交换机有8个802.3at端口，因此它可以为多达8个OAK PoE设备供电。
- US-16-150W (`这里 <https://store.ui.com/collections/unifi-network-switching/products/unifi-switch-16-150w?gclid=Cj0KCQjw8vqGBhC_ARIsADMSd1BGnUy93AYvz_Q9mRTlDp0DBQurdSAP6C5Jt-gMfgjirsKUF7NF40saAjT1EALw_wcB>`__).  16个802.3at端口，因此它可以为多达16个OAK PoE设备供电。
- US-24-250W (`这里 <https://store.ui.com/collections/unifi-network-switching/products/unifiswitch-24-250w>`__). 24个802.3at端口； 可同时为24台OAK PoE设备供电。
- US-48-500W (`这里 <https://store.ui.com/collections/unifi-network-switching/products/unifiswitch-48-500w?gclid=Cj0KCQjw8vqGBhC_ARIsADMSd1DQbB_-SavDwFeTIwSPKAT5PlYriHSGuRoP-jYay-zGfwhE7IPVoq4aAicjEALw_wcB>`__).  48个802.3at端口，可同时为48台OAK PoE设备供电。


使用PoE交换机的好处是拥有一个干净、即插即用的解决方案。

如果您想使用现有的非PoE交换机，可以使用所谓的PoE注入器来实现。

PoE 注入器向以太网电缆注入电力，因此它可以为各种PoE设备（例如我们的OAK）供电。因此，如果您更喜欢使用现有的非PoE交换机（例如，包含在ISP路由器/调制解调器/交换机组合中的交换机），您可以使用 PoE 注入器，例如这种TP-Link注入器，每个OAK PoE都会使用它设备。

如果需要室外额定 PoE 交换机，一个选项是 `EP-S16 <https://store.ui.com/collections/operator-edgemax-control-points/products/edgepoint-s16>`__ 。

使用电池供电
************

建议使用至少满足802.3af标准的PoE供电器。如果您使用汽车或太阳能电池为PoE设备供电，您会受到其输出电压的限制，标准汽车电池的输出电压为 12V。在这种情况下，您需要找到一个 PoE注入器，它可以输入12V并输出802.3af标准规定的电压。

以下PoE注入器在12V输入上进行了测试：

- `GPOE-48V-10W <https://www.aliexpress.com/item/32981044221.html>`__: 输入电压：10-30V，输出电压：48V，按预期工作
- Solis energy PoE-24 HPI-1112: 输入电压：12V / 24V DC，按预期工作。
- Tycon Power TPDIN-1256GD-BT: 输入电压：11~60V，按预期工作。
- Tycon Power TP-DC-1248GDX2-HP: 输入电压：10~15V，15V才开始给自己的LED供电，所以不可靠，即使IEEE标准是802.3af。

下面是一个将多个DepthAI PoE设备连接到UniFi交换机的示例：

.. image:: /_static/images/tutorials/poe/poe-switch.jpeg

以太网电缆和耐候性
********************

OAK PoE型号旨在允许户外使用，包括耐候性。作为这种耐候性的一部分，有一个电缆密封套可在以太网电缆周围提供密封。这些密封接头设计用于“专业”安装 - 这意味着它们被设计用于为安装而压接至一定长度的电缆 - 因此没有“引导”。

.. image:: /_static/images/tutorials/poe/boot.jpeg

带护套的电缆通常不适合这些水密封压盖（因为它们是为这些专业安装而设计的，它们是无护套安装）。因此，如果您在户外安装，建议要么使用无护套预制电缆，要么使用“专业”技术将电缆制作成一定长度，或者找一根带有适合密封套的小护套的电缆（有些人这样做）。另外值得注意的是，如果安装在室外，必须使用屏蔽网线，否则当遇到风/雨/冰雹/等恶劣天气时，会在电缆中的数据线上积聚静电，并最终破坏 PoE 交换机、OAK PoE设备或两者兼而有之。

对于定制长度的电缆路径，您可以将 `TOUGHCable PRO电缆 <https://www.amazon.com/Ubiquiti-Networks-TOUGHCable-Shielded-Ethernet/dp/B008L143VW>`__ 与 `TOUGHCable 连接器 <https://www.amazon.com/Ubiquiti-TOUGHCable-RJ45-Connectors-Piece/dp/B009XE6JY0>`__ 一起使用。我们的团队过去曾将这种组合广泛用于户外安装。（也需要压接，例如 `这种 <https://www.amazon.com/Ratcheting-Stripper-Klein-Tools-VDV226-011-SEN/dp/B002D3B97U/ref=pd_lpo_1?pd_rd_i=B002D3B97U&psc=1>`__ 压接，才能将连接器压接到电缆上。）


.. note::

    **我们建议使用Cat5e以太网电缆** ，因为较新的电缆更大，可能不适合IP67密封接头。

*请注意，我们与这些产品中的任何一个都没有联系，我们只是对它们有很好的经验，我们中的一些人过去可能或可能没有在UniFi团队工作过。*

.. include::  /pages/includes/footer-short.rst