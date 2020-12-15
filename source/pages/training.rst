定制培训
===============

.. toctree::
  :maxdepth: 2
  :hidden:

   教程 - MobileNetSSD 培训 <https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Easy_Object_Detection_Demo_Training.ipynb>
   教程 - 使用自定义数据进行MobileNetSSD 培训 <https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Easy_Object_Detection_With_Custom_Data_Demo_Training.ipynb>
   教程 - 基于YOLO-based 蒙面检测器 <https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Easy_TinyYolov3_Object_Detector_Training_on_Custom_Data.ipynb>
   工具 - Google 云端硬盘图片批量调整大小 <https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/GDrive-Resize.ipynb>


.. raw:: html

   <h1>总览</h1>


在这里，我们提供了经过各种数据集训练的Google Colaboratory（又名Colab或简称colabs）笔记本的示例。它们是免费的GPU实例，因此非常适合原型甚至简单的生产模型。


.. raw:: html

   <h2>教程</h2>


以下教程基于MobileNetv2-SSD，MobileNetv2-SSD是一个性能不错的框架对象对象检测器，它本身在DepthAI上运行。可以在Colab上培训/支持许多其他对象检测器并在DepthAI上运行它们，因此，如果您需要其他对象检测器/网络后端，请随时发出Github问题！

.. raw:: html

   <h3>简易物体检测器培训 <a href="https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Easy_Object_Detection_With_Custom_Data_Demo_Training.ipynb" target="_blank"><img alt="Open In Colab" src="https://colab.research.google.com/assets/colab-badge.svg"/></a></h2>

教程笔记本
*Easy_Object_Detection_With_Custom_Data_Demo_Training.ipynb* 显示了如何基于Mobilenet SSDv2网络快速训练对象检测器。

（可选）, 请参阅有关此模块的文档 (`此处 <https://docs.luxonis.com/tutorials/object_det_mnssv2_training/>`__)
以获取有关如何使用此笔记本的指南/演练。此外，可以随意跳进笔记本电脑，通过一些实验，训练模型相对简单。

训练完成后，它还将模型转换为在我们的DepthAI平台和模块上运行的.blob文件。首先，将模型转换为OpenVINO可以使用的称为中间表示（IR）的格式。然后使用我们为此设置的服务器将IR模型编译为.blob文件。 (IR模型也可以 `本地转换为Blob <https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__ )。

就是这样，在不到两个小时的时间内，相当先进的概念证明对象检测器就可以在DepthAI上运行，以检测您选择的对象及其相关的空间信息（即xyz位置）。例如，此笔记本用于训练DepthAI在3D空间中定位草莓，请参见下文:

.. image:: https://i.imgur.com/Cz7eZUo.jpg
  :alt: Real-time 3D Strawberry Detector
  :target: https://www.youtube.com/watch?v=Okjh2OCP-o8&

上面的示例使用了DepthAI模块化相机版本 (`BW1098FFC <https://shop.luxonis.com/products/depthai-usb3-edition>`__).


.. raw:: html

   <h3>COVID-19 蒙面/不蒙面训练 <a href="https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/Medical_Mask_Detection_Demo_Training.ipynb" target="_blank"><img alt="Open In Colab" src="https://colab.research.google.com/assets/colab-badge.svg"/></a></h2>

该 *Medical Mask Detection Demo Training.ipynb* 培训笔记本显示了更复杂的对象检测器的另一个示例。训练数据集包括戴着或不戴着口罩进行病毒防护的人员。大约有700张图片，带有大约3600个边框注释。图像很复杂：它们的比例和构图变化很大。尽管如此，对于这种任务而言，对象检测器在这个相对较小的数据集中却表现出色。同样，培训大约需要2个小时。取决于Colab彩票分配给笔记本实例的GPU，训练10k步骤可能需要2.5个小时或1.5个小时。无论哪种方式，对于如此艰巨的任务而言，在短期内就无法获得如此优质的概念证明。然后，我们执行了上面的步骤以转换为Blob，然后在DepthAI模块上运行它。

以下是在Luxonis DepthAI车载摄像头版本 (`BW1098OBC <https://shop.luxonis.com/products/bw10980bc>`__) 上使用此笔记本电脑生产的型号的快速测试:

.. image:: http://img.youtube.com/vi/d_oUxDzWHd0/0.jpg
  :alt: COVID19 Mask Detector
  :target: https://www.youtube.com/watch?v=d_oUxDzWHd0

.. raw:: html

   <h2>配套笔记本</h2>

.. raw:: html

   <h3>在Google云端硬盘中调整图片大小  <a href="https://colab.research.google.com/github/luxonis/depthai-ml-training/blob/master/colab-notebooks/GDrive-Resize.ipynb" target="_blank"><img alt="Open In Colab" src="https://colab.research.google.com/assets/colab-badge.svg"/></a></h2>

该笔记本将在您的Google云端硬盘中处理一组图像，以将其调整为培训笔记本所需的格式。有关更多详细信息，
请参见 `此处 <https://docs.luxonis.com/tutorials/object_det_mnssv2_training/#step-1-find-or-generate-images-of-the-objects-of-interest>`__ 的文档。

.. include::  /pages/includes/footer-short.rst



