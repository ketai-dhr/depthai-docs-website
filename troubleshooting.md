---
layout: default
title: 故障排查 ~ OAK OpenCV人工智能套件
toc_title: 故障排查
description: OAK/DepthAI常见问题的解决方法。
order: 6
---

# DepthAI 故障排查

{: #disable_demo data-toc-title="禁用自启动Demo"}
### 如何禁用树莓派计算模组（Compute Module）的自启动demo？

删除自启动文件:

```
rm /home/pi/.config/autostart/runai.desktop
```
<hr/>

{: #device_reset data-toc-title="重置 Myriad X"}
### 出现`depthai: Error initalizing xlink` 错误 and DepthAI无法运行.

Myriad X需要复位。按下底板上的 "MODULE RST "或 "RST "按钮。

在树莓派计算模组（Compute Module）上，您可以通过以下shell命令重置Myriad X。

```
raspi-gpio set 33 op  # set 33 as output
raspi-gpio set 33 dh  # drive high to reset Myriad X
sleep 1
raspi-gpio set 33 dl  # drive low to allow Myriad X to run
```

<hr/>

{: #depthai_import_error data-toc-title="No module named 'depthai'"}
### ImportError: No module named 'depthai'

这表明`depthai.*.so`文件无法加载。导致失败的可能原因如下：

1. 是否已经安装DepthAI API [installed](https://docs.luxonis.com/api/)? 输入以下命令以检查安装情况:
    ```
    pip3 list | grep depthai
    ```
2. 你是否在操作系统上使用[支持的Python版本](/api/#python_version)? 请确认你的python版本是[支持的](/api/#python_version):
    ```
    python3 --version
    ```

<hr/>

{: #slow_calibration data-toc-title="相机校正缓慢"}
### 为什么相机校正运行得这么慢?

较差的照片质量会在相机校正时[极大地影响图像处理的时间](https://stackoverflow.com/questions/51073309/why-does-the-camera-calibration-in-opencv-python-takes-more-than-30-minutes) 。在正常情况下，树莓派上每幅图像找到棋盘角应该只需要1秒或更短的时间。但在较差的条件下，每幅图像用时可能超过20秒。以下是有关配置合适的拍摄条件的建议：
* 确保格子板不变形，是一个真正的平面。一种保证高质量的选择是[在泡沫板上打印棋盘](https://discuss.luxonis.com/d/38-easy-calibration-targets-for-depthai-opencv-checkerboard)。
* 减少棋盘上的眩光（例如，确保棋盘附近没有光源，如台灯）。
* 尽量保持棋盘静止不动，以减少运动模糊的程度。

<hr/>

{: #python_api_permission_denied data-toc-title="pip install permission denied"}
### [Error 13] Permission denied: '/usr/local/lib/python3.7/dist-packages/...'

如果 `pip3 install`因为 `Permission denied` 错误而不成功，你当前的用户可能没有权限在系统范围的路径上安装软件包。你可以尝试加上`--user`选项，在用户的主目录下安装，。比如说:


```
pip3 install -e depthai-python-extras --user
```

[Stackoverflow上的更多信息](https://stackoverflow.com/questions/31512422/pip-install-failing-with-oserror-errno-13-permission-denied-on-directory).

{: #dev-video data-toc-title="设备未出现在/dev/video* 下"}
### DepthAI没有像网络摄像头一样出现在 /dev/video* 下

可以用`lsusb | grep 03e7`检查USB设备的枚举。它应该打印出:

`03e7:2485 after reset (bootloader running);`  
`03e7:f63b after the application was loaded.`

没有创建 `/dev/video*`的节点. 

DepthAI实现了VSC(Vendor Specific Class)协议，并采用libusb进行通信。

{: #usb3-cable data-toc-title="USB3.0数据线信号断断续续"}
### 用2米长的USB3.0数据线时信号断断续续

- 我们发现有些主机在使用USB3接口和2米长线时会出现问题，这可能跟主机的USB控制器有关。 
- 其他主机运行了好几天（在一些设备上测试了3天以上）都没出现任何问题，即使是用长的USB线（测试总长度超过2.4米）也没出问题。就比方说我们测试过的所有苹果电脑都没有出现过这个问题。
- Ubuntu 16.04有一个独立的USB3问题，但好像只出现在新机器上。 我们认为这可能时因为在这些新机器上市前后Ubuntu 16.04停止支持有关。 例如，[这台电脑](https://pcpartpicker.com/list/KTDFQZ)）在Ubuntu 16.04下有严重的USB3断开问题（使用1米长的线），但在Ubuntu 18.04下却没有（使用1米长的线）。

很不巧，我们是在DepthAI发货了之后才发现的这个问题。

所以，如果你的主机出现了这个问题，可能有3个选择。
1. 换一根稍短一些的USB3.0线（如1米长），可能问题就没了。
2. 用`--force_usb2`选项强制使用USB2模式（下面有示例）,这样就可以用长线了，许多DepthAI用例不需要跑到USB3的带宽，USB2已经足够了。
3. 从Ubuntu 16.04升级到Ubuntu 18.04。


#### 强制使用USB2通信
`python3 test.py --force_usb2`
或者更简单的:
`python3 test.py -usb2`

我们还看到一个未经证实的问题：一个在Linux Mint上运行Ubuntu编译的库的问题。 如果不是在Ubuntu 18.04/16.04或Raspbian上运行，请从源码编译DepthAI(参见[这里](https://github.com/luxonis/depthai-python-extras#python-modules)的说明)。
