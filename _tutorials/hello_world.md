---
layout: default
title: 教程 - OAK/DepthAI Hello World
toc_title: Hello World
description: 把OAK/DepthAI的视频串流到你的显示上。
order: 1
---

# {{ page.title }}

学习如何使用OAK/DepthAI Python API来显示彩色视频流。

## 依赖关系

先把开发环境设置好。本教程使用:

* Python 3.6 (Ubuntu) 或者 Python 3.7 (Raspbian).
* Python DepthAI API. [安装](/api#install) 或者 [升级](/api#upgrade).
* `cv2` Python 模块和numpy.  

本教程同时也使用了几个 `pip` 包. 后面我们就会[安装这些包](#install-pip-dependencies) 。

## 代码概述

`depthai` Python 模块可以访问OAK板子上的4k 60Hz彩色相机模组。我们将把这个相机模组的视频串流到你桌面上。你可以在GitHub上找到[本教程的完整源代码](https://github.com/luxonis/depthai-tutorials/tree/master/1-hello-world)。

## 文件设置

在电脑上设置以下文件结构：

```
cd ~
mkdir -p {{site.tutorials_dir}}/1-hello-world
touch {{site.tutorials_dir}}/1-hello-world/hello-world.py
cd {{site.tutorials_dir}}/1-hello-world
```

父目录名称里面的`-practice`后缀是干什么的呢? 我们的教程可以在[depthai-tutorials](https://github.com/luxonis/depthai-tutorials)代码库中获得。在我们加上`-practice`后，你就可以把你个人的代码和我们完整教程的代码区分开来（前提是你下载了这些教程）。


## 安装 pip 依赖

要显示DepthAI彩色视频流，我们需要导入少量的包。下载并安装本教程所需的包。

```
python3 -m pip install numpy opencv-python --user
```

## 安装 DepthAI 包

虽然后面可能会从PyPi直接安装，但最好还是现在先从源码安装deepai。

请键入以下命令
```
git clone https://github.com/luxonis/depthai.git
cd depthai
python3 -m pip install -r requirements.txt
python3 -m pip install --user -e .
```

## 测试你的环境

让我们验证一下是否能够加载所有的依赖关系。在你的代码编辑器中打开你[之前创建的](#file-setup)的`hello-world.py`文件。复制并粘贴以下内容到 `hello-world.py`中。


```py
import numpy as np # numpy - manipulate the packet data returned by depthai
import cv2 # opencv - display the video stream
import depthai # access the camera and its data packets
import consts.resource_paths # load paths to depthai resources
```

试着运行该脚本，并确保它的执行不出错。

```
python3 hello-world.py
```

如果你看到了如下的错误:

```py
ModuleNotFoundError: No module named `depthai`
```

...请按照 [我们的故障排除部分的这些步骤](/troubleshooting/#depthai_import_error)来操作.

## 初始化 DepthAI 设备

启动DepthAI设备:

```py
if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")
```

如果设备没有初始化，不要等到后续再出错，直接在这一步退出脚本.

子啊运行这个脚本，你应该会看到类似的输出：

```
No calibration file. Using Calibration Defaults.
XLink initialized.
Sending device firmware "cmd_file": /home/pi/Desktop/depthai/depthai.cmd
Successfully connected to device.
Loading config file
Attempting to open stream config_d2h
watchdog started 6000
Successfully opened stream config_d2h with ID #0!
```

但如果你看到的是如下的错误:

```
Traceback (most recent call last):
  File "/home/pi/{{site.tutorials_dir}}/1-hello-world/hello-world.py", line 7, in <module>
    raise RuntimeError("Error initializing device. Try to reset it.")
RuntimeError: Error initializing device. Try to reset it.
```

[重置你的DepthAI设备 ](/troubleshooting#device_reset), 然后再试一次.

## 创建DepthAI管道

现在我们将使用`previewout`流创建我们的数据管道。这个流包含了彩色相机模组的数据。
`ai`部分使用的模型是一个MobileNetSSD，有20个不同的类，详见[这里](https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json)。

```py
# 使用'previewout'流创建管道，建立与设备的第一个连接。
pipeline = depthai.create_pipeline(config={
    'streams': ['previewout', 'metaout'],
    'ai': {
        'blob_file': consts.resource_paths.blob_fpath,
        'blob_file_config': consts.resource_paths.blob_config_fpath
    }
})

if pipeline is None:
    raise RuntimeError('Pipeline creation failed!')
```

## 显示视频流

DepthAI 管道会生成一个数据包流。每个 `previewout `数据包都包含一个代表图像帧的3D数组。我们将图像帧的形状改变为 和`cv2 `兼容的格式，并显示出来。

```py
entries_prev = []

while True:
    # 从设备中检索数据包。
    # 一个数据包包含视频帧数据。
    nnet_packets, data_packets = pipeline.get_available_nnet_and_data_packets()

    for _, nnet_packet in enumerate(nnet_packets):
        entries_prev = []
        for _, e in enumerate(nnet_packet.entries()):
            if e[0]['id'] == -1.0 or e[0]['confidence'] == 0.0:
                break
            if e[0]['confidence'] > 0.5:
                entries_prev.append(e[0])

    for packet in data_packets:
        # 默认情况下，DepthAI会添加其他流（尤其是 "meta_2dh"）。只处理 "previewout"。
        if packet.stream_name == 'previewout':
            data = packet.getData()
            # 预览输出图像的格式是CHW（通道，高度，宽度），但OpenCV需要HWC，所以我们
            # 把 (3, 300, 300) 改成 (300, 300, 3)
            data0 = data[0,:,:]
            data1 = data[1,:,:]
            data2 = data[2,:,:]
            frame = cv2.merge([data0, data1, data2])

            img_h = frame.shape[0]
            img_w = frame.shape[1]

            for e in entries_prev:
                pt1 = int(e['left'] * img_w), int(e['top'] * img_h)
                pt2 = int(e['right'] * img_w), int(e['bottom'] * img_h)

            cv2.imshow('previewout', frame)

    if cv2.waitKey(1) == ord('q'):
        break

# 退出循环后，应删除管道对象。否则设备将继续工作。
# 如果你要在退出循环后添加代码，这是必须执行的移步。
del pipeline
```

运行该脚本。将焦点放在视频流，而不是终端上，按'Q'键以退出。

```
python3 hello-world.py
```

你已经上手了! 你可以在GitHub上找到[本教程的完整代码](https://github.com/luxonis/depthai-tutorials/blob/master/1-hello-world/hello_world.py)。
