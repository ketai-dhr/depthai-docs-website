快速上手
=============================

.. warning::
    提示：本章教程皆在Windows环境下安装运行

我们要想使用OAK需要几个帮手：

- 编辑软件-我们使用vscode编辑OAK程序。
- python运行环境-我们使用miniconda安装python环境。
- OAK专属的python第三方库-depthai。

安装IDE软件
*******************

- vscode下载地址：https://code.visualstudio.com/

进入vscode官网

.. image:: /_static/images/GetStartedQuickly/vscode1.png
    :alt: vscode1

选择Windows x64下载。

下载后一直点击下一步，就正常安装完成了。

.. image:: /_static/images/GetStartedQuickly/vscode2.png
    :alt: vscode2

安装完成即可打开vscode

.. image:: /_static/images/GetStartedQuickly/vscode3.png
    :alt: vscode3

安装python
*********************************

python官方网站：https://www.python.org/downloads/

我们推荐使用 miniconda 安装 python

1. 下载安装包

    下载地址： `Miniconda3-py37_4.9.2-Linux-x86_64.exe <https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-py37_4.9.2-Windows-x86_64.exe>`_

2. 双击 Miniconda 安装包安装（默认安装）

    .. image:: /_static/images/GetStartedQuickly/miniconda1.png
        :alt: miniconda1
    
    .. image:: /_static/images/GetStartedQuickly/miniconda2.png
        :alt: miniconda2
    
    选择Just Me

    .. image:: /_static/images/GetStartedQuickly/miniconda3.png
        :alt: miniconda3
    
    指定你要安装的路径

    .. image:: /_static/images/GetStartedQuickly/miniconda4.png
        :alt: miniconda4
    
    自动添加环境变量

    .. image:: /_static/images/GetStartedQuickly/miniconda5.png
        :alt: miniconda5
    
    .. image:: /_static/images/GetStartedQuickly/miniconda6.png
        :alt: miniconda6
    
    安装完成

    .. image:: /_static/images/GetStartedQuickly/miniconda7.png
        :alt: miniconda7

3. 测试python是否安装成功
    同时按下win+R

    .. image:: /_static/images/GetStartedQuickly/cmd1.png
        :alt: cmd1
    
    在输入框中输入cmd，调出命令提示符。

    .. image:: /_static/images/GetStartedQuickly/cmd2.png
        :alt: cmd2

    输入python

    .. image:: /_static/images/GetStartedQuickly/cmd3.png
        :alt: cmd3
    
    可以看到我们已经进入了python环境，说明安装成功。

vscode安装python插件
**********************************

在插件库中搜索python，安装第一个即可。

.. image:: /_static/images/GetStartedQuickly/vscode4.png
        :alt: vscode4

就此我们就可以在vscode 中运行python程序了。

安装depthai库
**************************

.. code-block:: bash

    pip install depthai

遇到安装超时问题：可以-i更换镜像源, 以下命令中更换的是清华镜像源。

.. code-block:: bash

    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple depthai

.. warning::

    第一次使用OAK需要配置udev规则 - :ref:`详情 <启用 USB 设备（仅在 Linux 上）>`

测试程序
********************************

hello-word： `github地址 <https://github.com/luxonis/depthai-tutorials>`_

.. code-block:: python

    from pathlib import Path
    import cv2
    import depthai
    import numpy as np

    # 管道告诉DepthAI运行时要执行哪些操作-您可以在此处定义所有使用和流动的资源
    pipeline = depthai.Pipeline()

    # 首先，我们希望将彩色摄像机作为输出
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(300, 300)  # 300x300将是预览帧的大小，可用作节点的“预览”输出
    cam_rgb.setInterleaved(False)

    # 接下来，我们需要一个能够产生检测结果的神经网络
    detection_nn = pipeline.createNeuralNetwork()
    # Blob是为MyriadX编译的神经网络文件。 它包含模型的定义和权重
    detection_nn.setBlobPath(str((Path(__file__).parent / Path('mobilenet-ssd/mobilenet-ssd.blob')).resolve().absolute()))
    # 接下来，我们将摄像机的“预览”输出链接到神经网络检测输入，以便可以产生检测结果
    cam_rgb.preview.link(detection_nn.input)

    # XLinkOut是设备的“出路”。 您要传输到主机的任何数据都需要通过XLink发送
    xout_rgb = pipeline.createXLinkOut()
    # 对于rgb摄像机输出，我们希望将XLink流命名为“ rgb”
    xout_rgb.setStreamName("rgb")
    # 将相机预览链接到XLink输入，以便将帧发送到主机
    cam_rgb.preview.link(xout_rgb.input)

    # 相同的XLinkOut机制将用于接收nn结果
    xout_nn = pipeline.createXLinkOut()
    xout_nn.setStreamName("nn")
    detection_nn.out.link(xout_nn.input)

    # 管道现已完成，我们需要找到可用的设备来运行管道
    device = depthai.Device(pipeline)
    # 开启管道。 至此，设备将处于“运行”模式，并将开始通过XLink发送数据
    device.startPipeline()

    # 为了消耗设备的结果，我们从设备中获得两个输出队列，这些队列具有我们先前分配的流名称。
    q_rgb = device.getOutputQueue("rgb")
    q_nn = device.getOutputQueue("nn")

    # 在此，定义了一些默认值。 帧将是来自“ rgb”流的图像，bbox将包含nn个结果
    frame = None
    bboxes = []


    # 由于nn返回的bbox的值在<0..1>范围内，因此需要将它们乘以帧的宽度/高度以得出
    # 接收边框在图像上的实际位置
    def frame_norm(frame, bbox):
        return (np.array(bbox) * np.array([*frame.shape[:2], *frame.shape[:2]])[::-1]).astype(int)


    # 主机端应用程序循环
    while True:
        # 我们尝试从nn / rgb队列中获取数据。 tryGet将返回数据包，如果没有则返回None
        in_rgb = q_rgb.tryGet()
        in_nn = q_nn.tryGet()

        if in_rgb is not None:
            # 收到来自rgb流的数据时，我们需要将其从一维平面数组转换为(3,高,宽)
            shape = (3, in_rgb.getHeight(), in_rgb.getWidth())
            # 而且，数组也从CHW形式转换为HWC
            frame = in_rgb.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
            frame = np.ascontiguousarray(frame)

        if in_nn is not None:
            # 当接收到来自nn的数据时，它最初也表示为一维数组，就像rgb帧一样
            bboxes = np.array(in_nn.getFirstLayerFp16())
            # nn detections数组是固定大小（且很长）的数组。 nn的实际数据可从
            # 数组的开头，并以-1值结束，此后数组以0填充
            # 我们需要裁剪数组，以便仅保留来自nn的数据
            bboxes = bboxes[:np.where(bboxes == -1)[0][0]]
            # 接下来，单个NN结果由7个值组成：id，label，confidence，x_min，y_min，x_max，y_max
            # 这就是为什么我们将数组从1D整形为2D数组的原因-其中每一行都是nn结果，包含7列
            bboxes = bboxes.reshape((bboxes.size // 7, 7))
            # 最后，我们只需要这些结果，其置信度（范围为<0..1>）大于0.8
            # 对边界框感兴趣（所以最后4列）
            bboxes = bboxes[bboxes[:, 2] > 0.8][:, 3:7]

        if frame is not None:
            for raw_bbox in bboxes:
                # 对于每个边界框，我们首先将其标准化以匹配帧大小
                bbox = frame_norm(frame, raw_bbox)
                # 然后在框架上绘制一个矩形以显示实际结果
                cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
            # 完成所有绘图后，我们在屏幕上显示框架
            cv2.imshow("preview", frame)

        # 您可以随时按“ q”并退出主循环，从而退出程序本身
        if cv2.waitKey(1) == ord('q'):
            break

运行效果
******************

.. image:: /_static/images/GetStartedQuickly/yunxing.png
    :alt: yunxing

在下面教程中有对 :ref:`Hello World` 示例程序的详细讲解。

.. include::  /pages/includes/footer-short.rst
