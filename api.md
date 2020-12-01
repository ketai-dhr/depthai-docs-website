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
Python API
==========

关于安装、升级和使用DepthAI Python API的说明。

支持的平台
----------

DepthAI API python模块是为Ubuntu, MaxOS 和 Windows 预制的。
对于其他操作系统或Python版本，可以 从源码编译 \<其他安装方式\>
DepthAI。.

安装系统依赖
------------

运行DepthAI库需要几个基本的系统依赖。
它们中的大多数应该已经安装在大多数系统中，但是如果没有安装，
我们准备了一个 安装脚本 \</\_static/install\_dependencies.sh\>
，以确保所有依赖项都已安装

``` {.sourceCode .bash}
curl -fL http://docs.luxonis.com/_static/install_dependencies.sh | bash
```

如果使用Windows系统，请使用此
批处理脚本 \</\_static/install\_dependencies.bat\> 进行依赖项安装

启用USB设备（仅在Linux上）
--------------------------

由于DepthAI是USB设备，因此为了在使用 udev 工具的系统上与之通信，
您需要添加udev规则以使设备可访问。

以下命令将向您的系统添加新的udev规则

``` {.sourceCode .bash}
echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
sudo udevadm control --reload-rules && sudo udevadm trigger
```

从PyPi安装
----------

