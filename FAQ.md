---
layout: default
title: 常见问题解答
toc_title: 常见问题解答
description: 初次学习或使用DepthAI/OAK时，常见的问题。
order: 6
---

# DepthAI常见问题解答

## 这个技术会不会被美国封杀啊

不会，Myriad X是在美国销售许可列表中，因为他的性能足够的低（算力只有4TOPS）所以美国政府没有必要去干预，美国政府只会去禁售100TOPS以上算力的芯片。

 

## opencv开发者要花钱用吗？

开发者需要购买任意一个套件才能将OpenCV的算法部署到硬件上，但我们会将价格控制到尽可能低。例如微软的Azure Kinect DK售价在400美金，而且只能做深度计算。

而我们的硬件可以做深度计算，人工智能，视频编码，无损变焦，特征跟踪以及其他海量的应用，OAK-D在KickStarter众筹期间特价149美金，零售价格299美金。

 

## OAK这个硬件，目前市面有卖吗？

是的，现在就可以购买，OAK-1建议零售价是199美金，OAK-D是299美金。

 

## 和openmv有啥区别？

主要在速度和深度计算性能上有本质性区别。OpenMV只有大概0.05TOPS的算力，而OAK具备4TOPS。对于计算机视觉和人工智能计算性能来说比OpenMV快80倍。

 

## 如何训练自定义模型？

我们有许多模型的范例教您如何进行自定义模型训练，请参照[自定义训练](/training/)部分

 

## 树莓派，你们知道哪里有卖的吗？

可以去树莓派授权的官方代理渠道购买。

 

## 这个开发板是cpu是啥？ARM，还是Itel？

开发板是基于Intel Movidius Myriad X系列的VPU（Vision Processing Unit）。

 

## 中国国内在哪买？

南京派驰电子科技有限公司是我们授权的中国代理商，联系电话025-84276229。

 

## 100多刀，上生产环境太贵了吧？还不如Jetson nano呢，对比一下有什么优势吗？

Jetson Nano开发套件并没有包含自带的相机模组。而且我们配套的是1200万像素60帧高性能相机模组，单独零售价高达200美金。

另外OAK-D套件中包含了3个相机模组，除了1200万的主摄像头外，还有两颗100万像素工业全局快门相机。

 

## ARM还是x86?

开发板是基于Intel Movidius Myriad X系列的VPU（Vision Processing Unit）。它可以用于基于ARM，Intel/AMD处理器上运行OpenCV环境的任何操作系统。

 

## 硬件开源吗？

是的，扩展板的硬件是全部开源的，核心板（SOM）需要从代理商购买，因为这个是基于Intel NDA协议下的硬件设计。具体请参考：https://github.com/luxonis/depthai-hardware

 

## 开发板和opencv的关系？

我们是合作关系，因此我们将这个平台打造成一个开源平台，得到OpenCV的CEO官方支持，并且已经合作很长时间了。

 

## 购买渠道？

我们正在和南京派驰电子科技合作在中国地区销售，并做好中国地区的技术支持。预售联系电话025-84276225。

 

## Nvidia 是不是也可以用？

是的，可以用于英伟达TX1，TX2，Jetson Nano，Xavier NX系列的平台上。

 

## Intel 开发这个也是用在hybrid mode吗？

是的

 

## 这个可以运行C/C++吗？还是只是支持python？

是的，我们开发的API除了支持Python外还可以支持C++。后期会支持microPython，并在芯片上直接运行microPython脚本。

 

## 中国区能用吗？

是的。

  

## OAK的具体功能是什么？有没有更多资料？

基于神经网络的推理功能，例如目标检测，图像ROI内目标分类。

3维物体定位，既支持单目目标检测+双目景深测量，也支持双目神经网络推理算法

目标跟踪

立体视觉

H.264/H.265编码

数字变焦/电子云台控制（利用1200万像素实现）

背景去除

特征跟踪

运动估计

任意分辨率裁剪，缩放，整合以及ROI处理

 

## 能不能开发票？

可以

 

## 有计划rust语言版吗？

是的， 我们社区里面已经有人在写这个支持包了，并且支持ROS。

 

## 硬件的Schemtatic 会提供吗

是的，请参考：https://github.com/luxonis/depthai-hardware

 

## 开源软件在github获取？

是的，请参考https://github.com/luxonis/depthai，https://docs.luxonis.com/faq/#githubs

 

## 在Google什么平台训练？中国区可以用吗？

任何CPU的训练平台都可以，例如微软的Azure，AWS，Google Colab或者本地计算机。

 

## 所以一般Ubuntu 即可吗？

是的，Ubuntu 16，18，20及其他Linux发行版都可以。并且也支持Mac OS X和Windows操作系统。

 

## 期间用的还是一些opencv的库，还是你们编写了一些库来帮助我们编写自己的训练模型？

我们编写了OpenCV的插件库来支持训练自己的模型，并且已经开源。

 

## 还是在intel 的Openvino平台对吧？主要用他们的推理框架吗？

是的，Openvino是用来优化训练好的模型，提高运行速度。

 

## 挺不错的 前期要先安装哪些套件 只要open champion 即可？才可以跑呢？

依赖不算多，基本和OpenCV一样的安装依赖。

 

## 中国区有微信群或者是QQ群来沟通？

我们正在创建QQ技术交流群。

 

## 支持支付宝和微信支付吗？

可以的。

 

## Nvidia card 和你们有什么差异吗？

Jetson Nano 算力是0.472TOPS，我们的算力是4TOPS，因此比Jetson Nano快了8倍。并且集成了1200万高清相机模块，而且超低功耗。
