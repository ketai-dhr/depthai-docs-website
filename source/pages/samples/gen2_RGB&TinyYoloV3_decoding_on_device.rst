Gen2 设备上的彩色相机和TinyYoloV3模型解码
================================================================

本示例说明如何在RGB输入帧上运行TinyYoloV3，以及如何在预览中同时显示RGB预览和TinyYoloV3的元数据结果。 解码是在Myriad上完成的，而不是在主机上完成的。

需要正确配置，依赖网络的参数才能正确解码：setNumClasses-YOLO类的数量setCoordinateSize-坐标setAnchors的大小-yolo锚setAnchorMasks-anchorMasks26，anchorMasks13（anchorMasks52-另外用于完整的YOLOv3）setIouThreshold-高于联合阈值setConfidenceThreshold的交集检测到哪些物体。

演示
**********************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=886993048&bvid=BV1eK4y1J7vX&cid=304671292&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个示例还需要 TinyYoloV3 blob ( :code:`TinyYoloV3.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`_ 下载它。


源代码
*********************

也可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/22_tiny_yolo_v3_device_side_decoding.py>`_ 上找到。

.. code-block:: python

    from pathlib import Path
    import sys
    import cv2
    import depthai as dai
    import numpy as np
    import time

    '''
    Tiny-Yolo-V3设备侧解码演示
      YOLO v3 Tiny是从此存储库<https：github.comdavid8862keras-YOLOv3-model-set>使用Keras实现的实时对象检测模型，并转换为TensorFlow框架。
      该模型在具有80个类别的COCO数据集上进行了预训练。
    '''

    # yolo v3的标签文本
    label_map = ["person",         "bicycle",    "car",           "motorbike",     "aeroplane",   "bus",           "train",
                 "truck",          "boat",       "traffic light", "fire hydrant",  "stop sign",   "parking meter", "bench",
                 "bird",           "cat",        "dog",           "horse",         "sheep",       "cow",           "elephant",
                 "bear",           "zebra",      "giraffe",       "backpack",      "umbrella",    "handbag",       "tie",
                 "suitcase",       "frisbee",    "skis",          "snowboard",     "sports ball", "kite",          "baseball bat",
                 "baseball glove", "skateboard", "surfboard",     "tennis racket", "bottle",      "wine glass",    "cup",
                 "fork",           "knife",      "spoon",         "bowl",          "banana",      "apple",         "sandwich",
                 "orange",         "broccoli",   "carrot",        "hot dog",       "pizza",       "donut",         "cake",
                 "chair",          "sofa",       "pottedplant",   "bed",           "diningtable", "toilet",        "tvmonitor",
                 "laptop",         "mouse",      "remote",        "keyboard",      "cell phone",  "microwave",     "oven",
                 "toaster",        "sink",       "refrigerator",  "book",          "clock",       "vase",          "scissors",
                 "teddy bear",     "hair drier", "toothbrush"]


    syncNN = True

    # 首先获取模型
    tiny_yolo_v3_path = str((Path(__file__).parent / Path('models/tiny_yolo_v3_6shaves.blob')).resolve().absolute())
    if len(sys.argv) > 1:
        tiny_yolo_v3_path = sys.argv[1]

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 创建彩色相机流
    cam_rgb = pipeline.createColorCamera()
    cam_rgb.setPreviewSize(416, 416)
    cam_rgb.setInterleaved(False)
    cam_rgb.setFps(40)

    # 特定于网络的设置
    detectionNetwork = pipeline.createYoloDetectionNetwork()
    detectionNetwork.setConfidenceThreshold(0.5)
    detectionNetwork.setNumClasses(80)
    detectionNetwork.setCoordinateSize(4)
    anchors = np.array([10,14, 23,27, 37,58, 81,82, 135,169, 344,319])
    detectionNetwork.setAnchors(anchors)
    anchorMasks26 = np.array([1,2,3])
    anchorMasks13 = np.array([3,4,5])
    anchorMasks = {
        "side26": anchorMasks26,
        "side13": anchorMasks13,
    }
    detectionNetwork.setAnchorMasks(anchorMasks)
    detectionNetwork.setIouThreshold(0.5)

    detectionNetwork.setBlobPath(tiny_yolo_v3_path)
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
                # 如果来自RGB相机的数据不为空，则将1D数据转换为HxWxC帧
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