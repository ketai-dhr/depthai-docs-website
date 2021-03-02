.. class:: XLinkIn
  :canonical: depthai.XLinkIn

  XLinkIn节点。通过XLink接收消息。

  **方法**

  .. list-table::

    * - getMaxDataSize(self)
      - 获取最大消息大小(以字节为单位)
    * - getNumFrames(self)
      - 获取池中的帧数
    * - getStreamFrames(self)
      - 获取流名称
    * - setMaxDataSize(self,maxDataSize)
      - 设置可以接收的最大数据大小
    * - setNumFrames(self,numFrames)
      - 设置池中用于转发消息的帧数
    * - setStreamFrame(self,streamName)
      - 指定要使用的XLink流名称。

  **属性**

  .. attribute:: out

    输出与主机发送相同类型的消息。

  .. function:: getMaxDataSize(self:depthai.XLinkIn) -> int

    获取最大消息大小(以字节为单位)
  
  .. function:: getNumFrames(self:depthai.XLinkIn) -> int

    获取池中的帧数
  
  .. function:: getStreamName(self:depthai.XLinkIn) -> str

    获取流名称
  
  .. function:: out

    输出与主机发送相同类型的消息。

  .. function:: setStreamName(self:depthai.XLinkIn,streamName: string)

    指定要使用的XLink流名称。

    该名称不应以双下划线“ __”开头,因为保留给内部使用。

      .. function:: 参数name:

        流名称

  .. function:: setMaxDataSize(self:depthai.XLinkIn,maxDataSize: int)

    设置可以接收的最大邮件大小

    .. function:: 参数maxDataSize:

      最大大小(以字节为单位)

  .. function:: setNumFrames(self:depthai.XLinkIn,numFrames: int)

    设置池中用于转发消息的帧数。

      .. function:: 参数numFrames:

        池中的最大帧数

.. class:: XLinkOut
  :canonical: depthai.XLinkOut

  XLinkOut节点。通过XLink发送消息。

  **方法**

  .. list-table::

    * - getFpsLimit(self)
      - 每秒获取消息中的速率限制
    * - getMetadataOnly(self)
      - 获取是否仅传输消息属性而不传输数据
    * - getStreamName(self)
      - 获取流名称
    * - setFpsLimit(self,fpsLimit)
      - 指定消息发送限制
    * - setMetadataOnly(self,arg0)
      - 指定是否仅传输消息属性,而不传输缓冲区数据
    * - setStreamName(self,streamName)
      - 指定要使用的XLink流名称。

  **属性**

  .. attribute:: input

    通过XLink流传输的任何类型的消息的输入

  .. function:: getFpsLimit(self: depthai.XLinkOut) -> float

    每秒获取消息中的速率限制
  
  .. function:: getMetadataOnly(self: depthai.XLinkOut) -> bool

    获取是否仅传输消息属性而不传输数据
  
  .. function:: getStreamName(self: depthai.XLinkOut) -> str

    获取流名称
  
  .. function:: input

    通过XLink流传输的任何类型的消息的输入

    默认队列被阻止,大小为8

  .. function:: setStreamName(self: depthai.XLinkOut,streamName: str)

    指定要使用的XLink流名称。

    该名称不应以双下划线“ __”开头,因为保留给内部使用。

      .. function:: 参数name:

        流名称

  .. function:: setFpsLimit(self: depthai.XLinkOut,fpsLimit: float)

    指定消息发送限制。它是从指定比率近似得出的。

      .. function:: 参数fps:

        每秒消息中的近似速率限制
  
  .. function:: setMetadataOnly(self: depthai.arg0: bool)

    指定是否仅传输消息属性,而不传输缓冲区数据

.. class:: XLinkDeviceState
  :canonical: depthai.XLinkDeviceState

  **成员**

    X_LINK_ANY_STATE

    X_LINK_BOOTED

    X_LINK_UNBOOTED

    X_LINK_BOOTLOADER

  **属性**

  .. attribute:: name

.. class:: XLinkPlatform
  :canonical: depthai.XLinkPlatform

  **成员**

    X_LINK_ANY_PLATFORM

    X_LINK_MYRIAD_2

    X_LINK_MYRIAD_X
  
  **属性**

  .. attribute:: name

.. class:: XLinkProtocol
  :canonical: depthai.XLinkProtocol

  **成员**

    X_LINK_USB_VSC

    X_LINK_USB_CDC

    X_LINK_PCIE

    X_LINK_IPC

    X_LINK_NMB_OF_PROTOCOLS

    X_LINK_ANY_PROTOCOL

  **属性**

  .. attribute:: name

.. class:: ColorCamera
  :canonical: depthai.ColorCamera

  ColorCamera节点。与颜色传感器一起使用。

  **Inputs**

  .. attribute:: inputConfig

    可链接到 :class:`ImageManipConfig`,ImageManipConfig消息的输入, 允许修改裁剪区域,水平翻转,帧数据类型等。默认队列是非阻塞的,大小为8。

  .. attribute:: inputControl

    适用于传感器的初始控制选项,可链接到 :class:`CameraControl`,CameraControl消息的输入, 允许将信号发送到相机,例如捕获静止图像。默认队列是非阻塞的,大小为8。

  **Outputs**

  .. attribute:: video

    彩色摄像机视频流NV12编码(YUV420,UV平面交错)输出,适用于VideoEncoder

  .. attribute:: preview

    彩色摄像机视频流按比例缩放至预览大小,可使用 :func:`setPreviewSize` ,输出ImgFrame消息,该消息携带BGR / RGB平面/交错编码帧数据。

    适用于NeuralNetwork节点。

  .. attribute:: still

   彩色摄像机输出,经过NV12编码(YUV420,UV平面交错),当从 :attr:`inputControl` 输入发送“拍摄静止”命令时捕获。

  **方法**

  .. function:: getCamId(self: depthai.ColorCamera) -> int

    获取使用的摄像头,彩色相机的ID为0

  .. function:: setCamId(self: depthai.ColorCamera,arg0: int)

    设置使用的摄像头 colorcamera的ID为0
  
  .. warning::

    getCamId/setCamId最新版depthai已不推荐使用,请使用以下两种方法。

  .. function:: setBoardSocket(boardSocket: CameraBoardSocket)

    将摄像机插槽分配给节点

  .. function:: getBoardSocket() -> CameraBoardSocket

    检索要使用的板卡插槽

  .. function:: setColorOrder(colorOrder: ColorCameraProperties.ColorOrder)

    设置框架的颜色顺序

  .. function:: getColorOrder() -> ColorCameraProperties.ColorOrder

    获取预览输出帧的颜色顺序。RGB或BGR。

  .. function:: setFp16(fp16: bool)

    强制帧值为FP16格式

  .. function:: getFp16() -> bool

    获取预览输出帧的fp16(0..255)数据

  .. function:: setFps(fps: float)

    限制节点的FPS,从而允许例如减少此节点产生的帧数量

  .. function:: getFps() -> float

    获取相机应产生帧的速率
  
  .. function:: getImageOrientation(self: depthai.ColorCamera) -> CameraImageOrientation

    获取相机图像方向
  
  .. function:: setImageOrientation(self: depthai.ColorCamera, boardSocket: CameraImageOrientation)

    设置相机图像方向
  
  .. function:: getInterleaved(self: depthai.ColorCamera) -> bool

    获取预览输出帧的平面或交错数据
  
  .. function:: setInterleaved(self: depthai.ColorCamera, interleaved: bool)

    设置预览输出帧的平面或交错数据

  .. function:: setPreviewSize(width: int, height: int)

    设置所需的预览尺寸,可用作 :attr:`preview` 输出

  .. function:: getPreviewSize() -> Tuple[int, int]

    以元组的形式返回先前使用 :func:`setPreviewSize` 设置的预览大小

  .. function:: getPreviewWidth() -> int

    返回先前使用 :func:`setPreviewSize` 设置的预览宽度

  .. function:: getPreviewHeight() -> int

    返回先前使用 :func:`setPreviewSize` 设置的预览的高度

  .. function:: setVideoSize(width: int, height: int)

    设置所需的视频尺寸,可用作 :attr:`video` 输出

  .. function:: getVideoSize() -> Tuple[int, int]

    返回先前使用 :func:`setVideoSize` 设置的视频尺寸

  .. function:: getVideoWidth() -> int

    返回先前使用 :func:`setVideoSize` 设置的视频的宽度

  .. function:: getVideoHeight() -> int

    返回先前使用 :func:`setVideoSize` 设置的视频的高度

  .. function:: setStillSize()

    设置 :attr:`still` 输出产生的所需图像尺寸

  .. function:: getStillSize()

    返回先前使用 :func:`setStillSize` 设置的框架尺寸

  .. function:: getStillWidth()

    返回先前使用 :func:`setStillSize` 设置的帧的宽度

  .. function:: getStillHeight()

    返回先前使用 :func:`setStillSize` 设置的框架的高度

  .. function:: setResolution(resolution: ColorCameraProperties.SensorResolution)

    设置相机传感器分辨率,确定可以从该节点获得的最大分辨率

  .. function:: getResolution() -> ColorCameraProperties.SensorResolution

    返回先前使用 :func:`setResolution` 设置的传感器分辨率

  .. function:: getResolutionSize() -> Tuple[int, int]

    返回传感器分辨率尺寸

  .. function:: getResolutionWidth() -> int

    返回传感器分辨率宽度

  .. function:: getResolutionHeight() -> int

    返回传感器分辨率高度

  .. function:: sensorCenterCrop()

    从原始传感器分辨率 (使用设置 :func:`setResolution`)启用中心裁切

  .. function:: setSensorCrop(x: float, y: float)

    将范围内的值设置为裁剪参数,确定裁剪框的大小 :code:`[0, 1]` 

  .. function:: getSensorCrop() -> Tuple[float, float]

    返回以前使用 :func:`setSensorCrop` 设置的传感器裁剪集的x和y参数

  .. function:: getSensorCropX() -> float

    返回以前使用  :func:`setSensorCrop` 设置的传感器裁剪集的x参数

  .. function:: getSensorCropY() -> float

    返回以前使用 :func:`setSensorCrop` 设置的传感器裁剪集的y参数

  .. function:: setWaitForConfigInput(wait: bool)

    如果设置为 :code:`True`, 则仅当配置信息到达时,才会处理来自摄像机传感器的帧并将其发送到输出

  .. function:: getWaitForConfigInput() -> bool

    返回先前使用 :func:`setWaitForConfigInput` 设置的值,如果等待inputConfig消息,则为true,否则为false

  .. function:: setPreviewKeepAspectRatio(keep: bool)

    如果设置为 :code:`True`, :attr:`preview` 则将裁剪输出并调整其大小以保留相机分辨率的h / w比例。否则,图像将被压缩到任一方向,以实现使用指定的精确输出 :func:`setPreviewSize`

  .. function:: getPreviewKeepAspectRatio() -> bool
 
    返回先前使用 :func:`setPreviewKeepAspectRatio` 设置的值,预览保持纵横比选项


.. class:: MonoCamera
  :canonical: depthai.MonoCamera

  MonoCamera节点。与灰度传感器一起使用。

  **Outputs**

  .. attribute:: out

    输出ImgFrame消息,该消息携带RAW8编码的(灰度)帧数据。

    适合使用StereoDepth节点。
  
  .. attribute:: initialControl
    
    适用于传感器的初始控制选项

  .. attribute:: inputControl

    CameraControl消息的输入,可以在运行时中修改摄像机参数默认队列被阻止,大小为8

  **方法**

  .. function:: getCamId(self: depthai.MonoCamera) -> int

    获取使用的摄像头id, 左边灰度相机的ID为1,右边灰度相机的ID为2
  
  .. function:: setCamId(self: depthai.ColorCamera,arg0: int)

    设置使用的摄像头id, 左边灰度相机的ID为1,右边灰度相机的ID为2
  
  .. warning::

    getCamId/setCamId最新版depthai已不推荐使用,请使用以下两种方法。

  .. function:: setBoardSocket()

    设置要使用的板卡插槽

  .. function:: getBoardSocket()

    检索要使用的板卡插槽
  
  .. function:: getImageOrientation(self: depthai.ColorCamera) -> CameraImageOrientation

    获取相机图像方向
  
  .. function:: setImageOrientation(self: depthai.ColorCamera, boardSocket: CameraImageOrientation)

    设置相机图像方向

  .. function:: setResolution(resolution: MonoCameraProperties.SensorResolution)

    设置传感器分辨率

  .. function:: getResolution() -> MonoCameraProperties.SensorResolution

    获取传感器分辨率

  .. function:: setFps(fps: float)

    设置相机产生帧的速率,每秒帧率。

  .. function:: getFps() -> float

    获取相机应产生帧的速率,每秒帧率。

  .. function:: getResolutionSize() -> Tuple[int, int]

    获取传感器分辨率大小

  .. function:: getResolutionWidth() -> int

    获取传感器分辨率宽度

  .. function:: getResolutionHeight() -> int

    获取传感器分辨率高度

.. class:: NNdata
  :canonical: depthai.NNData

  NNData消息。承载张量及其元数据

  **方法**

  .. function:: getAllLayerNames(self: depthai.NNData) → List[str]

    添加的所有图层的名称

  .. function:: getAllLayers(self: depthai.NNData) → List[depthai.TensorInfo]

    所有图层及其信息

  .. function:: getFirstLayerFp16(self: depthai.NNData) → List[float]

    便利功能，可从第一层FP16张量中检索浮点值,float
  .. function:: getFirstLayerInt32(self: depthai.NNData) → List[int]

    便利功能，可从第一层张量中检索INT32值,INT32数据

  .. function:: getFirstLayerUInt8(self: depthai.NNData) → List[int]

    便捷功能，可从第一层检索U8数据,U8二进制数据

  .. function:: getLayer(self: depthai.NNData, name: str, tensor: depthai.TensorInfo) → bool

    检索层张量信息

    .. function:: name:

      图层名称

    .. function:: datatype:

      输出该层的张量信息,如果存在图层则为true，否则为false

  .. function:: getLayerDatatype(self: depthai.NNData, name: str, datatype: depthai.TensorInfo.DataType) → bool

    检索图层张量的数据类型

    .. function:: name:

      图层名称，U8二进制数据

    .. function:: datatype:

      层张量的数据类型,如果存在图层则为true，否则为false

  .. function:: getLayerFp16(self: depthai.NNData, name: str) → List[float]

    便利功能，可从FP16张量张量中检索浮点值

    .. function:: name:

      图层名称，U8二进制数据
  .. function:: getLayerInt32(self: depthai.NNData, name: str) → List[int]

    便利功能，可从层张量中检索INT32值

    .. function:: name:

      图层名称，U8二进制数据

  .. function:: getLayerUInt8(self: depthai.NNData, name: str) → List[int]

    便捷功能，可从图层中检索U8数据

    .. function:: name:

      图层名称，U8二进制数据

  .. function:: hasLayer(self: depthai.NNData, name: str) → bool

    检查给定的图层是否存在

    .. function:: name:

      图层名称，如果存在图层则为true，否则为false

  .. function:: setLayer(*args, **kwargs)

    重载功能。

    1. setLayer(self: depthai.NNData, name: str, data: numpy.ndarray[numpy.uint8]) 

    设置数据类型为U8的图层。

    .. function:: 参数name:

      图层名称
    
    .. function:: 参数data:

      要存储的数据
    2. setLayer(self: depthai.NNData, name: str, data: List[int])

      设置数据类型为U8的图层。整数被强制转换为字节。

      .. function:: 参数name:

      图层名称
    
      .. function:: 参数data:

        要存储的数据

    3. setLayer(self: depthai.NNData, name: str, data: List[float])

      设置数据类型为FP16的图层。浮点值将转换为FP16。

      .. function:: 参数name:

      图层名称
    
      .. function:: 参数data:

        要存储的数据

    4. setLayer(self: depthai.NNData, name: str, data: List[float]) 

      设置数据类型为FP16的图层。将双精度值转换为FP16。

      .. function:: 参数name:

      图层名称
    
      .. function:: 参数data:

        要存储的数据
 

