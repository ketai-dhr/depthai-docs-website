Python API
=================================

关于安装、升级和使用 DepthAI Python API 的说明。

支持的平台
#################################

DepthAI API python 模块是为 Ubuntu, MaxOS 和 Windows 预制的。
对于其他操作系统或 Python 版本，可以 :ref:`从源码编译 <其他安装方式>` DepthAI。.

安装系统依赖
##############################

运行 DepthAI 库需要几个基本的系统依赖。
它们中的大多数应该已经安装在大多数系统中，但是如果没有安装，
我们准备了一个 :download:`安装脚本 </_static/install_dependencies.sh>`
，这将确保安装了所有依赖项以及方便的开发/编程工具。还有适用于 macOS ( `此处 <https://www.bilibili.com/video/BV1Vy4y1m7qG?from=search&seid=17057089443751489307>`__ ),Raspberry Pi ( `此处 <https://youtu.be/BpUMT-xqwqE>`__ ), Ubuntu( `此处 <https://www.bilibili.com/video/BV1TT4y1u7Fv?from=search&seid=17057089443751489307>`__ )和 Windows 10( `此处 <https://www.bilibili.com/video/BV1uA411s7Ly?from=search&seid=17057089443751489307>`__ )的视频指南。

macOS
******************************

.. code-block:: bash

  bash -c "$(curl -fL http://docs.luxonis.com/_static/install_dependencies.sh)"

执行此命令后，关闭并重新打开终端窗口。

该脚本还可以在 M1 Mac 上运行，并且在 Rosetta 2 下安装了 Homebrew，因为某些 Python 软件包仍缺少对M1的原生支持。如果您已经在本地安装了 Homebrew 并且无法正常工作，请参见 `此处 <https://github.com/luxonis/depthai/issues/299#issuecomment-757110966>`__ 以了解其他一些疑难解答步骤。

请注意，如果未出现视频流窗口，请考虑运行以下命令：

.. code-block:: bash

  python3 -m pip install opencv-python --force-reinstall --no-cache-dir

有关更多信息，请参见我们论坛上的 `macOS 视频预览窗口未能出现 <https://discuss.luxonis.com/d/95-video-preview-window-fails-to-appear-on-macos>`__ 话题讨论。

Raspberry Pi OS
*********************************

.. code-block:: bash

  sudo curl -fL http://docs.luxonis.com/_static/install_dependencies.sh | bash

Ubuntu
*********************************

.. code-block:: bash

  sudo wget -qO- http://docs.luxonis.com/_static/install_dependencies.sh | bash

openSUSE
********************************

对于 openSUSE,在这篇 `官方文章 <https://en.opensuse.org/SDB:Install_OAK_AI_Kit.>`__ 中提供了如何在 openSUSE 平台上安装 OAK 设备。

Windows
********************************

- 右键单击开始按钮
- 选择 Windows PowerShell (管理员)
- 安装 Chocolatey 软件包管理器(类似于 macOS 的 Homebrew)：

.. code-block:: bash

  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

- 关闭 PowerShell ，然后通过重复前两个步骤重新打开另一个 PowerShell (管理员)。
- 安装 Python 和 PyCharm

.. code-block:: bash

  choco install cmake git python pycharm-community -y

启用 USB 设备（仅在 Linux 上）
#######################################

由于 DepthAI 是 USB 设备，因此为了在使用 :code:`udev` 工具的系统上与之通信， 
您需要添加 udev 规则以使设备可访问。

以下命令将向您的系统添加新的 udev 规则

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

从 PyPi 安装
#################

我们的软件包是 `通过 PyPi <https://pypi.org/project/depthai/>`__ 分发的，为了可以在您的环境中安装，请使用

.. code-block:: bash

  python3 -m pip install depthai

有关其他安装选项，请参阅 :ref:`其他安装选项 <其他安装方式>` 。

测试安装
#################

我们在 GitHub 上有 `depthai <https://github.com/luxonis/depthai>`__ 存储库，
其中包含许多有用的示例和准备好的神经网络，
您可以使用它们来加快原型制作速度。它还包括由我们的贡献者维护的测试脚本，
该脚本应有助于您验证设置是否正确。

首先，克隆 `depthai <https://github.com/luxonis/depthai>`__ 存储库，并将目录更改为该目录：

.. code-block:: bash

  git clone https://github.com/luxonis/depthai.git
  cd depthai

接下来是安装此仓库的要求。请注意，我们建议将依赖项安装在虚拟环境中，以免它们干扰系统上的其他Python工具/环境。

