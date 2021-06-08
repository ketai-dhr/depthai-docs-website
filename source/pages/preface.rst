序言
====================================

OAK-OpenCV人工智能开发套件
##################################

.. image:: /_static/images/oak-d.jpeg
  :alt: OAK-D
  :align: center

OAK是个啥？
####################################

OAK（OpenCV AI Kit）是OpenCV官方指定的人工智能开发套件。

OAK小小的身材中集成了4K彩色像素、双目深度相机、惯性测量单元「IMU」、高性能AI处理芯片，浓缩的都是精华。

OAK能够帮助初学者快速上手实现AI神经推理、深度视觉、图像处理等学习研究和产品开发，并且其性能比一般神经推理芯片强大很多倍。它完美地适用于机器人、无人机、无人船、无人车等系统开发，软件部分开发非常简单，消除了嵌入式人工智能的编译痛苦，让你更加专注于项目的构思和实现。

OAK搭载了英特尔旗下子公司Movidius推出的Movidius MyRiad X视觉处理器(VPU)，这是一种低功耗的系统芯片(SoC)，用于在基于视觉的设备上加速深度学习和人工智能。

应用示例
########################

在这里，您将了解到DepthAI可以做些什么并从中获得灵感。

.. raw:: html

   <div class="items-container">
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/camDemo.png" alt="Camera Demo"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">OAK相机示例</h5>
                  <span class="item-descr">
                      <p>此示例展示了如何通过 USB 在 Gen2 Pipeline Builder 中使用 DepthAI/megaAI/OAK 相机。</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-camera-demo" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <a href="https://www.bilibili.com/video/BV15V41147NX?share_source=copy_web" target="_blank">
                    <img class="item-img" src="/_static/images/preface/mask.gif" alt="COVID-19 mask detection"/>
                  </a>
               </div>
               <div class="item-body">
                  <h5 class="item-title">COVID-19 口罩检测</h5>
                  <p class="item-descr">
                      <p>此实验允许您运行通过以下方式训练的 COVID-19 戴口罩/没戴口罩检测器</p>
                      <p>Google Colab 教程 <a href="https://gitee.com/oakchina/depthai-ml-training/tree/master/colab-notebooks#covid-19-maskno-mask-training" target="_blank">here</a>.</p>
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-coronamask" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/lookAt.gif" alt="Gaze estimation"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">注视估计</h5>
                  <span class="item-descr">
                      <p>此示例演示如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行3阶段推理（3个串行，2个并行）。</p>
                      <p>制作此示例的原始 OpenVINO 演示是<a target="_blank" href="https://docs.openvinotoolkit.org/2021.1/omz_demos_gaze_estimation_demo_README.html">here</a>.</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-gaze-estimation" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <a href="https://www.bilibili.com/video/BV1p44y167Ld?share_source=copy_web" target="_blank">
                    <img class="item-img" src="/_static/images/preface/LicensePlatesRecognition.gif" alt="License Plates Recognition"/>
                  </a>
               </div>
               <div class="item-body">
                  <h5 class="item-title">车牌识别</h5>
                  <span class="item-descr">
                      <p>此示例演示如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行2阶段推理。</p>
                      <p>首先，在图像上检测车牌，然后将裁剪后的车牌帧发送到文本检测网络，该网络尝试对车牌文本进行解码</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-license-plate-recognition" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/Segmentation.png" alt="Deeplabv3"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">Deeplabv3 (图像语义分割)</h5>
                  <span class="item-descr">
                      <p>此示例展示了如何在 Gen2 API 系统中的 DepthAI 上运行 Deeplabv3+。</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-deeplabv3_person" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/taichi.gif" alt="Pose Estimation"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">姿态估计</h5>
                  <span class="item-descr">
                      <p>在 DepthAI 上运行 Google Mediapipe 身体姿势跟踪模型</p>
                      <p>这个例子是由我们的贡献者创建的 - <a href="https://github.com/geaxgx" target="_blank">Geaxgx</a></p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://github.com/geaxgx/depthai_blazepose" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/PedestrianReidentification.png" alt="Deeplabv3"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">行人重新识别</h5>
                  <p class="item-descr">
                      <p>此示例演示如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行2阶段推理。</p>
                      <p>制作此示例的原始 OpenVINO 演示是 <a target="_blank" href="https://docs.openvinotoolkit.org/2020.1/_demos_pedestrian_tracker_demo_README.html">here</a>.</p>
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-pedestrian-reidentification" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/pose.gif" alt="Head posture detection"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">头部姿势检测</h5>
                  <span class="item-descr">
                      <p>此示例演示运行人脸检测网络和头部检测网络的 Gen2 Pipeline Builder</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-head-posture-detection" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/faceInfor.gif" alt="Age Gender Recognition"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">年龄性别识别</h5>
                  <span class="item-descr">
                      <p>此示例演示如何使用 Gen2 Pipeline Builder 在 DepthAI 上运行 2 阶段推理。</p>
                      <p>首先，在图像上检测人脸，然后将裁剪后的人脸框发送到年龄性别识别网络，产生估计结果</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-age-gender" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/fire_demo.gif" alt="Fire detection"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">烟火检测</h5>
                  <span class="item-descr">
                      <p>此示例演示运行火灾检测网络的 Gen2 Pipeline Builder</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-fire-detection" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/face_reg.png" alt="Face Recognition"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">人脸识别</h5>
                  <span class="item-descr">
                    <p>这个例子演示了运行人脸检测网络、头部姿势估计网络和人脸识别网络的 Gen2 Pipeline Builder</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-face-recognition" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/fatigue.gif" alt="Fatigue Detection"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">疲劳检测</h5>
                  <span class="item-descr">
                      <p>此示例演示运行人脸检测网络和头部检测网络的 Gen2 Pipeline Builder</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-fatigue-detection" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <a href="https://www.bilibili.com/video/BV1k5411M7SP?share_source=copy_web" target="_blank">
                     <img class="item-img" src="/_static/images/preface/textOCR.png" alt="Gen2 OCR"/>
                  </a>
               </div>
               <div class="item-body">
                  <h5 class="item-title">文本检测OCR</h5>
                  <span class="item-descr">
                    <p>此示例演示 Gen2 Pipeline Builder 运行文本检测 (EAST)，然后对检测到的文本进行光学字符识别</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-ocr" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/SignLanguageRecognition.gif" alt="Sign Language Recognition"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">手语识别</h5>
                  <span class="item-descr">
                      <p>此示例演示如何使用手部标志在 DepthAI 上识别美国手语 (ASL) </p>
                      <p>这个例子是由 <a href="https://www.cortic.ca/post/classifying-american-sign-language-alphabets-on-the-oak-d" target="_blank">Cortic Technology</a></p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://github.com/cortictechnology/hand_asl_recognition" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <a href="https://player.bilibili.com/player.html?aid=973213040&bvid=BV1e44y1r7GL&cid=340454737&page=1" target="_blank">
                     <img class="item-img" src="/_static/images/samples/traker.gif" alt="Gen2 OCR"/>
                  </a>
               </div>
               <div class="item-body">
                  <h5 class="item-title">目标跟踪</h5>
                  <span class="item-descr">
                    <p>此示例展示了利用depthai实现目标跟踪，通过云台将检测目标始终置于图像中央。</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-target-tracking" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <a href="https://player.bilibili.com/player.html?aid=798292065&bvid=BV1dy4y1m7hp&cid=270461353&page=1&t=0h2m17s" target="_blank">
                    <img class="item-img" src="/_static/images/samples/social-distancing9.png" alt="Sign Language Recognition"/>
                  </a>
               </div>
               <div class="item-body">
                  <h5 class="item-title">社交距离</h5>
                  <span class="item-descr">
                      <p>此示例使用的是第一代depthai </p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/social-distancing" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>
   </div>


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
*************************

运行不同程序功耗是不同的

- 空闲功率：0.7W
- 正常功率：2.5W
- 最大功率：5W

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