.. class:: NeuralNetwork
  :canonical: depthai.NeuralNetwork

  NeuralNetwork节点。对输入数据进行神经推理。

  .. attribute:: input

    输入消息,其中包含要推论为默认队列的数据,大小为5

  .. attribute:: out

    输出带有推断结果的NNData消息
  
  .. attribute:: passthrough

    执行推断的直通消息。

    适用于将输入队列设置为非阻塞行为的情况。

  **方法**

  .. function:: setBlobPath(path: str)

    将网络Blob加载到资产中,并在管道启动后使用。

    如果文件不存在或不是有效的网络Blob,则抛出该异常。

    .. function:: 参数path:

      神经网络模型文件(.blob)的路径

  .. function:: setNumPoolFrames(numFrames: int)

    指定池中可使用多少帧

    .. function:: 参数numFrames:

      池中将有多少帧
  
  .. function:: setNumInferenceThreads(self: depthai.NeuralNetwork,numThreads:int)

    节点应使用多少个线程来运行网络。

    .. function:: 参数numThreads:

      专用于该节点的线程数
  
  .. function:: setNumNCEPerInferenceThread(self: depthai.NeuralNetwork, numNCEPerThread: int)

    一个线程应使用多少个神经计算引擎进行推理

    .. function:: 参数numNCEPerThread:

      每个线程的NCE数量

.. class:: Node
  :canonical: depthai.Node

  抽象节点

  **属性** 

  .. attribute:: id

  节点编号

  **classes**
  
  .. function:: Connection:

    输入和输出之间的连接
  
  .. function:: Id

    节点标识符。对于单个管道中的每个节点都是唯一的

  **方法**

  .. function:: getAssets(self: depthai.Node) → List[depthai.Asset]

    检索所有节点资产

  .. function:: getInputs(self: depthai.Node) → List[dai::Node::Input]

    检索所有节点输入

  .. function:: getName(self: depthai.Node) → str

    检索节点名称

  .. function:: getOutputs(self: depthai.Node) → List[dai::Node::Output]

    检索所有节点输出

.. class:: OpenVINO
  :canonical: depthai.OpenVINO

  支持与OpenVINO相关的基本操作，例如神经网络Blob的版本识别，…

  *classes**

  .. function:: Version

    OpenVINO版本支持的版本信息

    **成员**

      VERSION_2020_1

      VERSION_2020_2

      VERSION_2020_3

      VERSION_2020_4

      VERSION_2021_1

      VERSION_2021_2

  **属性**

  .. attribute:: name

  **方法**

  .. function:: static areVersionsBlobCompatible(v1: dai::OpenVINO::Version, v2: dai::OpenVINO::Version) → bool

    检查两个Blob版本是否兼容

  .. function:: OpenVINO.getBlobLatestSupportedVersion(majorVersion: int, majorVersion: int) -> dai::OpenVINO::Version

    返回给定的blob版本最新受支持的版本。

    .. function:: 参数majorVersion:

      OpenVINO blob的主要版本
    
    .. function:: 参数minorVersion:

      OpenVINO blob的次要版本


  .. function::OpenVINO.getBlobSupportedVersions(majorVersion: int, majorVersion: int) -> List[dai::OpenVINO::Version]

    返回指定blob主版本和次版本的受潜在支持版本的列表。

    .. function:: 参数majorVersion:

      OpenVINO blob的主要版本
    
    .. function:: 参数minorVersion:

      OpenVINO blob的次要版本

  .. function:: static getVersionName(version: dai::OpenVINO::Version) → str

    返回给定版本的字符串表示形式

    .. function:: 参数version:

      OpenVINO版本
      
  .. function:: static getVersions() → List[dai::OpenVINO::Version]

    支持的版本

  .. function:: static parseVersionName(versionString: str) → dai::OpenVINO::Version

    从字符串表示形式创建Version。如果不可能，则抛出。

    .. function:: 参数versionString:
      
      版本为字符串

.. class:: SPIOut
  :canonical: depthai.SPIOut

  SPIOut节点。通过SPI发送消息。

  **属性**

  .. attribute:: input

    通过SPI流传输的任何类型消息的输入

    默认队列被阻止，大小为8

  .. function:: setBusId(self: depthai.SPIOut,id: int)

    指定要使用的SPI总线号

    .. function:: 参数id:
      
      SPI总线ID
  
  .. function:: setStreamName(self: depthai.SPIOut,name: str)

    指定节点将通过其发送数据的流名称

    .. function:: name:

      流名称
  
.. class:: ImageManip
  :canonical: depthai.ImageManip

  ImageManip节点。裁剪,调整大小,变形,…传入图像帧的能力

  **Inputs**

  .. attribute:: initialConfig
    
    操纵框架时要使用的初始配置

  .. attribute:: inputConfig

    输入ImageManipConfig消息,能够在运行时修改参数默认队列被阻止,大小为8

  .. attribute:: inputImage

    要修改的输入图像默认队列大小为8

  **Outputs**

  .. attribute:: out

    输出带有修改图像的ImgFrame消息。

  **方法**

  .. function:: setCropRect(xmin: float, ymin: float, xmax: float, ymax: float)

    仅允许裁剪图像的所需部分

  .. function:: setCenterCrop(ratio: float, whRatio: float)

    允许为图像指定任意裁剪。

    :code:`whRatio` 确定裁切部分的宽高比(下图中的白框 :code:`whRatio` 设置为 :code:`1`),

    :code:`ratio` 正在确定裁切部分的实际大小(下图中的绿色框 :code:`ratio` 设置为 :code:`0.7`)

  .. function:: setResize(w: int, h: int)

      将框架调整为指定的宽度和高度

  .. function:: setResizeThumbnail(w: int, h: int, bgRed: int, bgGreen: int, bgBlue: int)

      调整框架大小,保留原始长宽比,用指定的颜色填充缺少的空间

  .. function:: setFrameType(type: RawImgFrame.Type)

      将输入帧修改为指定的图像帧类型(之一 :class:`RawImgFrame.Type`)

  .. function:: setHorizontalFlip(flip: bool)

      如果设置为 :code:`True`, 则水平翻转接收的帧

  .. function:: setKeepAspectRatio(keep: bool)

      如果设置为 :code:`True`, 则将裁剪输出并调整其大小以保留输入分辨率的h / w比例。否则,图像将沿任一方向挤压以实现指定的精确输出

  .. function:: setWaitForConfigInput(wait: bool)

    如果设置为 :code:`True`, 则仅在配置信息到达时才处理帧

  .. function:: setNumFramesPool()

    指定如果使用者未收到该节点,则该节点将保留多少结果。达到限制后,节点将停止并等待结果被消耗,然后再生成新结果

  .. function:: setMaxOutputFrameSize(maxFrameSize: int)

    限制允许作为节点输出发送的最大帧大小

