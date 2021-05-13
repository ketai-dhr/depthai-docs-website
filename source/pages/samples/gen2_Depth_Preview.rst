Gen2 深度信息流
===========================

本示例说明如何设置SGBM（半全局匹配）视差深度节点，如何通过XLink连接以将结果实时传输到主机，以及如何在OpenCV中显示深度图。请注意，这种情况下会使用视差，因为视差会以更直观的方式着色。

演示
***************************

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="http://player.bilibili.com/player.html?aid=501922123&bvid=BV1YN4119785&cid=304671462&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

设置
***********************

请运行以下命令以安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

  python3 -m pip install --extra-index-url https://artifacts.luxonis.com/artifactory/luxonis-python-snapshot-local/ depthai==0.0.2.1+c9a19df719cb668e438d6eafd193cdf60a0d9354 numpy==1.19.5 opencv-python==4.5.1.48

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
***********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/gen2_develop/examples/03_depth_preview.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/03_depth_preview.py>`_ 上找到。

.. code-block:: python

    import cv2
    import depthai as dai
    import numpy as np

    # 定义管道
    pipeline = dai.Pipeline()

    # 创建左右两个灰度相机流
    left = pipeline.createMonoCamera()
    left.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    left.setBoardSocket(dai.CameraBoardSocket.LEFT)

    right = pipeline.createMonoCamera()
    right.setResolution(dai.MonoCameraProperties.SensorResolution.THE_400_P)
    right.setBoardSocket(dai.CameraBoardSocket.RIGHT)

    # 创建一个将产生深度图的节点（使用视差输出，因为这样更容易可视化深度）
    depth = pipeline.createStereoDepth()
    depth.setConfidenceThreshold(200)
    left.out.link(depth.left)
    right.out.link(depth.right)

    # 创建输出流
    xout = pipeline.createXLinkOut()
    xout.setStreamName("disparity")
    depth.disparity.link(xout.input)

    # 管道已创建，现在将设备连接管道
    with dai.Device(pipeline) as device:
        # 开启管道
        device.startPipeline()

        # 输出队列将用于从上面定义的输出中获取视差帧
        q = device.getOutputQueue(name="disparity", maxSize=4, blocking=False)

        while True:
            in_depth = q.get()  # 阻止呼叫，将等待直到新数据到达
            # 数据最初表示为平面1维数组，需要将其转换为HxW形式
            frame = in_depth.getData().reshape((in_depth.getHeight(), in_depth.getWidth())).astype(np.uint8)
            frame = np.ascontiguousarray(frame)
            # 使用applyColorMap方法给图像添加伪色彩，将应用颜色图以突出显示深度信息
            frame = cv2.applyColorMap(frame, cv2.COLORMAP_JET)
            # 使用OpenCV的imshow方法显示图像
            cv2.imshow("disparity", frame)

            if cv2.waitKey(1) == ord('q'):
                break

.. include::  /pages/includes/footer-short.rst