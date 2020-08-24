---
layout: default
title: 教程 - 如何在DepthAI上使用预先训练好的OpenVINO模型
toc_title: 预先训练好的OpenVINO模型
description: 学习如何用预先训练好的模型实时检测面部（就算用低算力的树莓派也能做到）。
og_image_path: "/images/tutorials/pretrained_model/previewout.png"
order: 2
---

# {{ page.title }}

在本教程中，你将学习如何用预先训练好的模型实时检测面部，就算用低算力的树莓派也能做到。我们将向您介绍OpenVINO模型库，以及如何从该库中运行模型。  

![模型图片](/images/tutorials/pretrained_model/previewout2.png)

还没听说过OpenVINO或者Open Model Zoo? 我先简单介绍一下为什么我们需要这些工具。

## 什么是OpenVINO?

核心部分，DepthAI使用英特尔MyriadX芯片来进行高速模型推理。然而，要获得高性能，不只是把神经网络塞到芯片中这么简单。这个时候，我们就需要[OpenVINO]（https://docs.openvinotoolkit.org/）了。OpenVINO是一个免费的工具包，它能把深度学习模型转换为可以在英特尔硬件上运行的格式。模型转换后，通常能获得25倍甚至更高的每秒帧数（FPS）提升。

## 什么是Open Model Zoo?

[Open Model Zoo](https://github.com/opencv/open_model_zoo)是一个免费的预训练模型库。 附注：在机器学习/AI中，预训练模型集合的名称叫做 "模型Zoo"。Zoo还包含将这些模型下载成可编译格式的脚本，以便在DepthAI上运行。

DepthAI能够运行许多Zoo中的对象检测模型，其中有几个模型已经预置在DepthAI的Github代码库中。在本教程中我们将使用的模型，是face-detection-retail-0004（该模型已在我们的Github上预编译[这里]（https://github.com/luxonis/depthai/tree/master/resources/nn/face-detection-retail-0004），在OpenVINO模型zoo上预编译[这里]（https://docs.openvinotoolkit.org/2020.1/_models_intel_face_detection_retail_0004_description_face_detection_retail_0004.html））。

我们将在后续的文档中介绍如何将OpenVINO模型转换为在DepthAI上运行。 现在，你可以在[这里](https://github.com/luxonis/depthai/tree/master/resources/nn)找到我们已经预转换的模型，以及关于如何转换的简要说明[这里](https://github.com/luxonis/depthai-python-extras#conversion-of-existing-trained-models-into-intel-movidius-binary-format)。

## 依赖

<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
  正在使用树莓派计算模组（Compute Module）或者已经烧录好的DepthAI树莓派SD卡？<strong>请跳过这一步.</strong><br/>
  <span class="small">所有的依赖都已经安装好了，代码库保存到了 `~/Desktop/depthai-python-extras`.</span>
</div>

本教程与[Hello World教程](/tutorials/hello_world#dependencies)具有相同的依赖--DepthAI API已经安装，并且可以在系统中访问。 如果你还没有安装API，请参见[这里](https://docs.luxonis.com/api/)

## 运行DepthAI默认模型

你可以直接修改`depthai.py`文件来进行binding，或者你可以简单地传递参数给它，让它运行你想运行的模型。 

为了简单起见，我们选择后者，简单地传递参数，让DepthAI运行`face-detection-retail-0004`而不是默认运行的模型。

在切换到使用`face-detection-retail-0004`之前，还有一小步。 在这里，我们将只传递在运行`python3 test.py`时默认运行的神经网络，从而确保我们操作无误。

```
python3 test.py -dd
```
然后运行一个典型的演示MobileNetv1 SSD物体检测器，该检测器基于[PASCAL 2007 VOC](http://host.robots.ox.ac.uk/pascal/VOC/voc2007/)的类进行训练，这些类包括
* 人：人
* 动物：鸟、猫、牛、狗、马、羊。
*交通工具：飞机、自行车、船、公共汽车、汽车、摩托车、火车。
* 室内：酒瓶、椅子、餐桌、盆栽、沙发、电视/显示器。

我在我的iMac上运行了这个程序（[在这里查看](https://docs.luxonis.com/api/#mac-os-x)OS X设置），一个[microAI](https://shop.luxonis.com/products/bw1093)放在我的办公桌上，随意地向上指着--它能看出我的iMac的一角（几乎看不到），并正确地将其识别为 `电视/显示器`。

![iMac](/images/tutorials/pretrained_model/tvmonitor.png)

## 运行 face-detection-retail-0004

在验证完之后，让我们继续尝试其他模型，首先是`face-detection-retail-0004`。

要使用这个模型，只需用`-cnn`标志指定要运行的模型名称，如下所示。
```
python3 test.py -dd -cnn face-detection-retail-0004。
```
执行该脚本以查看人脸检测的带注释的视频流。

![模型图像](/images/tutorials/pretrained_model/pfs.png)

就这么简单。 当然，你要换成另外一张脸。

如果你想尝试其他模型，只需浏览[这里](https://github.com/luxonis/depthai/tree/master/resources/nn)，然后按他们的名字运行，就像上面一样。

现在花点时间来玩玩这个模型。 例如，你可以检查模型能检测到你的脸有多远。
![model_image](/images/tutorials/pretrained_model/pfm.png)
![model_image](/images/tutorials/pretrained_model/pfl.png)


在第二张图片中，你可以看到我在一个很背光的环境下，这是人脸检测（和其他特征检测）的主要挑战之一。这种背光的情况很可能限制了人脸检测的最大范围。 从上面的测试来看，对于50%的置信度阈值，这个范围似乎是6米/20英尺左右。 你可以通过降低模型的置信度阈值（从 `0.5`[这里]降低(https://github.com/luxonis/depthai/blob/cdb902179590f0e7b684dde994369e137794a2ef/depthai.py#L233)），以增加假阳性概率为代价，从相同的模型中获得更长的范围。

另一个限制因素是，这是一个相对低分辨率的模型（300x300像素），所以远处的人脸就特别小。 因此，让我们尝试另一种使用更高分辨率的人脸检测模型。 

## 尝试其他模型

我们走过的流程也适用于我们代码库中([这部分](https://github.com/luxonis/depthai-python-extras/tree/master/resources/nn)的其他预训练对象检测模型)，其中包括：
 - 零售业的人脸检测  (`face-detection-retail-0004`)
 - 用于驾驶辅助的面部检测 (`face-detection-adas-0001`)
 - 面部特征点检测, 简单 (`landmarks-regression-retail-0009`)
 - 面部特征点检测，高级 (`facial-landmarks-35-adas-0002`)
 - 情绪识别 (`emotions-recognition-retail-0003`)
 - 用于驾驶辅助的行人检测 (`pedestrian-detection-adas-0002`)
 - 零售环境下的人员检测 (`person-detection-retail-0013`)
 - 用于驾驶辅助的车辆检测 (`vehicle-detection-adas-0002`)
 - 车辆和车牌检测 (`vehicle-license-plate-detection-barrier-0106`)

只需改变上面的路径，就可以运行其他模型。

我们可以试试`face-detection-adas-0001`，它的目的是检测车辆驾驶室内的人脸。(ADAS是Advanced Driver-Assistance Systems的缩写)

```
python3 test.py -dd -cnn face-detection-adas-0001
```

![model_image](/images/tutorials/pretrained_model/adas3.png)

所以这个模型虽然分辨率较高，但实际上探测距离比小模型短。 为什么这么说呢？ 很可能是因为它被有意训练成只检测近距离的人脸，因为它的目的是在车辆的驾驶室中使用。 (例如，你不会想检测路过的汽车中的人脸)。

此外，你可能会注意到像情感识别这样的网络实际上是为了作为第二阶段网络运行的（因为它们只应用于只包含人脸的图像）。 所以要使用情感识别网络，请使用下面的命令告诉DepthAI/megaAI将其作为第二阶段运行。

```
./depthai.py -cnn face-detection-retail-0004 -cnn2 emotions-recognition-retail-0003 -dd -sh 12 -cmx 12 -nce 2
````
![Multi-stage inference](https://i.imgur.com/uqhdqJG.png)

我们一直在运行的"-dd "选项是什么？ 为什么会有这个选项呢？ 

因为我们想把最好的东西留到最后。 它代表的是禁用深度（并且有长表选项`--disable_depth`）。 所以，如果你把它去掉，DepthAI现在会计算被检测物体的3D位置（在这个例子中是一张脸，但它适用于任何物体检测器）（如果你使用的是microAI，就不用管它，因为microAI只是单目相机，没有深度信息。

所以你会得到**检测对象的**全3D位置**，在本例中，我的脸。 

这样就返回了以米为单位的完整xyz位置。 请看下面的内容。

## 空间AI--用3D定位增强模型的功能

所以默认情况下，DepthAI被设置为返回完整的3D位置，所以在上面的命令中，我们实际上是用`-dd`（或`--disable_depth`）指定它不计算。 因此，在上面的命令中，我们实际上指定了不计算`-dd`（或`--disable_depth`）。

因此，让我们运行同样的命令，但省略这一行，这样3D结果就会返回（并显示）。

```
python3 test.py -cnn face-detection-adas-0001
```

![model_image](/images/tutorials/pretrained_model/fdwd.png)

在这里你可以找到我的杯子的3D位置！

你可以选择其他模型，改变标签，然后完成了--获得你关心的类的实时3D位置。

### 单目神经推断与双目深度的融合。
我们将这种空间 AI 模式称为「单目神经推断与双目深度的融合」。 把神经推理的边界框直接叠加在深度结果上，可以直观地看到这种模式是如何工作的。

为了可视化，让我们将结果直接叠加到原始深度信息上（在OpenCV HOT colormap中可视化）：

```
python3 test.py -s metaout depth_raw -bb

```

![AI overlaid on the RAW (uint16) Depth Map](https://i.imgur.com/AjH1T2l.jpg)

所以这种'单目神经推断与双目深度的融合'的技术对于物体，尤其是较大的物体（如人、人脸等）很有效。 

### 立体神经推理

下面我们将使用另一种技术，我们将其称为 "立体神经推理"（或 "立体AI"），这种技术对较小的物体，以及像面部特征点和姿态评估器结果等像素点特征都很有效。

![Stereo Neural inference mode](https://i.imgur.com/mKuzWI6.png)

使用下面的命令:
```
./depthai.py -cnn face-detection-retail-0004 -cnn2 landmarks-regression-retail-0009 -cam left_right -dd -sh 12 -cmx 12 -nce 2 -monor 400 -monof 30
```
需要注意的是，这既是在运行并行神经推理（即在两个摄像头上），也是在运行系列神经推理（特征点回归网络是在人脸检测器的结果上运行）。

