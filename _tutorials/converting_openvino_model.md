---
layout: default
title: 教程 - 如何将OpenVINO 中间表示文件（IR）转换为在DepthAI上本地运行所需的格式
toc_title: 本地 OpenVINO 模型转换
description: 学习如何将OpenVINO 中间表示文件（IR）转换为在DepthAI上运行所需的格式
og_image_path: "/images/tutorials/pretrained_model/previewout.png"
order: 6
---

# {{ page.title }}

在本教程中，您将学习如何将OpenVINO 中间表示文件（IR）转换为在DepthAI上运行所需的格式，即使是在低功耗的Raspberry Pi上也能使用。我将向您介绍OpenVINO工具集、Open Model Zoo（我们将在那里下载[face-detection-retail-0004](https://github.com/opencv/open_model_zoo/blob/2019_R3/models/intel/face-detection-retail-0004/description/face-detection-retail-0004.md)模型），并向您展示如何生成在DepthAI上运行模型推理所需的文件。

![model image](/images/tutorials/pretrained_model/previewout2.png)

还没听说过OpenVINO或者Open Model Zoo? 我先简单介绍一下为什么我们需要这些工具。

## 什么是OpenVINO?

核心部分，DepthAI使用英特尔MyriadX芯片来进行高速模型推理。然而，要获得高性能，不只是把神经网络塞到芯片中这么简单。这个时候，我们就需要[OpenVINO](https://docs.openvinotoolkit.org/)了。OpenVINO是一个免费的工具包，它能把深度学习模型转换为可以在英特尔硬件上运行的格式。模型转换后，通常能获得25倍甚至更高的每秒帧数（FPS）提升。

## 什么是Open Model Zoo?

[Open Model Zoo](https://github.com/opencv/open_model_zoo)是一个免费的预训练模型库。 附注：在机器学习/AI中，预训练模型集合的名称叫做 "模型Zoo"。Zoo还包含将这些模型下载成可编译格式的脚本，以便在DepthAI上运行。

DepthAI能够运行许多Zoo中的对象检测模型。

## 安装 OpenVINO

<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
  如果您已经安装过了OpenVINO或者想按照[官方教程](https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit.html)操作, <strong>请跳过这个步骤.</strong><br/>
  <span>请注意以下安装说明是针对__Ubuntu 18.04__的, 如果您打算使用其他操作系统，请按照官方的OpenVINO安装说明进行安装。</span>
</div>

DepthAI 需要 OpenVINO `2020.1` 版本来运行. 让我们用下面的命令为我们的操作系统获取一个软件包，并满足这个版本要求。

```
apt-get update
apt-get install -y software-properties-common
add-apt-repository -y ppa:deadsnakes/ppa
apt-get update
apt-get install -y wget pciutils python3.8 libpng-dev libcairo2-dev libpango1.0-dev libglib2.0-dev libgtk2.0-dev libswscale-dev libavcodec-dev libavformat-dev
cd
mkdir openvino_install && cd openvino_install
wget http://registrationcenter-download.intel.com/akdlm/irc_nas/16345/l_openvino_toolkit_p_2020.1.023.tgz
tar --strip-components=1 -zxvf l_openvino_toolkit_p_2020.1.023.tgz
./install_openvino_dependencies.sh
./install.sh # when finished, you can go ahead and do "rm -r ~/openvino_install"
```

现在，我们看到的第一屏是EULA，按`Enter`，滚动后输入`accept`即可。

下一个是同意英特尔软件改进计划，这没有关系，所以你可以选择是否同意（`1`）或不同意（`2`）。

接下来，你可能会看到 "缺失必要文件 "的画面，显示 `Intel® Graphics Compute Runtime for OpenCL™ Driver is missing`--你可以继续并忽略这个警告。

最后，我们会看到安装总结--请确认它指出的位置是否正确--`/opt/intel`。
如果一切看起来都很好，继续进行(`1`)。如果再次出现缺少必要文件的提示，请随意跳过。

让我们验证一下你的主机上是否安装了正确的版本。通过在终端会话中运行以下内容来检查你的版本。

```
cat /opt/intel/openvino/inference_engine/version.txt
```

你可能看到类似如下的输出:

```
Thu Jan 23 19:14:14 MSK 2020
d349c3ba4a2508be72f413fa4dee92cc0e4bc0e1
releases_2020_1_InferenceEngine_37988
```

确认您是否在输出中看到`releases_2020_1`。如果你看到了，继续。如果你使用的是不同的版本，请到[OpenVINO网站](https://docs.openvinotoolkit.org/2019_R3/index.html)下载适合你操作系统的2020.1版本。

![openvino_version](/images/tutorials/converting_openvino_model/openvino_version.png)

{: data-toc-title="模型下载器"}
## 检查模型下载器是否已安装

在安装OpenVINO时，您可以选择执行一个较小的安装以节省磁盘空间。这个自定义安装可能不包括模型下载器脚本。让我们检查下载器是否已经安装。在终端会话中，键入以下内容：

```
find /opt/intel/ -iname downloader.py
```

__如果看到了如下的输出，请继续__:

```
/opt/intel/openvino_2020.1.023/deployment_tools/open_model_zoo/tools/downloader/downloader.py
```

__没有看到任何输出?__ 如果没有找到 "downloader.py "也不要着急。我们将在下面安装这个。

{: data-toc-title="安装"}
### 安装 Open Model Zoo 下载器

如果没有找到下载工具，我们将通过克隆[Open Model Zoo Repo](https://github.com/openvinotoolkit/open_model_zoo/blob/2020.1/tools/downloader/README.md)来安装工具，并安装工具依赖关系。

启动一个终端会话，并在终端中运行以下命令：

```
apt-get install -y git curl
curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
python3 get-pip.py
rm get-pip.py
cd ~
git clone https://github.com/opencv/open_model_zoo.git
cd open_model_zoo
git checkout tags/2020.1
cd tools/downloader
python3 -m pip install --user -r ./requirements.in
```

这部分操作将把代码库克隆到`~/open_model_zoo`目录下，拷贝所需的`2020.1`版本，并安装下载程序的dependencies.zh。

{: data-toc-title="设置下载器环境变量"}
## 创建一个OPEN_MODEL_DOWNLOADER环境变量

输入`downloader.py`的完整路径可能会使用大量的按键。为了延长你的键盘寿命，让我们把这个脚本的路径存储在一个环境变量中。

在你的终端中运行以下内容。

```
export OPEN_MODEL_DOWNLOADER='INSERT PATH TO YOUR downloader.py SCRIPT'
```

其中`INSERT PATH TO YOUR downloader.py SCRIPT`可以通过以下方式找到：

```
find /opt/intel/ -iname downloader.py
find ~ -iname downloader.py
```

例如，如果你自己安装了`open_model_zoo`：

```
export OPEN_MODEL_DOWNLOADER="$HOME/open_model_zoo/tools/downloader/downloader.py"
```

{: data-toc-title="下载模型"}
## 下载face-detection-retail-0004 模型

我们已经安装了从Open Model Zoo下载模型所需的所有东西。 现在我们将使用[模型下载器](https://github.com/opencv/open_model_zoo/blob/2019_R3/tools/downloader/README.md) 下载 `face-detection-retail-0004` 模型文件。 在您的终端运行以下内容：

```
$OPEN_MODEL_DOWNLOADER --name face-detection-retail-0004 --output_dir ~/open_model_zoo_downloads/
```

这将会把模型文件下载到`~/open_model_zoo_downloads/`。具体来说，我们所需要的模型文件位于：

```
~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16
```

你会在目录中看到两个文件:

```
ls -lh 
total 1.3M
-rw-r--r-- 1 root root 1.2M Jul 28 12:40 face-detection-retail-0004.bin
-rw-r--r-- 1 root root 100K Jul 28 12:40 face-detection-retail-0004.xml
```

该模型采用OpenVINO中间表示文件（IR）格式。:

* face-detection-retail-0004.xml - Describes the network topology
* face-detection-retail-0004.bin - Contains the weights and biases binary data.

这意味着我们已经准备好为MyriadX编译模型了!

## 编译模型

我们的DepthAI板上使用的MyriadX芯片不能直接使用IR格式文件。相反，我们需要生成两个文件。

* `face-detection-retail-0004.blob`--我们将用`myriad_compile`命令创建这个文件。
* `face-detection-retail-0004.json` - JSON格式的`blob_file_config`文件。它描述了输出 tensors 的格式。你可以在[这里](/api#creating-blob-configuration-file) 阅读更多关于这个文件结构和例子。

我们将从创建`blob`文件开始。

### 定位 myriad_compile

让我们来寻找 "myriad_compile "的位置。在你的终端，运行:

```
find /opt/intel/ -iname myriad_compile
```

你将看到类似下面的输出：

```
find /opt/intel/ -iname myriad_compile
/opt/intel/openvino_2020.1.023/deployment_tools/inference_engine/lib/intel64/myriad_compile
```

由于路径很长，让我们把`myriad_compile`可执行文件存储在一个环境变量中（就像`OPEN_MODEL_DOWNLOADER`）一样:

```
export MYRIAD_COMPILE=$(find /opt/intel/ -iname myriad_compile)
```

### 激活 OpenVINO 的环境

为了使用`myriad_compile`工具，我们需要激活我们的OpenVINO环境。

首先，让我们找到`setupvars.sh`文件。

```
find /opt/intel/ -name "setupvars.sh"
/opt/intel/openvino_2020.1.023/opencv/setupvars.sh
/opt/intel/openvino_2020.1.023/bin/setupvars.sh
```

我们对 `bin/setupvars.sh`文件感兴趣，所以让我们继续用它来激活环境:

```
source /opt/intel/openvino_2020.1.023/bin/setupvars.sh
[setupvars.sh] OpenVINO environment initialized
```

如果你看到`[setupvars.sh] OpenVINO environment initialized`，那么你的环境应该是正确初始化的。

### 运行 myriad_compile

```
$MYRIAD_COMPILE -m ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.xml -ip U8 -VPU_MYRIAD_PLATFORM VPU_MYRIAD_2480 -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4
```

你将看到:

```
Inference Engine: 
	API version ............ 2.1
	Build .................. 37988
	Description ....... API
Done
```

blob文件在哪里？它和`face-detection-retail-0004.xml`在同一个文件夹里:

```
ls -lh ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/
total 2.6M
-rw-r--r-- 1 root root 1.2M Jul 28 12:40 face-detection-retail-0004.bin
-rw-r--r-- 1 root root 1.3M Jul 28 12:50 face-detection-retail-0004.blob
-rw-r--r-- 1 root root 100K Jul 28 12:40 face-detection-retail-0004.xml
```

{: #blob_file_config}
## 创建 blob 配置文件

MyriadX需要一个`blob`文件（我们刚刚创建了一个）和一个JSON格式的`blob_file_config`。我们将手动创建这个配置文件。在你的终端上请输入:

```
cd ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/
touch face-detection-retail-0004.json
```

把下面的代码复制粘贴到 `face-detection-retail-0004.json`:

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

这些值都代表什么意思呢?
* `output_dimensions`----神经网的[输出格式](https://docs.openvinotoolkit.org/latest/_models_intel_face_detection_retail_0004_description_face_detection_retail_0004.html#outputs)为`[1,1,N,7]`，其中`N`是检测到的边界盒的数量。每次检测的格式为 `["id"、"label"、"confidence"、"left"、"top"、"right"、"bottom"]`。我们指定 "100 "而不是 "N"，以表示我们将处理最多100个边界框，这应该是足够多的可能的面孔来检测。
* `output_entry_iteration_index` - 表示我们将使用数组位置来迭代检测。`N`是在第2个位置（索引开始=0）。
* `property_key_mapping` - 将属性映射为方便使用的名称，我们可以在我们的Python脚本中运行模型推理时访问。
* `output_properties_type` - 我们使用的模型精度为`f16`。

{: data-toc-title="运行模型"}
## 运行模型并显示模型的输出

有了`blob`和`json`blob配置文件，我们就可以开始了! 
为了验证模型是否正确运行，让我们稍微修改一下我们在[Hello World教程](/tutorials/hello_world/)中创建的程序。

具体的说，让我们修改`create_pipeline`调用来加载我们的模型 __记得将路径替换为你所拥有的正确路径！__

```diff
pipeline = depthai.create_pipeline(config={
    'streams': ['previewout', 'metaout'],
    'ai': {
-        'blob_file': consts.resource_paths.blob_fpath,
+        'blob_file': "/root/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.bin",
-        'blob_file_config': consts.resource_paths.blob_config_fpath
+        'blob_file_config': "/root/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.json"
    }
})
```

以上就是全部了。

你应该能看到一个带注释的输出，跟下面这个类似:

![model image](/images/tutorials/pretrained_model/previewout.png)

不过可能不是同一张脸.

## 审查流程

我们走过的流程适用于Open Model Zoo的其他预训练对象检测模型:

1. 下载模型:
    ```
    $OPEN_MODEL_DOWNLOADER --name [INSERT MODEL NAME] --output_dir ~/open_model_zoo_downloads/
    ```
2. 创建MyriadX blob文件:
    ```
    $MYRIAD_COMPILE -m [INSERT PATH TO MODEL XML FILE] -ip U8 -VPU_MYRIAD_PLATFORM VPU_MYRIAD_2480 -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4
    ```
3. 根据模型的输出创建[JSON配置文件](#blob_file_config).
4. 在你的脚本中使用这个模型。

你已经上手了! 你可以在Github上找到这个教程的 [完整代码。](https://github.com/luxonis/depthai-tutorials/blob/master/2-face-detection-retail/face-detection-retail-0004.py)


