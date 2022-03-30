使用 DepthAI 的第一步
========================

你好 DepthAI 的用户们！

在本教程中，我们假设您刚拿到您的 OAK 设备 (例如 `OAK-D <https://www.oakchina.cn/product/oak-d/>`__)
，并且您想第一次尝试探索它的可能性以及使用它可以实现的目标。

- 首先，我们将运行一个 DepthAI 演示脚本，它允许您预览 DepthAI 功能。
- 接下来，我将介绍演示脚本中的一些可用选项并展示它们的用法/结果。
- 在教程的最后，您将得到一些有用的链接，用来进一步扩展您的知识，并通过查看开源用例实现、代码示例和教程，您可以将其用作项目的起点。

让我们从下面的设备设置开始

连接 OAK 设备 (USB)
############################

如果您的 OAK 配赠了 USB 数据线，我们建议使用该数据线将 OAK 相机连接到主机。

.. warning::
  确保使用 **USB3 cable!** 如果没有，请在程序中 :ref:`强制使用 USB2 通信 <Forcing USB2 Communication>` 。

.. image:: /_static/images/tutorials/usb3.png

**USB3 数据线** 在 USB-C 数据线的 USB-A 连接器内侧 **呈蓝色** 。

请确保设备直接连接到您的主机（可以是 PC 或 Raspberry Pi 或其他有能力的设备）到 USB 端口，或通过有源 USB 集线器。

在 Ubuntu 上，您可以通过运行下面命令检查是否检测到新的 USB 设备

.. code-block:: bash

  $ lsusb | grep MyriadX
  Bus 003 Device 002: ID 03e7:2485 Intel Movidius MyriadX

.. note::
  如果您运行的是 Ubuntu 以外的其他操作系统，或者您认为出现问题，我们提供了详细的操作系统特定安装指南
  `点击这里 <https://docs.luxonis.com/projects/api/en/latest/install/#supported-platforms>`__。

连接 OAK 设备 (PoE)
############################

如果您使用的是 PoE 设备，请遵循 `OAK PoE 设备入门 <https://docs.luxonis.com/projects/hardware/en/latest/pages/guides/getting-started-with-poe.html>`__ 教程。

使用 Windows 安装程序
#####################

如果您愿意，我们将所有设置过程封装在一个 :code:`.exe` 文件中，您可以 |windows_installer_url| 下载并跳过下面的 :ref:`设置` 部分。

下载并运行后，它将安装所有必要的组件和软件包要求。完成后，它将自动运行演示脚本。

设置
#####

在本节中，我们将介绍如何使用命令行手动安装演示脚本。

下载演示脚本
********************

要下载演示脚本，您可以使用 :code:`git` 或直接下载 zip 文件

从 zip 文件下载
-------------

首先，`点击这里 <https://github.com/luxonis/depthai/archive/refs/heads/main.zip>`__
下载存储库包，然后将下载的存储库包解压到您想放的目录。接下来，在此目录中打开一个终端会话。

从 git 下载
--------

首先，打开终端会话并转到您想要下载演示脚本的目录。然后，运行以下代码下载演示脚本

.. code-block:: bash

  $ git clone https://github.com/luxonis/depthai.git

下载完存储库后，请确保通过运行进入下载的存储库

.. code-block:: bash

  $ cd depthai

创建 python virtualenv (可选)
***********************************

要创建和使用 virtualenv，您可以按照 `官方 python 指南创建 virtualenvs <https://docs.python.org/3/tutorial/venv.html>`__ 
或者你按照网络上特定于操作系统的指南，例如 `"如何在 Ubuntu 20.04 上创建 Python3 虚拟环境" <https://linoxide.com/how-to-create-python-virtual-environment-on-ubuntu-20-04/>`__

这将确保您使用的是新环境并且 Python 3 是默认解释器 - 这有助于预防潜在的问题。

我通常通过运行下面命令来创建和使用 virtualenvs

.. code-block:: bash

  $ python3 -m venv myvenv
  $ source myvenv/bin/activate
  $ pip install -U pip

这可能需要运行下面命令事先安装这些软件包

.. code-block:: bash

  $ apt-get install python3-pip python3-venv

安装需求
********************

下载演示源代码并设置好终端会话后，接下来要做的就是安装此脚本所需的所有其他软件包（连同 :code:`depthai` Python API 本身）。

要安装这些软件包，请运行 :code:`install_requirements.py` 脚本

.. code-block:: bash

  $ python3 install_requirements.py

.. warning::

  如果您使用的是 Linux 系统，大多数情况下您必须为我们的脚本添加新的 udev 规则才能正确访问设备。您可以通过运行下面命令添加和应用新规则

  .. code-block:: bash

    $ echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
    $ sudo udevadm control --reload-rules && sudo udevadm trigger 

现在，您应该可以开始使用演示脚本了，我们现在就来做吧

运行演示脚本
***************

设置完所有内容后，我们现在已准备好通过运行

.. code-block:: bash

  $ python3 depthai_demo.py

默认运行
###########

首次运行演示时，该脚本将编译并下载默认的 `mobilenet-ssd` 模型，
配置 OAK 相机，然后显示默认 :code:`color` 预览，其中包含来自您设备的 RGB 相机的缩放预览。

.. image:: https://user-images.githubusercontent.com/5244214/142722740-47e545b7-c7fe-4132-9704-ae3b47d60957.png
  :alt: Default run

更改预览
##############

要查看设备的其他预览，您可以使用 GUI 左上角的下拉框进行切换切换

.. image:: https://user-images.githubusercontent.com/5244214/141984256-4f9b9479-0907-4b04-bfcd-aae15ac28a0a.png
  :alt: preview selector location

.. list-table:: Available previews
  :widths: 15 65 20
  :header-rows: 1
  :align: center

  * - 名字
    - 描述
    - 局限性

  * - :code:`color`
    - 显示从彩色相机的预览
    -

  * - :code:`nnInput`
    - 显示从右相机的预览
    - 在没有 AI 模型运行时禁用

  * - :code:`left`
    - 显示从左相机的预览
    - 需要 **OAK-D**

  * - :code:`right`
    - 显示从右相机的预览
    - 需要 **OAK-D**

  * - :code:`depth`
    - 显示根据 :code:`depthRaw` 预览和 JET 颜色计算的视差图。最适合可视化深度
    - 需要 **OAK-D**

  * - :code:`depthRaw`
    - 显示原始深度图。最适合基于深度的计算
    - 需要 **OAK-D**

  * - :code:`disparity`
    - 显示设备上生成的视差图
    - 需要 **OAK-D**

  * - :code:`disparityColor`
    - 显示设备上产生的视差图和 JET 彩色。应与 :code:`depth` 预览相同，但在设备上生成。
    - 需要 **OAK-D**

  * - :code:`rectifiedLeft`
    - `修改 <https://en.wikipedia.org/wiki/Image_rectification>`__ 左相机帧
    - 需要 **OAK-D**

  * - :code:`rectifiedRight`
    - `修改 <https://en.wikipedia.org/wiki/Image_rectification>`__ 右相机帧
    - 需要 **OAK-D**


默认模型
#############

在演示运行时，您可以看到检测结果 - 如果您站在摄像机前，您应该看到自己被检测为一个概率非常高的人。

默认情况下使用的模型是在 `PASCAL 2007 VOC <http://host.robots.ox.ac.uk/pascal/VOC/voc2007/>`__ 上训练的 MobileNetv2 SSD 对象检测器，种类包括：

- Person： 人
- Animal： 鸟，猫，牛，狗，马，羊
- Vehicle： 飞机，自行车，船，公共汽车，汽车，摩托车，火车
- Indoor： 瓶子，椅子，餐桌，盆栽植物，沙发，电视/显示器

因此，请尝试检测不同的物体，例如瓶子或苹果

.. image:: https://user-images.githubusercontent.com/5244214/142192279-71e479ae-fef2-4ddb-a4d2-7ae677e1544d.png
  :alt: bottles and apples

甚至猫

.. image:: https://user-images.githubusercontent.com/5244214/142493174-85b894fb-7a2b-40a1-8fad-0937ca5f27e1.png
  :alt: cat

使用其他模型
##################

