.. Luxonis Docs documentation master file, created by
   sphinx-quickstart on Tue Nov  3 14:34:56 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

DepthAI 的文档
=======================

.. note::

   第一次使用DepthAI? 请看 :ref:`这里 <快速上手>`!

**了解如何设置 DepthAI 设备，查看教程，代码示例等。**

DepthAI 是围绕 `Myriad X <https://www.intel.com/content/www/us/en/products/details/processors/movidius-vpu/movidius-myriad-x.html>`__ 构建的嵌入式 `空间 <https://docs.oakchina.cn/en/latest/pages/faq/#spatialai-3d>`__ AI 平台 - 一个完整的定制 `硬件 <https://docs.oakchina.cn/en/latest/#id1>`__ 、固件、`软件 <https://docs.oakchina.cn/en/latest/pages/gen2_api/#gen2-python-api>`__ 和 AI 培训生态系统。它将神经推理、深度视觉和特征跟踪结合到一个易于使用的 30 秒内工作的解决方案中。

DepthAI 使用 Python 和 C++ 编写的简单易用的 API，在单个设备中为您提供强大的 AI、深度和跟踪功能。

最棒的是，它是模块化的(模块化系统) ，基于 MIT-licensed 开源硬件，可以为真正的商业产品提供这些 Spatial AI/CV 超级能力。

在下面的小节中，我们将展示您可以使用 deptai 构建什么。不同的例子将展示不同的使用 deptai 功能，这既可以鼓励你发展自己的想法或更深入地进入 deptai 功能自己发现他们。

Demo script
#####################

演示脚本是我们的多用途命令行演示工具，围绕 Gen2 Pipeline 构建，它允许您直接从命令行检查 deptai 功能——不需要编码！它可以同时工作 USB 和 POE，自动发现任何 POE deptai 在您的局域网和/或 USB deptai 连接到您的计算机。如果多个连接，它将提示您使用的演示。

.. image:: /_static/images/depthaiDemo.png
  :alt: Default run

要在 deptai 设备上安装并运行演示脚本，请在终端中键入以下命令。

.. code-block:: bash

  git clone https://github.com/luxonis/depthai.git
  cd depthai
  python3 install_requirements.py
  python3 depthai_demo.py

然后继续阅读 `README.md <https://github.com/luxonis/depthai/blob/main/README.md>`__ 以获得更多的使用示例。

如果您在安装过程中遇到问题，请参阅我们的 `安装页面 <https://docs.oakchina.cn/en/latest/pages/api/>`__ 以获得其他特定于操作系统的说明

应用示例
####################

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

教程
##################

在本节中，您将找到一种方法来扩展您在 depthai 相关主题方面的知识。下面列出的教程是特定主题的完整演练。

.. raw:: html

  <ul class="tutorials-list">
    <li class="tutorials-list-item">
      <div class="tutorials-list-item-desc">
        <h3>Hello World</h3>
        <p>了解如何使用 deptai Python API 来显示彩色视频流</p>
      </div>
      <div class="tutorials-list-item-cta">
          <a href="https://docs.oakchina.cn/en/latest/pages/tutorials/hello_world/" class="btn item-cta">Start now ›</a>
      </div>
    </li>
    <li class="tutorials-list-item">
      <div class="tutorials-list-item-desc">
        <h3>Multiple DepthAI per Host</h3>
        <p>了解如何使用 deptai Python API 来显示彩色视频流</p>
      </div>
      <div class="tutorials-list-item-cta">
          <a href="https://docs.oakchina.cn/en/latest/pages/tutorials/multiple/" class="btn item-cta">Start now ›</a>
      </div>
    </li>
    <li class="tutorials-list-item">
      <div class="tutorials-list-item-desc">
        <h3>Local OpenVINO Model Conversion</h3>
        <p>了解如何将 OpenVINO IR 模型转换为在 deptai 上运行所需的格式</p>
      </div>
      <div class="tutorials-list-item-cta">
          <a href="https://docs.oakchina.cn/en/latest/pages/tutorials/local_convert_openvino/" class="btn item-cta">Start now ›</a>
      </div>
    </li>
  </ul>

.. include::  /pages/includes/footer-long.rst


.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: 硬件设备

   pages/preface.rst
   pages/Support_format.rst
   pages/products.rst
   pages/getting-started-with-ESP32.rst
   pages/tutorials/getting-started-with-poe.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: OAK与SLAM

   pages/slam_oak.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: OAK与scratch

   pages/scratch/installOAKSoftware.rst
   pages/scratch/gettingStarted.rst
   pages/scratch/firstAcquaintanceBlocks.rst
   pages/scratch/caseStudy.rst
   pages/scratch/newFeaturePreview.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: 用户指南

   pages/Get_started_quickly.rst
   pages/tutorials/pretrained_openvino.rst
   pages/calibration.rst
   pages/stereo_neural_inference.rst
   pages/tutorials/applicationExample.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: 开发者指南

   pages/api.rst
   pages/gen2_api.rst
   pages/training.rst
   pages/model_conversion.rst
   pages/tutorials/local_convert_openvino.rst
   pages/tutorials/multiple.rst
   pages/tutorials/CodeSamples.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: 问题答疑

   pages/faq.rst
   pages/troubleshooting.rst
   pages/support.rst
