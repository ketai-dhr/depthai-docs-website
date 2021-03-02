Gen1 Python API
============================

关于DepthAI第一代管道构建器的API详细介绍。

.. class:: Device
  :canonical: depthai.Device

  用与之交互的方法表示 DepthAI 设备。

  .. warning::

    请注意，除 :func:`get_available_streams()` 之外的所有方法都要首先运行 :func:`create_pipeline()` 。


  .. _example:

  **例如**

  .. code-block:: python

    import depthai
    device = depthai.Device('', False)
    pipeline = device.create_pipeline(config={
        'streams': ['previewout', 'metaout'],
        'ai': {
            "blob_file": "/path/to/model.blob",
            "blob_file_config": "/path/to/config.json",
        },
    })


  **方法**

  .. function:: __init__(device_id: str, usb2_mode: bool) -> Device

    标准和推荐的方式来设置对象。

    **device_id** 代表设备连接的 USB 端口 ID。 如果设置为特定值 (例如： :code:`"1"`) 它将在特定的 USB 端口中查找设备， 而如果留空 - :code:`''` - 将在所有端口上查找设备。
    当我们连接了不止一台 DepthAI 设备并希望在代码中指定要使用哪一台设备时，这个方法很有用。

    **usb2_mode**, 为 :code:`True/False`, 允许 DepthAI 使用 USB2 协议（而不是 USB3）进行通信。 这降低了管道的吞吐量，但允许使用> 1m USB 电缆进行连接。

  .. function:: __init__(cmd_file: str, device_id: str) -> Device
    :noindex:

    初始化 DepthAI 设备的开发和调试方法。

    **cmd_file** 是固件 :code:`.cmd` 文件的路径，该文件将加载到设备上以进行引导。

    **device_id** 代表设备连接的 USB 端口 ID。 如果设置为特定值 (例如: :code:`"1"`) 它将在特定的 USB 端口中查找设备，而如果留空 - :code:`''` - 将在所有端口上查找设备。 当我们连接了不止一台 DepthAI 设备并希望在代码中指定要使用哪一台设备时，这个方法很有用。

  .. function:: create_pipeline(config: dict) -> depthai.CNNPipeline

    初始化 DepthAI 管道，如果成功，则返回创建的 :code:`CNNPipeline` 否则返回 :code:`None` 。

    **config(dict)** -  管道的配置使用 :code:`dict` 设置。 配置的示例键/值:

    .. code-block:: python

      {
          # Possible streams:
          #   'color' - 4K 彩色摄像机预览
          #   'left' - 左单声道相机预览
          #   'right' - 右单声道相机预览
          #   'rectified_left' - 校正左镜头预览
          #   'rectified_right' - 校正右镜头预览
          #   'previewout' - 神经网络输入预览
          #   'metaout' - CNN 输出张量
          #   'depth' - 原始深度图，视差转换为现实距离
          #   'disparity' - 视差图，左右摄像机之间的视差，以像素为单位
          #   'disparity_color' - 视差图着色
          #   'meta_d2h' - device metadata stream
          #   'video' - H.264/H.265 编码的彩色摄像机帧
          #   'jpegout' - JPEG 编码的彩色相机帧
          #   'object_tracker' - 对象跟踪器结果
          'streams': [
              'left',  # 如果使用left，它必须在第一个位置
              'right',
              {'name': 'previewout', 'max_fps': 12.0},  # 流可以指定为对象
              'metaout',
              # 与深度有关的流
              {'name': 'depth', 'max_fps': 12.0},
              {'name': 'disparity', 'max_fps': 12.0},
              {'name': 'disparity_color', 'max_fps': 12.0},
          ],
          'depth':
          {
              'calibration_file': consts.resource_paths.calib_fpath,
              'left_mesh_file': consts.resource_paths.left_mesh_fpath,
              'right_mesh_file': consts.resource_paths.right_mesh_fpath,
              'padding_factor': 0.3,
              'depth_limit_m': 10.0, # 以米为单位，用于在 x，y，z calc 期间进行过滤
              'median_kernel_size': 7,  # 视差/深度中值滤波器内核大小（N x N）。 0 = 禁用过滤
              'lr_check': True  # 启用双目“左右检查”功能。
              'warp_rectify':
              {
                  'use_mesh' : True, # 如果为 False，将使用单应性。
                  'mirror_frame': True, # 如果为 False，则视差将被镜像
                  'edge_fill_color': 0, # 灰色 0..255 或 -1 以复制像素值
              },
          },
          'ai':
          {
              'blob_file': blob_file,
              'blob_file_config': blob_file_config,
              'blob_file2': blob_file2,
              'blob_file_config2': blob_file_config2,
              'calc_dist_to_bb': True, # 带边界框输出的 CNN 模型的深度计算
              'keep_aspect_ratio': False, # 保持宽高比，不要对 NN 使用完整的 RGB FOV
              'camera_input': "left", # 'rgb', 'left', 'right', 'left_right', 'rectified_left', 'rectified_right', 'rectified_left_right'
              'shaves' : 7,  # 1 - 14 NN 使用的 shaves 数量。
              'cmx_slices' : 7,  # 1 - 14 NN 使用的 cmx_slices 数。
              'NN_engines' : 2,  # 1 - 2 NN 使用的 NN_engine 的数量。
          },
          # 对象追踪器
          'ot':
          {
              'max_tracklets'        : 20, # 最多支持 20 个
              'confidence_threshold' : 0.5, # 仅针对超过此阈值的检测跟踪对象
          },
          'board_config':
          {
              'swap_left_and_right_cameras': True, # 交换左右摄像机。
              'left_fov_deg': 71.86, # 立体摄像机的水平视场（HFOV），以[deg]为单位。
              'rgb_fov_deg': 68.7938, # RGB 摄像机的水平视场（HFOV），以[deg]为单位
              'left_to_right_distance_cm': 9.0, # 左/右摄像机基线，以[cm]为单位
              'left_to_rgb_distance_cm': 2.0, # RGB 相机与左相机的距离。
              'store_to_eeprom': False, # 将校准和 board_config（fov，baselines，swap-lr）存储在板载 EEPROM 中
              'clear_eeprom': False, # 使 EEPROM 中的 calib 和 board_config 无效
              'override_eeprom': False, # 使用主机上的 calib 和 board_config，如果编程则忽略 EEPROM 数据.
          },
          'camera':
          {
              'rgb':
              {
                  # 3840x2160, 1920x1080
                  # 目前仅支持 UHD / 1080p / 30 fps
                  'resolution_h': 3040, # possible - 1080, 2160, 3040
                  'fps': 30,
              },
              'mono':
              {
                  # 1280x720, 1280x800, 640x400 (启用分箱)
                  'resolution_h': 800, # possible - 400, 720, 800
                  'fps': 30,
              },
          },
          'app':
          {
              'sync_video_meta_streams': False,  # 同步“ previewout”和“ metaout”流
              'sync_sequence_numbers'  : False,  # 同步所有数据包的序列号。 实验性
              'usb_chunk_KiB' : 64, # 设备上的 USB 传输块。 更高（高达兆字节）可以提高吞吐量，或者为 0 则禁用分块
          },
          #'video_config':
          #{
          #    'rateCtrlMode': 'cbr', # 选件: cbr / vbr
          #    'profile': 'h265_main', # 选件: 'h264_baseline' / 'h264_main' / 'h264_high' / 'h265_main / 'mjpeg' '
          #    'bitrate': 8000000, # 使用 CBR 时（仅限 H264 / H265）
          #    'maxBitrate': 8000000, # 使用 CBR 时（仅限 H264 / H265）
          #    'keyframeFrequency': 30, (H264/H265 only)
          #    'numBFrames': 0, (H264/H265 only)
          #    'quality': 80 # (0-100％)使用 VBR 或 MJPEG 配置文件时
          #}
          #'video_config':
          #{
          #    'profile': 'mjpeg',
          #    'quality': 95
          #}
      }


  .. function:: get_available_streams() -> List[str]

    返回 DepthAI 库支持的所有流的列表。

    .. code-block::

      >>> device.get_available_streams()
      ['meta_d2h', 'color', 'left', 'right', 'rectified_left', 'rectified_right', 'disparity', 'depth', 'metaout', 'previewout', 'jpegout', 'video', 'object_tracker']


  .. function:: get_nn_to_depth_bbox_mapping() -> dict

    返回允许将 CNN 输出与视差信息匹配的字典。

    由于 RGB 相机具有 4K 分辨率，并且神经网络仅接受具有特定分辨率的图像(例如 300x300)，则原始图像会被裁剪以满足神经网络的要求。另一方面，由神经网络返回的视差帧在单声道相机上具有完整分辨率。

     为了能够确定 CNN 预览图像在视差帧上的位置，应使用此方法指定要使用的偏移量和尺寸。

    .. code-block::

      >>> device.get_nn_to_depth_bbox_mapping()
      {'max_h': 681, 'max_w': 681, 'off_x': 299, 'off_y': 59}


  .. function:: request_af_mode()

      将 4K RGB 相机自动对焦模式设置为可用的 :class:`AutofocusMode` 之一。


  .. function:: request_af_trigger()

      在 4k RGB 相机上手动将触发操作发送到 AutoFocus


  .. function:: request_jpeg()

      从 RGB 相机捕获 JPEG 帧，并将其发送到： :code:`jpegout` 流。框架具有完整的可用分辨率，但未裁剪为符合 CNN 输入的尺寸。


  .. function:: send_disparity_confidence_threshold(confidence: int)

     发送用于 StereoSGBM 算法的视差置信度阈值的功能。如果视差值置信度低于阈值，则将该值标记为无效视差并当作背景。

  .. function:: get_right_homography()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回一个 3x3 同构矩阵，用于校正右侧双目相机图像。


  .. function:: get_left_homography()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回一个 3x3 同构矩阵，用于校正左侧双目相机图像。


  .. function:: get_left_intrinsic()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回左侧双目相机的 3x3 本体校准矩阵。


  .. function:: get_right_intrinsic()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回右侧双目相机的 3x3 本体校准矩阵。


  .. function:: get_rotation()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回一个 3x3 旋转矩阵，表示右侧双目相机与左侧双目相机的旋转。

  .. function:: get_translation()

    .. warning::

      注：需要 :ref:`双重均质成像校准 <双匀质成像与单匀质成像校准>`.

     返回一个 3x1 向量，表示右侧双目相机中心与左侧双目相机中心的位置。