我们准备了其他模型，您可以尝试和轻松评估。
要运行演示脚本，例如 :code:`face-detection-retail-0004`, 单击组合框 :code:`CNN Model` 并选择提到的模型

.. image:: https://user-images.githubusercontent.com/5244214/144452804-b776461b-0b93-4397-a702-15cb1ec04fb7.png
  :alt: cnn model location

这将允许您检测人脸，如下所示


.. image:: https://user-images.githubusercontent.com/5244214/142645887-f04b90e4-c69f-4764-93d3-37bfb05fe67c.png
  :alt: face

您可以使用此组合框更改在深度AI上运行的模型。也可以使用命令行选择

.. code-block:: bash

  $ python3 depthai_demo.py -cnn face-detection-retail-0004

下面，有一个您可以使用的模型列表，仅下载演示脚本


.. list-table:: 可用模型
  :widths: 30 50 10 10
  :header-rows: 1
  :align: center

  * - 名字
    - 目的
    - 帧数
    - 链接
  * - :code:`deeplabv3p_person`
    - 人员细分
    - 22.1
    - :ref:`Usage <deeplabv3p_person>`
  * - :code:`face-detection-adas-0001`
    - 人脸检测
    - 13.4
    - :ref:`Usage <face-detection-adas-0001>`
  * - :code:`face-detection-retail-0004`
    - 人脸检测
    - 30.0
    - :ref:`Usage <face-detection-retail-0004>`
  * - :code:`mobilenet-ssd`
    - 物体检测（20种）
    - 30.0
    - :ref:`Usage <mobilenet-ssd>`
  * - :code:`pedestrian-detection-adas-0002`
    - 人员检测
    - 13.1
    - :ref:`Usage <pedestrian-detection-adas-0002>`
  * - :code:`person-detection-retail-0013`
    - 人员检测
    - 10.7
    - :ref:`Usage <person-detection-retail-0013>`
  * - :code:`person-vehicle-bike-detection-crossroad-1016`
    - 人员、车辆和自行车检测
    - 6.2
    - :ref:`Usage <person-vehicle-bike-detection-crossroad-1016>`
  * - :code:`yolo-v3`
    - 物体检测（80种）
    - 1.9
    - :ref:`Usage <yolo-v3>`
  * - :code:`tiny-yolo-v3`
    - 物体检测（80种）
    - 29.9
    - :ref:`Usage <tiny-yolo-v3>`
  * - :code:`vehicle-detection-adas-0002`
    - 车辆检测
    - 14.0
    - :ref:`Usage <vehicle-detection-adas-0002>`
  * - :code:`vehicle-license-plate-detection-barrier-0106`
    - 车牌检测
    - 30.0
    - :ref:`Usage <vehicle-license-plate-detection-barrier-0106>`
  * - :code:`openpose2`
    - 姿势估计 (openpose)
    - 6.5
    - :ref:`Usage <openpose2>`
  * - :code:`human-pose-estimation-0001`
    - 姿势估计 (intel)
    - 7.3
    - :ref:`Usage <human-pose-estimation-0001>`

.. _deeplabv3p_person:

- :code:`deeplabv3p_person` - 允许突出显示图像中检测到人的部分

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn deeplabv3p_person

  .. image:: https://user-images.githubusercontent.com/5244214/142645574-2da1b6cd-278b-44d8-8f1a-1d9921d976bf.png
    :alt: deeplabv3p_person

.. _face-detection-adas-0001:

- :code:`face-detection-adas-0001` - 允许检测图像上的人脸（较慢）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn face-detection-adas-0001

  .. image:: https://user-images.githubusercontent.com/5244214/142645887-f04b90e4-c69f-4764-93d3-37bfb05fe67c.png
    :alt: face-detection-adas-0001

.. _face-detection-retail-0004:

- :code:`face-detection-retail-0004` - 允许检测图像上的人脸（较快）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn face-detection-retail-0004

  .. image:: https://user-images.githubusercontent.com/5244214/142645887-f04b90e4-c69f-4764-93d3-37bfb05fe67c.png
    :alt: face-detection-retail-0004

.. _mobilenet-ssd:

