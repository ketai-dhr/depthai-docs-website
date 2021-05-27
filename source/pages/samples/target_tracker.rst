示例-目标跟踪
=====================

此示例展示了利用depthai实现目标跟踪，通过云台将检测目标始终置于图像中央。

演示
#######

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=973213040&bvid=BV1e44y1r7GL&cid=340454737&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>
    <br/>

示例源码代码
#####################

目标跟踪源代码可以在 `GitHub <https://github.com/OAKChina/depthai-target-tracking>`_ 代码仓库中获取。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-target-tracking>`_ 上获取。

请参考README运行程序.

硬件设备
##############

1. depthai
2. 树莓派
3. 云台

.. image:: /_static/images/samples/traker.jpg
    :alt: traker
    :height: 640px
    :width: 480px
    :align: center

模型介绍
############

此示例只使用了一个模型,如下：

.. list-table::

    * - mobilenet-ssd_openvino_2021.2_6shave.blob
      -

mobilenet模型一共可以检测15种目标类型。如下所示：

"background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow","diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"

实现重点
##################

1. 给检测到的目标指定ID
*************************

创建ObjectTracker节点。该节点拥有以下方法：

============================================= ====================
方法名                                         作用
============================================= ====================
setDetectionLabelsToTrack(self, labels)       指定要跟踪的检测标签。
setMaxObjectsToTrack(self, maxObjectsToTrack) 指定要跟踪的最大对象数。
setTrackerIdAssigmentPolicy(self, type)       指定跟踪器ID分配策略。
setTrackerThreshold(self, threshold)          指定跟踪器阈值。
setTrackerType(self, type)                    指定跟踪器类型算法。
============================================= ====================

- 使用setDetectionLabelsToTrack方法指定我们要跟踪的目标类型。
- 使用setTrackerIdAssigmentPolicy方法给检测到的目标分配ID的方法。
- 通过选择ID，指定跟踪对象。

2. 计算偏差距离
************************

模型输出目标检测框四点坐标。

利用左上角和右下角坐标计算检测框中心点，并计算与图像中心点偏差距离。

3. PID控制算法
****************

通过测试会发现直接偏差距离传给云台，会造成云台移动过快，从而丢失目标。而且云台自身转动也会产生惯性，造成无法在正确的位置停止。所以我们引入PID控制算法对偏差距离进行有效的调整。

PID算法实际上是三种反馈控制：比例控制，积分控制与微分控制的统称。

PID算法公式如下：

.. image:: /_static/images/samples/pid.png
    :alt: pid
    :align: center

**比例控制** ：按比例减小偏差值，单一的比例控制会造成稳态误差。 

**积分控制** ：引入积分控制的目的便是消除稳态误差。但是积分增益过大就会超调，从而使系统不稳定。而过小又难以消除稳态误差。

**微分控制** ：控制系统在克服误差的过程中有可能出现震荡现象。这是由于惯性的存在或有滞后可抑制误差，但是它的变化总是落后于误差的变化。微分控制就是解决误差变化总是超前的问题。

4. 控制云台
***************

将经过PID调整的误差距离传入控制云台方法中，使云台跟随目标转动。

效果演示
###################

.. image:: /_static/images/samples/traker.gif
    :alt: traker
    :align: center

.. include::  /pages/includes/footer-short.rst