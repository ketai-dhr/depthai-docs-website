.. Luxonis Docs documentation master file, created by
   sphinx-quickstart on Tue Nov  3 14:34:56 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

前言
=======================

相信此刻的你，已经收到了一台 OAK 设备，并准备好开始学习使用它了！

不过请不要着急查看 :ref:`OAK快速上手 <安装depthai>` 这部分内容，我们给你规划了由浅入深的 OAK 学习路线，能让你在学习的过程中尽量减少出错的可能。

.. image:: /_static/images/load.png

第一部分：认识OAK硬件
=======================

这部分内容，我们的学习目标有三个：

1.知道你现在使用的 OAK 设备属于哪个产品系列 

2.知道你现在使用的 OAK 应该如何正确接线 

3.学会你现在使用的 OAK 的特定教程 

请一定要注意接线！！！避免出现找不到设备、程序直接退出、设备被识别成 USB2.0 报错等情况！

第二部分：OAK快速上手
=======================

这部分内容，我们的学习目标是能成功在你的系统平台上安装 depthai ，并成功运行官方的示例。不知道 depthai 是啥的同学，请看这篇 `文章 <https://www.oakchina.cn/2021/07/29/opencv-ai-kit/>`__ 。

目前 OAK 支持的系统平台有：Windows、树莓派、Linux、MacOS、Jetson。Windows 有安装包（安装方式简单），树莓派有镜像文件（安装方式简单），其他平台可通过 git 的方式拉取安装 depthai。

第三部分：基础OAK玩法
=======================

这部分内容，我们的学习目标是能成功运行 Hello Word，了解API的基础知识（Python/C++）。通过学习官方的 API 示例，能掌握 API 的基本用法，能自主组合使用搭建自己所需的功能。

当然你还可以尝试官方的应用实例，了解到 DepthAI 可以做些什么并从中获得灵感。详细官方应用示例请查看 `此处 <https://gitee.com/oakchina/depthai-experiments/tree/master>`__ 。

.. raw:: html

   <div class="items-container">
      <div class="items-row">

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/triangulation.png" alt="Camera Demo"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">立体神经推理演示</h5>
                  <span class="item-descr">
                      <p>通常需要对立体神经推理结果进行特定于应用程序的主机端过滤，并且因为这些计算是轻量级的(即可以在 ESP32 上完成)，所以我们将三角剖分本身留给主机。</p>
                  </span>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-triangulation" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="/_static/images/preface/record-replay.png" alt="COVID-19 mask detection"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">Record and Replay</h5>
                  <p class="item-descr">
                      <p>这些工具允许您记录编码和同步的摄像机流并重放它们，包括重建立体深度感知。</p>
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="https://gitee.com/oakchina/depthai-experiments/tree/master/gen2-record-replay" class="btn item-cta">尝试一下 ›</a>
               </footer>
            </div>
         </div>

      </div>

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
                      <p>此示例展示了如何通过USB在Gen2 Pipeline Builder中使用OAK相机。可以了解到深度设置和点云图像。</p>
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

第四部分：进阶OAK玩法
=======================

这部分内容，你将学习与模型训练、转换、部署有关的内容，你也可以学习热门的开源项目，如 SLAM、VIO、ROS 等等，或者你也可以尝试我们整理的Github上的部分 `开源项目 <https://www.oakchina.cn/oak-opensource-projects/>`__。

第五部分：高阶OAK玩法
=======================

这部分内容主要是设备上编程，主要分为三个板块内容： 

1.使用脚本节点
      
2.创建自定义NN（神经网络）模型
   
3.创建自定义OpenCL内核
   
第六部分：问题答疑
=======================

如果在使用过程中出现报错，先 `查看 <https://www.oakchina.cn/tech-faq/>`__ 此处，看看有没有你出现的问题。
没有的话，再看看 `这里 <https://docs.oakchina.cn/en/latest/pages/faq.html>`__ 有没有。
如果还是无法排查你的问题，请添加 `OAK中国企业微信好友 <https://www.oakchina.cn/wp-content/uploads/2022/03/oakchina-qy-wechat-2-150x150.png>`__，备注 "OAK群" ，寻求技术支持。

其他补充资料
=======================

OAK中国Gitee： `查看 <https://gitee.com/oakchina>`__

OAK中国CSDN： `查看 <https://blog.csdn.net/oakchina>`__

OAK开源项目合集： `查看 <https://www.oakchina.cn/oak-opensource-projects/>`__

OAK官方GitHub： `查看 <https://github.com/luxonis>`__

OAK官方英文原版使用文档： `查看 <https://docs.luxonis.com/en/latest/>`__

.. include::  /pages/includes/footer-long.rst

.. .. toctree::
..    :maxdepth: 1
..    :numbered: 3
..    :hidden:
..    :caption: 前言

..    pages/foreword.rst

.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: 认识OAK硬件

   pages/hardware/Product-family-introduction.rst
   pages/hardware/Product-model-explanation.rst
   pages/hardware/Connection-considerations.rst
   pages/hardware/Supported-Formats.rst
   pages/hardware/Tutorials.rst

.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: OAK快速上手

   pages/developer_Guide.rst


.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: 基础OAK玩法

   pages/Basic/Helloworld.rst
   pages/Basic/gen2-ipa.rst
   pages/Basic.rst
   pages/tutorials/applicationExample.rst

.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: 进阶OAK玩法

   pages/Advanced/download.rst
   pages/Advanced/convert.rst
   pages/Advanced/Neural-networks.rst
   pages/Advanced/slam.rst
   pages/Advanced/ros.rst
   pages/Advanced/oak-slam.rst
   pages/Advanced/vio.rst
   pages/Advanced/scrarch.rst

.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: 高阶OAK玩法

   pages/High_level.rst

.. .. toctree::
..    :maxdepth: 1
..    :numbered: 3
..    :hidden:
..    :caption: 文章

..    文章地址 <https://www.oakchina.cn/blog/>
..    pages/link.rst

.. toctree::
   :maxdepth: 1
   :numbered: 3
   :hidden:
   :caption: 问题答疑

   主要问题解答 <https://www.oakchina.cn/tech-faq/>
   pages/faq.rst
   pages/troubleshooting.rst
   pages/support.rst
   