- :code:`mobilenet-ssd` - 对象检测器，可检测20个不同的种类（默认）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn mobilenet-ssd

  .. image:: https://user-images.githubusercontent.com/5244214/142192279-71e479ae-fef2-4ddb-a4d2-7ae677e1544d.png
    :alt: mobilenet-ssd

.. _pedestrian-detection-adas-0002:

- :code:`pedestrian-detection-adas-0002` - 允许检测图像上的人（较慢）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn pedestrian-detection-adas-0002

  .. image:: https://user-images.githubusercontent.com/5244214/142723213-93cfed86-cb53-4b9b-85cb-4435a0be631d.png
    :alt: pedestrian-detection-adas-0002

.. _person-detection-retail-0013:

- :code:`person-detection-retail-0013` - 允许检测图像上的人（较快）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn person-detection-retail-0013

  .. image:: https://user-images.githubusercontent.com/5244214/142723320-51d004ea-1163-4aa1-a53c-126ab2c259cb.png
    :alt: person-detection-retail-0013

.. _person-vehicle-bike-detection-crossroad-1016:

- :code:`person-vehicle-bike-detection-crossroad-1016` - 允许在图像上检测人，自行车和车辆

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn person-vehicle-bike-detection-crossroad-1016

  .. image:: https://user-images.githubusercontent.com/5244214/117144527-4fd3da00-adb2-11eb-89a4-2733cd9a39af.png
    :alt: person-vehicle-bike-detection-crossroad-1016

.. _yolo-v3:

- :code:`yolo-v3` - 对象检测器，可检测80个不同的类别（较慢）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn yolo-v3

  .. image:: https://user-images.githubusercontent.com/5244214/142192279-71e479ae-fef2-4ddb-a4d2-7ae677e1544d.png
    :alt: yolo-v3

.. _tiny-yolo-v3:
默认
- :code:`tiny-yolo-v3` - 对象检测器，可检测80个不同的类别（较快）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn tiny-yolo-v3

  .. image:: https://user-images.githubusercontent.com/5244214/142192279-71e479ae-fef2-4ddb-a4d2-7ae677e1544d.png
    :alt: tiny-yolo-v3

.. _vehicle-detection-adas-0002:

- :code:`vehicle-detection-adas-0002` - 允许检测图像上的车辆

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn vehicle-detection-adas-0002

  .. image:: https://user-images.githubusercontent.com/5244214/142653915-cce665f1-d646-4621-a3a5-0006dc2273cb.png
    :alt: vehicle-detection-adas-0002

.. _vehicle-license-plate-detection-barrier-0106:

- :code:`vehicle-license-plate-detection-barrier-0106` - 允许在图像上检测车辆和车牌（仅限中国车牌）

  .. code-block:: bash

    $ python3 depthai_demo.py -cnn vehicle-license-plate-detection-barrier-0106

  .. image:: https://user-images.githubusercontent.com/5244214/142654408-9e16c2bb-2ca7-451b-a384-d1fc631a365f.png
    :alt: vehicle-license-plate-detection-barrier-0106

.. _openpose2:

- :code:`openpose2` - 人体姿势估计模型


  .. code-block:: bash

    $ python3 depthai_demo.py -cnn openpose2

  .. image:: https://user-images.githubusercontent.com/5244214/142654924-7286b066-2ccc-4879-8d88-c5580dbbfcb4.png
    :alt: openpose2

.. _human-pose-estimation-0001:

- :code:`human-pose-estimation-0001` - 来自 `Open Model Zoo <https://github.com/openvinotoolkit/open_model_zoo/tree/master/models/intel/human-pose-estimation-0001>`__ 人体姿势估计模型


  .. code-block:: bash

    $ python3 depthai_demo.py -cnn human-pose-estimation-0001

  .. image:: https://user-images.githubusercontent.com/5244214/107493701-35f97100-6b8e-11eb-8b13-02a7a8dbec21.gif
    :alt: human-pose-estimation-0001


我们用于下载和编译模型的所有数据都可以 `点击这里 <https://github.com/luxonis/depthai/tree/main/resources/nn>`__ 找到。

演示用法
##########

在本节中，我们将介绍演示脚本中提供的配置选项，允许您尝试不同的配置

  .. image:: https://user-images.githubusercontent.com/5244214/142490950-61518cce-e6c0-429a-a381-e8082bc3fe9f.png
    :alt: Config options

AI 属性
*************


  .. image:: https://user-images.githubusercontent.com/5244214/142454791-4da7aaf1-9b89-4201-970e-1f0814e82322.png
    :alt: AI config

开关：

- **已启用**：打开/关闭 AI。关闭它将阻止任何神经网络运行，这也将节省一些内存。适用于我们更专注于深度/编码而不是AI处理的情况

基本属性：

- **CNN Model**: ：选择要在 DepthAI 上运行的模型，有关详细信息，请参阅 :ref:`使用其他模型`
- **SHAVEs**: ：确定用于编译神经网络的 SHAVE 内核数。该值越高，网络运行速度越快，但这也限制了可以立即启用的功能。
- **Model source**: 指定将哪些相机预览作为模型输入，以便将哪些帧发送到神经网络以执行推理
- **Full FOV**: 如果启用，它将缩小相机图像以满足 nn 输入大小。如果禁用，则在缩放之前，它将裁剪图像以满足 NN 宽高比

高级：

- **OpenVINO version**: 指定将用于编译 MyriadX blob 并运行管道的 OpenVINO 版本。在大多数情况下，建议使用最新版本
- **Label to count**: 允许显示演示中可见的特定标签的次数 （例如，如果要使用默认模型计算预览中的猫的数量，则可以通过选择 :code:`cat` 作为计数标签并重新启动脚本来执行此操作）
- **Spatial bounding box**: 启用后，将在深度预览上绘制一个边界框，显示检测区域的哪个部分已进入深度估计。
- **SBB Scale Factor**: 确定空间边界框与检测边界框相比的大小。


深度属性
****************

  .. image:: https://user-images.githubusercontent.com/5244214/142464446-5c358c7b-4770-4416-8a4d-ae397e8c2657.png
    :alt: Depth config

开关：

- **Enabled**: 打开/关闭深度。将其关闭将阻止创建立体声节点，这也将节省一些内存。对于我们更专注于AI处理/编码而不是深度的情况非常有用。
- **Use Disparity**: 如果未设置（默认），演示脚本将根据深度图计算主机上的视差图。如果启用，将在设备上执行相同的处理，消耗一些内存，但限制主机资源的使用。

基本属性：

- **Median Filtering**: 指定应用于深度图的噪声消除中值滤镜的类型
- **Subpixel**: 启用子像素模式，可提高深度精度，对于长距离测量特别有用
- **Left Right Check**: 启用左右检查，用于删除由于对象边框处的遮挡而计算错误的视差像素
- **Extended Disparity**: 启用扩展视差模式，允许给定基线的最小距离更近
- **Depth Range**: 指定设备计算的最小和最大距离
- **LRC Threshold**: 指定使像素失效的视差像素之间的最大差值（阈值越高，通过的点越多）

相机属性
*****************

  .. image:: https://user-images.githubusercontent.com/5244214/142468945-2c0374ee-ffa7-4ff6-8332-6dc134cc10aa.png
    :alt: Camera config

基本属性：

- **FPS**: 指定相机捕获帧的速度
- **Resolution**:  指定相机显示分辨率，从而指定捕获的帧大小

高级：

- **ISO**: 控制相机的聚光能力
- **Exposure**: 控制相机的曝光时间
- **Saturation**: 控制帧中颜色的强度
- **Contrast**: 控制帧中不同音调的视觉比例
- **Brightness**: 控制帧中颜色的暗度或亮度
- **Sharpness**: 控制帧中细节的清晰度

其他
****

  .. image:: https://user-images.githubusercontent.com/5244214/142469155-e02593ae-80f4-452d-9bed-a3e50dc31e4f.png
    :alt: Misc

录制：

- **Switches**: 启用指定摄像机的录制
- **FPS inputs**: 指定录制 FPS（默认 30）
- **Destination**: 指定将存储录制文件的 **目录路径** 

日志:

- **Switches**: 启用指定功能的日志记录
- **Destination**: 指定将存储报表文件的 **文件路径**

使用自定义模型
###################

.. warning::

  使用自定义模型需要本地下载的 depthai 存储库版本。
  **如果一直使用安装程序** 下载并运行脚本， **添加自定义模型将不起作用**。
  请按照 :ref:`设置` 部分了解如何下载和设置存储库。 


