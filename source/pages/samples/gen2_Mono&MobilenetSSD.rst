Gen2 灰度相机和运行MobilenetSSD模型
=========================================================

这个例子展示了如何在左侧灰度相机上运行 MobileetV2SSD模型，以及如何在右侧相机流的预览中显示神经网络结果。

演示
***************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=289377520&bvid=BV1Wf4y167yk&cid=304669352&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
*******************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

    python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

这个示例还需要 mobileenetsdd blob ( :code:`mobilenet.blob` 文件 )才能工作——您可以从 `这里 <https://artifacts.luxonis.com/artifactory/luxonis-depthai-data-local/network/mobilenet-ssd_openvino_2021.2_6shave.blob>`_ 下载它。

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/09_mono_mobilenet.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/09_mono_mobilenet.py>`_ 上找到。

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

    # 创建灰度相机流
    cam_right = pipeline.createMonoCamera()
    cam_right.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    cam_right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    # 定义一个将基于源帧进行预测的神经网络
    detection_nn = pipeline.createNeuralNetwork()
    detection_nn.setBlobPath(mobilenet_path)

    # 创建一个节点以将灰度框架转换为神经网络可接受的形式
    manip = pipeline.createImageManip()
    manip.initialConfig.setResize(300, 300)
    # NN模型需要BGR输入。默认情况下，ImageManip输出类型将与输入相同（在这种情况下为灰色）
    manip.initialConfig.setFrameType(dai.RawImgFrame.Type.BGR888p)
    cam_right.out.link(manip.inputImage)
    manip.out.link(detection_nn.input)

    # 创建输出
    xout_manip = pipeline.createXLinkOut()
    xout_manip.setStreamName("right")
    manip.out.link(xout_manip.input)

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

        # 输出队列将用于从上面定义的输出中获取灰度帧和nn数据
        q_right = device.getOutputQueue("right", maxSize=4, blocking=False)
        q_nn = device.getOutputQueue("nn", maxSize=4, blocking=False)

        frame = None
        bboxes = []
        confidences = []
        labels = []

        # nn数据（作为边界框的位置）在<0..1>范围内-需要使用图像的width/height对其进行归一化
        def frame_norm(frame, bbox):
            norm_vals = np.full(len(bbox), frame.shape[0])
            norm_vals[::2] = frame.shape[1]
            return (np.clip(np.array(bbox), 0, 1) * norm_vals).astype(int)


        while True:
            # 使用tryGet方法（非阻塞）而不是get方法来获取（阻塞），这将返回可用数据，否则返回None
            in_right = q_right.tryGet()
            in_nn = q_nn.tryGet()

            if in_right is not None:
                # 如果灰度帧数据不为空，则将一维数据转换为HxWxC帧
                shape = (3, in_right.getHeight(), in_right.getWidth())
                frame = in_right.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
                frame = np.ascontiguousarray(frame)

            if in_nn is not None:
                # 检测结果有7个数，最后一次检测后跟着-1位数，以后填充0
                bboxes = np.array(in_nn.getFirstLayerFp16())
                # 仅取-1位之前的结果
                bboxes = bboxes[:np.where(bboxes == -1)[0][0]]
                # 将一维数组转换为Nx7矩阵
                bboxes = bboxes.reshape((bboxes.size // 7, 7))
                # 筛选出置信度小于定义阈值的结果
                bboxes = bboxes[bboxes[:, 2] > 0.5]
                # 剪切bbox和标签
                labels = bboxes[:, 1].astype(int)
                bboxes = bboxes[:, 3:7]
                confidences = bboxes[:, 2]

            if frame is not None:
                # 如果图像不为空，请在其上绘制边框并显示图像
                for raw_bbox, label, conf in zip(bboxes, labels, confidences):
                    bbox = frame_norm(frame, raw_bbox)
                    cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
                    cv2.putText(frame, texts[label], (bbox[0] + 10, bbox[1] + 20), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                    cv2.putText(frame, f"{int(conf * 100)}%", (bbox[0] + 10, bbox[1] + 40), cv2.FONT_HERSHEY_TRIPLEX, 0.5, 255)
                cv2.imshow("right", frame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst