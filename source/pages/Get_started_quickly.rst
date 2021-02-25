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

.. code-block:: python

    pip install depthai

测试程序
********************************

hello-word： `github地址 <https://github.com/luxonis/depthai-tutorials>`_

.. code-block:: python

    from pathlib import Path

    import cv2  # opencv - display the video stream
    import depthai  # access the camera and its data packets

    device = depthai.Device('', False)

    # Create the pipeline using the 'previewout' stream, establishing the first connection to the device.
    pipeline = device.create_pipeline(config={
        'streams': ['previewout', 'metaout'],
        'ai': {
            "blob_file": str(Path('./mobilenet-ssd/mobilenet-ssd.blob').resolve().absolute()),
            "blob_file_config": str(Path('./mobilenet-ssd/mobilenet-ssd.json').resolve().absolute()),
        }
    })

    if pipeline is None:
        raise RuntimeError('Pipeline creation failed!')

    detections = []

    while True:
        # Retrieve data packets from the device.
        # A data packet contains the video frame data.
        nnet_packets, data_packets = pipeline.get_available_nnet_and_data_packets()

        for nnet_packet in nnet_packets:
            detections = list(nnet_packet.getDetectedObjects())

        for packet in data_packets:
            # By default, DepthAI adds other streams (notably 'meta_2dh'). Only process `previewout`.
            if packet.stream_name == 'previewout':
                data = packet.getData()
                # change shape (3, 300, 300) -> (300, 300, 3)
                data0 = data[0, :, :]
                data1 = data[1, :, :]
                data2 = data[2, :, :]
                frame = cv2.merge([data0, data1, data2])

                img_h = frame.shape[0]
                img_w = frame.shape[1]

                for detection in detections:
                    pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
                    pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

                    cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

                cv2.imshow('previewout', frame)

        if cv2.waitKey(1) == ord('q'):
            break

    # The pipeline object should be deleted after exiting the loop. Otherwise device will continue working.
    # This is required if you are going to add code after exiting the loop.
    del device

运行效果
******************

.. image:: /_static/images/GetStartedQuickly/yunxing.png
    :alt: yunxing

在下面教程中有对 :ref:`Hello World` 示例程序的详细讲解。

.. include::  /pages/includes/footer-short.rst