假设您要运行从 model zoo 下载或自己训练（或两者）的自定义模型。
要让模型可在DepthAI上运行，必须将其编译为MyriadX blob格式 - 这是模型的优化版本，能够利用MyriadX芯片作为处理单元。

在我们的演示脚本中，我们支持运行自定义 Blob 的几种方法，下面将介绍这些方法。例如，我将添加一个名为 :code:`custom_model` （替换为您的首选名称）
的自定义人脸检测网络，并使用演示脚本运行它

编译 MyriadX blob
********************

要接收 MyriadX blob，网络必须已经采用 OpenVINO IR 格式（由 :code:`.xml` 和 :code:`.bin`
文件组成），用于编译。我们不会在这里重点讨论如何为您的模型获取此表示的方法，但请务必查看 `官方 OpenVINO 转换指南 <https://docs.oakchina.cn/en/latest/pages/model_conversion.html>`__.

要转换 :code:`custom_model.xml` 和 :code:`custom_model.bin`，我们将使用 `blobconverter cli <https://pypi.org/project/blobconverter/>`__ - 我们的工具，利用 `Online MyriadX blob 转换器 <http://blobconverter.luxonis.com/>`__ 来执行转换。
在这种情况下，不需要本地 OpenVINO 安装，因为所有依赖项都已安装在服务器上。如果您的模型是 TensorFlow 或 Caffe 格式，您仍然可以使用我们的工具进行转换，只需注意，您必须使用不同的输入标志，有时还提供自定义模型优化器参数 （:ref:`阅读更多 <Converting model to MyriadX blob>`）


首先，让我们从 `PyPi <https://pypi.org/project/blobconverter/>`__ 安装 :code:`blobconverter`

.. code-block:: bash

  $ python3 -m pip install -U blobconverter

现在，安装 :code:`blobconverter` 后，我们可以使用以下命令编译IR文件

.. code-block:: bash

  $ python3 -m blobconverter --openvino-xml /path/to/custom_model.xml --openvino-bin /path/to/custom_model.bin

通过运行此命令，:code:`blobconverter` 向 BlobConverter API 发送请求，以对提供的文件执行模型编译。
编译后，API 会使用一个 :code:`.blob` 文件进行响应，并删除随请求一起发送的所有源文件。

编译成功后，:code:`blobconverter` 返回下载的 blob 文件的路径。
由于:code:`depthai` 存储库需要此 blob，因此让我们将其移动到此处 

.. code-block:: bash

  $ mkdir <depthai_repo>/resources/nn/custom_model
  $ mv <path_to_blob> <depthai_repo>/resources/nn/custom_model

配置
*************

我们需要为演示脚本提供一些其他配置来运行此 Blob。
演示脚本将查找有关 :code:`custom_model.json` 如何配置 pipeline 和分析结果的详细信息。

如果您的模型基于 MobileNetSSD 或 Yolo，则可以使用我们的 :code:`detection` 输出格式。
如果它是不同类型的网络，则可以使用 :code:`raw` （默认）输出格式并提供自定义处理程序文件来解码和显示 NN 结果。

您可以使用这些配置示例来自定义 :code:`custom_model.json` 内部 :code:`resources/nn/custom_model` 的目录

- **MobileNetSSD** （我们将使用此配置）

.. code-block:: json

  {
      "nn_config":
      {
          "output_format" : "detection",
          "NN_family" : "mobilenet",
          "confidence_threshold" : 0.5,
          "input_size": "300x300"
      },
      "mappings":
      {
          "labels":
          [
              "unknown",
              "face"
          ]
      }
  }

- **Yolo**

.. code-block:: json

  {
      "nn_config":
      {
          "output_format" : "detection",
          "NN_family" : "YOLO",
          "input_size": "416x416",
          "NN_specific_metadata" :
          {
              "classes" : 80,
              "coordinates" : 4,
              "anchors" : [10,14, 23,27, 37,58, 81,82, 135,169, 344,319],
              "anchor_masks" :
              {
                  "side26" : [1,2,3],
                  "side13" : [3,4,5]
              },
              "iou_threshold" : 0.5,
              "confidence_threshold" : 0.5
          }
      },
      "mappings":
      {
          "labels":
          [
                "unknown",
                "face"
          ]
      }
  }


- **Raw** （请参阅 :ref:`自定义处理程序 <Custom handler>` 关于如何创建 :code:`handler.py` 文件的详细信息）

.. code-block:: json

  {
      "nn_config": {
          "output_format" : "raw",
          "input_size": "300x300"
      },
      "handler": "handler.py"
  }

运行演示脚本
*******************

文件就位后，我们现在可以使用自定义模型运行演示

.. image:: https://user-images.githubusercontent.com/5244214/142723365-a3f1e369-1b2f-4e22-b0e6-600463134352.png
  :alt: custom model location

您应该看到输出和显示的NN结果 （或者如果 :code:`raw` 已选择并且没有处理程序文件，则在控制台中打印）


.. image:: https://user-images.githubusercontent.com/5244214/142645887-f04b90e4-c69f-4764-93d3-37bfb05fe67c.png
  :alt: custom model

请务必查看下面的高级部分或参阅 :ref:`Next steps <Next steps>` 

自定义处理程序  
**************

自定义处理程序是演示脚本将加载和执行以分析 NN 结果的文件。我们使用配置文件值指定
:code:`handler` 文件，并指定首选文件的路径。它还需要 :code:`raw` 输出格式，
因为它阻止脚本处理结果本身。

:code:`handler.py`文件应包含两种方法 - :code:`decode(nn_manager, packet)` 和 :code:`draw(nn_manager, data, frames)`

.. code-block:: python

  def decode(nn_manager, packet):
    pass

  def draw(nn_manager, data, frames):
    pass

The first method, :code:`decode`, is called whenever a NN packet arrives from the pipeline (stored as a :code:`packet` param)
also providing a :code:`nn_manager` object that contains all nn-related info that was used by the script (like input size etc.).
The goal of this function is to decode the received packets from the NN blob into meaningful results that can later be displayed.


The second method，:code:`draw`, is called with the NN results (returned from :code:`decode`), :code:`nn_manager` object and
:code:`frames` array, having :code:`[(<frame_name>, <frame>), (<frame_name>, <frame>), ...]` items. This array will
contain frames that were specified with the :code:`-s/--show` param.
The goal of this function is to draw the decoded results onto received frames.

在下面，您可以找到一个 :code:`handle.py` 示例文件，该文件解码并显示基于MobilenetSSD的结果。

