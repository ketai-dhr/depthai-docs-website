Gen2 Python API
=============================

欢迎使用Gen2 DpethAI API 文档，在此页面上，您可以找到有关DepthAI API的详细信息，该信息使您可以与DepthAI设备进行交互。我们同时支持Python API和C++ API。

什么是第二代？
#############################

Gen2是DepthAI集成的一大进步，它允许用户使用管道，节点和连接来定义自己的数据流。Gen2是根据Gen1的用户反馈以及DepthAI和支持软件（如OpenVINO）的功能创建的。

基本词汇
#######################

- **主机端** 是DepthAI连接到的设备，例如PC或RPi。如果主机端发生了某些情况，则意味着此设备参与其中，而不是DepthAI本身。
- **设备端** 是DepthAI本身。如果设备方面发生了某些情况，则意味着DepthAI对此负责。
- **管道** 是设备端的完整工作流程，由节点和它们之间的连接组成-这些不能存在于管道之外。
- **节点** 是DepthAI的单个功能。它具有输入或输出或两者兼有，以及要定义的属性（例如摄像机节点上的分辨率或神经网络节点中的Blob路径）。
- **连接** 是一个节点的输出与另一节点的输入之间的链接。为了定义管道数据流，连接定义了将数据发送到哪里以达到预期结果。
- **XLink** 是一种中间件，能够在设备和主机之间交换数据。XLinkIn节点允许将数据从主机发送到设备，XLinkOut则相反。

入门
######################

为了帮助您开始使用Gen2 API，我们准备了有关API用法的多个示例，还有应用示例，以及一些有见地的教程。

在运行示例之前，请使用以下命令安装DepthAI Python库

.. code:: bash

  python3 -m pip install depthai

API
##########################

.. class:: Device
  :canonical: depthai.Device

  用与之交互的方法表示DepthAI设备。


  **构造函数**

  .. function:: __init__(pipeline: Pipeline) -> Device

  .. function:: __init__(pipeline: Pipeline, usb2_mode: bool) -> Device

  .. function:: __init__(pipeline: Pipeline, cmd_path: str) -> Device

  .. function:: __init__(pipeline: Pipeline, device_info: DeviceInfo, usb2_mode: bool) -> Device

  .. function:: __init__(pipeline: Pipeline, device_info: DeviceInfo, cmd_path: str) -> Device

    **pipeline** 是一个 :class:`Pipeline` 对象，定义了所有节点和连接的对象。

    **usb2_mode**, 为 :code:`True/False`, 允许DepthAI使用USB2协议（而不是USB3）进行通信。这降低了管道的吞吐量，但允许使用> 1m USB电缆进行连接

    **cmd_path** 是要加载到DepthAI设备中的自定义固件的路径，而不是默认路径。主要用于固件开发和调试。

    **device_info** 是一个 :class:`DeviceInfo` 对象, 从 :class:`XLinkConnection` 获得. 存储关于单个depthai设备的信息，允许针对应该运行管道的特定设备。


  **静态方法**

  .. function:: getFirstAvailableDevice() -> bool, DeviceInfo

    返回找到的可用于运行管道的第一个设备

  .. function:: getAllAvailableDevices() -> List[DeviceInfo]

    返回所有已发现的deptai设备的列表

  .. function:: getEmbeddedDeviceBinary(usb2_mode: bool) -> List[int]

    返回用于引导设备的固件

  .. function:: getDeviceByMxId(mx_id: str) -> bool, DeviceInfo

    根据设备序列号以十六进制字符串 (例如 :code:`41440410B1933CBC00`).返回设备。可以使用获得 :func:`DeviceInfo.getMxId`


  **成员方法**

  .. function:: startPipeline()

    开始执行初始化期间提供的管道

  .. function:: isPipelineRunning() -> bool

    如果该 :func:`startPipeline` 方法被调用，则返回 :code:`True`，否则返回 :code:`False` 。

  .. function:: getOutputQueue(name: str, mazSize: int, overwrite: bool) -> DataOutputQueue

    创建 :class:`DataOutputQueue` 使用指定名称的队列中数据的对象。

  .. function:: getInputQueue(name: str, mazSize: int, overwrite: bool) -> DataInputQueue

    创建 :class:`DataInputQueue` 将数据发送到具有指定名称的队列的对象。

  .. function:: setLogLevel(level: LogLevel)

    设置设备日志记录级别，该级别确定要打印的日志的数量和详细程度。

  .. function:: getLogLevel() -> LogLevel

    返回当前设备的日志记录级别。


.. class:: Pipeline
  :canonical: depthai.Pipeline

  表示管道，是一组节点和它们之间的连接，表示所需的数据流。

  **一般方法**

  .. function:: setOpenVINOVersion()

    设置 openvino 的版本。 示例: `pipeline.setOpenVINOVersion(version = dai.OpenVINO.Version.VERSION_2021_2)`
    其中 `dai` 是depthai软件包: `import depthai as dai`; `pipeline` 是创建的管道对象。

  .. function:: getAssetManager() -> AssetManager

    返回 :class:`AssetManager` 分配给当前管道的实例。请注意，它未填充节点的资产-要获取已填充的实例，请使用 :func:`getAllAssets`

  .. function:: getGlobalProperties() -> GlobalProperties

    返回 :class:`GlobalProperties` 分配给当前管道的实例。

  .. function:: getAllAssets() -> AssetManager

    返回填充了所有节点数据的AssetManager

  .. function:: remove(node: object)

    从管道中删除指定的节点

  .. function:: getAllNodes() -> List[object]

    返回当前管道中定义的所有节点

  .. function:: getNode(idx: int) -> object

    返回具有指定标识或 or :code:`None` 其他标识的管道节点。 每个节点都有 :code:`id` 此方法中使用的字段。

  .. function:: getConnections() -> List[Connection]

    返回 :class:`Connection` 对象列表, 表示节点之间的互连。

  .. function:: link(out: object, inp: object)

    允许将一个节点的输出链接到另一节点的输入。

  .. function:: unlink(out: object, inp: object)

    允许删除两个节点之间先前创建的链接。

  **节点创建方法**

  .. function:: createXLinkIn() -> XLinkIn

    创建 :class:`XLinkIn` 节点

  .. function:: createXLinkOut() -> XLinkOut

    创建 :class:`XLinkOut` 节点

  .. function:: createNeuralNetwork() -> NeuralNetwork

    创建 :class:`NeuralNetwork` 节点

  .. function:: createColorCamera() -> ColorCamera

    创建 :class:`ColorCamera` 节点

  .. function:: createVideoEncoder() -> VideoEncoder

    创建 :class:`VideoEncoder` 节点

  .. function:: createSPIOut() -> SPIOut

    创建 :class:`SPIOut` 节点

  .. function:: createImageManip() -> ImageManip

    创建 :class:`ImageManip` 节点

  .. function:: createMonoCamera() -> MonoCamera

    创建 :class:`MonoCamera` 节点

  .. function:: createStereoDepth() -> StereoDepth

    创建 :class:`StereoDepth` 节点


.. class:: Connection
  :canonical: depthai.Pipeline

  表示管道，是一组节点和它们之间的连接，表示所需的数据流

  **属性**

  .. attribute:: outputId

    指定输出节点的ID

  .. attribute:: outputName

    指定节点的输出名称

  .. attribute:: inputId

    指定输入节点的ID

  .. attribute:: inputName

    指定节点的输入名称

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:

   depthai.Device <?dummy=http://#Device>
   depthai.Pipeline <?dummy=http://#Pipeline>
   depthai.Connection <?dummy=http://#Pipeline>


.. include::  /pages/gen2_api2.rst

.. class:: Buffer
  :canonical: depthai.Buffer

  基本消息-二进制数据缓冲区

  **方法**

  .. function:: getData(self: object) -> numpy.ndarray[numpy.uint8]

    引用内部缓冲区
  
  .. function:: setData(*args，**kwargs)

    重载功能。

    1. setData(self: depthai.Buffer，arg0: List [int])

      .. function:: 参数data:

        将数据复制到内部缓冲区
    
    2. setData(self: depthai.Buffer，arg0: numpy.ndarray[numpy.uint8])

      .. function::参数data:

        将数据复制到内部缓冲区

.. class:: Asset
  :canonical: depthai.Asset

  数据是任何需要与管道一起存储的任意对象，例如神经网络Blob。

  **领域**

  .. attribute:: key
    :type: string

    用于存储数据的字符串值

  .. attribute:: data
    :type: list

    要存储的文件本身（例如神经网络Blob）

  .. attribute:: alignment
    :type: int

    *[Advanced]* 告诉AssetManaget将资产放置到位置/地址（偏移），以便在设备上按指定的数量对齐数据-允许设备使用资产而无需移动/复制资产


.. class:: AssetManager
  :canonical: depthai.AssetManager

  表示管道，是一组节点和它们之间的连接，表示所需的数据流

  **方法**

  .. function:: add(asset: Asset)

    将数据对象添加到AssetManager

  .. function:: add(key: str, asset: Asset)

    在特定键下将数据对象添加到AssetManager。键值将分配给“数据”字段 :code:`key`

  .. function:: addExisting(assets: List[Asset])

    将数组中的所有数据添加到AssetManager

  .. function:: set(key: str, asset: Asset)

    将在特定键下添加数据。与相比 :func:`add`, 如果指定键下已经存在任何资产，则此方法将替换它而不是报告错误

  .. function:: get(key: str) -> Asset

    返回分配给指定键的数据，否则抛出错误

  .. function:: getAll() -> List[Asset]

    返回存储在当前管道中的所有数据

  .. function:: size() -> int

    返回存储在当前管道中的对象的计数

  .. function:: remove(key: str)

    删除指定键下的数据

.. include::  /pages/includes/footer-short.rst

..
  Below you can see ?dummy=http://, this is a workaround for a Sphinx, see here - https://github.com/sphinx-doc/sphinx/issues/701#issuecomment-697116337

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:

    depthai.Buffer <?dummy=http://#Buffer>
   depthai.Asset <?dummy=http://#Asset>
   depthai.AssetManager <?dummy=http://#AssetManager>