.. class:: AutofocusMode
  :canonical: depthai.AutofocusMode


  一个包含所有可用自动对焦模式的枚举

  **成员**

  .. attribute:: AF_MODE_AUTO

    该模式将自动对焦设置为手动模式，您需要调用 :func:`Device.request_af_trigger`
    来启动对焦程序。

  .. attribute:: AF_MODE_CONTINUOUS_PICTURE

    此模式连续调整焦点以提供最佳的对焦图像流，并且在拍摄时相机静止不动时应使用此模式。
    对焦过程会以最快的速度完成。

    这是 DepthAI 运行的默认模式。

  .. attribute:: AF_MODE_CONTINUOUS_VIDEO

    此模式不断调整焦距以提供最佳对焦图像流，应在相机尝试捕捉流畅的视频流时使用。
    对焦程序较慢，可避免对焦过冲。

  .. attribute:: AF_MODE_EDOF

    该模式将禁用自动对焦。
    EDOF 代表增强景深，是一种数字对焦。

  .. attribute:: AF_MODE_MACRO

    这和 :attr:`AF_MODE_AUTO` 的工作模式是一样的。


.. class:: CNNPipeline
  :canonical: depthai.CNNPipeline

  管道对象，设备可以用它向主机发送结果。

  **方法**

  .. function:: get_available_data_packets() -> List[depthai.DataPacket]

    只返回设备本身产生的数据包，不含 CNN 结果


  .. function:: get_available_nnet_and_data_packets() -> tuple[List[depthai.NNetPacket], List[depthai.DataPacket]]

    同时返回神经网络的结果和设备产生的数据