.. class:: ImageManipConfig
  :canonical: depthai.ImageManipConfig

  ImageManipConfig消息。指定图像处理选项，例如：

  - 庄稼
  - 调整大小
  - 经
  - …

  **方法**

  .. function:: getCropXMax(self: depthai.ImageManipConfig) → float

    作物区域的右下角X坐标

  .. function:: getCropXMin(self: depthai.ImageManipConfig) → float

    作物区域的左上X坐标

  .. function:: getCropYMax(self: depthai.ImageManipConfig) → float

    作物区域的右下角Y坐标

  .. function:: getCropYMin(self: depthai.ImageManipConfig) → float

    作物区域的左上Y坐标

  .. function:: getResizeHeight(self: depthai.ImageManipConfig) → int

    输出图像高度

  .. function:: getResizeWidth(self: depthai.ImageManipConfig) → int

    输出图像宽度

  .. function:: isResizeThumbnail(self: depthai.ImageManipConfig) → bool

    如果设置了调整大小缩略图模式，则为True，否则为False

  .. function:: setCenterCrop(self: depthai.ImageManipConfig, ratio: float, whRatio: float)

    指定居中裁切。

    .. function:: 参数ratio:

      输入图像与裁切区域之间的比率（0..1）

    .. function:: 参数whRatio:

      作物区域长宽比-1等于正方形，1.7等于16：9，...

  .. function:: setCropRect(self: depthai.ImageManipConfig, xmin: float, ymin: float, xmax: float, xmax: float)

    用具有标准化值（0..1）的矩形指定裁剪

    .. function:: 参数xmin:

      矩形的左上X坐标

    .. function:: 参数ymin:

      矩形的左上Y坐标
    
    .. function:: 参数xmax:

      矩形的右下X坐标

    .. function:: 参数ymax:

      矩形的右下角Y坐标

  .. function:: setCropRotatedRect(self: depthai.ImageManipConfig, rr: depthai.RotatedRect, normalizedCoords: bool) 

    指定带有旋转矩形的裁切。可选地作为非规范化坐标

    .. function:: 参数rr:

      指定作物的旋转矩形

    .. function:: 参数normalizedCoords:

      如果真实坐标在归一化范围内（0..1），则为绝对坐标

  .. function:: setFrameType(self: depthai.ImageManipConfig, name: depthai.RawImgFrame.Type) 

    指定输出帧的类型。
    .. function:: 参数name:

      图像类型

  .. function:: setHorizontalFlip(self: depthai.ImageManipConfig, flip: bool) 

    指定水平翻转

    .. function:: 参数flip:

      设置为true以启用翻转，否则为false

  .. function:: setKeepAspectRatio(self: depthai.ImageManipConfig, keep: bool)

    指定是否保持宽高比

  .. function:: setResize(self: depthai.ImageManipConfig, w: int, h: int) 

    指定输出图像的大小。裁剪阶段后，将拉伸图像以使其适合。

    .. function:: 参数w:

      宽度（以像素为单位）

    .. function:: 参数h:

      高度（以像素为单位）

  .. function:: setResizeThumbnail(self: depthai.ImageManipConfig, w: int, h: int, bgRed: int, bgGreen: int, bgBlue: int) 

    指定输出图像的大小。裁剪阶段后，将通过保留宽高比来调整图像大小。可以选择指定背景。

    .. function:: 参数w:

      宽度（以像素为单位）

    .. function:: 参数h:

      高度（以像素为单位）

    .. function:: 参数bgRed:

      红色成分

    .. function:: 参数bgGreem:

      绿色成分

    .. function:: 参数bgBlue:

      蓝色成分

  .. function:: setReusePreviousImage(self: depthai.ImageManipConfig, reuse: bool)

    指示ImageManip不要从其队列中删除当前图像，并将其用于下一条消息。

    .. function:: 参数reuse:

      设置为true以启用重用，否则为false

  .. function:: setRotationDegrees(self: depthai.ImageManipConfig, deg: float) 

    指定以度为单位的顺时针旋转

    .. function:: 参数deg:

      旋转角度

  .. function:: setRotationRadians(self: depthai.ImageManipConfig, rad: float)

    指定以弧度为单位的顺时针旋转

    .. function:: 参数rad:
    
      弧度旋转

  .. function:: setSkipCurrentImage(self: depthai.ImageManipConfig, skip: bool)

    指示ImageManip跳过当前图像并等待队列中的下一个图像。

    .. function:: 参数skip:

      设置为true以跳过当前图像，否则设置为false

  .. function:: setWarpBorderFillColor(self: depthai.ImageManipConfig, red: int, green: int, blue: int)

    指定边框像素的填充颜色。例子：

    - setWarpBorderFillColor（255,255,255）->白色
    - setWarpBorderFillColor（0,0,255）->蓝色

    .. function:: 参数red:

      红色成分

    .. function:: 参数greem:

      绿色成分

    .. function:: 参数blue:

      蓝色成分

  .. function:: setWarpBorderReplicatePixels(self: depthai.ImageManipConfig) 

    指定扭曲复制边框像素

  .. function:: setWarpTransformFourPoints(self: depthai.ImageManipConfig, pt: List[depthai.Point2f], normalizedCoords: bool)

    通过在绝对坐标或规范坐标中提供4个点来指定变形

    .. function:: 参数pt:

      4点指定翘曲
    
    .. function:: 参数normalizedCoords:

      如果将true pt解释为归一化，则为绝对

  .. function:: setWarpTransformMatrix3x3(self: depthai.ImageManipConfig, mat: List[float])

    用3x3矩阵指定变形

    .. function:: 参数mat:

      3*3矩阵

.. class:: ImgDetections
  :canonical: depthai.ImgDetections

  ImgDetections消息。携带标准化的检测结果

  **属性**

  .. attribute:: detections

    检测量

.. class:: LogLevel
  :canonical: depthai.LogLevel

  **成员**

    TRACE

    DEBUG

    INFO

    WARN

    ERR

    CRITICAL

    OFF
  
  **属性**

  .. attribute:: property name

.. class:: MobileNetDetectionNetwork
  :canonical: depthai.MobileNetDetectionNetwork

  MobileNetDetectionNetwork节点。解析MobileNet结果

.. class:: YoloDetectionNetwork
  :canonical: depthai.YoloDetectionNetwork

  YoloDetectionNetwork节点。解析Yolo结果

  **方法**

  .. function:: setAnchorMasks(self: depthai.YoloDetectionNetwork, anchorMasks: Dict[str, List[int]]) 

    设置锚mask
    
  .. function::setAnchors(self: depthai.YoloDetectionNetwork, anchors: List[float])

    设置锚点

  .. function:: setCoordinateSize(self: depthai.YoloDetectionNetwork, coordinates: int)

    设置coordinates大小

  .. function:: setIouThreshold(self: depthai.YoloDetectionNetwork, thresh: float)

    设置阈值

  .. function:: setNumClasses(self: depthai.YoloDetectionNetwork, numClasses: int)

    设置num类