.. code-block:: python

  import cv2
  import numpy as np
  from depthai_helpers.utils import frame_norm


  def decode(nn_manager, packet):
      bboxes = np.array(packet.getFirstLayerFp16())
      bboxes = bboxes.reshape((bboxes.size // 7, 7))
      bboxes = bboxes[bboxes[:, 2] > 0.5]
      labels = bboxes[:, 1].astype(int)
      confidences = bboxes[:, 2]
      bboxes = bboxes[:, 3:7]
      return {
          "labels": labels,
          "confidences": confidences,
          "bboxes": bboxes
      }


  decoded = ["unknown", "face"]


  def draw(nn_manager, data, frames):
      for name, frame in frames:
          if name == nn_manager.source:
              for label, conf, raw_bbox in zip(*data.values()):
                  bbox = frame_norm(frame, raw_bbox)
                  cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                  cv2.putText(frame, decoded[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                  cv2.putText(frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)

使用自定义人脸检测模型，使用此代码我们接收以下输出

.. image:: https://user-images.githubusercontent.com/5244214/142723407-0dde0f8c-1ce6-4fa7-94bb-9fce5dff9804.png
  :alt: custom model custom handler

我们已经使用此处理程序机制来解码 `deeplabv3p_person <https://github.com/luxonis/depthai/blob/main/resources/nn/deeplabv3p_person/handler.py>`__，
这是演示脚本中可用的网络之一。

按需编译
*********************

由于 IR 格式的文件可能很大，并且我们既要下载 blob，又要将 IR 格式上传到服务器，因此我们合并了类似 OpenVINO 的文件结构，BlobConverter 服务器也在内部使用。
您可以在 `OpenVINO model zoo <https://github.com/openvinotoolkit/open_model_zoo/blob/master/models/intel/face-detection-retail-0004/model.yml>`__
或者在 `演示脚本中的可用模型中 <https://github.com/luxonis/depthai/blob/main/resources/nn/tiny-yolo-v3/model.yml>`__查看此文件的样子。

此文件由 `OpenVINO 模型下载程序 <https://github.com/openvinotoolkit/open_model_zoo/tree/master/tools/downloader>`__ 使用，
该下载程序用于下载编译所需的文件。在我们的演示脚本中，我们使用这些文件来提供 NN 源文件的 URL，而不是将它们与源代码一起上传。它也很有用，因为按需编译允许我们在请求不同数量的 MyriadX SHAVE 内核时使用相同的配置。

要下载 blob 文件请使用 :code:`model.yml` 文件, 运行下面的命令

.. code-block:: bash

  $ python3 -m blobconverter --raw-config /path/to/model.yml --raw-name custom_model

您也可以将 `model.yml` 文件保留在 `resources/nn/<name>` 目录中。这将使演示脚本为你执行转换并运行收到的 blob

.. code-block:: bash

  $ python3 depthai_demo.py -cnn <name>

自定义演示代码
#######################

Callbacks 文件
**************

如果您想自己向演示中添加一些自定义功能，或者只是检查某些变量的外观，则可以使用 callbacks 文件，该文件应包含演示在执行特定事件期间将调用的方法。

存储库中 `可用的 <https://github.com/luxonis/depthai/blob/main/callbacks.py>`__ Callbacks 文件示例如下

.. code-block:: python

    def shouldRun():
      pass  # Called to determine if the demo should be running


    def onNewFrame(frame, source):
      pass  # Called when a new frame is available


    def onShowFrame(frame, source):
        pass  # Called when a frame is about to be displayed


    def onNn(nn_packet):
        pass  # Called when a new NN packet is available


    def onReport(report):
        pass  # Called when a new report is available


    def onSetup(*args, **kwargs):
        pass  # Called when the demo script is setting up


    def onTeardown(*args, **kwargs):
        pass  # Called when the demo script is finishing


    def onIter(*args, **kwargs):
        pass  # Called on each demo script iteration (internal loop)

这些方法允许在演示脚本本身之上构建自定义功能，无论是打印或计算来自 NN 的数据，还是修改如何显示帧，甚至建立自定义数据库/API 连接以将数据发送到外部目标。

默认情况下，演示脚本将使用存储库中的 :code:`callbacks.py` 文件，但此路径可以使用
:code:`-cb <path> / --callback <path>` 参数更改信号

将演示作为类导入
*************************

如果愿意，演示脚本也可以像常规类一样导入 - 这允许控制演示何时启动以及何时运行。如何从 Python 代码运行演示的简单示例如下所示

.. code-block:: python

    from depthai_demo import Demo
    from depthai_helpers.arg_manager import parseArgs
    from depthai_helpers.config_manager import ConfigManager

    args = parseArgs()
    conf = ConfigManager(args)
    demo = Demo(onNewFrame=<fn>, onShowFrame=<fn>, onNn=<fn>, onReport=<fn>, onSetup=<fn>, onTeardown=<fn>, onIter=<fn>) # all params are optional
    demo.run_all(conf)

请记住，:code:`PYTHONPATH` 必须包含在环境变量内为 depthai 存储库导入以用来使用。
或者，您可以使用放在存储库中 :code:`depthai_demo.py` 脚本。

Next steps
##########

在前面的部分中，我们学习了如何预览基本的深度AI功能。从这一点开始，您可以进一步探索 DeptheAI 世界

- **Looking for inspiration?**

  Check our :ref:`Example Use Cases` for ready to use applications that solve a specific problem on DepthAI

- **Want to start coding?**

  Be sure to check `hello world tutorial on API section <https://docs.luxonis.com/projects/api/en/latest/tutorials/hello_world/>`__ for a step-by-step introduction to the API

- **Want to train and deploy a custom model to DepthAI?**

  Visit :ref:`Custom training` page for ready to use Colab notebooks

.. include::  /pages/includes/footer-short.rst
