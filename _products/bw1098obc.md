---
layout: default
title: DepthAI USB3 板载相机 (OAK-D)
toc_title: OAK-D
screenshot: /images/products/depthai-edition-usb3-cameras.png
description: 板载一个彩色相机模组，一对全局快门黑白相机模组。
order: 2
show_on_home: true
test_args: "-co '{\"board_config\": {\"left_to_right_distance_cm\": 7.5}}'"
---

# {{page.title}}

{% include model_number.md %}

![screenshot]({{page.screenshot}})

在你的现有主机上使用OAK-D。 因为AI和图像处理是直接在英特尔Myriad X VPU上做的, 所以能接多少个OAK-D，其实就是看你的主机能处理多少个USB设备。比如，一台主流的台式机就能接几十个OAK-D。

## 彩色相机模组规格

* 4K、60 Hz视频（最大4K/30fps编码h.265）。
* 12个百万像素剧照
* 4056 x 3040像素
* 81 DFOV°
* 68.8 HFOV°
* 镜头尺寸：1/2.3英寸
* 自动对焦：8厘米 - ∞
* F号：2.0

## 双目相机模组规格

* 720p, 120 Hz视频
* 同步的全局快门
* 优秀的低光环境表现
* 1280 x 720像素
* 83° 对角视场角
* 71° 水平视场角
* 56° 垂直视场角
* 镜头尺寸：1/2.3英寸
* 固定焦距：19.6厘米-∞
* F-number：2.2
* 传感器动态范围：68dB

## 运行要求

* Ubuntu 18.04或Raspbian 10。
* USB3 Type-C线
* 主机上有USB3.0接口
* 主机上安装了[支持的Python版本](/api/#python_version)。


{: #in_box}
## 包装清单

* {{page.title}}转接板
* 2米长的USB3.0数据线
* 电源适配器

## 设置设备

请按照以下步骤设置OAK-D设备.

<h3 class="step js-toc-ignore"><span></span> 把你的主机连接到DepthAI USB转接板</h3>

<h3 class="step js-toc-ignore"><span></span> 连接你的DepthAI USB电源适配器(已包含在内)</h3>

<h3 class="step js-toc-ignore"><span></span> 安装 Python DepthAI API</h3>

[参见我们的说明](/api#python_version).

<h3 class="step js-toc-ignore"><span></span> 校正双目相机</h3>

使用DepthAI [校正脚本](/products/stereo_camera_pair/#calibration)，带上以下的参数:

```
-brd bw1098obc
```

这样就把双目相机之间的距离配置成了他们在板子上的间距.

<h3 class="step js-toc-ignore"><span></span> 下载并运行 DepthAI Python 示例</h3>

{% include test_step.md param="arg" %}