.. class:: NNetPacket
  :canonical: depthai.NNetPacket

  对于任何神经网络推理输出，都可以使用 :func:`NNPacket.get_tensor` 。
  对于 :code:`Mobilenet-SSD` 的具体情况，可以在固件中进行 :code:`YOLO-v3` 解码。
  解码后的对象除了原始输出外，还可以通过 :func:`getDetectedObjects` 访问解码后的对象，以便轻松访问此常用网络的结果。
  有关不同神经网络输出格式以及如何在这些格式之间进行选择的更多详细信息，请参见 :ref:`blob 配置文件 <创建 Blob 配置文件>` 。

  神经网络结果包。这不是单个结果，而是附加了其他元数据的一批结果

  **方法**

  .. function:: getMetadata() -> depthai.FrameMetadata

    返回包含与此数据包相关的所有专有数据的元数据对象


  .. function:: get_tensor(name: Union[int, str]) -> numpy.ndarray

    .. warning::

      只有在 :ref:`blob 配置文件 <创建 Blob 配置文件>` 中 :code:`output_format` 被设置为 :code:`raw` 时才有效。

    根据神经网络的输出层信息，返回特定网络输出张量的形状 numpy 数组。

    例如：在使用 :code:`Mobilenet-SSD` 的情况下，它返回一个 :code:`[1, 1, 100, 7]` 形状的数组，其中 :code:`numpy.dtype` 是 :code:`float16`。

    用法示例：

    .. code-block::

      nnetpacket.get_tensor(0)
      # or
      nnetpacket.get_tensor('detection_out')

  .. function:: __getitem__(name: Union[int, str]) -> numpy.ndarray

    和 :func:`get_tensor` 相同

    :code:`Mobilenet-SSD` 的使用实例：

    .. code-block::

      nnetpacket[0]
      # or
      nnetpacket['detection_out']

  .. function:: getOutputsList() -> list

    以列表形式返回网络中的所有输出张量。

  .. function:: getOutputsDict() -> dict

    以字典形式返回网络中的所有输出张量。
    :code:`key` 是输出层的名称，值是形状为 Numpy 的数组。

  .. function:: getOutputLayersInfo() -> depthai.TensorInfo

    返回网络的输出层信息。

  .. function:: getInputLayersInfo() -> depthai.TensorInfo

    返回网络的输入层信息。

  .. function:: getDetectedObjects() -> depthai.Detections

    .. warning::

      当 :ref:`blob 配置文件 <创建 Blob 配置文件>` 中的 :code:`output_format` 设置为 :code:`detection` 并使用检测网络
      (以 :code:`Mobilenet-SSD`, :code:`(tiny-)YOLO-v3` 为基础的网络)时，可以使用。

    以 :class:`Detections` 格式返回检测到的对象。在设备端对网络进行解码。


.. class:: TensorInfo
  :canonical: depthai.TensorInfo

  网络的输入/输出层、张量的描述符。

  当网络加载时，会自动打印张量信息。

  **属性**

  .. attribute:: name
    :type: str

    张量的名称。

  .. attribute:: dimensions
    :type: list

    张量阵列的形状。例如： :code:`[1, 1, 100, 7]`

  .. attribute:: strides
    :type: list

    张量阵列的步幅。

  .. attribute:: data_type
    :type: string

    张量的数据类型。例如： :code:`float16`

  .. attribute:: offset
    :type: int

    原始输出数组中的偏移量。

  .. attribute:: element_size
    :type: int

    数组中一个元素的大小（以字节为单位）。

  .. attribute:: index
    :type: int

    张量的索引。例如：在网络中有多个输入/输出的情况下，它标志着输入/输出的顺序。

  **方法**

  .. function:: get_dict() -> dict

    以字典形式返回张量信息，:code:`key` 是属性的名称。

  .. function:: get_dimension() -> int

    返回张量的具体维度

    .. code-block::

      tensor_info.get_dimension(depthai.TensorInfo.Dimension.WIDTH)  # returns width of tensor


.. class:: Detections
  :canonical: depthai.Detections

  在设备端解码的神经网络结果的容器。

  **访问 detections 的示例**

  假设检测到的对象存储在 :code:`detections` 对象中。

  * detection 的数量

    .. code-block::

      detections.size()
      # or
      len(detections)

  * 访问第 n 个 detection

    .. code-block::

      detections[0]
      detections[1]  # ...

  * 遍历所有 detection

    .. code-block::

      for detection in detections:


