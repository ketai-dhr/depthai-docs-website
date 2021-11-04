Gen2 灰度相机控制
=========================

本示例说明了如何控制设备端的裁剪和相机触发器。TWo输出是显示的单幅裁剪帧，可以使用以下按键进行操作:

- 按 :code:`a` 将使裁剪范围向左移动
- 按 :code:`d` 将使裁剪范围向右移动
- 按 :code:`w` 将使裁剪范围向上移动
- 按 :code:`s` 将使裁剪范围向下移动
- 按 :code:`e` 将触发自动曝光
- 按 :code:`i` 和 :code:`o` 将减少/增加曝光时间
- 按 :code:`k` 和 :code:`l` 将降低/增加iso的灵敏度

演示
**********************

设置
********************

请运行以下命令来安装所需的依赖项

.. warning::
    说明：此处安装的是第二代depthai库

.. code-block:: bash

    python3 -m pip install -U pip
    python3 -m pip install opencv-python
    python3 -m pip install -U --force-reinstall depthai

有关更多信息，请参阅 :ref:`Python API 安装指南 <Python API安装详解>`

源代码
*********************

可以在 `GitHub <https://github.com/luxonis/depthai-python/blob/main/examples/MonoCamera/mono_camera_control.py>`_ 上找到。国内用户也可以在 `gitee <https://gitee.com/oakchina/depthai-python/blob/main/examples/MonoCamera/mono_camera_control.py>`_ 上找到。

.. code-block:: python

    #!/usr/bin/env python3

    """
    本示例显示了在修剪模式下使用灰度相机并可以移动修剪的可能性。
    使用“ WASD”控件移动裁剪窗口，使用“ T”触发自动对焦“ IOKL”。 手动曝光/对焦:
    Control:      key[dec/inc]  min..max
    exposure time:     I   O      1..33000 [us]
    sensitivity iso:   K   L    100..1600
    要返回自动控制，请执行以下操作:
    'E' - autoexposure
    """


    import cv2
    import depthai as dai

    # 步长 ('W','A','S','D' 控制)
    stepSize = 0.02
    # 手动曝光/对焦设置步骤
    expStep = 500  # us
    isoStep = 50

    # 开始定义管道
    pipeline = dai.Pipeline()

    # 定义两个单（灰度）相机
    camRight = pipeline.createMonoCamera()
    camRight.setBoardSocket(dai.CameraBoardSocket.RIGHT)
    camRight.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)
    camLeft = pipeline.createMonoCamera()
    camLeft.setBoardSocket(dai.CameraBoardSocket.LEFT)
    camLeft.setResolution(dai.MonoCameraProperties.SensorResolution.THE_720_P)

    # 作用范围
    topLeft = dai.Point2f(0.4, 0.4)
    bottomRight = dai.Point2f(0.6, 0.6)

    manipRight = pipeline.createImageManip()
    manipRight.initialConfig.setCropRect(topLeft.x, topLeft.y, bottomRight.x, bottomRight.y)
    manipLeft = pipeline.createImageManip()
    manipLeft.initialConfig.setCropRect(topLeft.x, topLeft.y, bottomRight.x, bottomRight.y)
    manipRight.setMaxOutputFrameSize(camRight.getResolutionHeight()*camRight.getResolutionWidth()*3)

    # 相机移动配置 (wasd)
    configIn = pipeline.createXLinkIn()
    configIn.setStreamName('config')
    configIn.out.link(manipRight.inputConfig)
    configIn.out.link(manipLeft.inputConfig)

    # 相机控制 (exp, iso, focus)
    controlIn = pipeline.createXLinkIn()
    controlIn.setStreamName('control')
    controlIn.out.link(camRight.inputControl)
    controlIn.out.link(camLeft.inputControl)

    # 与USB连结
    camRight.out.link(manipRight.inputImage)
    camLeft.out.link(manipLeft.inputImage)

    # 创建输出流
    manipOutRight = pipeline.createXLinkOut()
    manipOutRight.setStreamName("right")
    manipRight.out.link(manipOutRight.input)

    manipOutLeft = pipeline.createXLinkOut()
    manipOutLeft.setStreamName("left")
    manipLeft.out.link(manipOutLeft.input)

    def clamp(num, v0, v1):
        return max(v0, min(num, v1))

    # 连接并启动管道
    with dai.Device(pipeline) as device:

        # 输出队列将用于获取灰度帧
        qRight = device.getOutputQueue(manipOutRight.getStreamName(), maxSize=4, blocking=False)
        qLeft = device.getOutputQueue(manipOutLeft.getStreamName(), maxSize=4, blocking=False)
        configQueue = device.getInputQueue(configIn.getStreamName())
        controlQueue = device.getInputQueue(controlIn.getStreamName())

        def displayFrame(name, frame):
            cv2.imshow(name, frame)

        sendCamConfig = False

        # 手动对焦/曝光控制的默认值和限制
        expTime = 20000
        expMin = 1
        expMax = 33000

        sensIso = 800
        sensMin = 100
        sensMax = 1600

        while True:
            inRight = qRight.get()
            inLeft = qLeft.get()
            frameRight = inRight.getCvFrame()
            frameLeft = inLeft.getCvFrame()
            displayFrame("right", frameRight)
            displayFrame("left", frameLeft)

            # 更新画面
            key = cv2.waitKey(1)
            if key == ord('q'):
                break
            elif key == ord('c'):
                ctrl = dai.CameraControl()
                ctrl.setCaptureStill(True)
                controlQueue.send(ctrl)
            elif key == ord('e'):
                print("Autoexposure enable")
                ctrl = dai.CameraControl()
                ctrl.setAutoExposureEnable()
                controlQueue.send(ctrl)
            elif key in [ord('i'), ord('o'), ord('k'), ord('l')]:
                if key == ord('i'): expTime -= expStep
                if key == ord('o'): expTime += expStep
                if key == ord('k'): sensIso -= isoStep
                if key == ord('l'): sensIso += isoStep
                expTime = clamp(expTime, expMin, expMax)
                sensIso = clamp(sensIso, sensMin, sensMax)
                print("Setting manual exposure, time:", expTime, "iso:", sensIso)
                ctrl = dai.CameraControl()
                ctrl.setManualExposure(expTime, sensIso)
                controlQueue.send(ctrl)
            elif key == ord('w'):
                if topLeft.y - stepSize >= 0:
                    topLeft.y -= stepSize
                    bottomRight.y -= stepSize
                    sendCamConfig = True
            elif key == ord('a'):
                if topLeft.x - stepSize >= 0:
                    topLeft.x -= stepSize
                    bottomRight.x -= stepSize
                    sendCamConfig = True
            elif key == ord('s'):
                if bottomRight.y + stepSize <= 1:
                    topLeft.y += stepSize
                    bottomRight.y += stepSize
                    sendCamConfig = True
            elif key == ord('d'):
                if bottomRight.x + stepSize <= 1:
                    topLeft.x += stepSize
                    bottomRight.x += stepSize
                    sendCamConfig = True


            if sendCamConfig:
                cfg = dai.ImageManipConfig()
                cfg.setCropRect(topLeft.x, topLeft.y, bottomRight.x, bottomRight.y)
                configQueue.send(cfg)
                sendCamConfig = False

.. include::  /pages/includes/footer-short.rst