---
layout: default
title: 示例 - 自拍器 - 看见包含你面部的帧，按空格键保存。
toc_title: 彩色相机自拍器
description: 这个示例演示了如何根据神经网络的输出结果裁切画面并保存到硬盘。
order: 2
---

# {{ page.title }}

这个例子需要[TK库](https://docs.oracle.com/cd/E88353_01/html/E37842/libtk-3.html)来运行(用于打开文件对话框)

还需要人脸检测模型，请查看[这个教程](/tutorials/converting_openvino_model)，学习如何编译一个。

## 演示

__捕获过程__

<video muted autoplay controls>
    <source src="/images/samples/face_rgb.mp4" type="video/mp4">
</video>

__捕获图片__

![captured](/images/samples/face_rgb_selfie.png)

## 源代码

```python
import cv2
import depthai
import consts.resource_paths

if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")

pipeline = depthai.create_pipeline(config={
    'streams': ['previewout', 'metaout'],
    'ai': {
        "blob_file": "/path/to/face-detection-retail-0004.blob",
        "blob_file_config": "/path/to/face-detection-retail-0004.json",
    }
})

if pipeline is None:
    raise RuntimeError('Pipeline creation failed!')

entries_prev = []
face_frame = None

while True:
    nnet_packets, data_packets = pipeline.get_available_nnet_and_data_packets()

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

            for i, e in enumerate(entries_prev):
                left = int(e['left'] * img_w)
                top = int(e['top'] * img_h)
                right = int(e['right'] * img_w)
                bottom = int(e['bottom'] * img_h)

                face_frame = frame[top:bottom, left:right]
                if face_frame.size == 0:
                    continue
                cv2.imshow('face', face_frame)

    key = cv2.waitKey(1)
    if key == ord('q'):
        break
    if key == ord(' ') and face_frame is not None:
        from tkinter import Tk, messagebox
        from tkinter.filedialog import asksaveasfilename
        Tk().withdraw()
        filepath = asksaveasfilename(defaultextension=".png", filetypes=(("Image files", "*.png"),("All Files", "*.*")))
        cv2.imwrite(filepath, face_frame)
        messagebox.showinfo("Success", "Image saved successfully!")
        Tk().destroy()

del pipeline
```

## 解释
<div class="alert alert-primary" role="alert">
<i class="material-icons info">
contact_support
</i>
  <strong>刚开始使用DepthAI?</strong><br/>
  <span class="small">DepthAI的基础知识在[DepthAI工作的最短代码](/examples/minimal_working_example/#explanation)和[OAK套件的Hello World](/tutorials/hello_world/)中有解释。</span>
</div>

我们的网络会返回它检测到的人脸的边界框（我们将它们存储在 `entries_prev` 数组中）。所以在这个示例中，我们要做两件主要的事情：__裁剪画面__，使其只包含人脸，并将其 __保存__ 到用户指定的位置。

### 进行裁切
__裁剪画面__ 需要我们修改[最短工作代码示例](/examples/minimal_working_example/)，这样我们就不会产生矩形的两个点，而是需要全部四个点：其中两个点决定裁剪的开始（`top`开始Y轴裁剪，`left`开始X轴裁剪），另外两个点作为裁剪的结束（`bottom`结束Y轴裁剪，`right`结束X轴裁剪）。

```python
                left = int(e['left'] * img_w)
                top = int(e['top'] * img_h)
                right = int(e['right'] * img_w)
                bottom = int(e['bottom'] * img_h)
```

现在，由于我们的帧是`HWC`格式（高度、宽度、通道），我们首先裁剪Y轴（高度），然后裁剪X轴（宽度）。所以裁剪代码是这样的。

```python
                face_frame = frame[top:bottom, left:right]
```

现在，还有一件事要做。因为有时网络可能会产生这样的边界框，当被裁剪后会产生一个空框，我们必须保证自己不受这种情况的影响，因为如果在空框的情况下调用`cv2.imshow`会抛出一个错误。

```python
                if face_frame.size == 0:
                    continue
                cv2.imshow('face', face_frame)
```

### 保存帧

为了 __保存图片__，我们将只使用一行代码，调用`cv2.imwrite`。其余的代码，利用tkinter包，是可选的，如果你不需要用户交互来保存帧，可以删除。

在这个例子中，我们使用`tkinter`来保存两个对话框。

- 获取目标文件路径(存储为`filepath`),允许我们调用`cv2.imwrite`,因为它需要路径作为第一个参数。
- 确认文件已成功保存

```python
    key = cv2.waitKey(1)
    if key == ord('q'):
        break
    if key == ord(' ') and face_frame is not None:
        from tkinter import Tk, messagebox
        from tkinter.filedialog import asksaveasfilename
        Tk().withdraw()  # do not open root TK window
        filepath = asksaveasfilename(defaultextension=".png", filetypes=(("Image files", "*.png"),("All Files", "*.*")))
        cv2.imwrite(filepath, face_frame)  # save the image to user-specified path
        messagebox.showinfo("Success", "Image saved successfully!")  # show confirmation dialog
        Tk().destroy()  # destroy confirmation dialog
```
