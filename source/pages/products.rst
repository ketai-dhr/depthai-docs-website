产品展示
========

.. _bw1097:

BW1097 - RaspberryPi 计算模块
###################################

.. image:: /_static/images/products/bw1097.jpg
  :alt: RaspberryPi Compute Module

Raspberry Pi 计算模块版提供了所需的一切：板载带有 4K，60 Hz 彩色摄像机的预先校准的立体摄像机以及带有在启动时自动运行的 Raspbian 和 DepthAI Python 代码的µSD 卡。这允许使用 DepthAI 的功能，而实际上无需键入或单击即可：它只是启动它的工作。然后，您可以通过单行更改来修改 Python 代码，从而为要定位的对象替换神经模型。

- 内置 RaspberryPi 计算模块
- 三个继承摄像头
- 完整的系统；包括您需要的一切

电路板布局
*****************************************

.. image:: /_static/images/products/bw1097-top.jpg
  :alt: 1097 top

.. image:: /_static/images/products/bw1097-bottom.jpg
  :alt: 1097 bottom

.. list-table:: Reference table
  :widths: 50 50
  :align: center

  * - **A.** 720p 120 Hz 全局快门（右）
    - **J.** 1x Solderable USB2.0
  * - **B.** DepthAI Module
    - **K.** 720p 120 Hz 全局快门（左）
  * - **C.** DepthAI 复位按钮
    - **L.** 4K 60 Hz Color
  * - **D.** 5 V IN
    - **M.** RPi 40-Pin GPIO Header
  * - **E.** HDMI
    - **O.** RPi USB-Boot
  * - **F.** 16 GB µSD Card, Pre-configured
    - **P.** RPi Display Port
  * - **G.** 3.5 mm Audio
    - **Q.** RPi Camera Port
  * - **H.** Ethernet
    - **R.** Raspberry Pi Compute Module 3B+
  * - **I.** 2个 USB2.0
    -

盒子里有什么东西?
*******************************************

- BW1097 载板
- 预先加载了 Raspbian 10 和 DepthAI 的µSD卡

  - 默认密码: :code:`luxonis`

- Wi-Fi USB 加密狗
- 电源供应

设定
********************************************

开始:

#. **将显示器连接到 HDMI端口。**

    请注意，不包括HDML电缆。

#. **通过 USB 端口连接键盘和鼠标**
#. **连接电源（随附）。**

    在启动时, Pi 将运行 `Python演示脚本 <https://github.com/luxonis/depthai/blob/main/depthai_demo.py>`__ 该脚本显示带有对象本地化元数据注释的视频流：:

    .. image:: /_static/images/products/bw1097-detection.png
      :alt: 1097 top

    在上面的屏幕截图中，DepthAI确定了电视监视器（距离相机1.286 m）和椅子（距离相机3.711 m）。
    请参阅GitHub上的 `对象标签列表 <https://github.com/luxonis/depthai/blob/main/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L10>`__ 。

#. **连接到互联网。**

    将Pi连接到Internet，开始尝试DepthAI教程和示例。

    * **连接到 WiFi 网络**

        要连接到WiFi网络，请使用随附的Linux兼容USB WiFi加密狗。Pi应识别加密狗并在Raspbian Desktop UI的右上角显示可用的WiFi网络。

    * **通过以太网连接到网络**

        该评估板包括一个以太网端口。将以太网电缆连接到端口将启用Internet访问。

#. **运行示例脚本**.

  请参阅 :ref:`验证安装`

[可选] 使用自己的SD卡
*********************************

如果您希望自己(例如更大)的 SD 卡设置 DetphAI，则有两种选择：:

#.  在此处下载我们为 BW1097(Raspberry Pi 计算模块版)预先配置的 Raspbian 映像: `BW1097 Raspian Image <https://drive.google.com/open?id=19JRcRkdmiJ96lsoMdCu2_zbbYrSG7wsu>`__. 然后, 在下载后，更新DepthAI固件/软件（通过在桌面上检出的DepthAI代码库上执行git pull）。
#.  根据您的喜好设置自己的 Raspbian，例如从 Raspbian 下载新文件，然后使用以下两个文件在/ boot 中替换 dt-blob.bin 和 configtxt:

    - `dt-blob.bin <https://drive.google.com/open?id=1OarNtX58YUtVcqHog8NnnCWmCgYpN-z_>`__ - 用于启用Pi MIPI显示
    - `config.txt <https://drive.google.com/open?id=1cg8OZVFwq6NB1judrsUNV6T7YIcYX1eD>`__ - 用于启用3.5mm耳机接口


.. _bw1094:

BW1094 - RaspberryPi Hat
###############################


.. image:: /_static/images/products/bw1094.jpg
  :alt: RPi HAT Labeled

Raspberry Pi HAT Edition 允许使用您已经拥有的 Raspberry Pi，并通过 Pi GPIO 进行传递，以便它们仍可在您的系统中访问和使用。它的模块化摄像机可将其安装到您需要的平台上，距 HAT 最多六英寸。

- 作为 HAT 挂载到 Raspberry Pi 上，以便轻松集成
- 仍然可以通过传递标头访问所有 Raspberry Pi GPIO
- 使用 6 英寸柔性扁平电缆的柔性摄像机安装
- 包括三个 FFC 摄像机端口

要求
*******************************

- 具有扩展的 40 引脚 GPIO 接头的 RaspberryPi。

电路板布局
*******************************


.. image:: /_static/images/products/bw1094-layout.jpg
  :alt: RPi HAT Labeled

.. list-table:: Reference table
  :widths: 50 50
  :align: center

  * - **A.** 左摄像头端口
    - **E.** 直通 40 针 Raspberry Pi接头连接器
  * - **B.** 右摄像头端口
    - **F.** 彩色相机端口
  * - **C.** USB 3.0 Type-C
    - **G.** 40 针 Raspberry Pi接头连接器
  * - **D.** DepthAI 模块
    -

盒子里有什么东西?
********************************

- BW1094 载板
- 预先加载了 Raspbian 10 和 DepthAI 的µSD卡
- USB3C 电缆 (6 英寸)

设定
*****

请按照以下步骤设置 DepthAI 设备。

#. **关闭 Raspberry Pi的电源。**

    安全关闭Raspberry Pi的电源并拔下电源。

#. **将预闪过的µSD 卡插入RPi。**

    µSD卡已预先配置了Raspbian 10和DepthAI。

#. **安装 DepthAI RPi HAT。**

    使用随附的硬件将DepthAI RPi HAT安装到您的Raspberry Pi。

#. **重新连接RPi电源**

#. **校准相机**.

    请参阅 :ref:`校准`

#. **运行示例脚本**.

    请参阅 :ref:`验证安装`


.. _bw1098ffc:

BW1098FFC - USB3 模块化相机版
#####################################

.. image:: /_static/images/products/bw1098ffc.jpg
  :alt: BW1098FFC

在现有主机上使用 DepthAI。由于 AI /视觉处理是在 Myriad X 上完成的，因此典型的台式机可以处理插入的数十个 DepthAI(有效限制是主机可以处理的 USB 端口数量)。

要求
***************************************************

- Ubuntu 18.04 or Raspbian 10
- 相机

  - :ref:`模块化彩色相机 <DepthAI 彩色相机>`
  - :ref:`双目相机 <DepthAI 单目相机>` (如果需要深度)

- USB3C 电缆
- 主机上的 USB3C端口

电路板布局
**************************************************

.. image:: /_static/images/products/bw1098ffc-layout.jpg
  :alt: BW1098FFC layout

.. list-table:: Reference table
  :widths: 50 50
  :align: center

  * - **A.** 5V IN
    - **E.** 左摄像头端口
  * - **B.** USB3C
    - **F.** DepthAI 模块
  * - **C.** 右摄像头端口
    - **G.** Myriad X GPIO Access
  * - **D.** 彩色相机端口
    -

盒子里有什么东西?
*******************************************

- BW1098FFC 载板
- USB3C 电缆 (6 英尺)
- 电源供应


设定
*******************************************

请按照以下步骤设置 DepthAI 设备。

#. **连接您的模块化相机。**

    BW1098FFC上的FFC(柔性扁平电缆)连接器在搬运时需要小心。一旦插入并闩锁，连接器就很坚固，但是在处理连接器时，在解锁过程中很容易受到损坏，特别是在此过程中施加了很大的力时。

    下面的视频显示了一种无需任何工具即可安全地闩锁和释放这些连接器的技术。

    .. image:: https://i.imgur.com/z3O0LXr.jpg
      :alt: Connecting the Modular Cameras to BW1098FFC
      :target: https://www.youtube.com/watch?v=KQlFvodQ3nM

    一旦将柔性扁平电缆牢固地锁定，您将看到以下内容：

    .. image:: /_static/images/products/bw1098ffc-connected.jpg
      :alt: BW1098FFC Connected to Modular Cameras


    .. note::

      请注意，当查看连接器时，蓝色条纹应朝上。


    .. image:: /_static/images/products/modular-camera-sides.jpg
      :alt: BW1098FFC modular camera top side


    .. warning::

      确保FFC电缆连接到相机位于最终设置的顶部，以避免图像倒置和错误的 :code:`swap_left_and_right_cameras` 设置。

#. **将主机连接到 DepthAI USB载板。**

#. **连接 DepthAI USB 电源（随附）。**

#. **校准相机**.

    请参阅 :ref:`校准`

#. **运行示例脚本**.

    请参阅 :ref:`验证安装`


.. _bw1098obc:

BW1098OBC - USB3 板载摄像头版
#####################################

.. image:: /_static/images/products/bw1098obc.png
  :alt: BW1098OBC

在现有主机上使用 DepthAI。由于 AI /视觉处理是在 Myriad X 上完成的，因此典型的台式机可以处理插入的数十个 DepthAI(有效限制是主机可以处理的 USB 端口数量)。

要求
******************************************

- USB3C 电缆
- 主机上的 USB3C端口

盒子里有什么东西?
********************************************

- BW1098OBC 载板
- USB3C 电缆 (6 英尺)
- 电源供电

设置
********************************************

请按照以下步骤设置 DepthAI 设备。

#. **将主机连接到 DepthAI USB载板**
#. **连接 DepthAI USB 电源（包括在内）**
#. **运行示例脚本**.

  请参阅 :ref:`验证安装`


.. _bw1099:

BW1099 - 模块化系统
################################

.. image:: /_static/images/products/bw1099.jpg
  :alt: BW1099

所有 DepthAI 版本都使用模块系统(SoM)，也可以单独使用它来集成到您自己的设计中。与直接集成 VPU 所需的高密度集成(HDI)堆叠(带有激光过孔和堆叠过孔)相比，SoM 允许携带它的板成为简单易用的四层标准密度板本身。

技术指标
**************************************************

- 2x 2-通道 MIPI 摄像头接口
- 1x 4-通道 MIPI 摄像头接口
- 具有 2 个专用芯片选择的 Quad SPI
- I²C
- UART
- USB2
- USB3
- 多个 GPIO (1.8 V and 3.3 V)
- 支持板载 eMMC 或 SD卡
- 板载 NOR 引导闪存（可选）
- 板载 EEPROM（可选）
- 模块上的所有功率调节，时钟生成等
- 通过单个 100 针连接器(DF40C-100DP-0.4V(51))进行所有连接

**数据表** 在 `这里 <https://github.com/luxonis/depthai-hardware/blob/master/SoMs/BW1099/BW1099_Datasheet.pdf>`__ 可用
EMB板在 `这里 <https://github.com/luxonis/depthai-hardware/blob/master/SoMs/BW1099EMB/R1M0E1/BW1099EMB_Datasheet.pdf>`__ 可用。


集成到产品中的入门
**********************************************

都是基于模块的 DepthAI 系统板可在 GitHub 在 MIT 许可证 `这里 <https://github.com/luxonis/depthai-hardware>`__.

这些是 Altium Designer 格式。因此，如果您使用 Altium Designer，那么您会很幸运！您可以使用经过验证的最新设计(可以在 `此处 <https://shop.luxonis.com/>`__ 购买的相同设计)快速/轻松地将DepthAI SoM集成到您的产品中。


.. _bw1093:

BW1093 - MegaAI 4K USB3 AI 相机
#############################################

.. image:: /_static/images/products/bw1093.png
  :alt: MegaAI 4K USB3 AI Camera

在现有主机上使用 megaAI。由于 AI /视觉处理是在 Myriad X 上完成的，因此典型的台式机可以处理插入的数十个 megaAI(有效限制是主机可以处理的 USB 端口数量)。

由于 megaAI 可以编码 1080p 和 4K 视频（请参阅 :ref:`here <如何使用 DepthAI 录制（或编码）视频？>` ），您现在甚至可以将4K视频保存在Pi Zero上！

要求
*******************************************

- USB3C 电缆
- 主机上的 USB2 或 USB3端口

盒子里有什么东西？
***********************************************

- BW1098OBC 载板
- USB3C 电缆 (6 英尺)

设置
********************************************

#. **将您的主机连接到 MegaAI**

#. **运行示例脚本**.

  请参阅 :ref:`验证安装`


.. _color_camera:

DepthAI 彩色相机
######################################

.. image:: /_static/images/products/color-camera.jpg
  :alt: Color Camera

具有 12 MP 静止图像和 4056 x 3040 像素分辨率的 4K，60Hz 摄像机。

技术指标
*********************************************

- 4K, 60 Hz 视频
- 12 MP Stills
- 与 Raspberry Pi Camera v2.1 相同的尺寸，安装孔和相机中心
- 4056 x 3040 像素
- 81 DFOV°
- 镜头尺寸: 1/2.3 英尺
- 自动对焦: 8 cm - ∞
- F-number: 2.0


.. _mono_camera:

DepthAI 单目相机
########################################

.. image:: /_static/images/products/mono-cameras.jpg
  :alt: Mono Cameras

对于需要“深度+ AI”的应用，我们提供了模块化，高帧率，出色深度质量的相机，可以将其分离到 30 厘米的基线。

技术指标
*******************************************

- 720p, 120 Hz 视频
- 同步全局快门
- 优秀的弱光
- 与 Raspberry Pi Camera v2.1 相同的尺寸，安装孔和相机中心
- 1280 x 720 像素
- 83 DFOV°
- 镜头尺寸: 1/2.3 英尺
- 定焦: 19.6 cm - ∞
- F-number: 2.2

校准
############################

.. note::

  将 :ref:`BW1097 - RaspberryPi 计算模块` or :ref:`BW1098OBC - USB3 板载摄像头版`? **您的设备已预先校准**


对于 DepthAI 的模块化相机版本 (:ref:`BW1098FFC - USB3 模块化相机版` and :ref:`BW1094 - RaspberryPi Hat`)
，在将摄像头安装到适用于您的应用程序的基线/配置中之后，有必要进行立体相机校准。


For the :ref:`BW1097 - RaspberryPi 计算模块` and :ref:`BW1098OBC - USB3 板载摄像头版` ，这些设备已经过预先校准-但是您可能需要重新校准以获得更好的安装质量（例如，将板安装到某物后），或者是否进行了校准使用/处理的质量已经开始下降。

以下是显示 :ref:`BW1097 - RaspberryPi 计算模块` (重新)校准的快速视频。.

观看下面的视频将为您提供校准自己的 DepthAI 所需的步骤。有关校准选项的更多信息/详细信息，请参阅以下步骤，并且还将打印出所有校准选项。 :code:`./calibrate.py --help` 。


.. image:: https://i.imgur.com/oJm0s8o.jpg
  :alt: DepthAI Calibration Example
  :target: https://www.youtube.com/watch?v=lF01f0p1oZM

#. 检出 `depthai <https://github.com/luxonis/depthai>`__ GitHub 存储库。
    .. warning::

      已经检出 `depthai <https://github.com/luxonis/depthai>`__ 了吗? **跳过此步骤**

    .. code-block:: bash

      git clone https://github.com/luxonis/depthai.git
      cd depthai
      python3 -m pip install -r requirements.txt

#. 打印棋盘校准图像。

    将校准棋盘打印在平坦的表面上，或者将棋盘显示在平坦的显示器上(不要弯曲！)。请注意，如果您确实要打印校准目标，请确保将其固定在平坦的表面上并且没有褶皱或“波浪”。

    通常，使用监视器显示校准目标更加容易/快捷。

    .. image:: https://github.com/luxonis/depthai/raw/main/resources/calibration-chess-board.png
      :alt: Print this chessboard calibration image
      :target: https://github.com/luxonis/depthai/raw/main/resources/calibration-chess-board.png

    整个电路板应放在一张纸上(缩放以适合)。如果在监视器上显示，请以白色背景全屏显示图像。

#. 启动校准脚本。

    用有效的条目替换占位符参数值:

    .. code-block:: bash

      python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd [BOARD]

    参数参考:

    - :code:`-s SQUARE_SIZE_IN_CM`, :code:`--square_size_cm SQUARE_SIZE_IN_CM`: 以厘米为单位测量印刷棋盘的平方大小。
    - :code:`-brd BOARD`, :code:`--board BOARD`: BW1097，BW1098OBC-资源/面板/中的面板类型(不区分大小写)。或自定义.json板配置的路径。与[-fv -b -w]互斥，可以手动指定视场，基线和相机方向（已交换或未交换）。

    用直尺或游标卡尺从校准目标中检索正方形的大小，然后输入该数字(以厘米为单位)代替[SQUARE_SIZE_IN_CM]。

    例如，如果正方形尺寸为2.35厘米，则带有板载相机的BW1098OBC-USB3的参数如下所示:

    .. code-block:: bash

      python3 calibrate.py -s 2.35 -brd bw1098obc

    并请注意，在校准时对显示进行镜像通常很有用(这样，运动方向就不会向后看)。看到自己时，我们习惯于向后看自己(因为这是我们在镜子中看到的)，因此，请使用以下 :code:`-ih` 选项:

    .. code-block:: bash

      python3 calibrate.py -s 2.35 -brd bw1098obc -ih

    因此，当我们在内部运行校准时，几乎总是使用该 :code:`-ih` 选项，因此我们会将其包括在以下所有示例命令中:

    - **BW1098OBC (USB3 板载摄像头版)):**

      .. code-block:: bash

        python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd bw1098obc -ih

    - **BW1097 (RPi 计算模块版):**

      .. code-block:: bash

        python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd bw1097 -ih


模块化相机校准
*****************************************

使用 `此处 <https://github.com/luxonis/depthai/tree/main/resources/boards>`__ 的电路板 :code:`*.json` 文件之一来定义立体摄像机之间, 左摄像机与彩色摄像机之间的基线，替换下面括号中的项目。

- 向左/向右交换 (即，摄像机朝向，设置为 :code:`true` 或 :code:`false`)
- 该 :code:`BASELINE` 灰度左/右摄影机之间厘米
- 灰度相机和彩色相机 :code:`RGBLEFT` 之间的距离间隔 :code:`Left` ，以厘米为单位。

.. code-block::

  {
      "board_config":
      {
          "name": "ACME01",
          "revision": "V1.2",
          "swap_left_and_right_cameras": [true | false],
          "left_fov_deg": 71.86,
          "rgb_fov_deg": 68.7938,
          "left_to_right_distance_cm": [BASELINE],
          "left_to_rgb_distance_cm": [RGBLEFT]
      }
  }

因此，例如，如果您将 BW1098FFC 的立体基准设置为 2.5cm，并且彩色摄像机恰好位于两个灰度摄像机之间，如下所示，请进一步使用以下 JSON:

.. image:: /_static/images/products/mono-cameras-min-dist.png
  :alt: Color Camera

.. code-block:: json

  {
      "board_config":
      {
          "name": "ACME01",
          "revision": "V1.2",
          "swap_left_and_right_cameras": true,
          "left_fov_deg": 71.86,
          "rgb_fov_deg": 68.7938,
          "left_to_right_distance_cm": 2.5,
          "left_to_rgb_distance_cm": 5.0
      }
  }

请注意，在照相机的此方向上， :code:`"swap_left_and_right_cameras"` 设置为true。

然后，使用以下电路板名称运行校准:

.. code-block:: bash

  python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd ACME01 -ih

运行 :code:`python3 calibrate.py --help` (或 :code:`-h`) 获取参数和用法示例的完整列表。

放置棋盘并捕获图像。
*******************************************

显示左右视频流，每个视频流包含一个多边形叠加层。

举起印刷好的棋盘(或笔记本电脑，屏幕上显示图像)，以便整个棋盘显示在两个视频流中。

匹配叠加多边形的方向，然后按[SPACEBAR]捕获图像。棋盘格图案不需要与多边形完全匹配，但是将多边形用作相对于摄影机的角度和位置的准则很重要。有 13 个必需的多边形位置。

在捕获了所有多边形位置的图像之后，校准图像处理步骤将开始。如果成功，将在创建校准文件 :code:`depthai/resources/depthai.calib`.
默认情况下，此文件是通过中的 :code:`calib_fpath` 变量加载的 :code:`consts/resource_paths.py`.

测试深度
*****************************************

我们将查看深度流以确保正确校准摄像机:

#. 开始一个终端会话。
#. 访问本地的 `depthai <https://github.com/luxonis/depthai>`__ 副本。

  .. code-block:: bash

    cd [depthai repo]

3. 运行测试脚本。

  .. code-block:: bash

    python3 depthai_demo.py -s depth_raw -o

  该脚本将启动一个窗口，启动摄像头，并显示深度视频流:

  .. image:: /_static/images/products/calibration-depth.png
    :alt: Depth projection

  在上面的屏幕截图中，手靠近相机。

将校准和电路板参数写入板载eeprom
*********************************************************

如果您对上述深度质量感到满意，则可以将其写入 DephtAI 上的板载 eeprom，以便带有 DepthAI 的校准棒(所有具有双目深度支持的设计均具有板载 eeprom)。

要将校准和相关的板信息写入 DepthAI 上的 EEPROM，请使用以下命令:

.. code-block:: bash

  python3 depthai_demo.py -brd [BOARD] -e

其中 :code:`[BOARD]` 要么 :code:`BW1097` (Raspberry Pi 计算模块版), :code:`BW1098OBC` (USB3 板载摄像头版)
或者定制电路板文件 (在 :ref:`这里 <模块化相机校准>`)，所有不区分大小写。

因此，例如，要将(更新的)校准和电路板信息写入 BW1098OBC，请使用以下命令:

.. code-block:: bash

  python3 depthai_demo.py -brd bw1098obc -e

为了验证在 DepthAI 上写入 EEPROM 的内容，只要运行 DetphAI，就可以查看检查输出，只需使用：

.. code-block:: bash

  python3 depthai_demo.py

在运行上述命令后，在终端中查找打印内容 :code:`EEPROM data:` :

.. code-block::

  EEPROM data: valid (v2)
    Board name     : BW1098OBC
    Board rev      : R0M0E0
    HFOV L/R       : 71.86 deg
    HFOV RGB       : 68.7938 deg
    L-R   distance : 7.5 cm
    L-RGB distance : 3.75 cm
    L/R swapped    : yes
    L/R crop region: top
    Calibration homography:
      1.002324,   -0.004016,   -0.552212,
      0.001249,    0.993829,   -1.710247,
      0.000008,   -0.000010,    1.000000,


如果有任何不正确的地方，您可以再次进行校准和/或更改板信息，并使用如上所述的 :code:`-brd` 和 :code:`-e` 标志覆盖存储的eeprom信息和校准数据。

验证安装
###############################

我们将执行 DepthAI 示例 Python 脚本，以确保正确配置您的设置。请按照以下步骤测试 DepthAI:

#. 开始一个终端会话。
#. 访问本地的 `depthai <https://github.com/luxonis/depthai>`__ 副本。

  .. code-block:: bash

    cd [depthai repo]

#. 运行演示脚本。<br/>

  .. code-block:: bash

    python3 depthai_demo.py

  该脚本将启动一个窗口，启动摄像机，并显示带有对象本地化元数据注释的视频流:

  .. image:: /_static/images/products/bw1097-detection.png
    :alt: Depth projection

  在上面的屏幕截图中，DepthAI确定了电视监视器(距离相机1.286 m)和椅子（距离相机3.711 m）。

  请参阅我们的预训练的OpenVINO模型教程中的 `对象标签列表 <https://docs.luxonis.com/tutorials/openvino_model_zoo_pretrained_model/#run-depthai-default-model>`__ 。

.. include::  /pages/includes/footer-short.rst