.. class:: Detection
  :canonical: depthai.Detection

  检测到的对象描述符。

  **属性**

  .. attribute:: label
    :type: int

    检测到的对象的标签ID。

  .. attribute:: confidence
    :type: float

    检测到的对象的置信度得分，区间为 [0, 1].

  .. attribute:: x_min
    :type: float

    检测到的边界框的左上 :code:`X` 坐标。
    归一化，区间为 [0, 1]。

  .. attribute:: y_min
    :type: float

    检测到的边界框的左上 :code:`Y` 坐标。
    归一化，区间为 [0, 1]。

  .. attribute:: x_max
    :type: float

    检测到的边界框的右下角 :code:`X` 坐标。
    归一化，区间为 [0, 1]。

  .. attribute:: y_max
    :type: float

    检测到的边界框的右下角 :code:`Y` 坐标。
    归一化，区间为 [0, 1]。

  .. attribute:: depth_x
    :type: float

    到 :code:`X` 轴上检测到的边界框的距离。仅当深度计算被启用时（板上有双目相机）。

  .. attribute:: depth_y
    :type: float

    到 :code:`Y` 轴上检测到的边界框的距离。仅当深度计算被启用时（板上有双目相机）。

  .. attribute:: depth_z
    :type: float

    到 :code:`Z` 轴上检测到的边界框的距离。仅当深度计算被启用时（板上有双目相机）。

  **方法**

  .. function:: get_dict() -> dict

    以字典形式返回检测对象, :code:`key` 是属性的名称。


.. class:: Dimension
  :canonical: depthai.TensorInfo.Dimension

  张量形状的维度描述符。
  由于并非所有神经网络模型都尊重输出张量的 :code:`维度` 语义，因此对输入张量的意义最大。


  **Values**

  .. attribute:: W / WIDTH
    :type: str

    宽

  .. attribute:: H / HEIGHT
    :type: str

    高

  .. attribute:: C / CHANNEL
    :type: str

    通道数

  .. attribute:: N / NUMBER
    :type: str

    推断数

  .. attribute:: B / BATCH
    :type: str

    推断批次


.. class:: DataPacket
  :canonical: depthai.DataPacket

  DepthAI 数据包，包含设备上生成的信息。
  与 NNetPacket 不同的是，它包含一个单一的 "结果 "和源流信息。

  **属性**

  .. attribute:: stream_name
    :type: str

    返回数据包源流。
    用于确定信息包的来源，因此允许正确处理信息包，并根据此值应用适当的处理

  **Methods**

  .. function:: getData() -> numpy.ndarray

    返回数据为 NumPy 数组，你可以使用 OpenCV :code:`imshow` 对其进行进一步转换或显示。

    用于返回帧的流，例如 :code:`previewout`, :code:`left`, :code:`right`, 或编码数据，例如 :code:`video`, :code:`jpegout`.

  .. function:: getDataAsStr() -> str

    将数据以字符串的形式返回，可以进一步解析。

    用于返回非数组结果的数据流，例如 :code:`meta_d2h` 用于返回非数组结果的数据流，例如 JSON 对象。

  .. function:: getMetadata() -> FrameMetadata

    返回包含与此数据包相关的所有专有数据的元数据对象

  .. function:: getObjectTracker() -> ObjectTracker

    .. warning::

      仅对来自 :code:`object_tracker` 流的数据包有效。

    返回包含 :class:`ObjectTracker` 对象的元数据对象。

  .. function:: size() -> int

    返回数据包的大小


.. class:: FrameMetadata
  :canonical: depthai.FrameMetadata

  附加在通过管道发送的数据包上的元数据对象

  **Methods**

  .. function:: getCameraName() -> str

    返回产生该帧的相机的名称

  .. function:: getCategory() -> int

    返回数据包的类型，无论是常规帧还是静止拍摄

  .. function:: getFrameBytesPP() -> int

    返回数据包帧中每个像素的字节数

  .. function:: getFrameHeight() -> int

    返回数据包帧的高度

  .. function:: getFrameWidth() -> int

    返回数据包帧的宽度

  .. function:: getFrameType() -> int

    返回该数据包包含的数据类型

  .. function:: getInstanceNum() -> int

    返回当前数据包来源的相机ID

  .. function:: getSequenceNum() -> int

    为相机生成的每个帧分配序列号。
    它可以用来确保帧是在同一时间拍摄的 - 例如，如果来自左侧和右侧摄像机的帧具有相同的序列号，则可以假定它们是同时拍摄的

  .. function:: getStride() -> int

    指定数据包帧中下一行像素的字节数

  .. function:: getTimestamp() -> float

    创建数据包时，会为其分配一个创建时间戳，可以使用此方法获得该时间戳


.. class:: ObjectTracker
  :canonical: depthai.ObjectTracker

  表示跟踪器当前状态的对象，通过对来自 :code:`object_tracker` 流的数据包调用 :func:`DataPacket.getObjectTracker` 方法获得


  **方法**

  .. function:: getNrTracklets() -> int

    返回可用 tracklet 的数量

  .. function:: getTracklet(tracklet_nr: int) -> Tracklet

    返回具有指定 :code:`tracklet_nr` 的 tracklet。
    要检查有多少个 tracklet，请使用 :func:`getNrTracklets` 方法


.. class:: Tracklet
  :canonical: depthai.Tracklet

  Tracklet 表示单个被跟踪的对象，由 :class:`ObjectTracker` 类生成。
  要获取它，请调用 :func:`ObjectTracker.getTracklet` 方法。

  **方法**

  .. function:: getId() -> int

    返回 tracklet id

  .. function:: getLabel() -> int

    返回 tracklet 标签，是神经网络返回的结果。用于标识一类识别对象

  .. function:: getStatus() -> str

    返回 tracklet 状态 - :code:`NEW`, :code:`TRACKED` 或 :code:`LOST`

  .. function:: getLeftCoord() -> int

    返回被追踪对象的边界框的左坐标

  .. function:: getRightCoord() -> int

    返回被追踪对象的边界框的右坐标

  .. function:: getTopCoord() -> int

    返回被跟踪对象的边界框的顶部坐标

  .. function:: getBottomCoord() -> int

    返回被追踪对象的边框的底部坐标

.. include::  /pages/includes/footer-short.rst

..
  Below you can see ?dummy=http://, this is a workaround for a Sphinx, see here - https://github.com/sphinx-doc/sphinx/issues/701#issuecomment-697116337

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:

   depthai.Device <?dummy=http://#Device>
   depthai.AutofocusMode <?dummy=http://#AutofocusMode>
   depthai.CNNPipeline <?dummy=http://#CNNPipeline>
   depthai.NNetPacket <?dummy=http://#NNetPacket>
   depthai.TensorInfo <?dummy=http://#TensorInfo>
   depthai.Detections <?dummy=http://#Detections>
   depthai.Dimension <?dummy=http://#Dimension>
   depthai.DataPacket <?dummy=http://#DataPacket>
   depthai.FrameMetadata <?dummy=http://#FrameMetadata>
   depthai.ObjectTracker <?dummy=http://#ObjectTracker>
   depthai.Tracklet <?dummy=http://#Tracklet>