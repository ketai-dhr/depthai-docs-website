序言
====================================

我们正处在人工智能飞速发展的风口浪尖，近年来人工智能在教育、金融、交通、医疗、零售、家居、科研、军事等方向的发展的速度超乎了我们的想象。如果我们不改变，毫无疑问，未来和我们抢市场、抢利润、抢工作、甚至抢爱人的会是工作效率极高，不知疲倦，比我们还懂另一半的机器人。

如果您不想被时代淘汰，想投身于人工智能的浪潮，拥有高薪工作，但苦于不知道从哪里开始；或者您的学校或者科研机构，面对琳琅满目的AI平台却找不到一款算力高，功耗低，体积小、开发难度低的AI平台；亦或您和您的企业想实现从产品原型验证到产品落地的快速转化，那么我们可以陪您完成您AI梦想之旅！OAK系列产品将帮助您从零基础入门到快速成长为AI领域专家。不需要花费大量精力在软硬件的适配上，可以聚焦于您的行业应用，快速将创意转化成产品并产生效益。

OAK-OpenCV人工智能开发套件
##################################

.. image:: /_static/images/oak-d.jpeg
  :alt: OAK-D

OAK是个啥？
####################################

OAK（OpenCV AI Kit）是OpenCV官方指定的人工智能开发套件。

OAK小小的身材中集成了4K彩色像素、双目深度相机、惯性测量单元「IMU」、高性能AI处理芯片，浓缩的都是精华。

OAK能够帮助初学者快速上手实现AI神经推理、深度视觉、图像处理等学习研究和产品开发，并且其性能比一般神经推理芯片强大很多倍。它完美地适用于机器人、无人机、无人船、无人车等系统开发，软件部分开发非常简单，消除了嵌入式人工智能的编译痛苦，让你更加专注于项目的构思和实现。

OAK搭载了英特尔旗下子公司Movidius推出的Movidius MyRiad X视觉处理器(VPU)，这是一种低功耗的系统芯片(SoC)，用于在基于视觉的设备上加速深度学习和人工智能。

已知用到了Intel Movidius技术的产品
*************************************

三星 JetBot 90 AI +
------------------------

.. image:: /_static/images/JetBot90AI.png
  :alt: JetBot90AI

JetBot 90 AI +是有史以来第一个由Intel®AI驱动的智能吸尘器，使用3D传感器和增强目标识别来规划路线。

DJI Phantom 4
-------------------------

.. image:: /_static/images/DJIPhantom4.png
  :alt: DJIPhantom4

大疆phantom4 避障系统和自动追踪功能，实现这些智能飞行功能全赖Movidius的Myriad 2 MA2100来实现。

DJI Spark
---------------------------

.. image:: /_static/images/dji-drone-spark-sm.jpg
  :alt: dji-drone-spark-sm

大疆Spark使用Myriad 2 VPU高速执行车载计算机视觉处理和深度学习算法，从而使诸如避免和感知，光学跟踪和手势识别等应用成为可能。

PhiSat-1
------------------------------

.. image:: /_static/images/star.jpeg
  :alt: star

PhiSat-1是欧洲航空局发射的一颗实验卫星，用于监视极地冰层和土壤水分。PhiSat-1搭载了新款高光谱热像仪，具备机载AI处理能力，这要归功于英特尔®Movidius™Myriad™ 2 VPU.

OAK能做哪些事？
##################################

能做的事情，多到你怀疑人生。甭管你是天上飞的无人机，还是路上跑的无人驾驶小汽车，又或是医疗，安防，视频处理等方面。一切和图像处理有关的地方，OAK都能大放异彩。

    以下是OAK的实际应用案例：

    - 物体检测：免费模型包括MobileNet-SSDv2, Tiny-YOLOv3, Strawberry Detector, Mask / No Mask Detector, Safety-Glasses等，并且还提供免费Google Colab项目以供定制。
    - 人脸检测: 支持面部特征识别，情绪识别，年龄识别，体型识别，身份识别与二次验证/核对等。
    - 车辆检测: 支持车型识别，车牌检测，车牌号识别等。
    - 行人检测: 支持单人追踪，比对/核验/计数等。
    - 姿态检测: 支持人体关节点3D坐标记录。
    - 文字识别（OCR）: 支持文字识别，读取。
    - 图像语义分割: 支持基于深度检测的图像分割。
    - 双目测距：实现深度视觉，3D物体定位，SLAM建图。
    - H.264和H.265编码：HEVC，1080p和4K视频。

OAK相比其他嵌入式人工智能产品的优势？
#############################################

- OAK不需要其他繁琐的配置，30秒快速上手。
- 硬件直接集成了高性能相机模组，集成一个1200万高像素彩色相机和可选的2个黑白工业相机，无需额外购买其他硬件。
- 固件直接支持双目深度测距功能，无需额外开发双目算法。
- 支持所有深度学习算法框架，包含主流的Tensorflow，Caffe，Pytorch等。
- 支持海量的开源神经网络模型，包含MobileNetv2SSD ， TinyYOLOv3 and v4 ，Deeplabv3+。
- 支持多种编程语言，包含C/C++，Python等。
- 无缝支持Opencv软件库，是OpenCV官方唯一指定AI学习和竞赛套件。
- 支持业界领先的OpenVino深度学习部署平台。
- 支持4K 30fps H.264/H.265视频编码功能。
- 支持快速原型验证到产品化落地整个过程。
- 以下是OAK与其他嵌入式平台的详细对比表：

