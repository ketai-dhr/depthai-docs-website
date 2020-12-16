.. Luxonis Docs documentation master file, created by
   sphinx-quickstart on Tue Nov  3 14:34:56 2020.
   You can adapt this file completely to your liking, but it should at least
   contain the root `toctree` directive.

DepthAI 的文档
=======================

*了解如何设置DepthAI设备，查看教程，代码示例等。*

DepthAI是嵌入式空间AI平台，可帮助您构建具有真正实时3D对象定位（认为3D对象检测）和跟踪的产品。DepthAI减轻了AI，深度视觉等工作的负担-直接从内置摄像头进行处理-使您的主机有能力处理特定于应用程序的数据。最棒的是，它是模块化的并获得MIT许可的开源，可以将这些Spatial AI / CV超级功能添加到实际的商业产品中。 

设置您的设备
##########################

.. raw:: html

   <div class="items-container">
      <div class="items-row">
         <!-- BW1093 -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1093.png" alt="MegaAI | Tiny but Mighty"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">MegaAI | 小而强大</h5>
                  <p class="item-descr">
                     MegaAI是一款微型USB3 4K AI相机，具有4K / 30 h.265编码和强大的硬件加速ML / CV。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1093" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
         <!-- BW1098OBC -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1098obc.png" alt="RPi HAT"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">USB3 | 机载相机</h5>
                  <p class="item-descr">
                     带有机载彩色摄像头模块和全局快门同步立体声对的DepthAI为您选择的主机。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1098obc" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
      </div>
      <div class="items-row">
         <!-- BW1098FFC -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1098ffc.jpg" alt="USB3 | Modular Cameras"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">USB3 | 模块化相机</h5>
                  <p class="item-descr">
                     DepthAI提供给您选择的主机，并带有模块化摄像头，可轻松集成到您的平台中和自定义立体声基准。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1098ffc" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
         <!-- BW1099 -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1099.jpg" alt="System on Module"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">系统模块</h5>
                  <p class="item-descr">
                     使您可以将DepthAI的功能集成到您自己的产品中。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1099" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
      </div>
      <div class="items-row">
         <!-- BW1097 -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1097.jpg" alt="RPi Compute" />
               </div>
               <div class="item-body">
                  <h5 class="item-title">RPi Compute</h5>
                  <p class="item-descr">
                     完整的DepthAI系统，包括Raspberry Pi计算模块，预装有Raspbian 10的microSD卡和DepthAI Python接口。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1097" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
         <!-- BW1094 -->
         <div class="items-col">
            <div class="item">
               <div class="item-img-wrapper">
                  <img class="item-img" src="./_images/bw1094.jpg" alt="RPi HAT"/>
               </div>
               <div class="item-body">
                  <h5 class="item-title">RPi HAT</h5>
                  <p class="item-descr">
                     适用于Raspberry Pi（3、3B+和4）的DepthAI HAT。添加您选择的相机。
                  </p>
               </div>
               <footer class="item-footer">
                  <a href="pages/products.html#bw1094" class="btn item-cta">Get Started ›</a>
               </footer>
            </div>
         </div>
      </div>
   </div>

.. include::  /pages/includes/footer-long.rst


.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Contents:

   pages/api.rst
   pages/faq.rst
   pages/support.rst
   pages/troubleshooting.rst
   pages/products.rst
   pages/training.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Tutorials:

   pages/tutorials/hello_world.rst
   pages/tutorials/pretrained_openvino.rst
   pages/tutorials/multiple.rst
   pages/tutorials/local_convert_openvino.rst
   pages/tutorials/windows_install.rst

.. toctree::
   :maxdepth: 1
   :hidden:
   :caption: Code samples:

   pages/samples/minimal.rst
   pages/samples/color_camera_selfie.rst
   pages/samples/mono_camera_selfie.rst
   pages/samples/object_tracker.rst
   pages/samples/social_distancing.rst
   pages/samples/helper_depthai_class.rst
   pages/samples/helper_depthai_generator.rst