.. class:: StereoDepth
  :canonical: depthai.StereoDepth

  StereoDepth节点。从左右图像对计算立体视差和深度。

  **Inputs**

  .. attribute:: left

    输入来自左侧灰度相机的帧

  .. attribute:: right

    输入来自右侧灰度相机的帧

  **Outputs**

  .. attribute:: depth

    输出原始的深度框架,使通过调用 :func:`setOutputDepth` 和 :code:`True`. 不能与 :attr:`disparity: output` 一起使用

  .. attribute:: disparity

    输出视差矩阵。不能与 :attr:`depth`

  .. attribute:: syncedLeft

    输出用于深度计算的左框架

  .. attribute:: syncedRight

    输出用于深度计算的右框架

  .. attribute:: rectifiedLeft

   输出左校正帧, 请参见 `有关校正的更多信息 <https://en.wikipedia.org/wiki/Image_rectification>`__

  .. attribute:: rectifiedRight

    输出右校正帧, 请参见 `有关校正的更多信息 <https://en.wikipedia.org/wiki/Image_rectification>`__

  **方法**

  .. function:: loadCalibrationFile(path: str)

    指定要加载用于深度计算的校准文件路径。

    .. function:: 参数path:

      校准数据。如果为空,则使用EEPROM

  .. function:: loadCalibrationData(data: List[int])

    将校准数据指定为字节向量。

    .. function:: 参数path:

      校准数据。如果为空,则使用EEPROM

  .. function:: setEmptyCalibration()

    指定当输入帧已得到纠正(例如,来自主机上的记录的源)时,应使用通过/虚拟校准

  .. function:: setInputResolution(width: int, height: int)

    指定 :code:`left` / :code:`right` 输入的输入大小。在大多数情况下,不需要调用此功能,当输入帧不是来自单反相机而是直接来自主机时,此功能很有用

  .. function:: setMedianFilter(median: StereoDepthProperties.MedianFilter)

    指定在深度计算期间应使用哪个中值滤波器。 :class:`StereoDepthProperties.MedianFilter` 可能是其中之一

  .. function:: setConfidenceThreshold(confThr: int)

    视差计算的置信度阈值

    .. function:: 参数confThr:
     
     置信度阈值0..255

  .. function:: setLeftRightCheck(enable: bool)

    在深度计算过程中启用左/右检查

  .. function:: setSubpixel(enable: bool)

    通过子像素插值(5个小数位)计算视差。

    适用于远距离

  .. function:: setExtendedDisparity(enable: bool)

    视差范围从96扩大到192,结合了全分辨率和缩小后的图像。

    适用于短距离物体

  .. function:: setRectifyEdgeFillColor(color: int)

    为边框边缘的缺失数据填充颜色

    .. function:: 参数color:

      灰度0..255或-1以复制像素

  .. function:: setRectifyMirrorFrame(enable: bool)

    启用启动校正帧的镜像

    .. function:: 参数enable:

      对于正常的视差/深度为真,否则为镜像

  .. function:: setOutputRectified(enable: bool)

    启用输出校正的帧。禁用时优化设备端的计算

  .. function:: setOutputDepth(enable: bool)

    启用输出“深度”流(从视差转换)。在某些配置中,这将禁用“视差”流

.. class:: SystemInformation
  :canonical: depthai.SystemInformation

  SystemInformation消息。携带内存使用量，CPU使用量和芯片温度。

.. class:: SystemLogger
  :canonical: depthai.SystemLogger

  SystemLogger节点。定期发送系统信息。

  **属性**
  .. function:: out

    输出SystemInformation消息，其中包含各种系统信息，例如内存和CPU使用率，温度等。
  
  **方法**

  .. function:: setRate(self: depthai.SystemLogger,hz: float)

    指定日志记录速率，以该速率将消息发送到输出

    .. function:: 参数hz:
      
      发送速率，以赫兹为单位（每秒的消息数）

.. class:: VideoEncoder
  :canonical: depthai.VideoEncoder

  VideoEncoder节点。将帧编码为MJPEG,H264或H265。

  **Inputs**

  .. attribute:: input

    NV12 ImgFrame的输入要编码默认队列被阻塞,其大小由“ setNumFramesPool”(4)设置。

  **Outputs**

  .. attribute:: bitstream

    输出ImgFrame消息,该消息携带BITSTREAM编码的(MJPEG,H264或H265)帧数据。

  **Methods**

  .. function:: setDefaultProfilePreset(*args,**kwargs)

    重载功能。

    1. setDefaultProfilePreset(self: depthai.VideoEncoder,width: int,height: int,fps: float,profile: depthai.VideoEncoderProperties.Profile)
    
    根据指定的输入大小,帧频和配置文件设置默认预设

    .. function:: 参数width:

      输入框宽度

    .. function:: 参数height:

      输入框高度
    
    .. function:: 参数fps:

      帧速率(每秒帧数)
    
    .. function:: 参数profile:

      编码配置文件

    2. setDefaultProfilePreset(self: depthai.VideoEncoder,size: Tuple[int,int],fps: float,profile: depthai.VideoEncoderProperties.Profile)

      根据指定的输入大小,帧频和配置文件设置默认预设

      .. function:: 参数size:

        输入框尺寸

      .. function:: 参数fps:

        帧速率(每秒帧数)

      .. function:: 参数profile:

        编码配置文件

  .. function:: setNumFramesPool(self: depthai.VideoEncoder,numBFrames: int)
    
    设置要插入的B帧数

  .. function:: getNumFramesPool(self: depthai.VideoEncoder) -> int

    获取池中的帧数

  .. function:: setRateControlMode(self: depthai.VideoEncoder,mode: depthai.VideoEncoderProperties.RateControlMode)

    设定速率控制模式

  .. function:: getRateControlMode(self: depthai.VideoEncoder) -> VideoEncoderProperties.Profile

    获取速率控制模式

  .. function:: setProfile(self: depthai.VideoEncoder,width: int,height: int,profile: dai.VideoEncoderProperties.Profile)

    设置编码配置文件

  .. function:: getProfile(self: depthai.VideoEncoder) -> VideoEncoderProperties.Profile

    获取资料

  .. function:: setBitrate(self: depthai.VideoEncoder,bitrateKbps: int)

    将输出比特率设置为kbps。最终比特率取决于速率控制模式

  .. function:: getBitrate(self: depthai.VideoEncoder) -> int

    获得比特率

  .. function:: setKeyframeFrequency(self: depthai.VideoEncoder,freq: int)

    设置关键帧频率。每第N个帧插入一个关键帧。

    仅适用于H264和H265配置文件

    例子:

      - 30 FPS视频,关键帧频率:30。每1s将插入一个关键帧
      - 60 FPS视频,关键帧频率:180。每3秒钟将插入一个关键帧

  .. function:: getKeyframeFrequency(self: depthai.VideoEncoder) -> int

    获取关键帧频率

  .. function:: setNumBFrames(self: depthai.VideoEncoder,numBFrames: int)

    设置要插入的B帧数

  .. function:: getNumBFrames(self: depthai.VideoEncoder) -> int

    获取B帧数

  .. function:: setQuality(self: depthai.VideoEncoder,quality: int)

    设置质量,图像质量越大,经过压缩后显示越清晰。

  .. function:: getQuality(self: depthai.VideoEncoder) -> int

    获得质量,图像质量越大,经过压缩后显示越清晰。

  .. function:: getWidth(self: depthai.VideoEncoder) -> int

    获取输入宽度

  .. function:: getHeight(self: depthai.VideoEncoder) -> int

    获取输入高度

  .. function:: getSize(self: depthai.VideoEncoder) -> Tuple[int, int]

    获取输入大小

  .. function:: setFrameRate(self: depthai.VideoEncoder,frameRate: int)

    设置预期的帧率

    .. function:: frameRate:

      帧速率(每秒帧数)

  .. function:: getFrameRate(self: depthai.VideoEncoder) -> int

    获取帧率

.. class:: CameraControl
  :canonical: depthai.CameraControl

  CameraControl消息指定各种摄像机控制命令,例如:

  - 仍然捕捉
  - 自动对焦
  - 反捆扎
  - 自动白平衡
  - 场景
  - 影响
  - …

  **class**

  .. list-table::

    * - :class:`AntiBandingMode <AntiBandingMode>`
      - 成员:
    * - :class:`AutoFocusMode <AutoFocusMode>`
      - 成员:
    * - :class:`AutoWhiteBalanceMode <AutoWhiteBalanceMode>`
      - 成员:
    * - :class:`EffectMode <EffectMode>`
      - 成员:
    * - :class:`SceneMode <SceneMode>`
      - 成员:

  **方法**

  .. list-table::

    * - getCaptureStill(self)
      - 检查是否设置了捕获静止图像的命令
    * - setAntiBandingMode(self, mode)
      - 设置命令以指定自动组合模式
    * - setAutoExposureCompensation(self, compensation)
      - 设置命令以指定自动曝光补偿
    * - setAutoExposureEnable(self)
      - 设置命令以启用自动曝光
    * - setAutoExposureLock(self, lock)
      - 设置命令以指定锁定自动曝光
    * - setAutoExposureRegion(self, startX, startY, …)
      - 设置命令以像素为单位指定自动曝光区域
    * - setAutoFocusMode(self, mode)
      - 设置命令以指定自动对焦模式
    * - setAutoFocusRegion(self, startX, startY, …)
      - 设置命令以像素为单位指定焦点区域
    * - setAutoFocusTrigger(self)
      - 设置命令以触发自动对焦
    * - setAutoWhiteBalanceLock(self, lock)
      - 设置命令以指定自动白平衡锁定
    * - setAutoWhiteBalanceMode(self, mode)
      - 设置命令以指定自动白平衡模式
    * - setBrightness(self, value)
      - 设置命令以指定自动白平衡锁定
    * - setCaptureStill(self, capture)
      - 设置命令以捕获静止图像
    * - setChromaDenoise(self, value)
      - 设置命令以指定色度降噪值
    * - setContrast(self, value)
      - 设置命令以指定自动白平衡锁定
    * - setEffectMode(self, mode)
      - 设置命令以指定效果模式
    * - setLumaDenoise(self, value)
      - 设置命令以指定亮度降噪值
    * - setManualExposure(self, exposureTimeUs, …)
      - 设置命令以手动指定曝光
    * - setManualFocus(self, lensPosition)
      - 设置命令以指定手动对焦位置
    * - setNoiseReductionStrength(self, value)
      - 设置命令以指定降噪强度
    * - setSaturation(self, value)
      - 设置命令以指定饱和度值
    * - setSceneMode(self, mode)
      - 设置命令以指定场景模式
    * - setSharpness(self, value)
      - 设置命令以指定清晰度值
    * - setStartStreaming(self)
      - 设置命令以开始流式传输
    * - setStopStreaming(self)
      - 设置命令以停止流式传输

  .. function:: AntiBandingMode

    **成员**

      OFF

      MAINS_50_HZ

      MAINS_60_HZ

      AUTO

    **属性**

      .. function:: property name
  
  .. function:: AutoFocusMode

    **成员**

      OFF

      AUTO

      MACRO

      CONTINUOUS_VIDEO

      CONTINUOUS_PICTURE

      EDOF

    **属性**

      .. function:: property name
  
  .. function:: AutoWhiteBalanceMode

    **成员**

      OFF

      AUTO

      INCANDESCENT

      FLUORESCENT

      WARM_FLUORESCENT

      DAYLIGHT

      CLOUDY_DAYLIGHT

      TWILIGHT

      SHADE

    **属性**

      .. function:: property name
  
  .. function:: EffectMode

    **成员**

      OFF

      MONO

      NEGATIVE

      SOLARIZE

      SEPIA

      POSTERIZE

      WHITEBOARD

      BLACKBOARD

      AQUA

    **属性**

      .. function:: property name
  
  .. function:: SceneMode

    **成员**

      UNSUPPORTED

      FACE_PRIORITY

      ACTION

      PORTRAIT

      LANDSCAPE

      NIGHT

      NIGHT_PORTRAIT

      THEATRE

      BEACH

      SNOW

      SUNSET

      STEADYPHOTO

      FIREWORKS

      SPORTS

      PARTY

      CANDLELIGHT

      BARCODE

    **属性**

      .. function:: property name
  
  .. function:: getCaptureStill(self: depthai.CameraControl) -> bool

    检查是否设置了捕获静止图像的命令,如果设置了捕获静止命令,则为True。
  
  .. function:: setAntiBandingMode(self: depthai.CameraControl,mode: depthai.RawCameraControl.AntiBandingMode)

    设置命令以指定自动组合模式 
    .. function:: 参数mode:

      使用自动分组模式
  
  .. function:: setAutoExposureCompensation(self: depthai.CameraControl,compensation: int)

    设置命令以指定自动曝光补偿

    .. function:: 参数compensation:

      补偿值在-128..127之间
  
  .. function:: setAutoExposureEnable(self: depthai.CameraControl, lock: bool)

    设置命令以指定锁定自动曝光

    .. function:: 参数lock:

      启用或禁用自动曝光锁定模式
  
  .. function:: setAutoExposureRegion(self: depthai.CameraControl,startX: int,startY: int,width: int,height: int)

    设置命令以像素为单位指定自动曝光区域

    .. function:: 参数startX:

      区域左上角的X坐标

    .. function:: 参数startY:

      区域左上角的Y坐标
    
    .. function:: 参数width:

      区域宽度
    
    .. function:: height:

      区域高度

  .. function:: setAutoFocusMode(self : depthai.CameraControl,mode : depthai.RawCameraControl.AutoFocusMode)

    设置命令以指定自动对焦模式

  .. function:: setAutoFocusRegion(self : depthai.CameraControl,startX : int,startY : int,width : int,height : int)

    设置命令以像素为单位指定焦点区域

    .. function:: 参数startX:

      区域左上角的X坐标

    .. function:: 参数startY:

      区域左上角的Y坐标
    
    .. function:: 参数width:

      区域宽度
    
    .. function:: height:

      区域高度

  .. function:: setAutoFocusTrigger(self : depthai.CameraControl)

    设置命令以触发自动对焦

  .. function:: setAutoWhiteBalanceLock(self : depthai.CameraControl,lock : bool)

    设置命令以指定自动白平衡锁定

    .. function:: 参数lock:

      自动白平衡锁定模式启用或禁用
  
  .. function:: setAutoWhiteBalanceMode(self : depthai.CameraControl,mode : depthai.RawCameraControl.AutoWhiteBalanceMode)

    设置命令以指定自动白平衡模式

    .. function:: 参数mode:

      自动白平衡模式使用
  
  .. function:: setBrightness(self : depthai.CameraControl,value : int)

    设置命令以指定自动白平衡锁定

    .. function:: 参数lock:

      自动白平衡锁定模式启用或禁用

  .. function:: setCaptureStill(self : depthai.CameraControl,捕获: bool)

    设置命令以捕获静止图像

  .. function:: setChromaDenoise(self : depthai.CameraControl,value : int)

    设置命令以指定色度降噪值

    .. function:: 参数value:

      色度降噪
  
  .. function:: setContrast(self : depthai.CameraControl,value : int)

    设置命令以指定自动白平衡锁定

    .. function:: 参数lock:

      自动白平衡锁定模式启用或禁用

  .. function:: setEffectMode(self : depthai.CameraControl,mode: depthai.RawCameraControl.EffectMode)

    设置命令以指定效果模式

    .. function:: 参数mode:

      效果模式

  .. function:: setLumaDenoise(self : depthai.CameraControl,value : int)

    设置命令以指定亮度降噪值

    .. function:: 参数value:

      亮度降噪
  
  .. function:: setManualExposure(自: depthai.CameraControl,exposureTimeUs : INT,sensitivityIso : INT)

    设置命令以手动指定曝光

    .. function:: 参数exposureTimeUs:

      曝光时间(以微秒为单位)
    
    .. function:: 参数sensitivityIso:

      感光度作为ISO值
  
  .. function:: setManualFocus(self : depthai.CameraControl,lensPosition : int)

    设置命令以指定手动对焦位置

    .. function:: 参数lensPosition:

      指定镜头位置0..255

  .. function:: setNoiseReductionStrength(self : depthai.CameraControl,value : int)

    设置命令以指定降噪强度

    .. function:: 参数value:

      降噪强度

  .. function:: setSaturation(self : depthai.CameraControl,value : int)

    设置命令以指定饱和度值

    .. function:: 参数value:

      饱和

  .. function:: setSceneMode(self : depthai.CameraControl,mode : depthai.RawCameraControl.SceneMode)

    设置命令以指定场景模式
  
    .. function:: 参数mode:

      场景模式
  
  .. function:: setSharpness(self : depthai.CameraControl,value : int)

    设置命令以指定清晰度值

    .. function:: 参数value:

      清晰度

  .. function:: setStartStreaming(self : depthai.CameraControl)

    设置命令以开始流式传输

  .. function:: setStopStreaming(self : depthai.CameraControl)

    设置命令以停止流式传输

.. class:: CameraBoardSocket
  :canonical: depthai.CameraBoardSocket

  **成员**

    AUTO
    
    RGB
    
    LEFT

    RIGHT

  **属性**

    .. attribute:: property name

.. class:: CameraImageOrientation
  :canonical: depthai.CameraImageOrientation

  **成员**

    AUTO

    NORMAL

    HORIZONTAL_MIRROR

    VERTICAL_FLIP

    ROTATE_180_DEG
  
  **属性**

    .. attribute:: property name
  
.. class:: DataInputQueue
  :canonical: depthai.DataInputQueue

  通过XLink流访问发送消息

  **方法**

  .. list-table::

    * - getBlocking(self)
      - 满时获取当前队列行为(最大大小)
    * - getMaxSize(self, arg0)
      - 获取队列的最大大小
    * - getName(self)
      - 获取队列名称
    * - send(*args, **kwargs)
      - 重载功能。
    * - setBlocking(self, blocking)
      - 设置满时的队列行为(maxSize)
    * - setMaxSize(self, maxSize)
      - 设置队列的最大大小

  .. function:: getBlocking(self : depthai.DataInputQueue)

    满时获取当前队列行为(最大大小),如果阻塞则为true,否则为false

  .. function:: getMaxSize(self : depthai.DataInputQueue,arg0 : int)

    获取队列的最大大小,最大队列大小

  .. function:: getName(self : depthai.DataInputQueue)

    获取队列名称

  .. function:: send(*args,**kwargs)

    重载功能。

    1. send(self:depthai.DataInputQueue,msg:depthai.ADatatype)

    将消息添加到队列,该消息将被提取并发送到设备。如果“阻止”行为为真,则可以阻止,也可以覆盖最旧的

    .. function:: 参数msg:

      要添加到队列中的消息

    2. send(self:depthai.DataInputQueue,rawMsg:depthai.RawBuffer)

    将原始消息添加到队列,该消息将被提取并发送到设备。如果“阻止”行为为真,则可以阻止,也可以覆盖最旧的

    .. function:: 参数msg:

      要添加到队列中的消息
  
  .. function:: setBlocking(self : depthai.DataInputQueue,阻塞: bool)

    指定是阻止还是覆盖队列中最旧的消息

    .. function:: 参数blocking:

      指定是阻止还是覆盖队列中最旧的消息

  .. function:: setMaxSize(self : depthai.DataInputQueue,maxSize : int)

    设置队列的最大大小

    .. function:: 参数maxSize:

      指定队列中的最大消息数

.. class:: DataOutputQueue
  :canonical: depthai.DataOutputQueue

  访问接收来自XLink流的消息

  **方法**

  .. list-table::

    * - addCallback(*args,**kwargs)
      - 重载功能。
    * - get(self)
      - 阻止直到有消息可用。
    * - getAll(self)
      - 阻塞直到队列中至少有一条消息。
    * - getBlocking(self)
      - 满时获取当前队列行为(最大大小)
    * - getMaxSize(self,arg0)
      - 获取队列的最大大小
    * - getName(self)
      - 获取队列名称
    * - has(self)
      - 检查队列前面是否有消息(不为空)
    * - removeCallback(self,callbackId)
      - 删除回调
    * - setBlocking(self,blocking)
      - 设置满时的队列行为(maxSize)
    * - setMaxSize(self,maxSize)
      - 设置队列的最大大小
    * - tryGet(self)
      - 尝试从队列中检索消息。
    * - tryGetAll(self)
      - 尝试检索队列中的所有消息。
  
  .. function:: addCallback(*args, **kwargs)

    重载功能。

    1. addCallback(self: depthai.DataOutputQueue, callback: std::function<void (std::__cxx11::basic_string<char, std::char_traits<char>, std::allocator<char> >, std::shared_ptr<dai::ADatatype>)>) -> int

    在收到的消息上添加回调

    .. function:: 参数callback:

      带有队列名称和消息指针的回调函数

    2. addCallback(self: depthai.DataOutputQueue, callback: std::function<void (std::shared_ptr<dai::ADatatype>)>) -> int

    在收到的消息上添加回调

    .. function:: 参数callback:

      带消息指针的回调功能

    3. addCallback(self: depthai.DataOutputQueue, callback: std::function<void ()>) -> int

    在收到的消息上添加回调

    .. function:: 参数callback:

      没有任何参数的回调函数

  .. function:: get(self: depthai.DataOutputQueue) → depthai.ADatatype

    阻止直到有消息可用。

  .. function:: getAll(self: depthai.DataOutputQueue) → List[depthai.ADatatype]

    阻塞直到队列中至少有一条消息。然后返回队列中的所有消息。

  .. function:: getBlocking(self: depthai.DataOutputQueue) → bool


    满时获取当前队列行为（最大大小）,如果阻塞则为true，否则为false

  .. function:: getMaxSize(self: depthai.DataOutputQueue, arg0: int) → int

    获取队列的最大大小

  .. function:: getName(self: depthai.DataOutputQueue) → str

    获取队列名称

  .. function:: has(self: depthai.DataOutputQueue) → bool

    检查队列前面是否有消息（不为空）,如果队列不为空，则为true，否则为false

  .. function:: removeCallback(self: depthai.DataOutputQueue, callbackId: int) → bool

    删除回调,如果删除了回调，则为true，否则为false

  .. function:: setBlocking(self: depthai.DataOutputQueue, blocking: bool)

    设置满时的队列行为（maxSize），指定是阻止还是覆盖队列中最旧的消息

  .. function:: setMaxSize(self: depthai.DataOutputQueue, maxSize: int)

    设置队列的最大大小，指定队列中的最大消息数

  .. function:: tryGet(self: depthai.DataOutputQueue) → depthai.ADatatype

    尝试从队列中检索消息。如果没有可用的消息，则立即返回

  .. function:: tryGetAll(self: depthai.DataOutputQueue) → List[depthai.ADatatype]

    尝试检索队列中的所有消息。

.. class:: DetectionNetwork
  :canonical: depthai.DetectionNetwork

  检测网络。不同网络专业的基础

  **属性**

  .. list-table::

    * - input
      - 输入消息，其中包含要推论为默认队列的数据，大小为5
    * - out
      - 输出ImgDetections消息，其中包含已解析的检测结果。
    * - passthrough
      - 执行推断的直通消息。
  
  .. attribute:: input

    输入消息，其中包含要推论为默认队列的数据，大小为5

  .. attribute:: out

    输出ImgDetections消息，其中包含已解析的检测结果。

  .. attribute:: passthrough

    执行推断的直通消息。

    适用于将输入队列设置为非阻塞行为的情况。

  **方法**

  .. function:: setConfidenceThreshold(self: depthai.DetectionNetwork，thresh: float)

    指定用于过滤其余检测的置信度阈值。

    .. function:: 参数thresh:

      检测置信度必须大于指定的阈值才能添加到列表中

.. class:: ColorCameraProperties
  :canonical: depthai.ColorCameraProperties

  **Outputs**

  .. attribute:: boardSocket

  .. attribute:: colorOrder

  .. attribute:: interleaved

  .. attribute:: previewHeight

  .. attribute:: previewWidth

  .. attribute:: resolution

  .. attribute:: fps

  .. attribute:: videoHeight

  .. attribute:: videoWidth

  .. attribute:: stillHeight

  .. attribute:: stillWidth

  .. attribute:: sensorCropX

  .. attribute:: sensorCropY


.. class:: ColorCameraProperties.SensorResolution
  :canonical: depthai.ColorCameraProperties.SensorResolution

  **Outputs**

  .. attribute:: THE_1080_P

  .. attribute:: THE_4_K

  .. attribute:: THE_12_MP


.. class:: ColorCameraProperties.ColorOrder
  :canonical: depthai.ColorCameraProperties.ColorOrder

  **Outputs**

  .. attribute:: BGR

  .. attribute:: RGB


.. class:: MonoCameraProperties
  :canonical: depthai.MonoCameraProperties

  **Outputs**

  .. attribute:: boardSocket

  .. attribute:: resolution

  .. attribute:: fps


.. class:: MonoCameraProperties.SensorResolution
  :canonical: depthai.MonoCameraProperties.SensorResolution

  **Outputs**

  .. attribute:: THE_720_P

  .. attribute:: THE_800_P

  .. attribute:: THE_400_P


.. class:: StereoDepthProperties
  :canonical: depthai.StereoDepthProperties

  **Outputs**

  .. attribute:: calibration

  .. attribute:: median

  .. attribute:: confidenceThreshold

  .. attribute:: enableLeftRightCheck

  .. attribute:: enableSubpixel

  .. attribute:: enableExtendedDisparity

  .. attribute:: rectifyMirrorFrame

  .. attribute:: rectifyEdgeFillColor

  .. attribute:: enableOutputRectified

  .. attribute:: enableOutputDepth

  .. attribute:: width

  .. attribute:: height


.. class:: StereoDepthProperties.MedianFilter
  :canonical: depthai.StereoDepthProperties.MedianFilter

  **Outputs**

  .. attribute:: MEDIAN_OFF

  .. attribute:: KERNEL_3x3

  .. attribute:: KERNEL_5x5

  .. attribute:: KERNEL_7x7


.. class:: VideoEncoderProperties
  :canonical: depthai.VideoEncoderProperties

  **Outputs**

  .. attribute:: bitrate

  .. attribute:: keyframeFrequency

  .. attribute:: maxBitrate

  .. attribute:: numBFrames

  .. attribute:: numFramesPool

  .. attribute:: profile

  .. attribute:: quality

  .. attribute:: rateCtrlMode

  .. attribute:: width

  .. attribute:: height


.. class:: VideoEncoderProperties.Profile
  :canonical: depthai.VideoEncoderProperties.Profile

  **Outputs**

  .. attribute:: H264_BASELINE

  .. attribute:: H264_HIGH

  .. attribute:: H264_MAIN

  .. attribute:: H265_MAIN

  .. attribute:: MJPEG


.. class:: VideoEncoderProperties.RateControlMode
  :canonical: depthai.VideoEncoderProperties.RateControlMode

  **Outputs**

  .. attribute:: CBR

  .. attribute:: VBR

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:

   depthai.XLinkIn <?dummy=http://#XLinkIn>
   depthai.XLinkOut <?dummy=http://#XLinkOut>
   depthai.XLinkDeviceState <?dummy=http://#XLinkDeviceState>
   depthai.XLinkPlatform <?dummy=http://#XLinkPlatform>
   depthai.XLinkProtocol <?dummy=http://#XLinkProtocol>
   depthai.ColorCamera <?dummy=http://#ColorCamera>
   depthai.MonoCamera <?dummy=http://#MonoCamera>
   depthai.NNData <?dummy=http://#NNData>
   depthai.NeuralNetwork <?dummy=http://#NeuralNetwork>
   depthai.Node <?dummy=http://#Node>
   depthai.OpenVINO <?dummy=http://#OpenVINO>
   depthai.SPIOut <?dummy=http://#SPIOut>
   depthai.ImageManip <?dummy=http://#ImageManip>
   depthai.ImageManipConfig <?dummy=http://#ImageManipConfig>
   depthai.ImgDetections <?dummy=http://#ImgDetections>
   depthai.LogLevel <?dummy=http://#LogLevel>
   depthai.MobileNetDetectionNetwork <?dummy=http://#MobileNetDetectionNetwork>
   depthai.YoloDetectionNetwork <?dummy=http://#YoloDetectionNetwork>
   depthai.StereoDepth <?dummy=http://#StereoDepth>
   depthai.SystemInformation <?dummy=http://#SystemInformation>
   depthai.SystemLogger <?dummy=http://#SystemLogger>
    depthai.VideoEncoder <?dummy=http://#VideoEncoder>
    depthai.CameraBoardSocket <?dummy=http://#CameraBoardSocket>
    depthai.CameraImageOrientation <?dummy=http://#CameraImageOrientation>
    depthai.CameraControl <?dummy=http://#CameraControl>
    depthai.DataInputQueue <?dummy=http://#DataInputQueue>
    depthai.DataOutputQueue <?dummy=http://#DataOutputQueue>
    depthai.DetectionNetwork <?dummy=http://#DetectionNetwork>