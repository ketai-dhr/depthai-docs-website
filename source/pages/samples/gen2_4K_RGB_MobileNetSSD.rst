Gen2 4K彩色相机运行MobileNetSSD模型
==============================================

这个例子展示了如何在RGB预览流中运行mobileenetv2ssd模型，以及如何在预览中同时显示RGB预览和mobileenetv2ssd模型的元数据结果。预览大小设置为4K分辨率。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=331947869&bvid=BV12A411K7aG&cid=304660792&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
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

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/MobileNet/rgb_mobilenet_4k.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/MobileNet/rgb_mobilenet_4k.py>`_ 上找到。

.. code-block:: python

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np

    # 首先获取模型
    mobilenet_path = str((Path(__file__).parent / Path('models/mobilenet.blob')).resolve().absolute())
    if len(sys.argv) > 1:
        mobilenet_path = sys.argv[1]

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机流
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(300, 300)    # 设置神经网络输入大小
    # 设置彩色相机分辨率为4K
    cam_rgb.setResolution(dai.ColorCameraProperties.SensorResolution.THE_4_K)
    # 设置是否交错
    cam_rgb.setInterleaved(False)
    # 设置是否预览保持宽高比
    cam_rgb.setPreviewKeepAspectRatio(False)

    # 定义一个将基于源帧进行预测的神经网络
    detection_nn = pipeline.createNeuralNetwork()
    detection_nn.setBlobPath(mobilenet_path)
    cam_rgb.preview.link(detection_nn.input)

    # 创建输出流
    xout_video = pipeline.createXLinkOut()
    xout_video.setStreamName("video")
    cam_rgb.video.link(xout_video.input)

    xout_preview = pipeline.createXLinkOut()
    xout_preview.setStreamName("preview")
    cam_rgb.preview.link(xout_preview.input)

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

        # 输出队列将用于从上面定义的输出中获取帧和nn数据
        q_video = device.getOutputQueue(name="video", maxSize=4, blocking=False)
        q_preview = device.getOutputQueue(name="preview", maxSize=4, blocking=False)
        q_nn = device.getOutputQueue(name="nn", maxSize=4, blocking=False)

        preview_frame = None
        video_frame = None
        bboxes = []
        labels = []
        confidences = []


        # nn数据（作为边界框的位置）在<0..1>范围内-需要使用图像的width/height对其进行归一化
        def frame_norm(frame, bbox):
            norm_vals = np.full(len(bbox), frame.shape[0])
            norm_vals[::2] = frame.shape[1]
            return (np.clip(np.array(bbox), 0, 1) * norm_vals).astype(int)


        def display_frame(name, frame, bboxes):
            for raw_bbox in bboxes:
                bbox = frame_norm(frame, raw_bbox)
                cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]),
                            (255, 0, 0), 2)
            cv2.imshow(name, frame)


        while True:
            # 使用tryGet方法（非阻塞）而不是get方法来获取（阻塞），这将返回可用数据，否则返回None
            in_video = q_video.tryGet()
            in_preview = q_preview.tryGet()
            in_nn = q_nn.tryGet()

            if in_video is not None:
                # 如果摄像机的数据不为空，则将一维数据转换为HxWxC帧
                packetData = in_video.getData()
                w = in_video.getWidth()
                h = in_video.getHeight()
                yuv420p = packetData.reshape((h * 3 // 2, w))
                video_frame = cv2.cvtColor(yuv420p, cv2.COLOR_YUV2BGR_NV12)

            if in_preview is not None:
                shape = (3, in_preview.getHeight(), in_preview.getWidth())
                preview_frame = in_preview.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
                preview_frame = np.ascontiguousarray(preview_frame)

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

            # 如果图像不为空，请在其上绘制边框并显示图像
            if video_frame is not None:
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(video_frame, raw_bbox)
                    cv2.rectangle(video_frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(video_frame, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(video_frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                display_frame("video", video_frame, bboxes)

            if preview_frame is not None:
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(preview_frame, raw_bbox)
                    cv2.rectangle(preview_frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(preview_frame, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(preview_frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                display_frame("preview", preview_frame, bboxes)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst