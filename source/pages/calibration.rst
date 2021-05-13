校准
############################

.. note::

  :ref:`BW1097 - RaspberryPi 计算模块` 和 :ref:`BW1098OBC <bw1098obc>` 都已预先校准。


对于 DepthAI 的模块化相机版本 (:ref:`BW1098FFC - USB3 模块化相机版` and :ref:`BW1094 - RaspberryPi Hat`)
，在将摄像头安装到适用于您的应用程序的基线/配置中之后，有必要进行立体相机校准。


对于 :ref:`BW1097 - RaspberryPi 计算模块` 和 :ref:`BW1098OBC <bw1098obc>` ，这些设备是预先校准的 - 但是您可能需要在安装过程中重新校准以获得更好的质量（例如，将板安装到某物后），或者是如果校准质量在使用/处理过程中开始减弱时。

以下是显示 :ref:`BW1097 - RaspberryPi 计算模块` (重新)校准的快速视频。
简而言之，校准使用相交点来确定棋盘格的方向和距离。 
因此，通过在平面上清晰地打印或显示所提供的棋盘格图像，可以获得最高的准确性。

校准棋盘的平整度非常重要。
不要使用曲面显示器，或有任何 "波浪" 的校准目标。
因此，如果你打印棋盘，请确保将棋盘贴在一个已知的平面上，没有任何波纹。
也就是说，使用笔记本电脑与平面显示器通常是最简单的技术。

观看下面的视频将为您提供校准自己的 DepthAI 所需的步骤。有关校准选项的更多信息/详细信息，请参阅以下步骤，并且还将打印出所有校准选项。 :code:`./calibrate.py --help` 。


.. image:: /_static/images/calibration/oJm0s8o.jpg
  :alt: DepthAI Calibration Example
  :target: https://www.youtube.com/watch?v=lF01f0p1oZM

#. 检出 `depthai <https://gitee.com/oakchina/depthai.git>`__ GitHub 存储库。
    .. warning::

      已经检出 `depthai <https://gitee.com/oakchina/depthai.git>`__ 了吗? **跳过此步骤**

    .. code-block:: bash

      git clone https://gitee.com/oakchina/depthai.git
      cd depthai
      python3 -m pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

#. 打印棋盘格校准图像。

    将校准棋盘格打印在平坦的表面上，或者将棋盘格显示在平坦的显示器上(不要弯曲！)。请注意，如果您确实要打印校准目标，请确保将其固定在平坦的表面上并且没有褶皱或“波浪”。

    通常，使用监视器显示校准目标更加容易/快捷。

    .. image:: /_static/images/calibration/calibration-chess-board.png
      :alt: 打印此棋盘格校准图像
      :target: https://gitee.com/oakchina/depthai/blob/main/resources/calibration-chess-board.png

    整个电路板应放在一张纸上(缩放以适合)。如果在监视器上显示，请以白色背景全屏显示图像。

#. 启动校准脚本。

    用有效的条目替换占位符参数值:

    .. code-block:: bash

      python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd [BOARD]

    参数参考:

    - :code:`-s SQUARE_SIZE_IN_CM`, :code:`--square_size_cm SQUARE_SIZE_IN_CM`: 以厘米为单位测量印刷棋盘格的平方大小。
    - :code:`-brd BOARD`, :code:`--board BOARD`: BW1097，BW1098OBC-资源/面板/中的面板类型(不区分大小写)。或自定义.json板配置的路径。与[-fv -b -w]互斥，可以手动指定视场，基线和相机方向（已交换或未交换）。

    用直尺或游标卡尺从校准目标中检索正方形的大小，然后输入该数字(以厘米为单位)代替[SQUARE_SIZE_IN_CM]。

    例如，如果正方形尺寸为 2.35 厘米，则 :ref:`BW1098OBC <bw1098obc>` 的参数如下所示:

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

使用 `此处 <https://gitee.com/oakchina/depthai/tree/main/resources/boards>`__ 的电路板 :code:`*.json` 文件之一来定义立体摄像机之间, 左摄像机与彩色摄像机之间的基线，替换下面括号中的项目。

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

放置棋盘格并捕获图像。
*******************************************

显示左右视频流，每个视频流包含一个多边形叠加层。

举起印刷好的棋盘格(或笔记本电脑，屏幕上显示图像)，以便整个棋盘格显示在两个视频流中。

匹配叠加多边形的方向，然后按[SPACEBAR]捕获图像。棋盘格图案不需要与多边形完全匹配，但是将多边形用作相对于摄影机的角度和位置的准则很重要。有 13 个必需的多边形位置。

在捕获了所有多边形位置的图像之后，校准图像处理步骤将开始。如果成功，将在创建校准文件 :code:`depthai/resources/depthai.calib`.
默认情况下，此文件是通过中的 :code:`calib_fpath` 变量加载的 :code:`consts/resource_paths.py`.

测试深度
*****************************************

我们将查看深度流以确保正确校准摄像机:

#. 开始一个终端会话。
#. 访问本地的 `depthai <https://gitee.com/oakchina/depthai>`__ 副本。

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

.. include::  /pages/includes/footer-short.rst
