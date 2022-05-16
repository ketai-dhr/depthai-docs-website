Gen2 设备上的彩色相机和MobilenetSSD模型解码
========================================================

这个例子展示了如何在 RGB 预览流上运行mobileetv2ssd模型，以及如何在预览上同时显示 RGB 预览和 mobileetv2ssd 的元数据结果。类似于 :ref:`Gen2 彩色相机和运行MobilenetSSD模型` 的例子，只不过解码是在 Myriad 上完成的。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=844404096&bvid=BV1C54y1h7by&cid=304670052&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个示例还需要 mobileenetsdd blob ( :code:`mobilenet.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`_ 下载它。


源代码
*********************

.. code-block:: python

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np
    import time

    '''
    Mobilenet SSD设备端解码演示
      "mobilenet-ssd"模型是旨在执行对象检测的单发多框检测（SSD）网络。该模型是使用Caffe框架实现的。
      有关此模型的详细信息，请查看github仓库库 <https://github.com/chuanqi305/MobileNet-SSD>.
    '''

    # MobilenetSSD标签文本
    label_map = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow",
                 "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]

    syncNN = True

    # 首先获取模型
    mobilenet_path = str((Path(__file__).parent / Path('models/mobilenet.blob')).resolve().absolute())
    if len(sys.argv) > 1:
        mobilenet_path = sys.argv[1]

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机流
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(300, 300)
    cam_rgb.setInterleaved(False)
    cam_rgb.setFps(40)

    # 定义一个将基于源帧进行预测的神经网络
    detectionNetwork = pipeline.createMobileNetDetectionNetwork()
    detectionNetwork.setConfidenceThreshold(0.5)
    detectionNetwork.setBlobPath(mobilenet_path)
    detectionNetwork.setNumInferenceThreads(2)
    detectionNetwork.input.setBlocking(False)

    cam_rgb.preview.link(detectionNetwork.input)

    # 创建输出
    xout_rgb = pipeline.createXLinkOut()
    xout_rgb.setStreamName("rgb")
    if(syncNN):
        detectionNetwork.passthrough.link(xout_rgb.input)
    else:
        cam_rgb.preview.link(xout_rgb.input)

    xout_nn = pipeline.createXLinkOut()
    xout_nn.setStreamName("detections")
    detectionNetwork.out.link(xout_nn.input)


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取rgb帧和nn数据
        q_rgb = device.getOutputQueue(name="rgb", maxSize=4, blocking=False)
        q_nn = device.getOutputQueue(name="detections", maxSize=4, blocking=False)

        frame = None
        bboxes = []

        start_time = time.time()
        counter = 0
        fps = 0
        while True:
            if(syncNN):
                in_rgb = q_rgb.get()
                in_nn = q_nn.get()
            else:
                in_rgb = q_rgb.tryGet()
                in_nn = q_nn.tryGet()

            if in_rgb is not None:
                # 如果来自RGB相机的数据不为空，则将1维数据转换为HxWxC帧
                shape = (3, in_rgb.getHeight(), in_rgb.getWidth())
                frame = in_rgb.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
                frame = np.ascontiguousarray(frame)

            if in_nn is not None:
                bboxes = in_nn.detections
                counter+=1
                current_time = time.time()
                if (current_time - start_time) > 1 :
                    fps = counter / (current_time - start_time)
                    counter = 0
                    start_time = current_time

            color = (255, 255, 255)

            if frame is not None:
                # 如果图像不为空，请在其上绘制边框并显示图像
                height = frame.shape[0]
                width  = frame.shape[1]
                for bbox in bboxes:
                    x1 = int(bbox.xmin * width)
                    x2 = int(bbox.xmax * width)
                    y1 = int(bbox.ymin * height)
                    y2 = int(bbox.ymax * height)
                    try:
                        label = label_map[bbox.label]
                    except:
                        label = bbox.label
                    cv2.putText(frame, str(label), (x1 + 10, y1 + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                    cv2.putText(frame, "{:.2f}".format(bbox.confidence*100), (x1 + 10, y1 + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, color)
                    cv2.rectangle(frame, (x1, y1), (x2, y2), color, cv2.FONT_HERSHEY_SIMPLEX)

                cv2.putText(frame, "NN fps: {:.2f}".format(fps), (2, frame.shape[0] - 4), cv2.FONT_HERSHEY_TRIPLEX, 0.4, color)
                cv2.imshow("rgb", frame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst