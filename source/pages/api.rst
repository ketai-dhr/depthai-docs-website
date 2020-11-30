Python API
==========

关于安装、升级和使用DepthAI Python API的说明。

支持的平台
###################

DepthAI API python模块是为Ubuntu, MaxOS 和 Windows 预制的。
对于其他操作系统或Python版本，可以 :ref:`从源码编译 <其他安装方式>` DepthAI。.

安装系统依赖
##############################

运行DepthAI库需要几个基本的系统依赖。
它们中的大多数应该已经安装在大多数系统中，但是如果没有安装，
我们准备了一个 :download:`安装脚本 </_static/install_dependencies.sh>`
，以确保所有依赖项都已安装

.. code-block:: bash

  curl -fL http://docs.luxonis.com/_static/install_dependencies.sh | bash

如果使用Windows系统，请使用此 :download:`批处理脚本 </_static/install_dependencies.bat>` 进行依赖项安装

启用USB设备（仅在Linux上）
#######################################

由于DepthAI是USB设备，因此为了在使用 :code:`udev` 工具的系统上与之通信， 
您需要添加udev规则以使设备可访问。

以下命令将向您的系统添加新的udev规则

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

从PyPi安装
#################

我们的软件包是 `通过PyPi <https://pypi.org/project/depthai/>`__ 分发的，为了可以在您的环境中安装，请使用

.. code-block:: bash

  python3 -m pip install depthai

有关其他安装选项，请参阅 :ref:`其他安装选项 <其他安装方式>` 。

测试安装
#################

我们在GitHub上有 `depthai <https://github.com/luxonis/depthai>`__ 存储库，
其中包含许多有用的示例和准备好的神经网络，
您可以使用它们来加快原型制作速度。它还包括由我们的贡献者维护的测试脚本，
该脚本应有助于您验证设置是否正确。

首先，克隆 `depthai <https://github.com/luxonis/depthai>`__ 存储库并安装其依赖项

.. code-block:: bash

  git clone https://github.com/luxonis/depthai.git
  cd depthai
  python3 -m pip install -r requirements.txt

现在，从 depthai 内部运行演示脚本，以确保一切正常：

.. code-block:: bash

  python3 depthai_demo.py

如果一切顺利的话，会弹出一个小视频窗口。
如果画面中的物体属于 `物体检测示例20类 <https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22>`__ ）中的某一类，画面上会叠加该物体的信息。


准备 MyriadX blob 文件和它的配置文件
###########################################

正如你在 :ref:`本例 <Example>` 中所看到的，:func:`Device.create_pipeline` 方法的基本用法
包括指定所需的输出流和AI部分，在其中指定 MyriadX blob 及其配置。

在本节中，我们将介绍如何同时获取 :code:`blob_file` 和 :code:`blob_file_config` 。

获取 MyriadX Blob 
**********************

由于我们使用的是 MyriadX VPU，
您的模型需要被编译（或准确地进行优化和转换）需要被 MyriadX Blob 文件，然后将其发送到设备并执行。

最简单的方法是使用我们的在线 `BlobConverter应用程序 <http://69.164.214.171:8083/>`__ 来获取这个blob文件。
它有编译所需的所有工具，
所以你不需要设置任何东西–你甚至可以从 `OpenVINO模型Zoo <https://github.com/openvinotoolkit/open_model_zoo>`__ 下载一个模型的blob。

如果你愿意，你也可以自己编译blob。
你需要安装 `OpenVINO工具包 <https://docs.openvinotoolkit.org/latest/index.html>`__，
然后使用 `模型优化器和  <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html>`__ 
和 `Myriad编译器 <https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#myriad_platform_option>`__ 来获得 MyriadX blob。 
我们已经在 `这里 <https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__ 记录了这些编译器的使用实例。

创建Blob配置文件
********************************

如果未提供配置文件或将 :code:`output_format` 设置为 :code:`raw`，
则设备上不会进行解码，用户必须在主机端手动进行解码。

当前支持在设备上对基于 :code:`Mobilenet-SSD` 和 :code:`(tiny-)YOLO-v3` 的网络进行解码。
对于该配置文件，需要使用网络特定的参数。

`tiny-yolo-v3` 网络示例：

.. code-block:: json

  {
      "NN_config":
      {
          "output_format" : "detection",
          "NN_family" : "YOLO",
          "NN_specific_metadata" :
          {
              "classes" : 80,
              "coordinates" : 4,
              "anchors" : [10,14, 23,27, 37,58, 81,82, 135,169, 344,319],
              "anchor_masks" :
              {
                  "side26" : [1,2,3],
                  "side13" : [3,4,5]
              },
              "iou_threshold" : 0.5,
              "confidence_threshold" : 0.5
          }
      },
      "mappings":
      {
          "labels":
          [
              "person",
              "bicycle",
              "car",
              "..."
          ]
      }
  }


* :code:`NN_config` - 网络配置
    * :code:`output_format`
        * :code:`"detection"` - 设备上完成解码， 接收到的数据包为 :class:`Detections` 格式
        * :code:`"raw"` - 在主机上完成解码
    * :code:`NN_family` - `"YOLO"` 或 `"mobilenet"`
    * :code:`NN_specific_metadata` - 仅用于 `"YOLO"`
        * :code:`classes` - classes 数量
        * :code:`coordinates` - coordinates 数量
        * :code:`anchors` - YOLO 网络的锚点
        * :code:`anchor_masks` - 每个输出层的锚定遮罩 : :code:`26x26`, :code:`13x13` (+ `52x52` for full YOLO-v3)
        * :code:`iou_threshold` - 检测到的对象的联合阈值交集
        * :code:`confidence_threshold` - 检测物体的得分置信度阈值
* :code:`mappings.labels` - 使用 :code:`depthai_demo.py` 脚本来解码ID的标签

将 :code:`output_format` 设置为 :code:`detection` 时的解码示例：

.. code-block:: python

  nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

  for nnet_packet in nnet_packets:
    in_layers = nnet_packet.getInputLayersInfo()

    input_width  = in_layers[0].get_dimension(depthai.TensorInfo.Dimension.W)
    input_height = in_layers[0].get_dimension(depthai.TensorInfo.Dimension.H)

    detections = nnet_packet.getDetectedObjects()
    objects = list()

    for detection in detections:
        detection_dict = detection.get_dict()
        # scale normalized coordinates to image coordinates
        detection_dict["x_min"] = int(detection_dict["x_min"] * input_width)
        detection_dict["y_min"] = int(detection_dict["y_min"] * input_height)
        detection_dict["x_max"] = int(detection_dict["x_max"] * input_width)
        detection_dict["y_max"] = int(detection_dict["y_max"] * input_height)
        objects.append(detection_dict)

  print(objects)

主机和设备上基于完整 :code:`yolo-v3` 和 :code:`tiny-yolo-v3` 的解码示例在 `此处 <https://github.com/luxonis/depthai/blob/develop/depthai_helpers/tiny_yolo_v3_handler.py>`__


主机和设备上基于 :code:`mobilenet` 的网络的解码示例在 `此处 <https://github.com/luxonis/depthai/blob/develop/depthai_helpers/mobilenet_ssd_handler.py>`__


其他安装方式
##########################

要从我们的源代码中获取最新但尚未发布的功能，您可以继续手动编译depthai软件包。

从源构建的依赖项
*********************************

- CMake > 3.2.0
- 生成工具 (Ninja, make, ...)
- C/C++ 编译器
- libusb1 开发包

.. _raspbian:

Ubuntu, Raspberry Pi OS, ... (基于Debian的系统)
---------------------------------------------------

在基于Debian的系统 (Raspberyy Pi OS, Ubuntu, ...)上，可以通过运行以下命令获取安装依赖：

.. code-block:: bash

  sudo apt-get -y install cmake libusb-1.0-0-dev build-essential

macOS (Mac OS X)
----------------