.. list-table:: OAK与其他嵌入式平台的详细对比表
    :header-rows: 1 

    * - 产品
      - OAK
      - 比特大陆BM1880
      - TB-RK3399Pro
      - JETSON NANO
      - KOI K210系列
      - openMV系列
      - 树莓派4B
    * - 性能
      - 4.0TOPS @INT8/FP16
      - 1TOPS @INT8
      - 3.0TOPS @INT16/FP16
      - 0.472TOPS @PF16/FP32/INT8
      - 0.8TOPS @INT8
      - 0.4TOPS
      - 0.1TOPS
    * - 支持神经网络
      - MobileNetv2SSD、TinyYOLOv3 and v4、Deeplabv3+等所有神经网络模型。
      - DNN、CNN、RNN、LSTM
      - Inception V3、ResNet34、VGG16
      - AlexNet、ResNet-18、YOLO
      - TinyYOLOv2
      - 不适合做深度学习
      - MobileNetv2SSD、TinyYOLOv3 and v4、Deeplabv3+等所有神经网络模型。
    * - 摄像头
      - 4K摄像头和两颗双目摄像头
      - 无
      - 无
      - 无
      - QVGA @60fps/VGA @30fps摄像头
      - 30万像素摄像头
      - 无
    * - 使用难度
      - 非常容易
      - 比较难
      - 比较难
      - 比较难
      - 容易
      - 中等难度
      - 比较难
    * - 适用人群
      - 人工智能研究人员，专业技术开发人员，学生
      - 人工智能研究人员，专业技术开发人员
      - 人工智能研究人员，专业技术开发人员
      - 人工智能研究人员，专业技术开发人员
      - 人工智能，机器视觉爱好者，学生
      - 机器视觉爱好者，学生
      - 人工智能研究人员，专业技术开发人员，学生
    * - 产品化
      - 可以
      - 可以
      - 可以
      - 可以
      - 不可以
      - 不可以
      - 可以
    * - 支持框架
      - Kaldi、Caffe、TensorFlow、ONNX、Pytorch、MXNet
      - Caffe、TensorFlow、pytorch、paddle、MXNet
      - TensorFlow、Pytorch、Caffe、Mxnet、Darknet、Onnx
      - Caffe、TensorFlow、ONNX、Pytorch
      - TensorFlow、Keras、Darknet
      - 无
      - Kaldi、Caffe、TensorFlow、ONNX、Pytorch、MXNet

OAK详细参数
############################

处理器参数
************************

.. list-table:: 处理器参数
  :header-rows: 1

  * - Movidius MyRiad X视觉处理单元
    - 内置于每个OAK模块
  * - 计算能力
    - 4万亿次运算每秒

相机参数
*************************
  
.. list-table:: 相机参数
  :header-rows: 1

  * - 相机
    - 彩色相机
    - 黑白工业相机
  * - Shutter Type
    - Rolling Shutter
    - Sync Global Shutter
  * - Image Sensor
    - IMX378
    - OV9282
  * - Max Framerate
    - 60fps
    - 120fps
  * - H.265 Framerate
    - 30fps
    - /
  * - Resolution
    - 12MP (4056×3040 px/ 1.55um)
    - 1MP (1280×800 px/3um)
  * - Field of View
    - 81° DFoV – 68.8° HFoV
    - 81° DFoV – 71.8° HFoV
  * - Lens Size
    - 1/2.3 Inch
    - 1/2.3 Inch
  * - Focus
    - 8cm – ∞ (AutoFocus)
    - 19.6cm – ∞ (FixedFocus)
  * - F-number
    - 2.0
    - 2.2

关于功耗
**************************

.. list-table:: 关于功耗
  :header-rows: 1

  * - 热阻
    - 功耗值（°C/W）
    - 描述
  * - θJB
    - 5.8
    - 结到周围环境的热阻
  * - θJC
    - 3.1
    - 结到管壳的热阻
  * - θJA
    - 21.4
    - 结到电路板的热阻

OAK适合哪些人？
#############################

- 对人工智能感兴趣的人。
- 对嵌入式机器视觉感兴趣的人。
- 想要学习人工智能和计算机视觉的小伙伴，通过OAK可以为您的学习添砖加瓦。
- 对目标检测和图像识别有开发需求的人

深入使用OAK需要哪些知识？
##################################

- 零基础可以快速运行起OAK，但是想要深入还是需要一些Python基础知识的。
- Python 基础教程：https://www.runoob.com/python/python-tutorial.html
- OpenCV基础知识
- opencv-python中文教程：https://www.kancloud.cn/aollo/aolloopencv/259610

.. include::  /pages/includes/footer-long.rst