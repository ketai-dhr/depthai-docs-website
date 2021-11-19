故障排除
===============

如何禁用 RPi 计算模块版本上的启动演示?
################################################################

删除自动启动文件:

.. code-block:: bash

  rm /home/pi/.config/autostart/runai.desktop

如果pip安装depthai时出现"is not a supported wheel on this platform"错误:
##############################################################################

可以尝试更新pip版本：

.. code-block:: bash

    python -m pip install --upgrade pip

depthai：初始化 xlink 时出错'错误，DepthAI 无法运行
###################################################################

Myriad X 需要重置。单击载板上的“ MODULE RST”或“ RST”按钮。

在 RPi 计算模块版本上，您可以通过以下 Shell 命令重置 Myriad X:

.. code-block:: bash

  raspi-gpio set 33 op  # set 33 as output
  raspi-gpio set 33 dh  # drive high to reset Myriad X
  sleep 1
  raspi-gpio set 33 dl  # drive low to allow Myriad X to run

ImportError: 没有名为 'depthai' 的模块
######################################

这表明您的 Python 解释器未找到 :code:`depthai` 模块。失败的原因右很多:

#. 是否安装了 :ref:`Python API安装详解` ? 验证键入时是否显示:

    .. code-block:: bash

      python3 -m pip list | grep depthai

#. 您是否在使用受 :ref:`支持的平台 <支持的平台>` ? 如果没有, 您可以从 :ref:`从源安装 <从源安装>`:

    .. code-block:: bash

      cat /etc/os-release


为什么相机校准运行缓慢?
###########################################

在相机校准期间，不良的照片条件 `会极大地影响图像处理时间 <https://stackoverflow.com/questions/51073309/why-does-the-camera-calibration-in-opencv-python-takes-more-than-30-minutes>`__
。 在正常情况下，要在RPi上每个图像找到棋盘角需要1秒或更短的时间，但在恶劣条件下每个图像要超过20秒。设置合适的照片条件的提示:

- 确保棋盘格没有翘曲，并且确实是平坦的表面。高质量的选择: `将棋盘打印在泡沫板上。 <https://discuss.luxonis.com/d/38-easy-calibration-targets-for-depthai-opencv-checkerboard>`__.
- 减少棋盘上的眩光（例如，确保没有任何光源像台灯那样靠近棋盘）。
- 通过尝试尽可能保持棋盘格不动来减少运动模糊量。

权限拒绝错误
#################

如果 :code:`python3 -m pip install` 因为 :code:`Permission denied` 错误而不成功, 你当前的用户可能没有权限在系统范围的路径上安装软件包。

.. code-block:: bash

   [Errno 13] Permission denied: '/usr/local/lib/python3.7/dist-packages/...'

你可以尝试加上 :code:`--user` 选项。 例如:

.. code-block:: bash

  python3 -m pip install depthai --user

`有关 Stack Overflow 的更多信息 <https://stackoverflow.com/questions/31512422/pip-install-failing-with-oserror-errno-13-permission-denied-on-directory>`__.


DepthAI 不会像网络摄像机那样显示在 /dev/video* 下。 为什么?
#######################################################################

可以用 lsusb | grep 03e7 检查 USB 设备的枚举。 它应该打印出:

- :code:`03e7:2485` after reset (bootloader running)
- :code:`03e7:f63b` after the application was loaded

没有创建 :code:`/dev/video*` 的节点。

DepthAI 实现了 VSC(Vendor Specific Class)协议，并采用 libusb 进行通信。

用2米长的USB3.0数据线时信号断断续续
#########################################################

- 我们发现有些主机在使用 USB3 接口和 2 米长线时会出现问题，这可能跟主机的 USB 控制器有关。
- 其他主机运行了好几天(在一些设备上测试了 3 天以上)都没出现任何问题，即使是用长的 USB 线(测试总长度超过 2.4 米)也没出问题。就比方说我们测试过的所有苹果电脑都没有出现过这个问题。
- Ubuntu 16.04 有一个独立的 USB3 问题，但好像只出现在新机器上。 我们认为这可能时因为在这些新机器上市前后 Ubuntu 16.04 停止支持有关。 例如, (`这台电脑 <https://pcpartpicker.com/list/KTDFQZ>`__) h在Ubuntu 16.04下有严重的USB3断开问题（使用1米长的线），但在Ubuntu 18.04下却没有（使用1米长的线）。

很不巧，我们是在 DepthAI 发货了之后才发现的这个问题。

所以，如果你的主机出现了这个问题，可能有 3 个选择:

#. 切换到较短的 USB3 电缆(例如 1 米)很可能会使问题消失。  `这些 <https://www.amazon.com/gp/product/B07S4G4L4Z/ref=ppx_yo_dt_b_asin_title_o00_s00?ie=UTF8&psc=1>`__ 1米（3.3英尺）长的电缆非常长，现在是我们随DepthAI USB3变体一起提供的电缆。
#. 强制带 :code:`--force_usb2` 选项的 USB2 模式(以下示例)。这将允许仍然使用长电缆，并且许多 DepthAI用例不需要USB3通信带宽-USB2足够。
#. 从 Ubuntu 16.04 升级到 Ubuntu 1804。

强制 USB2 通信
**************************

如果您在与 DepthAI/OAK 通信时遇到问题，强制 USB2 有时可以解决问题。

.. code-block:: bash

  python3 depthai_demo.py --usbSpeed usb2

或者，简写:

.. code-block:: bash

  python3 depthai_demo.py -usbs usb2

对于 gen2，在创建设备时将 **usb2Mode** 设置为 **True** ：

.. code-block:: python

  dai.Device(pipeline, usb2Mode=True)

我们还看到了在 Linux Mint 上运行 Ubuntu 编译的库的未确认问题。  如果不是在 Ubuntu 18.04/16.04 或 Raspbian 上运行， 请 :ref:`从源码编译DepthAI <从源安装>`.

DepthAI 的输出一直冻结
#########################

如果设备的输出每隔几秒钟一直冻结，则 USB3 连接可能存在问题，强制设备进入 USB2 模式可以解决此问题 - 说明在上面的章节中。

当连接速度为 USB2 时（由于某些主机 - 特别是 Windows - 或 USB 控制器/端口/电缆为 USB2） - 启用 USB3 的固件或在几帧后进行流媒体的初始化可能会失败。这里的解决方法是强制设备使用 USB2-only 固件（在上面的章节中提到）。

无法启动设备：1.3-ma2480,错误代码 3
#############################################

如果未在Linux上设置udev规则，则经常会发生此错误。这与DepthAI一致：初始化xlink时出错。

要解决此问题，请使用以下命令设置udev规则，拔出DepthAI，然后再将其重新插入USB。

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

在某些情况下，这些设置已经设置好了，但是一直都插着DepthAI，因此Linux无法重置规则。

因此，请确保在运行完这些后拔出插头，然后重新插入DepthAI。

Failed to find device (ma2480), error message: X_LINK_DEVICE_NOT_FOUND.
##################################################################################

当您收到以下错误时，表示您遇到了与上述相同的问题，未设置 udev 规则。参考 :ref:`这里 <无法启动设备：1.3-ma2480,错误代码 3>` 的解决方案。

.. code-block:: bash

  RuntimeError: Failed to find device (ma2480), error message: X_LINK_DEVICE_NOT_FOUND.

CTRL-C没有停止程序！
####################

如果您试图用 :code:`CTLR-C` 杀死一个程序，但它不起作用，请尝试 :code:`CTRL-\ ` 。通常这会起作用。

您的Raspberry Pi是锁定还是DepthAI在Raspberry Pi上崩溃？
###########################################################

Raspberry Pi 在其所有 USB 端口上的最大限制为 1.2A，而 DepthAI/megaAI/OAK 最多可以占用 1A（在最大功率下，通常接近 500mA）。

因此，如果您看到锁定，则可能是由于从 Pi 汲取的 USB 设备的总功率超过了此 1.2A 限制。使用有源集线器可以防止这种情况，或者通过 USB 为 Pi 提供更少的其他电源。

