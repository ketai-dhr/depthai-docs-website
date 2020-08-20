---
layout: default
title: DepthAI Python API
toc_title: Python API
description: 用于与您的DepthAI平台进行交互的官方API。
order: 5
---

# {{page.title}}

关于安装、升级和使用DepthAI Python API的说明。

{: #python_version data-toc-title="Python 版本"}
## 支持的平台

DepthAI API python模块是为Ubuntu 18.04和Raspbian 10预制的。对于其他操作系统或Python版本，DepthAI可以[从源码编译](#compile_api)。

* [Ubuntu 18.04](#ubuntu) - Python 3.6
* [Raspberry Pi OS (Raspbian)](#raspbian) - Python 3.7
* [macOS](#macos) (Mac OS X) - Homebrew的安装设置/许可有相当大的差异，所以针对MacOS，我们目前需要从源码编译，请看[这里](#macos)。
* [Windows 10](https://discuss.luxonis.com/d/39-depthai-sneak-peak-into-windows-support) - 目前处于实验性阶段 (截至2020年5月18日). 
* [其他操作系统](#compile_api) - DepthAI的代码库是开源的，所以它可以在各种其他平台上从源码编译。[点此查看步骤](#compile_api)。我们还将很快发布一个迭代版本，它不需要主机运行操作系统，甚至不需要USB支持。
* 嵌入式平台 - 我们正在努力支持与MSP430、STM32等处理器的SPI、I2C和/或UART通信（并将为树莓派准备一套SPI、I2C和UART的参考库，这将有助于在通过这些接口与DepthAI集成定制应用时进行调试）。

## 安装系统依赖
<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
  正在使用树莓派计算模组（Compute Module）或已经烧录好的DepthAI 树莓派 Micro SD卡? <strong>请跳过此步骤.</strong><br/>
  <span class="small">依赖安装和代码库 check out 路径 `~/Desktop/depthai-python-extras`.</span>
</div>
 
{: #raspbian}
### Raspberry Pi OS (Raspbian)
很多用户可能已经安装过以下的软件。这部分主要是详细介绍了如何从一个全新的Raspbian开始安装 (我们测试了一个[内含推荐软件的Raspbian](https://www.raspberrypi.org/downloads/raspbian/))。

在全新安装的情况下，以下是让DepthAI（和megaAI）启动和运行所需的依赖项。确保将你的树莓派已连接到互联网，然后运行以下命令:
```
sudo apt update
sudo apt upgrade
sudo apt install python3-opencv libcurl4 libatlas-base-dev libhdf5-dev libhdf5-serial-dev libatlas-base-dev libqtgui4 libqt4-test
echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
sudo udevadm control --reload-rules && sudo udevadm trigger
git clone https://github.com/luxonis/depthai.git
cd depthai
python3 -m pip install -r requirements.txt
```

请注意，这个过程中通过`apt`更新和升级Pi将耗时最长。

在运行以上命令以后, 请移步至下面的[快速测试](#quicktest)，在你的树莓派上首次运行DepthAI.

{: #ubuntu}
### Ubuntu 
```
sudo apt install git python3-pip python3-opencv libcurl4 libatlas-base-dev libhdf5-dev libhdf5-serial-dev libatlas-base-dev libqtgui4 libqt4-test
echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
sudo udevadm control --reload-rules && sudo udevadm trigger
git clone https://github.com/luxonis/depthai.git
cd depthai
python3 -m pip install -r requirements.txt
```

{: #quicktest}
## 快速测试

在depthai文件夹内运行 `python3 test.py` 以确保一切正常:

```
python3 test.py
```

如果一切顺利的话，会弹出一个小视频窗口。如果画面中的物体属于[物体检测示例20类](https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22)中的某一类，画面上会叠加该物体的信息。

<h2 id="install" data-toc-title="安装">安装 DepthAI API</h2>


由于我们还没有使用标准的`pip install`（后面可能会使用），DepthAI的Python模块和附件（实用程序、示例和教程）是通过check out我们的[depthai](https://github.com/luxonis/depthai) GitHub代码库来安装的。

因此，有必要指示pip把这个repo安装成全局可用。请使用以下的命令。


```
pip3 install --user -e depthai
```

<h2 id="upgrade" data-toc-title="升级">升级 DepthAI API</h2>

<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
正在使用树莓派计算模组（Compute Module）或已经烧录好的DepthAI Micro SD卡?<br/>
  <span class="small">代码库已被 checked out 到 `~/Desktop/depthai`.</span>
</div>


按以下步骤把DepthAI Python API升级到最新版本:

1. `cd` 到你本地拷贝的 [depthai](https://github.com/luxonis/depthai) repository.
2. Pull 最新的变动:
    ```
    git pull
    ```
3. 确保 `depthai` 对你所有的python脚本都可用:
    ```
    pip3 install --user -e .
    ```

{: #reference }
## API 参考

{: #depthai_init_device}
### depthai.init_device(cmd_file_path) → bool

初始化 DepthAI 设备, 设备初始化成功则返回 `True`，否则返回 `False`.

#### 参数

* cmd_file_path(str) - The full path to the DepthAI `cmd` file.

#### 示例

```py
import depthai
import consts.resource_paths
if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")
```

{: #depthai_create_pipeline}
### depthai.create_pipeline(config=dict) → CNNPipeline

初始化一个 DepthAI 管道, 初始化成功则返回创建的`CNNPipeline`，否则返回 `None`.

#### 参数

* __config(dict)__ -  管道设置配置的一个 `dict` 。
    <br/>配置的键/值示例:
    ```py
    {
        # Possible streams:
        #   'left' - left mono camera preview
        #   'right' - right mono camera preview
        #   'previewout' - 4K color camera preview
        #   'metaout' - CNN output tensors
        #   'depth_raw' - the raw depth map, disparity converted to real life distance
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
            {'name': 'depth_raw', 'max_fps': 12.0},
            {'name': 'disparity', 'max_fps': 12.0},
            {'name': 'disparity_color', 'max_fps': 12.0},
        ],
        'depth':
        {
            'calibration_file': consts.resource_paths.calib_fpath,
            'padding_factor': 0.3,
            'depth_limit_m': 10.0, # In meters, for filtering purpose during x,y,z calc
            'confidence_threshold' : 0.5, #Depth is calculated for bounding boxes with confidence higher than this number 
        },
        'ai':
        {
            'blob_file': blob_file,  # MyriadX CNN blob file path
            'blob_file_config': blob_file_config,  # Configuration file for CNN output tensor mapping on host side
            'calc_dist_to_bb': True,  # if True, will include depth information to CNN output tensor
            'keep_aspect_ratio': not args['full_fov_nn'],
        },
        # object tracker
        'ot':
        {
            'max_tracklets'        : 20, # maximum 20 is supported
            'confidence_threshold' : 0.5, # object is tracked only for detections over this threshold
        },
        'board_config':
        {
            'swap_left_and_right_cameras': args['swap_lr'], # True for 1097 (RPi Compute) and 1098OBC (USB w/onboard cameras)
            'left_fov_deg': args['field_of_view'], # Same on 1097 and 1098OBC
            'rgb_fov_deg': args['rgb_field_of_view'],
            'left_to_right_distance_cm': args['baseline'], # Distance between stereo cameras
            'left_to_rgb_distance_cm': args['rgb_baseline'], # Currently unused
            'store_to_eeprom': args['store_eeprom'],
            'clear_eeprom': args['clear_eeprom'],
            'override_eeprom': args['override_eeprom'],
        },
        
        #'video_config':
        #{
        #    'rateCtrlMode': 'cbr',
        #    'profile': 'h265_main', # Options: 'h264_baseline' / 'h264_main' / 'h264_high' / 'h265_main'
        #    'bitrate': 8000000, # When using CBR
        #    'maxBitrate': 8000000, # When using CBR
        #    'keyframeFrequency': 30,
        #    'numBFrames': 0,
        #    'quality': 80 # (0 - 100%) When using VBR
        #}
    }
    ```

#### 示例

```py
pipeline = depthai.create_pipeline(config={
    'streams': ['previewout'],
    'ai': {
        'blob_file': consts.resource_paths.blob_fpath,
        'blob_file_config': consts.resource_paths.blob_config_fpath
    }
})
```

{: #depthai_cnnpipeline}
### depthai.CNNPipeline

管道对象，设备可以用它向主机发送结果。 使用 `depthai.create_pipeline`创建。

* __get_available_data_packets() -> depthai.DataPacketList__

    只返回设备本身产生的数据包，不含CNN结果。

* __get_available_nnet_and_data_packets() -> tuple[depthai.NNetPacketList, depthai.DataPacketList]__

    同时返回神经网络的结果和设备产生的数据。

{: #depthai_nnetpacket}
### depthai.NNetPacket

神经网络结果包。它不是一个单一的结果，而是一批附加了额外元数据的结果。

* __entries() -> depthai.TensorEntryContainer__

    返回可以遍历的depthai.TensorEntry列表。

* __getMetadata() -> depthai.FrameMetadata__

    返回包含与此数据包相关的所有专有数据的元数据对象。

* __get_tensor(Union[int, str]) -> numpy.ndarray__

    返回来自特定张量的原始输出，你可以通过索引或在[blob配置文件](#creating-blob-configuration-file)中指定的`output_tensor_name`属性选择。
    
{: #depthai_datapacket}
### depthai.DataPacket

DepthAI数据包，包含设备上生成的信息。与NNetPacket不同的是，它包含一个单一的结果与源流信息。

* __getData() -> numpy.ndarray__

    返回数据为NumPy数组，你可以使用OpenCV的`imshow`显示数据。
    
    用于返回帧的流，如："previewout"、"left"、"right"，或编码数据，如："video"、"jpegout"。

* __getDataAsStr() -> str__

    以字符串的形式返回数据，可以进一步解析。
    
    用于返回非数组结果的数据流，如`meta_d2h`，返回JSON对象。

* __getMetadata() -> depthai.FrameMetadata__

    返回包含该数据包所有专有数据的元数据对象。

* __getObjectTracker() -> ObjectTracker__

    返回结果为ObjectTracker实例，仅用于来自`object_tracker`流的数据包。

* __size() -> int__

    返回数据包的大小

* __stream_name: str__

    返回数据包源流。用于确定数据包的来源，因此允许正确处理数据包，根据该值进行适当处理。

## 准备MyriadX blob文件和它的配置文件

正如你在[本例](#example-1)中所看到的，"create_pipeline "方法的基本用法包括指定所需的输出流和AI部分，其中你指定MyriadX blob和它的配置。

在本节中，我们将介绍如何获取`blob_file`和`blob_file_config`。

### 获取MyriadX blob

由于我们使用的是MyriadX VPU，您的模型需要被编译（或准确地优化和转换）成MyriadX blob文件，它将被发送到设备并执行。

最简单的方法是使用我们的[在线BlobConverter应用程序](http://69.164.214.171:8080/)来获取这个blob文件。它有编译所需的所有工具，所以你不需要设置任何东西--你甚至可以从[OpenVINO模型Zoo](https://github.com/openvinotoolkit/open_model_zoo)下载一个模型的blob。

如果你愿意，你也可以自己编译blob。你需要安装[OpenVINO工具包](https://docs.openvinotoolkit.org/latest/index.html)，然后使用[模型优化器](https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html)和[Myriad编译器](https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#myriad_platform_option)]来获得MyriadX blob。
我们已经在[这里](https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format)
记录了这些编译器的使用实例。
    
### 创建Blob配置文件
    
配置文件需要在主机端创建CNN输出和Python API之间的映射。
基本上，整个配置都是围绕`tensors`数组解析的。一个张量代表一个CNN的输出，所以一般来说
你将只有一个对象在里面，但如果你想使用如[年龄性别识别](https://docs.openvinotoolkit.org/latest/omz_models_intel_age_gender_recognition_retail_0013_description_age_gender_recognition_retail_0013.html)
你需要定义两个tenor。



让我们拿一个模板配置文件（基于MobileNetSSD），通过张量对象字段来进行描述

```json
{
"tensors":
[
    {       
        "output_tensor_name": "out",
        "output_dimensions": [1, 1, 100, 7],
        "output_entry_iteration_index": 2,
        "output_properties_dimensions": [3],
        "property_key_mapping":
        [
            [],
            [],
            [],
            ["id", "label", "confidence", "left", "top", "right", "bottom"]
        ],
        "output_properties_type": "f16"
    }
]
}
```

* `output_tensor_name` - is a custom name as a string that you choose for this specific tensor. In the code, you can access a specific tensor by this name using [`get_tensor`](#depthai_nnetpacket) method ([example](https://github.com/luxonis/depthai/blob/master/depthai_helpers/tiny_yolo_v3_handler.py#L120))
* `output_dimensions` - determines the dimensions of the CNN model output. If your model, e.x. [mobilenet-ssd](https://docs.openvinotoolkit.org/latest/omz_models_public_mobilenet_ssd_mobilenet_ssd.html), contains `N` as one of the output dimentions.
(specifying that it's dependent of the number of detected items), you should set this variable to a relatively high value - like in the example above, it's `100`. If your network produces a fixed-size output, and you insert the dimensions that are higher than actual output, the DepthAI will crash. If less, it will work but sometimes won't produce results (depending on the network)
* `output_entry_iteration_index` - if your network returns multiple results (just like mentioned above with mobilenet having `N` as a dimension), you can specify the index to iterate over. Since in our case we set `100` as third argument in array, iteration index should be set to `2` (third index).  You can set it to `0` if you don't need iteration.
* `property_key_mapping` - contains field names as string which you can change according to your preference, it's how you'll access the fields in the code, it has to match the number of properties returned by your network. Taking mobilenet as an example, for non-depth it will be __7__, and with depth info will be __10__ (as distances x, y and z are added). If you don't need the mapping, you can set it to `[]`
* `output_properties_type` - c-type data type specifying size of the output variables


If your network returns tensors with only one dimension other than `1`, you can ship the leading empty arrays (which are added to fit the output dimensions)

For instamce,  MobienetSSD returns results in array with dimensions `1, 1, N, 7`, so in `property_key_mapping` we have 4 leading arrays

On the other hand, Age/Gender detector, one of the tensors returns results in array with dimensions `1, 2, 1, 1`, so in `property_key_mapping` we have a single array with two fields specified, no need to follow it with 3 empty leading arrays

#### 示例

##### MobilenetSSD

```json
{
    "tensors":
    [
        {       
            "output_tensor_name": "out",
            "output_dimensions": [1, 1, 100, 7],
            "output_entry_iteration_index": 2,
            "output_properties_dimensions": [3],
            "property_key_mapping":
            [
                [],
                [],
                [],
                ["id", "label", "confidence", "left", "top", "right", "bottom"]
            ],
            "output_properties_type": "f16"
        }
    ]
}
```

##### 带有深度信息的MobilenetSSD

```json
{
    "tensors":
    [
        {       
            "output_tensor_name": "out",
            "output_dimensions": [1, 1, 100, 10],
            "output_entry_iteration_index": 2,
            "output_properties_dimensions": [3],
            "property_key_mapping":
            [
                [],
                [],
                [],
                ["id", "label", "confidence", "left", "top", "right", "bottom", "distance_x", "distance_y", "distance_z"]
            ],
            "output_properties_type": "f16"
        }
    ]
}
```

##### 识别年龄和性别

```json
{
    "tensors":
    [
        {       
            "output_tensor_name": "out",
            "output_dimensions": [1, 1, 1, 1],
            "output_entry_iteration_index": 0,
            "output_properties_dimensions": [0],
            "property_key_mapping":
            [
                ["age"]
            ],
            "output_properties_type": "f16"
        },
	{       
            "output_tensor_name": "out1",
            "output_dimensions": [1, 2, 1, 1],
            "output_entry_iteration_index": 0,
            "output_properties_dimensions": [0],
            "property_key_mapping":
            [
                ["female", "male"]
            ],
            "output_properties_type": "f16"
       }
    ]
}
```
    

{: #compile_api }
## 为其他平台编译DepthAI API

DepthAI的API是开源的，所以可以针对各种平台和Python3版本进行编译。

以下是Luxonis员工和DepthAI用户所做的一个简答的总结。

* Mac OS X - 从源码编译，说明[如下](#mac-os-x)。
* Linux Mint - 应该可以和 Ubuntu 18.04 预制的 python 模块一起工作。
* Manjaro/Arch - 当[从源代码编译](#compile_linux)时能工作。
* 其他Linux发行版--检查Ubuntu pymodule是否正常工作(使用`ldd`检查是否有破损的依赖关系)，或者从源码编译[如下](/api#compile_linux)。


{: #macos}
### macOS (Mac OS X)
如果安装的是Mac OS X，DepthAI可以通过以下命令进行安装和测试，感谢[HomeBrew](https://brew.sh/)。

#### 安装 HomeBrew
(如果没有安装的话)
```
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)" 
```
#### 安装Python和其他开发工具
(如果也没有安装的话)
```
brew install coreutils python3 cmake libusb wget opencv curl
pip3 install numpy opencv-python --user
```
现在你已经准备好克隆DepthAI的Github并构建给Mac OS X用的DepthAI了。

#### 在Mac OS X上构建DepthAI并进行测试:
```
git clone https://github.com/luxonis/depthai.git
cd depthai
git submodule update --init --recursive
./depthai-api/install_dependencies.sh
./depthai-api/build_py_module.sh
python3 test.py
```
如果一切顺利的话，会弹出一个小视频窗口。如果画面中的物体属于[物体检测示例20类](https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22)中的某一类，画面上会叠加该物体的信息。

{: #compile_linux }
### 从源代码构建 DepthAI

如果你正在使用非标准的Python版本(比如在旧的操作系统上使用旧的Python)，或者自己修改DepthAI API，或者不管出于什么原因你需要从源码中构建，显然都需要这个步骤。

#### 安装开发者工具
要从头开始编译Python API，根据机器的配置，可能需要安装必要的包。你可以通过你的Linux发行版的包管理器，或者根据需要从源码构建它们，才能成功地从源码构建DepthAI python模块。
* cmake
* gcc
* g++
* libusb
* opencv
* libcurl4-openssl-dev
* python3
  * 包括 `pip3 install numpy opencv-python --user`
  
值得注意的是，你一般可以通过类似 "build-essential "这样的命令来安装cmake、gcc、g++等（就像在Ubuntu中一样）。

一旦安装了这些依赖项（你可能已经装过了），使用下列命令从源代码构建pymodule并进行测试。

#### 从源代码构建 DepthAI
```
git clone https://github.com/luxonis/depthai.git
cd depthai
git submodule update --init --recursive
./depthai-api/install_dependencies.sh
./depthai-api/build_py_module.sh
python3 test.py
```

这里也一样，如果一切顺利的话，会弹出一个小视频窗口。如果画面中的物体属于[物体检测示例20类](https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22)中的某一类，画面上会叠加该物体的信息。

#### 从特定（实验）分支的源代码重建DepthAI。
下面的命令可能有些过度保险，但可以确保实验性build需要的一切内容都完全更新。 该过程中主要的延迟是递归更新，不过一旦你在某台机器上更新过一次后，应该就不用再花很长时间，除非有特别大的上层依赖变化。

```
git checkout [commit-hash or branch_name] --recurse-submodules=yes -f
git submodule update --init --recursive && ./depthai-api/install_dependencies.sh && ./depthai-api/build_py_module.sh --clean
```