- 对于 Mac / Windows / Ubuntu 等开发工具，我们建议使用 PyCharm IDE，因为它会自动为您创建和管理虚拟环境，以及许多其他好处。可替代的有： `conda` , `pipenv` 或者 `virtualenv` 可以直接使用(或用您的首选IDE)。
- 有关资源受限的系统，如树莓派或其他小型Linux系统的安装，我们建议 `conda` , `pipenv` 或者 `virtualenv` 中的一个。要使用 `virtualenv` 设置虚拟环境，请运行 `virtualenv venv && source venv/bin/activate` 。

使用虚拟环境(或者如果需要在整个系统范围内)，运行以下命令以安装此示例存储库的依赖：

.. code-block:: bash

  python3 install_requirements.py

现在，从 DepthAI 内部运行演示脚本，以确保一切正常：

.. code-block:: bash

  python3 depthai_demo.py

如果一切顺利的话，会弹出一个小视频窗口。
如果画面中的物体属于 `物体检测示例 20 类 <https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22>`__ 中的某一类，画面上会叠加该物体的信息。

运行其他示例
##########################################

运行此演示之后，您可以运行 :code:`python3 depthai_demo.py -h` 来查看默认情况下可以运行的其他神经网络。

检查完之后，继续执行以下操作：

- 我们的教程，如何使用 OpenVINO 的预训练模型， `此处 <https://docs.luxonis.com/en/latest/pages/tutorials/pretrained_openvino/>`__
- 我们的实验性示例，在 `这里 <https://github.com/luxonis/depthai-experiments>`__ 可以学习更多使用 DepthAI 的方法。

您还可以在下面继续学习如何转换自己的神经网络以在DepthAI上运行。

另外，我们还在下面提供了在线模型训练，该演示向您展示了如何为 DepthAI 训练和转换模型。

- 在线机器学习训练和模型转换： `此处 <https://github.com/luxonis/depthai-ml-training/tree/master/colab-notebooks>`__

准备 MyriadX blob 文件和它的配置文件
###########################################

正如你在 :ref:`本例 <example>` 中所看到的，:func:`Device.create_pipeline()` 方法的基本用法包括指定所需的输出流和 AI 部分，在其中指定 MyriadX blob 及其配置。

在本节中，我们将介绍如何同时获取 :code:`blob_file` 和 :code:`blob_file_config` 。

获取 MyriadX Blob 
**********************

由于我们使用的是 MyriadX VPU，
您的模型需要被编译(或准确地进行优化和转换)为 MyriadX Blob 文件，然后将其发送到设备并执行。

最简单的方法是使用我们的在线 `BlobConverter应用程序 <http://69.164.214.171:8083/>`__ 来获取这个 blob 文件。
它有编译所需的所有工具，
所以你不需要设置任何东西–你甚至可以从 `OpenVINO模型Zoo <https://github.com/openvinotoolkit/open_model_zoo>`__ 下载一个模型的 blob。

如果你愿意，你也可以自己编译 blob。
你需要安装 `OpenVINO 工具包 <https://docs.openvinotoolkit.org/latest/index.html>`__，
然后使用 `Model Optimizer  <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html>`__ 
和 `Myriad Compiler <https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#myriad_platform_option>`__ 来获得 MyriadX blob。 
我们已经在 `这里 <https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__ 记录了这些编译器的使用实例。

创建 Blob 配置文件
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
        # 将标准化坐标缩放为图像坐标
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

要从我们的源代码中获取最新但尚未发布的功能，您可以继续手动编译 DepthAI 软件包。

从源构建的依赖项
*********************************

- CMake > 3.2.0
- 生成工具 (Ninja, make, ...)
- C/C++ 编译器
- libusb1 开发包

.. _raspbian:

Ubuntu, Raspberry Pi OS, ... (基于 Debian 的系统)
---------------------------------------------------

在基于 Debian 的系统 (Raspberyy Pi OS, Ubuntu, ...)上，可以通过运行以下命令获取安装依赖：

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

Pip 允许用户从特定的 commit 安装软件包，即使它们尚未在 PyPi 上发布。


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
  python3 install_requirements.py

从源安装
*******************

如果需要，您还可以从源代码本身安装该软件包 
- 它将允许您对 API 进行更改，并看到它们的实际操作。

为此，请先下载存储库，然后在开发模式下将该包添加到您的 python 解释器中

.. code-block:: bash

  git clone https://github.com/luxonis/depthai-python.git
  cd depthai-python
  git submodule update --init --recursive
  python3 setup.py develop  # 如果使用系统解释器而不是虚拟环境，则可能需要添加sudo

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


API 参考
#############

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