我们的软件包是 [通过PyPi](https://pypi.org/project/depthai/)
分发的，为了可以在您的环境中安装，请使用

``` {.sourceCode .bash}
python3 -m pip install depthai
```

有关其他安装选项，请参阅 其他安装选项 \<其他安装方式\> 。

测试安装
--------

我们在GitHub上有 [depthai](https://github.com/luxonis/depthai) 存储库，
其中包含许多有用的示例和准备好的神经网络，
您可以使用它们来加快原型制作速度。它还包括由我们的贡献者维护的测试脚本，
该脚本应有助于您验证设置是否正确。

首先，克隆 [depthai](https://github.com/luxonis/depthai)
存储库并安装其依赖项

``` {.sourceCode .bash}
git clone https://github.com/luxonis/depthai.git
cd depthai
python3 -m pip install -r requirements.txt
```

现在，从 depthai 内部运行演示脚本，以确保一切正常：

``` {.sourceCode .bash}
python3 depthai_demo.py
```

如果一切顺利的话，会弹出一个小视频窗口。 如果画面中的物体属于
[物体检测示例20类](https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22)
中的某一类，画面上会叠加该物体的信息。

准备 MyriadX blob 文件和它的配置文件
------------------------------------

正如你在 本例 \<Example\> 中所看到的，Device.create\_pipeline
方法的基本用法包括指定所需的输出流和AI部分，在其中指定 MyriadX blob
及其配置。

在本节中，我们将介绍如何同时获取 blob\_file 和 blob\_file\_config 。

### 获取 MyriadX Blob

由于我们使用的是 MyriadX VPU，
您的模型需要被编译（或准确地进行优化和转换）为 MyriadX Blob
文件，然后将其发送到设备并执行。

最简单的方法是使用我们的在线
[BlobConverter应用程序](http://69.164.214.171:8083/)
来获取这个blob文件。 它有编译所需的所有工具，
所以你不需要设置任何东西–你甚至可以从
[OpenVINO模型Zoo](https://github.com/openvinotoolkit/open_model_zoo)
下载一个模型的blob。

如果你愿意，你也可以自己编译blob。 你需要安装
[OpenVINO工具包](https://docs.openvinotoolkit.org/latest/index.html)，
然后使用
[模型优化器](https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html)
和
[Myriad编译器](https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#myriad_platform_option)
来获得 MyriadX blob。 我们已经在
[这里](https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format)
记录了这些编译器的使用实例。

### 创建Blob配置文件

如果未提供配置文件或将 output\_format 设置为 raw，
则设备上不会进行解码，用户必须在主机端手动进行解码。

当前支持在设备上对基于 Mobilenet-SSD 和 (tiny-)YOLO-v3 的网络进行解码。
对于该配置文件，需要使用网络特定的参数。

tiny-yolo-v3 网络示例：

``` {.sourceCode .json}
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
```

-   NN\_config - 网络配置
    :   -   output\_format
            :   -   "detection" - 设备上完成解码， 接收到的数据包为
                    Detections 格式
                -   "raw" - 在主机上完成解码

        -   NN\_family - "YOLO" 或 "mobilenet"
        -   NN\_specific\_metadata - 仅用于 "YOLO"
            :   -   classes - classes 数量
                -   coordinates - coordinates 数量
                -   anchors - YOLO 网络的锚点
                -   anchor\_masks - 每个输出层的锚定遮罩 : 26x26, 13x13
                    (+ 52x52 for full YOLO-v3)
                -   iou\_threshold - 检测到的对象的联合阈值交集
                -   confidence\_threshold - 检测物体的得分置信度阈值

-   mappings.labels - 使用 depthai\_demo.py 脚本来解码ID的标签

将 output\_format 设置为 detection 时的解码示例：

``` {.sourceCode .python}
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
```

主机和设备上基于完整 yolo-v3 和 tiny-yolo-v3 的解码示例在
[此处](https://github.com/luxonis/depthai/blob/develop/depthai_helpers/tiny_yolo_v3_handler.py)

主机和设备上基于 mobilenet 的网络的解码示例在
[此处](https://github.com/luxonis/depthai/blob/develop/depthai_helpers/mobilenet_ssd_handler.py)

其他安装方式
------------

要从我们的源代码中获取最新但尚未发布的功能，您可以继续手动编译depthai软件包。

### 从源构建的依赖项

-   CMake \> 3.2.0
-   生成工具 (Ninja, make, ...)
-   C/C++ 编译器
-   libusb1 开发包

#### Ubuntu, Raspberry Pi OS, ... (基于Debian的系统)

在基于Debian的系统 (Raspberyy Pi OS, Ubuntu,
...)上，可以通过运行以下命令获取安装依赖：

``` {.sourceCode .bash}
sudo apt-get -y install cmake libusb-1.0-0-dev build-essential
```

#### macOS (Mac OS X)

假设安装了 Mac OS X , 则
[depthai-python](https://github.com/luxonis/depthai-python)
库需要以下依赖项

-   HomeBrew (如果尚未安装)

    ``` {.sourceCode .bash}
    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"
    ```

-   Python, libusb, CMake, wget

    ``` {.sourceCode .bash}
    brew install coreutils python3 cmake libusb wget
    ```

现在你已经准备好克隆
[depthai-python](https://github.com/luxonis/depthai-python) ，并在 Mac
OSX 上构建。

### 使用 GitHub commit 进行安装

Pip允许用户从特定的 commit 安装软件包，即使它们尚未在PyPi上发布。

为此，请使用以下命令 - 并确保使用正确的 [commit
hash](https://github.com/luxonis/depthai-python/commits) 替换
\<commit\_sha\>

``` {.sourceCode .bash}
python3 -m pip install git+https://github.com/luxonis/depthai-python.git@<commit_sha>
```

### 使用/测试特定的 分支/PR

有时，使用特定分支可能会引起您的注意。
例如，这可能是因为我们已经听取了您的功能要求并在分支中实现。
或者可能是出于稳定性目的，在合并到 main 中之前，在 develop 中实现。

因此，当在 [depthai](https://github.com/luxonis/depthai) 存储库中工作时,
可以通过以下命令来使用分支。 在此示例中， 我们将尝试使用 develop 分支
(这是在将新功能合并到 main 之前我们用来吸收新功能的分支)：

在运行以下命令之前， 您可以独立克隆存储库 (以免覆盖任何本地更改)
也可以先执行 git pull 。

``` {.sourceCode .bash}
git checkout develop
python3 -m pip install -U pip
python3 -m pip install -r requirements.txt
```

### 从源安装

如果需要，您还可以从源代码本身安装该软件包 - 它将允许您对 API
进行更改，并看到它们的实际操作。

为此，请先下载存储库，然后在开发模式下将该包添加到您的 python 解释器中

``` {.sourceCode .bash}
git clone https://github.com/luxonis/depthai-python.git
cd depthai-python
git submodule update --init --recursive
python3 setup.py develop  # you may need to add sudo if using system interpreter instead of virtual environment
```

如果您要使用默认(main)以外的其他分支(e.g. develop)， 可以通过键入

``` {.sourceCode .bash}
git checkout develop  # replace the "develop" with a desired branch name
git submodule update --recursive
python3 setup.py develop
```

或者，如果您要使用特定的 commit，请键入

``` {.sourceCode .bash}
git checkout <commit_sha>
git submodule update --recursive
python3 setup.py develop
```

API Reference
-------------

> canonical
> :   depthai.Device
>
> Represents the DepthAI device with the methods to interact with it.
>
> > **warning**
> >
> > Please be aware that all methods except get\_available\_streams
> > require create\_pipeline to be run first,
>
> **Example**
>
> ``` {.sourceCode .python}
> import depthai
> device = depthai.Device('', False)
> pipeline = device.create_pipeline(config={
>     'streams': ['previewout', 'metaout'],
>     'ai': {
>         "blob_file": "/path/to/model.blob",
>         "blob_file_config": "/path/to/config.json",
>     },
> })
> ```
>
> **Methods**
>
> > noindex
> > :   
> >
> > Development and debug way to initialize the DepthAI device.
> >
> > **cmd\_file** is a path to firmware .cmd file that will be loaded
> > onto the device for boot.
> >
> > **device\_id** represents the USB port id that the device is
> > connected to. If set to specific value (e.x. "1") it will look for
> > the device in specific USB port, whereas if left empty - '' - it
> > will look for the device on all ports. It's useful when we have more
> > than one DepthAI devices connected and want to specify which one to
> > use in the code

> canonical
> :   depthai.AutofocusMode
>
> An enum with all autofocus modes available
>
> **Members**

> canonical
> :   depthai.CNNPipeline
>
> Pipeline object using which the device is able to send it's result to
> the host.
>
> **Methods**

> canonical
> :   depthai.NNetPacket
>
> For any neural network inference output NNPacket.get\_tensor can be
> used. For the specific case of Mobilenet-SSD, YOLO-v3 decoding can be
> done in the firmware. Decoded objects can be accessed through
> getDetectedObjects as well in addition to raw output to make the
> results of this commonly used networks easily accessible. See
> blob config file \<Creating Blob configuration file\> for more details
> about different neural network output formats and how to choose
> between these formats.
>
> Neural network results packet. It's not a single result, but a batch
> of results with additional metadata attached
>
> **Methods**

> canonical
> :   depthai.TensorInfo
>
> Descriptor of the input/output layers/tensors of the network.
>
> When network is loaded the tensor info is automatically printed.
>
> **Attributes**
>
> > type
> > :   str
> >
> > Name of the tensor.
>
> > type
> > :   list
> >
> > Shape of tensor array. E.g. : [1, 1, 100, 7]
>
> > type
> > :   list
> >
> > Strides of tensor array.
>
> > type
> > :   string
> >
> > Data type of tensor. E.g. : float16
>
> > type
> > :   int
> >
> > Offset in the raw output array.
>
> > type
> > :   int
> >
> > Size in bytes of one element in the array.
>
> > type
> > :   int
> >
> > Index of the tensor. E.g. : in case of multiple inputs/outputs in
> > the network it marks the order of input/output.
>
> **Methods**

> canonical
> :   depthai.Detections
>
> Container of neural network results decoded on device side.
>
> **Example of accessing detections**
>
> Assuming the detected objects are stored in detections object.
>
> -   Number of detections
>
>     ``` {.sourceCode .}
>     detections.size()
>     # or
>     len(detections)
>     ```
>
> -   Accessing the nth detection
>
>     ``` {.sourceCode .}
>     detections[0]
>     detections[1]  # ...
>     ```
>
> -   Iterating through all detections
>
>     ``` {.sourceCode .}
>     for detection in detections:
>     ```
>
> canonical
> :   depthai.Detection
>
> Detected object descriptor.
>
> **Attributes**
>
> > type
> > :   int
> >
> > Label id of the detected object.
>
> > type
> > :   float
> >
> > Confidence score of the detected object in interval [0, 1].
>
> > type
> > :   float
> >
> > Top left X coordinate of the detected bounding box. Normalized, in
> > interval [0, 1].
>
> > type
> > :   float
> >
> > Top left Y coordinate of the detected bounding box. Normalized, in
> > interval [0, 1].
>
> > type
> > :   float
> >
> > Bottom right X coordinate of the detected bounding box. Normalized,
> > in interval [0, 1].
>
> > type
> > :   float
> >
> > Bottom right Y coordinate of the detected bounding box. Normalized,
> > in interval [0, 1].
>
> > type
> > :   float
> >
> > Distance to detected bounding box on X axis. Only when depth
> > calculation is enabled (stereo cameras are present on board).
>
> > type
> > :   float
> >
> > Distance to detected bounding box on Y axis. Only when depth
> > calculation is enabled (stereo cameras are present on board).
>
> > type
> > :   float
> >
> > Distance to detected bounding box on Z axis. Only when depth
> > calculation is enabled (stereo cameras are present on board).
>
> **Methods**

> canonical
> :   depthai.TensorInfo.Dimension
>
> Dimension descriptor of tensor shape. Mostly meaningful for input
> tensors since not all neural network models respect the semantics of
> Dimension for output tensor
>
> **Values**
>
> > type
> > :   str
> >
> > Width
>
> > type
> > :   str
> >
> > Height
>
> > type
> > :   str
> >
> > Number of channels
>
> > type
> > :   str
> >
> > Number of inferences
>
> > type
> > :   str
> >
> > Batch of inferences

> canonical
> :   depthai.DataPacket
>
> DepthAI data packet, containing information generated on the device.
> Unlike NNetPacket, it contains a single "result" with source stream
> info
>
> **Attributes**
>
> > type
> > :   str
> >
> > Returns packet source stream. Used to determine the origin of the
> > packet and therefore allows to handle the packets correctly,
> > applying proper handling based on this value
>
> **Methods**

> canonical
> :   depthai.FrameMetadata
>
> Metadata object attached to the packets sent via pipeline.
>
> **Methods**

> canonical
> :   depthai.ObjectTracker
>
> Object representing current state of the tracker, obtained by calling
> DataPacket.getObjectTracker method on a packet from object\_tracker
> stream
>
> **Methods**

> canonical
> :   depthai.Tracklet
>
> Tracklet is representing a single tracked object, is produced by
> ObjectTracker class. To obtain it, call ObjectTracker.getTracklet
> method.
>
> **Methods**

	function:: getId() -> int

>    Return the tracklet id

	function:: getLabel() -> int

>    Return the tracklet label, being the neural network returned result. Used to identify a class of recognized objects

	function:: getStatus() -> str

>    Return the tracklet status - either :code:`NEW`, :code:`TRACKED`, or :code:`LOST`.

	function:: getLeftCoord() -> int

>    Return the left coordinate of the bounding box of a tracked object

    function:: getRightCoord() -> int

>    Return the right coordinate of the bounding box of a tracked object

	function:: getTopCoord() -> int

>    Return the top coordinate of the bounding box of a tracked object

	function:: getBottomCoord() -> int

>    Return the bottom coordinate of the bounding box of a tracked object
