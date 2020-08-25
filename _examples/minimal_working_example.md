---
layout: default
title: 示例 - 访问 DepthAI 相机所需的最少代码
toc_title: DepthAI工作的最短代码
description: 在60行代码以内把DepthAI的视频串流到你的显示器上。
order: 1
---

# {{ page.title }}

## 演示

<video muted autoplay controls>
    <source src="/images/samples/minimal.mp4" type="video/mp4">
</video>

## 源代码

```python
import consts.resource_paths
import cv2
import depthai

if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")

p = depthai.create_pipeline(config={
    "streams": ["metaout", "previewout"],
    "ai": {
        "blob_file": consts.resource_paths.blob_fpath,
        "blob_file_config": consts.resource_paths.blob_config_fpath
    }
})

if p is None:
    raise RuntimeError("Error initializing pipelne")

entries_prev = []

while True:
    nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

    for nnet_packet in nnet_packets:
        entries_prev = []
        for e in nnet_packet.entries():
            if e[0]['id'] == -1.0 or e[0]['confidence'] == 0.0:
                break
            if e[0]['confidence'] > 0.5:
                entries_prev.append(e[0])

    for packet in data_packets:
        if packet.stream_name == 'previewout':
            data = packet.getData()
            data0 = data[0, :, :]
            data1 = data[1, :, :]
            data2 = data[2, :, :]
            frame = cv2.merge([data0, data1, data2])

            img_h = frame.shape[0]
            img_w = frame.shape[1]

            for e in entries_prev:
                pt1 = int(e['left'] * img_w), int(e['top'] * img_h)
                pt2 = int(e['right'] * img_w), int(e['bottom'] * img_h)

                cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

            cv2.imshow('previewout', frame)

    if cv2.waitKey(1) == ord('q'):
        break

del p
depthai.deinit_device()
```

## 解释

该代码分为三个阶段。__初始化__、 __处理结果__ 和 __去初始化__ 。

这里要做的是 __初始化__ ，因为它要初始化设备，并确保创建了管道

```python
if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")

p = depthai.create_pipeline(config={
    "streams": ["metaout", "previewout"],
    "ai": {
        "blob_file": "/path/to/model.blob",
        "blob_file_config": "/path/to/config.json"
    }
})

if p is None:
    raise RuntimeError("Error initializing pipelne")
```

__去初始化__ 基本上只有两行代码，虽然不是必要的，但肯定推荐使用

```python
del p
depthai.deinit_device()
```

现在，结果处理包括两个阶段--解析nnet结果和显示帧。

### 解析神经网络结果
下面，你将看到从神经网络解析结果的部分。

```python
entries_prev = []

while True:
    nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

    for nnet_packet in nnet_packets:
        entries_prev = []
        for e in nnet_packet.entries():
            if e[0]['id'] == -1.0 or e[0]['confidence'] == 0.0:
                break
            if e[0]['confidence'] > 0.5:
                entries_prev.append(e[0])
```
这里，我们使用了一个小技巧。注意，`entries_prev`只填充了正确的条目。
并且只有当我们收到一个新的nnet包时才会被重置（设置为`[]`），因为它是`nnet_packets`的for循环中的第一条指令。

这样一来，如果神经网络没有新的结果，我们就会保留旧的结果，因此边界框就不会闪动。

另外，采用这种方法，我们可以确保神经网络的输出只显示一个（如果`nnet_packets`包含两个的话）。

条件 `e[0]['id'] == -1.0 or e[0]['confidence'] == 0.0` 将可能产生的背景噪声进行了限定。

条件 `e[0]['confidence'] > 0.5` 是我们的置信度阈值。您可以根据您的需求和使用情况修改`0.5`。

这个处理步骤的结果是填充（或空）数组`entries_prev`。

### 显示图像帧

```python
for packet in data_packets:
    if packet.stream_name == 'previewout':
        data = packet.getData()
        data0 = data[0, :, :]
        data1 = data[1, :, :]
        data2 = data[2, :, :]
        frame = cv2.merge([data0, data1, data2])

        img_h = frame.shape[0]
        img_w = frame.shape[1]

        for e in entries_prev:
            pt1 = int(e['left'] * img_w), int(e['top'] * img_h)
            pt2 = int(e['right'] * img_w), int(e['bottom'] * img_h)

            cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

        cv2.imshow('previewout', frame)

if cv2.waitKey(1) == ord('q'):
    break
```

这个阶段也分为三个阶段--准备帧、增强帧和添加控制信号。

__Preparing the frame__ 表示我们要将帧转换为OpenCV可用的形式。

首先，我们需要确保我们操作的数据包来自`previewout`流，所以它是4K彩色相机的一帧。

接下来，我们从数据包中获取数据，并将其从DepthAI使用的`CHW`（Channel，Height，Width）形式转换为OpenCV使用的`HWC`（Height，Width，Channel）形式。

```python
for packet in data_packets:
    if packet.stream_name == 'previewout':
        data = packet.getData()  # e.x. shape (3, 300, 300)
        data0 = data[0, :, :]
        data1 = data[1, :, :]
        data2 = data[2, :, :]
        frame = cv2.merge([data0, data1, data2])  # e.x. shape (300, 300, 3)
```

__增强帧__ 是指改变正在显示的内容的任何过程。在这个例子中，我在检测到的项目周围添加了红色的矩形。你也可以在这里添加文本显示、延迟信息--基本上是你的所有的 
业务逻辑的要求。

由于边界框的位置是从神经网络返回的范围`(0，1)`中的浮点数，它指定了点相对于它的宽度/高度的位置，所以我们需要将它转化为图像上的实际点（你可以看到我们在做e.x.`int(e['left'] * img_w)`）。

接下来，使用`cv2.rectangle`，我们在`frame`上打印实际的矩形。最后，当框架准备好后，我们使用`cv2.imshow`函数显示它。

```python
img_h = frame.shape[0]
img_w = frame.shape[1]

for e in entries_prev:
    pt1 = int(e['left'] * img_w), int(e['top'] * img_h)
    pt2 = int(e['right'] * img_w), int(e['bottom'] * img_h)

    cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

cv2.imshow('previewout', frame)
```

__添加控制信号__ 是最后一部分，在这里你可以给显示的图像添加交互性。
我们只增加一个命令--当你按下`q`按钮时终止程序。

```python
if cv2.waitKey(1) == ord('q'):
    break
```

---

您有什么问题或者建议吗？请随时[联系并告诉我们！](/support)
