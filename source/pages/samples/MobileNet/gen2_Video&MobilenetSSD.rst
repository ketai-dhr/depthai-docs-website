Gen2 视频流&MobilenetSSD神经网络模型
======================================================

这个例子展示了如何在 RGB 输入帧上运行mobileetv2ssd模型，这是从指定的文件读取的，而不是从 RGB 摄像头读取的，以及如何在帧上同时显示 RGB 帧和 mobileetv2ssd 的元数据结果。Depthai 在这里只用作处理单元。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=246884479&bvid=BV16v411Y71j&cid=304671640&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个例子还需要 mobileenetsdblob ( :code:`mobilenet.blob` 文件 )和预先录制的视频( :code:`construction_vest.mp4` 文件 )才能工作——您可以在这里下载它们: `mobilenet.blob <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`__ 和 `construction_vest.mp4 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/construction_vest.mp4>`__ 。

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/MobileNet/video_mobilenet.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/MobileNet/video_mobilenet.py>`_ 上找到。

.. code-block:: python

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np

    # 首先获取模型
    mobilenet_path = str((Path(__file__).parent / Path('models/mobilenet.blob')).resolve().absolute())
    video_path = str(Path("./construction_vest.mp4").resolve().absolute())
    if len(sys.argv) > 2:
        mobilenet_path = sys.argv[1]
        video_path = sys.argv[2]

    # 开始定义管道
    pipeline = dai.Pipeline()


    # 创建神经网络输入
    xin_nn = pipeline.createXLinkIn()
    xin_nn.setStreamName("in_nn")

    # 定义一个将基于源帧进行预测的神经网络
    detection_nn = pipeline.createNeuralNetwork()
    detection_nn.setBlobPath(mobilenet_path)
    xin_nn.out.link(detection_nn.input)

    # 创建输出
    xout_nn = pipeline.createXLinkOut()
    xout_nn.setStreamName("nn")
    detection_nn.out.link(xout_nn.input)

    # MobilenetSSD标签文本
    texts = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow",
             "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]


    # 管道已定义，现在设备已连接到管道
    with dai.Device(pipeline) as device:
        # 启动管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取rgb帧和nn数据
        q_in = device.getInputQueue(name="in_nn")
        q_nn = device.getOutputQueue(name="nn", maxSize=4, blocking=False)

        frame = None
        bboxes = []
        labels = []
        confidences = []

        # nn数据（作为边界框的位置）在<0..1>范围内-需要使用图像的width/height对其进行归一化
        def frame_norm(frame, bbox):
            norm_vals = np.full(len(bbox), frame.shape[0])
            norm_vals[::2] = frame.shape[1]
            return (np.clip(np.array(bbox), 0, 1) * norm_vals).astype(int)


        def to_planar(arr: np.ndarray, shape: tuple) -> list:
            return [val for channel in cv2.resize(arr, shape).transpose(2, 0, 1) for y_col in channel for val in y_col]


        cap = cv2.VideoCapture(video_path)
        while cap.isOpened():
            read_correctly, frame = cap.read()
            if not read_correctly:
                break

            nn_data = dai.NNData()
            nn_data.setLayer("data", to_planar(frame, (300, 300)))
            q_in.send(nn_data)


            in_nn = q_nn.tryGet()

            if in_nn is not None:
                # 检测结果有7个数，最后一次检测后跟着-1位数，以后填充0
                bboxes = np.array(in_nn.getFirstLayerFp16())
                # 将一维数组转换为Nx7矩阵
                bboxes = bboxes.reshape((bboxes.size // 7, 7))
                # 筛选出置信度小于定义阈值的结果
                bboxes = bboxes[bboxes[:, 2] > 0.5]
                # 剪切bbox和标签
                labels = bboxes[:, 1].astype(int)
                confidences = bboxes[:, 2]
                bboxes = bboxes[:, 3:7]

            if frame is not None:
                # 如果图像不为空，请在其上绘制边框并显示图像
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(frame, raw_bbox)
                    cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("rgb", frame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst