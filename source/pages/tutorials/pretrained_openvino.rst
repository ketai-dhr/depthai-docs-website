使用预训练的 OpenVINO 模型
================================

在本教程中，你将学习如何用预先训练好的模型实时检测面部，就算用低算力的树莓派也能做到。
我们将向您介绍 OpenVINO 模型库，以及如何从该库中运行模型。

.. image:: /_static/images/tutorials/pretrained_openvino/face-1.png
  :alt: preview

还没听说过 OpenVINO 或者 Open Model Zoo? 
我先简单介绍一下为什么我们需要这些工具。

什么是 OpenVINO ?
#################

核心部分，DepthAI 使用英特尔 MyriadX 芯片来进行高速模型推理。
然而，要获得高性能，不只是把神经网络塞到芯片中这么简单。
这个时候，我们就需要 `OpenVINO <https://docs.openvinotoolkit.org/>`__ 了。
OpenVINO 是一个免费的工具包，它能把深度学习模型转换为可以在英特尔硬件上运行的格式。
模型转换后，通常能获得25倍甚至更高的每秒帧数（FPS）提升。
几个小步骤是否能够将FPS提高25倍？ 通常，答案是肯定的！

什么是 Open Model Zoo?
###########################

`Open Model Zoo <https://github.com/opencv/open_model_zoo>`__  是一个免费的预训练模型库。
附注：在机器学习 /AI 中，预训练模型集合的名称叫做 “模型 Zoo”。
Zoo 还包含将这些模型下载成可编译格式的脚本，以便在 DepthAI 上运行。

DepthAI 能够运行许多 Zoo 中的对象检测模型，其中有几个模型已经预置在 DepthAI 的 Github 代码库中。
在本教程中我们将使用的模型，是 face-detection-retail-0004 (该模型已在我们的 
`Github 上预编译 <https://github.com/luxonis/depthai/tree/master/resources/nn/face-detection-retail-0004>`__ , 也已在 `OpenVINO 模型 zoo 上预编译 <https://docs.openvinotoolkit.org/2020.1/_models_intel_face_detection_retail_0004_description_face_detection_retail_0004.html>`__ )。

我们将在后续的文档中介绍如何将 OpenVINO 模型转换为在 DepthAI 上运行。
现在，你可以在 `这里 <https://github.com/luxonis/depthai/tree/master/resources/nn>`__ 找到我们已经预转换的模型，
以及 `关于如何转换的简要说明 <https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__.

依赖
############

.. warning::

  正在使用树莓派计算模组（Compute Module）或者已经烧录好的 DepthAI 树莓派 SD 卡？
  **请跳过这一步**

  所有的依赖都已经安装好了，代码库保存到了 :code:`~/Desktop/depthai`。

本教程与 :ref:`Hello World 教程 <hello_world_dependencies>` 具有相同的依赖 
– DepthAI API 已经安装，并且可以在系统中访问。
如果你还没有安装 API，请参见 :ref:`这里 <Python API安装详解>` 。


运行 DepthAI 默认模型
#########################

你可以直接修改 :code:`depthai.py` 文件来进行 binding，
或者你可以简单地传递参数给它，让它运行你想运行的模型。

为了简单起见，我们选择后者，简单地传递参数，让 DepthAI 运行 :code:`face-detection-retail-0004`
而不是默认运行的模型。

在切换到使用 :code:`face-detection-retail-0004` 之前，还有一小步。
在这里，我们将只传递在运行 :code:`python3 depthai_demo.py` 时默认运行的神经网络，从而确保我们操作无误：

.. code-block:: bash

  python3 depthai_demo.py -dd

然后运行一个典型的演示 MobileNetv1 SSD 物体检测器，该检测器基于 `PASCAL 2007 VOC <http://host.robots.ox.ac.uk/pascal/VOC/voc2007/>`__ 的类进行训练，这些类包括：
- 人：人
- 动物：鸟、猫、牛、狗、马、羊
- 交通工具：飞机、自行车、船、公共汽车、汽车、摩托车、火车
- 室内：酒瓶、椅子、餐桌、盆栽、沙发、电视/显示器

我在我的 iMac 上运行了这个程序 (macOS 设置 :ref:`here <macOS (Mac OS X)>` )，
一个 `microAI <https://shop.luxonis.com/products/bw1093>`__ 放在我的办公桌上，随意地向上指着
- 它能看出我的 iMac 的一角（几乎看不到）， 并正确地将其识别为 `电视/显示器`：

.. image:: /_static/images/tutorials/pretrained_openvino/tvmonitor.png
  :alt: iMac

运行模型
#########

在验证完之后，让我们继续尝试其他模型，首先是 :code:`face-detection-retail-0004`.

要使用这个模型，只需用 :code:`-cnn` 标志指定要运行的模型名称，如下所示:

.. code-block:: bash

  python3 depthai_demo.py -dd -cnn face-detection-retail-0004

执行该脚本以查看人脸检测的带注释的视频流：

.. image:: /_static/images/tutorials/pretrained_openvino/face-2.png
  :alt: face

就这么简单。 当然，用你的脸代替我的。

如果你想尝试其他模型，只需浏览 `这里 <https://github.com/luxonis/depthai/tree/master/resources/nn>`__
然后按他们的名字运行，就像上面一样。

现在花一些时间来试用模型。 
例如，您可以检查模型可以检测到您的脸部的距离：

.. image:: /_static/images/tutorials/pretrained_openvino/face-3.png
  :alt: face

.. image:: /_static/images/tutorials/pretrained_openvino/face-4.png
  :alt: face

在第二张图片中，你可以看到我在一个很背光的环境下, 这是人脸检测（和其他特征检测）的主要挑战之一。
在这种情况下，很可能会限制可以检测到脸部的最大范围。
根据上面的测试，对于50％的置信度阈值，此范围似乎约为6米/20英尺。
你可以通过降低模型的置信度阈值 （从 :code:`0.5` `这里 <https://github.com/luxonis/depthai/blob/cdb902179590f0e7b684dde994369e137794a2ef/depthai.py#L233>`__ 降低）来获得更长的范围，但代价是增加了误报的可能性。

另一个限制因素是，这是一个相对低分辨率的模型（300x300像素），所以远处的人脸就特别小。
因此，让我们尝试另一种使用更高分辨率的人脸检测模型。

尝试其他模型
###################

我们走过的流程也适用于我们代码库中
(`这部分 <https://github.com/luxonis/depthai-python-extras/tree/master/resources/nn>`__ 的其他预训练对象检测模型)，其中包括：

- 零售业的人脸检测 (:code:`face-detection-retail-0004`)
- 用于驾驶辅助的面部检测 (:code:`face-detection-adas-0001`)
- 面部特征点检测, 简单 (:code:`landmarks-regression-retail-0009`)
- 面部特征点检测，高级 (:code:`facial-landmarks-35-adas-0002`)
- 情绪识别 (:code:`emotions-recognition-retail-0003`)
- 用于驾驶辅助的行人检测 (:code:`pedestrian-detection-adas-0002`)
- 零售环境下的人员检测 (:code:`person-detection-retail-0013`)
- 用于驾驶辅助的车辆检测 (:code:`vehicle-detection-adas-0002`)
- 车辆和车牌检测 (:code:`vehicle-license-plate-detection-barrier-0106`)

只需更改上述路径即可在其中运行其他模型，并添加正确的标签(或有趣的标签，如果您选择)

我们可以试试 :code:`face-detection-adas-0001`, 它的目的是检测车辆驾驶室内的人脸。
（ADAS 是 Advanced Driver-Assistance Systems 的缩写）

.. code-block:: bash

  python3 depthai_demo.py -dd -cnn face-detection-adas-0001

.. image:: /_static/images/tutorials/pretrained_openvino/face-5.png
  :alt: face

因此，尽管具有较高的分辨率，但该模型实际上具有较短的检测距离(相对较小的模型来说)。
为什么这么说呢？ 可能是因为它的目的是在车辆的驾驶室中使用，所以经过有意训练只能检测近距离的人脸。 （例如，您不想检测经过的汽车中的人脸。）

而且您可能还会注意到诸如情感识别之类的网络…… 
这些网络实际上旨在作为第二阶段网络运行（因为它们仅适用于仅包含面部的图像）。 
因此，要使用情绪识别网络，请使用以下命令告诉 DepthAI / megaAI 将其作为第二阶段运行：

.. code-block:: bash

  ./depthai.py -cnn face-detection-retail-0004 -cnn2 emotions-recognition-retail-0003 -dd -sh 12 -cmx 12 -nce 2

.. image:: /_static/images/tutorials/pretrained_openvino/uqhdqJG.png
  :alt: face

我们一直在运行的 :code:`-dd` 选项是什么？ 为什么会有这个选项呢？

之所以在这里，是因为我们想把最好的留到最后。 它代表禁用深度(并具有长格式选项 :code:`--disable_depth`)。 因此，如果删除它，DepthAI 现在将计算被检测对象的 3D 位置（在此示例中为人脸，但适用于任何对象检测器。）（如果您使用的是 microAI，则将其保留在那里，因为 microAI是单目相机，没有深度信息。）

这样您就可以获得被 **检测物体** （在本例中为我的脸）的 **完整3D位置** 。

这样就返回了以米为单位的完整 xyz 位置。 请看下面的内容。

空间AI–用3D定位增强模型的功能
#################################################

默认情况下，DepthAI 被设置为返回完整的 3D 位置。
所以在上面的命令中，我们实际上是用 :code:`-dd` （或 :code:`--disable_depth`）指定它不计算。 

因此，让我们运行同样的命令，但省略这一行，以便返回(并显示)3D 结果：

.. code-block:: bash

  python3 depthai_demo.py -cnn face-detection-adas-0001

.. image:: /_static/images/tutorials/pretrained_openvino/face-6.png
  :alt: face

在这里，您可以找到我杯子的 3D 位置！

你可以选择其他模型，改变标签，然后完成了 – 获得你关心的类的实时 3D 位置。

试用该功能，并在 `discuss.luxonis.com <https://discuss.luxonis.com/>`__ 
上分享您想出的演示（特别是如果您制造的机器人会跟踪您的猫），如果遇到任何问题，请在我们的  `Github <https://github.com/luxonis/depthai>`__ 上对我们进行ping操作。

并且，如果您在这些文档中发现任何错误，请在此页面底部的 `docs github <https://github.com/luxonis/depthai-docs-website>`__ 上报告问题，给予我们更正！

单目神经推理与双目深度的融合
**************************************************

我们将这种空间 AI 模式称为 '单目神经推理与双目深度的融合'。
把神经推理的边界框直接叠加在深度结果上，可以直观地看到这种模式是如何工作的。

为了直观显示，我们将结果直接叠加到原始深度信息上(在 OpenCV HOT colormap 中显示)：

.. code-block:: bash

  python3 depthai_demo.py -s metaout depth_raw -bb

.. image:: /_static/images/tutorials/pretrained_openvino/AjH1T2l.jpg
  :alt: AI overlaid on the RAW (uint16) Depth Map

所以这种 ’单目神经推理与双目深度的融合’ 的技术对于物体，尤其是较大的物体(如人、人脸等)很有效。

立体神经推理
***********************

下面我们将使用另一种技术，我们将其称为 '立体神经推理' (或 '立体 AI’)，这种技术对较小的物体，以及像面部特征点和姿态评估器结果等像素点特征都很有效。

.. image:: /_static/images/tutorials/pretrained_openvino/mKuzWI6.png
  :alt: Stereo Neural inference mode

可以使用以下命令运行：

.. code-block:: bash

  ./depthai_demo.py -cnn face-detection-retail-0004 -cnn2 landmarks-regression-retail-0009 -cam left_right -dd -sh 12 -cmx 12 -nce 2 -monor 400 -monof 30

需要注意的是，这既是在运行并行神经推理(即在两个摄像头上)，也是在运行串行神经推理(特征点回归网络是在人脸检测器的结果上运行)。

.. include::  /pages/includes/footer-short.rst

