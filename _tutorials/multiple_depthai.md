---
layout: default
title: 教程 - 在一个主机上使用多个DepthAI
toc_title: 在一个主机上使用多个DepthAI
description: 在一个主机上使用多个DepthAI
order: 3
---

# {{ page.title }}

学习如何使用DepthAI`-dev`选项来发现连接到你系统的DepthAI，并单独使用它们。

![multiple_depthai](/images/tutorials/multiple_depthai/IMG_7721.jpg)

画面左侧的是Luxonis uAI(BW1093，它实际上是插在DepthAI 树莓派计算模组版本(BW1097)上。

所以在这种情况下，所有的东西都在BW1097后面的（单个）Raspberry Pi 3B+上运行。

## 依赖

如果你有树莓派计算机模组版本，python API已经设置好了。  
如果您还没有在系统上安装DepthAI Python API，请参见[这里](/api)。

## 从Github上升级到最新版本
`git pull https://github.com/luxonis/depthai-python-extras.git`

## 发现DepthAI-USB口映射
目前，DepthAI多设备支持是通过选择DepthAI接入的USB端口来完成的。

如果你想要把某个DepthAI设备和运行的特定代码（如神经模型)相关联, 建议一次插入一个设备，然后使用以下命令确定哪个设备在哪个端口上。
`python3 test.py -dev list`

一个系统接两个DepthAI的结果示例
```...
XLink initialized.
Detected 2 device(s):
  2-ma2480     on USB port: 1
  1.1-ma2480   on USB port: 2.1
```
## 选择要使用的特定DepthAI设备

选择要使用的特定DepthAI设备。

从上面检测到的设备中，使用下面的命令选择您想在代码中使用的设备。
例如，你想使用上面的第一个设备（USB端口1上的设备），使用以下命令。
`python3 test.py -dev 1`。

同样地，如果要在另一个设备上再次运行同样的脚本，可以用以下方法运行它
`python3 test.py -dev 2. 1`

值得注意的是，test.py实际上是depthai.py的一个封装器。 它有一个监视功能，可以捕获任何的库问题或其他可能出现的异常。 它调用depthai.py。

你也可以不使用这个监视功能，直接使用 depthai.py 运行，如下所示。
`python3 depthai.py -dev 1`
`python3 depthai.py -dev 2.1`

你可以使用这些脚本作为你自己修改版本的基础，这样你就可以运行不同的神经模型了 
在不同的DepthAI/uAI模型上。 

## 主机负担的总结和概述
现在，您可以根据需要使用任意数量的DepthAI设备。 

由于DepthAI完成了所有繁重的工作，因此您通常可以使用非常多的DepthAI设备而对主机造成非常小的负担。 

而且值得注意的是，你可以随时只请求 `metaout`[详见这里](https://github.com/luxonis/depthai-python-extras/blob/232d1e7529e0278b75192d0870a969b6c0e2d1ae/depthai.py#L104)来禁用视频流。

所以如果说你用元数据来驱动机器人或者用代码做决策，而不需要视频，你可以这样做来大大减轻主机的负担--因为所有的神经推理工作都是在主机之前的DepthAI上完成的--几乎所有的主机负担都只是来自于显示视频。

所以在禁用视频的情况下，主机每秒只需要处理几千字节的元数据。