这也可能表现为 DepthAI 在 Pi 上随机崩溃。如果 DepthAI 被配置为一次做很多事情，这会变得特别频繁。随着我们使 DepthAI 能够同时做越来越多的事情，这变得越来越有可能——从而增加 DepthAI 可以拉动的最大功率。似乎来自 DepthAI 的峰值功率（电流）尖峰可以超过 Pi 的处理能力，导致 DepthAI 掉电并返回错误。

因此，如果您在 Raspberry 上遇到 DepthAI 稳定性问题，请尝试通过电源和/或有源 USB 集线器为 DepthAI 供电，以查看错误是否消失。

Windows上的“导入cv2时DLL加载失败”
############################################

如果在为Windows安装depthai之后看到以下错误：

.. code-block:: bash

  (venv) C:\Users\Context\depthai>python depthai_demo.py
   Traceback (most recent call last):
     File "C:\Users\Context\depthai\depthai_demo.py", line 7, in <module>
       import cv2
     File "C:\Users\Context\depthai\venv\lib\site-packages\cv2\__init__.py", line 5, in <module>
       from .cv2 import *
   ImportError: DLL load failed while importing cv2: The specified module could not be found.

然后通常要解决Windows Media Feature Pack( `此处 <https://support.microsoft.com/en-us/help/3145500/media-feature-pack-list-for-windows-n-editions>`__ )的问题，因为必须为Windows 10 N版本安装Media Feature Pack。

(还有来自OpenCV的更多背景 `信息 <https://github.com/skvark/opencv-python/blob/master/README.md#:~:text=Q%3A%20Import%20fails%20on%20Windows%3A%20ImportError%3A%20DLL%20load%20failed%3A%20The%20specified%20module%20could%20not%20be%20found.%3F>`__ )

:code:`python3 depthai_demo.py` 返回非法指令
##############################################

到目前为止，这始终意味着主机上的 OpenCV 安装存在问题（实际上与 depthai 库无关）。要检查这一点，请运行：

.. code-block:: bash

  python3 -c "import cv2; import numpy as np; blank_image = np.zeros((500,500,3), np.uint8); cv2.imshow('s', blank_image); cv2.waitKey(0)"

如果没有显示一个窗口，或者如果你得到 :code:`Illegal instruction` 指令结果,这意味着 OpenCV 安装有问题。 `这里 <https://docs.luxonis.com/en/latest/pages/api/#supported-platforms>`__ 的安装脚本通常会修复 OpenCV 问题。但如果他们不这样做，运行 :code:`python3 -m pip install opencv-python --force-reinstall` 也可以解决OpenCV的问题。

用不兼容的 openvino 版本编译的神经网络 blob
##############################################

.. code-block:: bash

  [NeuralNetwork(2)] [error] Neural network blob compiled with uncompatible openvino version. Selected openvino version 2020.3. If you want to select an explicit openvino version use: setOpenVINOVersion while creating pipeline

出现此错误的原因是depthai无法从blob解析OpenVINO版本。解决方案很简单，用户必须指定编译blob的OpenVINO版本(如错误消息中所述):

.. code-block:: python

  pipeline = depthai.Pipeline()
  # Set the correct version:
  pipeline.setOpenVINOVersion(depthai.OpenVINO.Version.VERSION_2020_1)

RPI出现 "realloc(): invalid pointern Aborted"
#################################################

在树莓派上，运行 :code:`sudo upgrade` 后，在导入cv2的时候可能会出现 :code:`realloc(): invalid pointern Aborted` 错误。我们也遇到了同样的问题，并且找到了 **解决方案** :

- 通过运行 :code:`sudo apt install -y --allow-downgrades libc6=2.28-10+rpi1` 命令使libc6降级。
- 通过运行以下 :code:`sudo curl -fL http://docs.luxonis.com/_static/install_dependencies.sh | bash` 命令重新安装depthai依赖。


.. include::  /pages/includes/footer-short.rst
