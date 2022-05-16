OAK支持的格式与神经网络
==============================

OAK支持的格式
***********************

OAK搭载的是英特尔旗下子公司Movidius推出的Movidius MyRiad X视觉处理器（VPU）， OAK支持MyRiad X所支持的任何格式。

术语介绍
----------------------

.. list-table:: 格式名词表
    :header-rows: 1

    * - 格式名
      - 描述
    * - FP32
      - 单精度浮点格式
    * - FP16
      - 半精度浮点格式
    * - BF16
      - 16位脑浮点格式
    * - I16
      - 2字节有符号整数格式
    * - I8
      - 1字节有符号整数格式
    * - U16
      - 2字节无符号整数格式
    * - U8
      - 1字节无符号整数格式
    * - NHWC，NCHW
      - 图像数据格式
    * - NCDHW
      - 图像序列数据格式
    * - CHW，NC，C
      - 张量存储数据格式

注解：

- N-图像数
- D-深度，取决于模型，它可以是空间或时间维度
- H-高度
- W-宽度
- C-通道数

支持的模型格式
---------------------------

.. list-table:: 模型格式
    :header-rows: 1

    * - 格式
      - FP32
      - FP16
      - I8
    * - 是否支持
      - 不支持
      - 支持
      - 不支持

注解：

- 模型格式不支持I8，但是计算精度是支持的。

支持的输入精度
--------------------------

.. list-table:: 输入精度
    :header-rows: 1

    * - 精度
      - FP32
      - FP16
      - I8
      - I16
      - U8
      - U16
    * - 是否支持
      - 支持
      - 支持
      - 不支持
      - 不支持
      - 支持
      - 不支持

支持的输出精度
-------------------------

.. list-table:: 输出精度
    :header-rows: 1

    * - 精度
      - FP32
      - FP16
    * - 是否支持
      - 支持
      - 支持

支持的输入数据格式
--------------------------

.. list-table:: 输入数据格式
    :header-rows: 1

    * - 数据格式
      - NCDHW
      - NCHW
      - NHWC
      - NC
    * - 是否支持
      - 不支持
      - 支持
      - 支持
      - 支持

支持的输出数据格式
-----------------------------

.. list-table:: 输出数据格式
    :header-rows: 1

    * - 尺寸数
      - 5
      - 4
      - 3
      - 2
      - 1
    * - 数据格式
      - NCDHW
      - NCHW
      - CHW
      - NC
      - C

OAK支持的网络
******************************

Caffe*
------------------------

- AlexNet
- CaffeNet
- GoogleNet (Inception) v1, v2, v4
- VGG family (VGG16, VGG19)
- SqueezeNet v1.0, v1.1
- ResNet v1 family (18***, 50, 101, 152)
- MobileNet (mobilenet-v1-1.0-224, mobilenet-v2)
- Inception ResNet v2
- DenseNet family (121,161,169,201)
- SSD-300, SSD-512, SSD-MobileNet, SSD-GoogleNet, SSD-SqueezeNet

TensorFlow*
--------------------------

- AlexNet
- Inception v1, v2, v3, v4
- Inception ResNet v2
- MobileNet v1, v2
- ResNet v1 family (50, 101, 152)
- ResNet v2 family (50, 101, 152)
- SqueezeNet v1.0, v1.1
- VGG family (VGG16, VGG19)
- Yolo family (yolo-v2, yolo-v3, tiny-yolo-v1, tiny-yolo-v2, tiny-yolo-v3)
- faster_rcnn_inception_v2, faster_rcnn_resnet101
- ssd_mobilenet_v1
- DeepLab-v3+

MXNet*
----------------------------

- AlexNet and CaffeNet
- DenseNet family (121,161,169,201)
- SqueezeNet v1.1
- MobileNet v1, v2
- NiN
- ResNet v1 (101, 152)
- ResNet v2 (101)
- SqueezeNet v1.1
- VGG family (VGG16, VGG19)
- SSD-Inception-v3, SSD-MobileNet, SSD-ResNet-50, SSD-300

最受欢迎的三大网络
-----------------------------

- MobileNetv2SSD（50fps）
- TinyYOLOv3 and v4 (40fps)
- Deeplabv3+ (30fps)

.. include::  /pages/includes/footer-long.rst