假设安装了 Mac OS X , 则 `depthai-python <https://github.com/luxonis/depthai-python>`__ 库需要以下依赖项

- HomeBrew (如果尚未安装)

  .. code-block:: bash

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

- Python, libusb, CMake, wget

  .. code-block:: bash

      brew install coreutils python3 cmake libusb wget

现在你已经准备好克隆 `depthai-python <https://github.com/luxonis/depthai-python>`__ ，并在 Mac OSX 上构建。

使用 GitHub commit 进行安装
***************************

Pip允许用户从特定的 commit 安装软件包，即使它们尚未在PyPi上发布。


为此，请使用以下命令 - 并确保使用正确的 `commit hash <https://github.com/luxonis/depthai-python/commits>`__ 替换 :code:`<commit_sha>` 

.. code-block:: bash

    python3 -m pip install git+https://github.com/luxonis/depthai-python.git@<commit_sha>

使用/测试特定的 分支/PR
**********************************

有时，使用特定分支可能会引起您的注意。 
例如，这可能是因为我们已经听取了您的功能要求并在分支中实现。
或者可能是出于稳定性目的，在合并到 :code:`main` 中之前，在 :code:`develop` 中实现。

因此，当在 `depthai <https://github.com/luxonis/depthai>`__ 存储库中工作时, 可以通过以下命令来使用分支。
在此示例中， 我们将尝试使用 :code:`develop` 分支
(这是在将新功能合并到 :code:`main` 之前我们用来吸收新功能的分支)：

在运行以下命令之前， 您可以独立克隆存储库
(以免覆盖任何本地更改) 也可以先执行 :code:`git pull` 。

.. code-block:: bash

  git checkout develop
  python3 -m pip install -U pip
  python3 -m pip install -r requirements.txt

从源安装
*******************

如果需要，您还可以从源代码本身安装该软件包 
- 它将允许您对 API 进行更改，并看到它们的实际作用。

为此，请先下载存储库，然后在开发模式下将该包添加到您的 python 解释器中

.. code-block:: bash

  git clone https://github.com/luxonis/depthai-python.git
  cd depthai-python
  git submodule update --init --recursive
  python3 setup.py develop  # you may need to add sudo if using system interpreter instead of virtual environment

如果您要使用默认(:code:`main`)以外的其他分支(e.g. :code:`develop`)， 可以通过键入

.. code-block:: bash

  git checkout develop  # replace the "develop" with a desired branch name
  git submodule update --recursive
  python3 setup.py develop

或者，如果您要使用特定的 commit，请键入

.. code-block:: bash

  git checkout <commit_sha>
  git submodule update --recursive
  python3 setup.py develop


API Reference
#############

.. class:: Device
  :canonical: depthai.Device

  Represents the DepthAI device with the methods to interact with it.

  .. warning::

    Please be aware that all methods except :func:`get_available_streams` require :func:`create_pipeline` to be run first,


  .. _example:

  **Example**

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


  **Methods**

  .. function:: __init__(device_id: str, usb2_mode: bool) -> Device

    Standard and recomended way to set up the object.

    **device_id** represents the USB port id that the device is connected to. If set to specific value (e.x. :code:`"1"`) it will
    look for the device in specific USB port, whereas if left empty - :code:`''` - it will look for the device on all ports.
    It's useful when we have more than one DepthAI devices connected and want to specify which one to use in the code

    **usb2_mode**, being :code:`True/False`, allows the DepthAI to communicate using USB2 protocol, not USB3. This lowers the
    throughput of the pipeline, but allows to use >1m USB cables for connection

  .. function:: __init__(cmd_file: str, device_id: str) -> Device
    :noindex:

    Development and debug way to initialize the DepthAI device.

    **cmd_file** is a path to firmware :code:`.cmd` file that will be loaded onto the device for boot.

    **device_id** represents the USB port id that the device is connected to. If set to specific value (e.x. :code:`"1"`) it will
    look for the device in specific USB port, whereas if left empty - :code:`''` - it will look for the device on all ports.
    It's useful when we have more than one DepthAI devices connected and want to specify which one to use in the code

  .. function:: create_pipeline(config: dict) -> depthai.CNNPipeline

    Initializes a DepthAI Pipeline, returning the created :code:`CNNPipeline` if successful and :code:`None` otherwise.

    **config(dict)** -  A :code:`dict` of pipeline configuration settings. Example key/values for the config:

    .. code-block:: python

      {
          # Possible streams:
          #   'color' - 4K color camera preview
          #   'left' - left mono camera preview
          #   'right' - right mono camera preview
          #   'rectified_left' - rectified left camera preview
          #   'rectified_right' - rectified right camera preview
          #   'previewout' - neural network input preview
          #   'metaout' - CNN output tensors
          #   'depth' - the raw depth map, disparity converted to real life distance
          #   'disparity' - disparity map, the diaparity between left and right cameras, in pixels
          #   'disparity_color' - disparity map colorized
          #   'meta_d2h' - device metadata stream
          #   'video' - H.264/H.265 encoded color camera frames
          #   'jpegout' - JPEG encoded color camera frames
          #   'object_tracker' - Object tracker results
          'streams': [
              'left',  # if left is used, it must be in the first position
              'right',
              {'name': 'previewout', 'max_fps': 12.0},  # streams can be specified as objects with additional params
              'metaout',
              # depth-related streams
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
              'depth_limit_m': 10.0, # In meters, for filtering purpose during x,y,z calc
              'median_kernel_size': 7,  # Disparity / depth median filter kernel size (N x N) . 0 = filtering disabled
              'lr_check': True  # Enable stereo 'Left-Right check' feature.
              'warp_rectify':
              {
                  'use_mesh' : True, # if False, will use homography
                  'mirror_frame': True, # if False, the disparity will be mirrored instead
                  'edge_fill_color': 0, # gray 0..255, or -1 to replicate pixel values
              },
          },
          'ai':
          {
              'blob_file': blob_file,
              'blob_file_config': blob_file_config,
              'blob_file2': blob_file2,
              'blob_file_config2': blob_file_config2,
              'calc_dist_to_bb': True, # depth calculation on CNN models with bounding box output
              'keep_aspect_ratio': False, # Keep aspect ratio, don't use full RGB FOV for NN
              'camera_input': "left", # 'rgb', 'left', 'right', 'left_right', 'rectified_left', 'rectified_right', 'rectified_left_right'
              'shaves' : 7,  # 1 - 14 Number of shaves used by NN.
              'cmx_slices' : 7,  # 1 - 14 Number of cmx slices used by NN.
              'NN_engines' : 2,  # 1 - 2 Number of NN_engines used by NN.
          },
          # object tracker
          'ot':
          {
              'max_tracklets'        : 20, #maximum 20 is supported
              'confidence_threshold' : 0.5, #object is tracked only for detections over this threshold
          },
          'board_config':
          {
              'swap_left_and_right_cameras': True, # Swap the Left and Right cameras.
              'left_fov_deg': 71.86, # Horizontal field of view (HFOV) for the stereo cameras in [deg].
              'rgb_fov_deg': 68.7938, # Horizontal field of view (HFOV) for the RGB camera in [deg]
              'left_to_right_distance_cm': 9.0, # Left/Right camera baseline in [cm]
              'left_to_rgb_distance_cm': 2.0, # Distance the RGB camera is from the Left camera.
              'store_to_eeprom': False, # Store the calibration and board_config (fov, baselines, swap-lr) in the EEPROM onboard
              'clear_eeprom': False, # Invalidate the calib and board_config from EEPROM
              'override_eeprom': False, # Use the calib and board_config from host, ignoring the EEPROM data if programmed
          },
          'camera':
          {
              'rgb':
              {
                  # 3840x2160, 1920x1080
                  # only UHD/1080p/30 fps supported for now
                  'resolution_h': 3040, # possible - 1080, 2160, 3040
                  'fps': 30,
              },
              'mono':
              {
                  # 1280x720, 1280x800, 640x400 (binning enabled)
                  'resolution_h': 800, # possible - 400, 720, 800
                  'fps': 30,
              },
          },
          'app':
          {
              'sync_video_meta_streams': False,  # Synchronize 'previewout' and 'metaout' streams
              'sync_sequence_numbers'  : False,  # Synchronize sequence numbers for all packets. Experimental
              'usb_chunk_KiB' : 64, # USB transfer chunk on device. Higher (up to megabytes) may improve throughput, or 0 to disable chunking
          },
          #'video_config':
          #{
          #    'rateCtrlMode': 'cbr', # Options: cbr / vbr
          #    'profile': 'h265_main', # Options: 'h264_baseline' / 'h264_main' / 'h264_high' / 'h265_main / 'mjpeg' '
          #    'bitrate': 8000000, # When using CBR (H264/H265 only)
          #    'maxBitrate': 8000000, # When using CBR (H264/H265 only)
          #    'keyframeFrequency': 30, (H264/H265 only)
          #    'numBFrames': 0, (H264/H265 only)
          #    'quality': 80 # (0 - 100%) When using VBR or MJPEG profile
          #}
          #'video_config':
          #{
          #    'profile': 'mjpeg',
          #    'quality': 95
          #}
      }


  .. function:: get_available_streams() -> List[str]

    Return a list of all streams supported by the DepthAI library.

    .. code-block::

      >>> device.get_available_streams()
      ['meta_d2h', 'color', 'left', 'right', 'rectified_left', 'rectified_right', 'disparity', 'depth', 'metaout', 'previewout', 'jpegout', 'video', 'object_tracker']


  .. function:: get_nn_to_depth_bbox_mapping() -> dict

    Returns dict that allows to match the CNN output with the disparity info.

    Since the RGB camera has a 4K resolution and the neural networks accept only images with specific resolution
    (like 300x300), the original image is cropped to meet the neural network requirements.
    On the other side, the disparity frames returned by the neural network are in full resolution available on the mono cameras.

    To be able to determine where the CNN previewout image is on the disparity frame, this method should be used as it
    specifies the offsets and dimensions to use.

    .. code-block::

      >>> device.get_nn_to_depth_bbox_mapping()
      {'max_h': 681, 'max_w': 681, 'off_x': 299, 'off_y': 59}


  .. function:: request_af_mode()

      Set the 4K RGB camera autofocus mode to one of the available :class:`AutofocusMode`


  .. function:: request_af_trigger()

      Manually send trigger action to AutoFocus on 4k RGB camera


  .. function:: request_jpeg()

      Capture a JPEG frame from the RGB camera and send it to :code:`jpegout` stream.
      The frame is in full available resolution, not cropped to meet the CNN input dimensions.


  .. function:: send_disparity_confidence_threshold(confidence: int)

     Function to send disparity confidence threshold for StereoSGBM algorithm.
     If the disparity value confidence is below the threshold, the value is marked as invalid disparity
     and treated as background


  .. function:: send_disparity_confidence_threshold(confidence: int)

     Function to send disparity confidence threshold for StereoSGBM algorithm.
     If the disparity value confidence is below the threshold, the value is marked as invalid disparity
     and treated as background


  .. function:: get_right_homography()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x3 homography matrix used to rectify the right stereo camera image.


  .. function:: get_left_homography()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x3 homography matrix used to rectify the left stereo camera image.


  .. function:: get_left_intrinsic()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x3 intrinisc calibration matrix of the left stereo camera.


  .. function:: get_right_intrinsic()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x3 intrinisc calibration matrix of the right stereo camera.


  .. function:: get_rotation()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x3 rotation matrix representing the rotation of the right stereo camera w.r.t left stereo camera.


  .. function:: get_translation()

    .. warning::

      Note: Requires :ref:`dual-homography calibration <Dual-Homography vs. Single-Homography Calibration>`.

     Return a 3x1 vector repesenting the position of the right stereo camera center w.r.t left stereo camera center.


.. class:: AutofocusMode
  :canonical: depthai.AutofocusMode


  An enum with all autofocus modes available

  **Members**

  .. attribute:: AF_MODE_AUTO

    This mode sets the Autofocus to a manual mode, where you need to call :func:`Device.request_af_trigger`
    to start focusing procedure.

  .. attribute:: AF_MODE_CONTINUOUS_PICTURE

    This mode adjusts the focus continually to provide the best in-focus image stream and should be used when the
    camera is standing still while capturing. Focusing procedure is done as fast as possible.

    This is the defaut mode the DepthAI operates in.

  .. attribute:: AF_MODE_CONTINUOUS_VIDEO

    This mode adjusts the focus continually to provide the best in-focus image stream and should be used when the
    camera is trying to capture a smooth video steam. Focusing procedure is slower and avoids focus overshoots

  .. attribute:: AF_MODE_EDOF

    This mode disables the autofocus. EDOF stands for Enhanced Depth of Field and is a digital focus.

  .. attribute:: AF_MODE_MACRO

    It's the same operating mode as :attr:`AF_MODE_AUTO`


.. class:: CNNPipeline
  :canonical: depthai.CNNPipeline

  Pipeline object using which the device is able to send it's result to the host.

  **Methods**

  .. function:: get_available_data_packets() -> List[depthai.DataPacket]

    Returns only data packets produced by the device itself, without CNN results


  .. function:: get_available_nnet_and_data_packets() -> tuple[List[depthai.NNetPacket], List[depthai.DataPacket]]

    Return both neural network results and data produced by device


.. class:: NNetPacket
  :canonical: depthai.NNetPacket

  For any neural network inference output :func:`NNPacket.get_tensor` can be used. For the specific case
  of :code:`Mobilenet-SSD`, :code:`YOLO-v3` decoding can be done in the firmware. Decoded objects can be accessed
  through :func:`getDetectedObjects` as well in addition to raw output to make the results of this commonly used
  networks easily accessible. See :ref:`blob config file <Creating Blob configuration file>` for more details about
  different neural network output formats and how to choose between these formats.

  Neural network results packet. It's not a single result, but a batch of results with additional metadata attached

  **Methods**

  .. function:: getMetadata() -> depthai.FrameMetadata

    Returns metadata object containing all proprietary data related to this packet


  .. function:: get_tensor(name: Union[int, str]) -> numpy.ndarray

    .. warning::

      Works only, when in :ref:`blob config file <Creating Blob configuration file>` :code:`output_format` is set to :code:`raw`.

    Returns a shaped numpy array for the specific network output tensor, based on the neural network's output layer information.

    For example: in case of :code:`Mobilenet-SSD` it returns a :code:`[1, 1, 100, 7]` shaped array, where :code:`numpy.dtype` is :code:`float16`.

    Example of usage:

    .. code-block::

      nnetpacket.get_tensor(0)
      # or
      nnetpacket.get_tensor('detection_out')

  .. function:: __getitem__(name: Union[int, str]) -> numpy.ndarray

    Same as :func:`get_tensor`

    Example of usage for :code:`Mobilenet-SSD`:

    .. code-block::

      nnetpacket[0]
      # or
      nnetpacket['detection_out']

  .. function:: getOutputsList() -> list

    Returns all the output tensors in a list for the network.

  .. function:: getOutputsDict() -> dict

    Returns all the output tensors in a dictionary for the network.
    The key is the name of the output layer, the value is the shaped numpy array.

  .. function:: getOutputLayersInfo() -> depthai.TensorInfo

    Returns informations about the output layers for the network.

  .. function:: getInputLayersInfo() -> depthai.TensorInfo

    Returns informations about the input layers for the network.

  .. function:: getDetectedObjects() -> depthai.Detections

    .. warning::

      Works when in :ref:`blob config file <Creating Blob configuration file>` :code:`output_format` is set to :code:`detection` and with detection networks
      (:code:`Mobilenet-SSD`, :code:`(tiny-)YOLO-v3` based networks)

    Returns the detected objects in :class:`Detections` format. The network is decoded on device side.


.. class:: TensorInfo
  :canonical: depthai.TensorInfo

  Descriptor of the input/output layers/tensors of the network.

  When network is loaded the tensor info is automatically printed.

  **Attributes**

  .. attribute:: name
    :type: str

    Name of the tensor.

  .. attribute:: dimensions
    :type: list

    Shape of tensor array. E.g. : :code:`[1, 1, 100, 7]`

  .. attribute:: strides
    :type: list

    Strides of tensor array.

  .. attribute:: data_type
    :type: string

    Data type of tensor. E.g. : :code:`float16`

  .. attribute:: offset
    :type: int

    Offset in the raw output array.

  .. attribute:: element_size
    :type: int

    Size in bytes of one element in the array.

  .. attribute:: index
    :type: int

    Index of the tensor. E.g. : in case of multiple inputs/outputs in the network it marks the order of input/output.

  **Methods**

  .. function:: get_dict() -> dict

    Returns TensorInfo in a dictionary where the `key` is the name of attribute.

  .. function:: get_dimension() -> int

    Returns the specific dimension of the tensor

    .. code-block::

      tensor_info.get_dimension(depthai.TensorInfo.Dimension.WIDTH)  # returns width of tensor


.. class:: Detections
  :canonical: depthai.Detections

  Container of neural network results decoded on device side.

  **Example of accessing detections**

  Assuming the detected objects are stored in :code:`detections` object.

  * Number of detections

    .. code-block::

      detections.size()
      # or
      len(detections)

  * Accessing the nth detection

    .. code-block::

      detections[0]
      detections[1]  # ...

  * Iterating through all detections

    .. code-block::

      for detection in detections:


.. class:: Detection
  :canonical: depthai.Detection

  Detected object descriptor.

  **Attributes**

  .. attribute:: label
    :type: int

    Label id of the detected object.

  .. attribute:: confidence
    :type: float

    Confidence score of the detected object in interval [0, 1].

  .. attribute:: x_min
    :type: float

    Top left :code:`X` coordinate of the detected bounding box. Normalized, in interval [0, 1].

  .. attribute:: y_min
    :type: float

    Top left :code:`Y` coordinate of the detected bounding box. Normalized, in interval [0, 1].

  .. attribute:: x_max
    :type: float

    Bottom right :code:`X` coordinate of the detected bounding box. Normalized, in interval [0, 1].

  .. attribute:: y_max
    :type: float

    Bottom right :code:`Y` coordinate of the detected bounding box. Normalized, in interval [0, 1].

  .. attribute:: depth_x
    :type: float

    Distance to detected bounding box on :code:`X` axis. Only when depth calculation is enabled (stereo cameras are present on board).

  .. attribute:: depth_y
    :type: float

    Distance to detected bounding box on :code:`Y` axis. Only when depth calculation is enabled (stereo cameras are present on board).

  .. attribute:: depth_z
    :type: float

    Distance to detected bounding box on :code:`Z` axis. Only when depth calculation is enabled (stereo cameras are present on board).

  **Methods**

  .. function:: get_dict() -> dict

    Returns detected object in a dictionary where the :code:`key` is the name of attribute.


.. class:: Dimension
  :canonical: depthai.TensorInfo.Dimension

  Dimension descriptor of tensor shape. Mostly meaningful for input tensors since not all neural network models
  respect the semantics of :code:`Dimension` for output tensor


  **Values**

  .. attribute:: W / WIDTH
    :type: str

    Width

  .. attribute:: H / HEIGHT
    :type: str

    Height

  .. attribute:: C / CHANNEL
    :type: str

    Number of channels

  .. attribute:: N / NUMBER
    :type: str

    Number of inferences

  .. attribute:: B / BATCH
    :type: str

    Batch of inferences


.. class:: DataPacket
  :canonical: depthai.DataPacket

  DepthAI data packet, containing information generated on the device. Unlike NNetPacket, it contains a single "result"
  with source stream info

  **Attributes**

  .. attribute:: stream_name
    :type: str

    Returns packet source stream. Used to determine the origin of the packet and therefore allows to handle the packets
    correctly, applying proper handling based on this value

  **Methods**

  .. function:: getData() -> numpy.ndarray

    Returns the data as NumPy array, which you can be further transformed or displayed using OpenCV :code:`imshow`.

    Used with streams that returns frames e.x. :code:`previewout`, :code:`left`, :code:`right`, or encoded data e.x. :code:`video`, :code:`jpegout`.

  .. function:: getDataAsStr() -> str

    Returns the data as a string, capable to be parsed further.

    Used with streams that returns non-array results e.x. :code:`meta_d2h` which returns JSON object

  .. function:: getMetadata() -> FrameMetadata

    Returns metadata object containing all proprietary data related to this packet

  .. function:: getObjectTracker() -> ObjectTracker

    .. warning::

      Works only with packets from :code:`object_tracker` stream

    Returns metadata object containing :class:`ObjectTracker` object

  .. function:: size() -> int

    Returns packet data size


.. class:: FrameMetadata
  :canonical: depthai.FrameMetadata

  Metadata object attached to the packets sent via pipeline.

  **Methods**

  .. function:: getCameraName() -> str

    Returns the name of the camera that produced the frame.

  .. function:: getCategory() -> int

    Returns the type of the packet, whether it's a regular frame or arrived from taking a still

  .. function:: getFrameBytesPP() -> int

    Returns number of bytes per pixel in the packet's frame

  .. function:: getFrameHeight() -> int

    Returns the height of the packet's frame

  .. function:: getFrameWidth() -> int

    Returns the width of the packet's frame

  .. function:: getFrameType() -> int

    Returns the type of the data that this packet contains.

  .. function:: getInstanceNum() -> int

    Returns the camera id that is the source of the current packet

  .. function:: getSequenceNum() -> int

    Sequence number is assigned for each frame produced by the camera.
    It can be used to assure the frames are captured at the same time - e.x. if frames from left and right camera have
    the same sequence number, you can assume they were taken at the same time

  .. function:: getStride() -> int

    Specifies number of bytes till the next row of pixels in the packet's frame

  .. function:: getTimestamp() -> float

    When packet is created, it is assigned a creation timestamp, which can be obtained using this method


.. class:: ObjectTracker
  :canonical: depthai.ObjectTracker

  Object representing current state of the tracker, obtained by calling :func:`DataPacket.getObjectTracker`
  method on a packet from :code:`object_tracker` stream

  **Methods**

  .. function:: getNrTracklets() -> int

    Return the number of available tracklets

  .. function:: getTracklet(tracklet_nr: int) -> Tracklet

    Returns the tracklet with specified :code:`tracklet_nr`.
    To check how many tracklets there are, please use :func:`getNrTracklets` method


.. class:: Tracklet
  :canonical: depthai.Tracklet

  Tracklet is representing a single tracked object, is produced by :class:`ObjectTracker` class.
  To obtain it, call :func:`ObjectTracker.getTracklet` method.

  **Methods**

  .. function:: getId() -> int

    Return the tracklet id

  .. function:: getLabel() -> int

    Return the tracklet label, being the neural network returned result. Used to identify a class of recognized objects

  .. function:: getStatus() -> str

    Return the tracklet status - either :code:`NEW`, :code:`TRACKED`, or :code:`LOST`.

  .. function:: getLeftCoord() -> int

    Return the left coordinate of the bounding box of a tracked object

  .. function:: getRightCoord() -> int

    Return the right coordinate of the bounding box of a tracked object

  .. function:: getTopCoord() -> int

    Return the top coordinate of the bounding box of a tracked object

  .. function:: getBottomCoord() -> int

    Return the bottom coordinate of the bounding box of a tracked object

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