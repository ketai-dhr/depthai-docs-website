故障排除
===============

如何禁用 RPi 计算模块版本上的启动演示?
################################################################

删除自动启动文件:

.. code-block:: bash

  rm /home/pi/.config/autostart/runai.desktop

'depthai：初始化 xlink 时出错'错误，DepthAI 无法运行
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

#. 是否安装了 :ref:`Python API` ? 验证键入时是否显示:

    .. code-block:: bash

      python3 -m pip list | grep depthai

#. 您是否在使用受 :ref:`支持的平台 <支持的平台>` ? 如果没有, 您可以从 :ref:`从源安装 <从源安装>`:

    .. code-block:: bash

      cat /etc/lsb-release


为什么相机校准运行缓慢?
###########################################

在相机校准期间，不良的照片条件 `会极大地影响图像处理时间 <https://stackoverflow.com/questions/51073309/why-does-the-camera-calibration-in-opencv-python-takes-more-than-30-minutes>`__
。 在正常情况下，要在RPi上每个图像找到棋盘角需要1秒或更短的时间，但在恶劣条件下每个图像要超过20秒。设置合适的照片条件的提示:

- 确保棋盘格没有翘曲，并且确实是平坦的表面。高质量的选择: `将棋盘打印在泡沫板上。 <https://discuss.luxonis.com/d/38-easy-calibration-targets-for-depthai-opencv-checkerboard>`__.
- 减少棋盘上的眩光（例如，确保没有任何光源像台灯那样靠近棋盘）。
- 通过尝试尽可能保持棋盘格不动来减少运动模糊量。

[Errno 13] 权限被拒绝: '/usr/local/lib/python3.7/dist-packages/...'
##########################################################################

如果 :code:`python3 -m pip install` 因为 :code:`Permission denied` 错误而不成功, 你当前的用户可能没有权限在系统范围的路径上安装软件包。你可以尝试加上 :code:`--user` 选项. For example:

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

.. code-block:: bash

  python3 depthai_demo.py --force_usb2

或者，简写:

.. code-block:: bash

  python3 depthai_demo.py -usb2

我们还看到了在 Linux Mint 上运行 Ubuntu 编译的库的未确认问题。  如果不是在 Ubuntu 18.04/16.04 或 Raspbian 上运行， 请 :ref:`从源码编译DepthAI <从源安装>`.

.. include::  /pages/includes/footer-short.rst
