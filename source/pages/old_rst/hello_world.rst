Hello World
===========

学习如何使用 DepthAI Python API 来显示彩色视频流

演示
##############

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="/_static/images/samples/minimal.mp4" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

依赖关系
####################

首先把开发环境设置好。本教程使用:

- Python 3.6 (Ubuntu) or Python 3.7 (Raspbian)。
- 安装或升级 DepthAI :ref:`Python API安装详解`
- 需要用到 :code:`cv2` 和 :code:`numpy` Python 模块。


代码概述
#############

:code:`depthai` Python 模块可以让您访问主板上的4k 60Hz彩色相机。
我们将把这个相机模组的视频串流到你桌面上。
你可以在GitHub上找到 `本教程的完整源代码 <https://github.com/luxonis/depthai-tutorials/tree/master/1-hello-world>`__ ,也可以到 `gitee <https://gitee.com/oakchina/depthai-tutorials/tree/master/1-hello-world>`_ 上找完整源代码。

文件设置
##########

在你的计算机上设置以下文件结构:

.. code-block:: bash

  cd ~
  mkdir -p depthai-tutorials-practice/1-hello-world
  touch depthai-tutorials-practice/1-hello-world/hello-world.py
  cd depthai-tutorials-practice/1-hello-world

父目录名称里面的 :code:`-practice` 后缀是干什么的? 我们的教程可以在 `depthai-tutorials <https://gitee.com/oakchina/depthai-tutorials>`__ 代码库中获得。
在我们加上 :code:`-practice` 后，你就可以把你个人的代码和我们完整教程的代码区分开来（前提是你下载了这些教程）。


安装 pip 依赖
########################

要显示 DepthAI 彩色视频流，我们需要导入少量的软件包。
下载并安装本教程所需的包:

.. code-block:: bash

  python3 -m pip install numpy opencv-python depthai --user -i https://pypi.tuna.tsinghua.edu.cn/simple


测试你的环境
#####################

让我们验证一下是否能够加载所有的依赖项。 在你的代码编辑器中打开你之前 :ref:`创建 <文件设置>` 的 :code:`hello-world.py` 文件。 
复制并粘贴以下内容到 :code:`hello-world.py` 中：


.. code-block:: python

  import numpy as np # numpy - manipulate the packet data returned by depthai
  import cv2 # opencv - display the video stream
  import depthai # access the camera and its data packets

尝试运行脚本并确保其执行无误：

.. code-block:: bash

  python3 hello-world.py

如果你看到了如下的错误:

.. code-block::

  ModuleNotFoundError: No module named 'depthai'

...请按照 :ref:`我们的故障排除部分的这些步骤 <ImportError: 没有名为 'depthai' 的模块>` 来操作。

定义管道
#####################

DepthAI的任何动作，无论是神经推理还是彩色摄像机输出，都需要定义 **管道** ，包括与我们需求相对应的节点和连接。

在这种情况下，我们要查看 **彩色摄像机** 的帧，以及要在它们之上运行的简单 **神经网络**。

让我们从一个空 :class:`Pipeline` 对象开始

.. code-block:: python

  pipeline = depthai.Pipeline()

现在，我们要添加的第一个节点是 :class:`ColorCamera` 。我们将使用 :code:`preview` 调整为300x300的输出以适合 `mobilenet-ssd <https://docs.openvinotoolkit.org/latest/omz_models_public_mobilenet_ssd_mobilenet_ssd.html>`__ 输入大小（我们将在以后定义）

.. code-block:: python

  cam_rgb = pipeline.createColorCamera()
  cam_rgb.setPreviewSize(300, 300)
  cam_rgb.setInterleaved(False)

接下来，让我们NeuralNetwork用mobilenet-ssd network定义一个节点。此示例的Blob文件可在 `此处 <https://github.com/luxonis/depthai-tutorials/raw/e37989e07a36a57ffef624b7aa8cf20ab493fa07/1-hello-world/mobilenet-ssd/mobilenet-ssd.blob>`__ 找到。

.. code-block:: python

  detection_nn = pipeline.createNeuralNetwork()
  detection_nn.setBlobPath("/path/to/mobilenet-ssd.blob")

现在，让我们将彩色摄像头 :code:`preview` 输出连接到神经网络输入

.. code-block:: python

  cam_rgb.preview.link(detection_nn.input)

最后，我们希望同时接收彩色相机的帧和神经网络的推理结果-由于这些是在设备上生成的，因此需要将它们传输到我们的机器（主机）上。设备和主机之间的通信由来处理 :code:`XLink` ，在本例中，由于我们要从设备到主机接收数据，因此我们将使用 :class:`XLinkOut` 节点。

.. code-block:: python

  xout_rgb = pipeline.createXLinkOut()
  xout_rgb.setStreamName("rgb")
  cam_rgb.preview.link(xout_rgb.input)

  xout_nn = pipeline.createXLinkOut()
  xout_nn.setStreamName("nn")
  detection_nn.out.link(xout_nn.input)

初始化DepthAI设备
#############################

定义管道后，我们现在可以初始化设备并启动它

启动 DepthAI 设备:

.. code-block:: python

  device = depthai.Device(pipeline)
  device.startPipeline()

.. note::

  默认情况下，DepthAI被作为USB3设备访问。这有 :ref:`几个限制 <用2米长的USB3.0数据线时信号断断续续>` 。

  如果您想通过USB2进行通信（不受这些限制但带宽有限），请使用以下代码初始化DepthAI

  .. code-block:: python 

    device = depthai.Device(pipeline, True)
  
从这一点开始，管道将在设备上运行，并产生我们要求的结果。抓住他们

添加助手
#####################

由于 :class:`XLinkOut` 已经在管道中定义了节点，因此我们现在将定义主机端输出队列以访问产生的结果

.. code-block:: python

  q_rgb = device.getOutputQueue("rgb")
  q_nn = device.getOutputQueue("nn")

这些将充满结果，因此下一步是消耗结果。我们将需要两个占位符-一个用于rgb框架，一个用于nn结果

.. code-block:: python

  frame = None
  bboxes = []

同样，由于神经网络的实现细节，推理结果中的边界框坐标表示为<0..1>范围内的浮点数-因此相对于帧的宽度/高度（例如，如果图像的宽度为200px，并且nn返回的x_min坐标等于0.2 ，这意味着实际的（规范化的）x_min坐标为40px）。

这就是为什么我们需要定义一个辅助函数，:code:`frame_form` 将这些<0..1>值转换为实际像素位置的原因

.. code-block:: python

  def frame_norm(frame, bbox):
    return (np.array(bbox) * np.array([*frame.shape[:2], *frame.shape[:2]])[::-1]).astype(int)

消耗结果
####################

准备好一切之后，我们就可以开始主程序循环了

.. code-block:: python

  while True:
    # ...

现在，在此循环中，首先要做的是从nn节点和彩色摄像机获取最新结果

.. code-block:: python

  in_rgb = q_rgb.tryGet()
  in_nn = q_nn.tryGet()

如果队列为空，则该 :code:`tryGet` 方法将返回最新结果 :code:`None` 。

来自rgb摄像机或神经网络的结果将以一维数组的形式提供，因此它们都需要进行转换才能用于显示（我们已经定义了所需的转换之一 :code:`frame_norm` 函数）

首先，如果我们收到了来自RGB相机的帧，则需要将其从1D阵列转换为HWC形式（HWC代表“高度宽度通道”，因此是3D阵列，第一个尺寸是宽度，第二个高度，第三个是颜色通道）

.. code-block:: python

  if in_rgb is not None:
    shape = (3, in_rgb.getHeight(), in_rgb.getWidth())
    frame = in_rgb.getData().reshape(shape).transpose(1, 2, 0).astype(np.uint8)
    frame = np.ascontiguousarray(frame)

其次，神经网络的结果也将需要转换。它们也以一维数组的形式返回，但是这次数组的大小是固定的（无论神经网络实际产生多少结果，它的大小都是恒定的）。数组中的实际结果后跟 :code:`-1` ,然后用 :code:`0` 填充以满足固定大小。一个结果有7个字段，每个字段分别为 :code:`image_id, label, confidence, x_min, y_min, x_max, y_max` 。我们只需要最后四个值（即边界框），但我们还将过滤出 :code:`confidence` 低于某个阈值的值-它可以在<0..1>之间，在此示例中，我们将使用临界点 :code:`0.8`

.. code-block:: python

  if in_nn is not None:
    bboxes = np.array(in_nn.getFirstLayerFp16())
    bboxes = bboxes[:np.where(bboxes == -1)[0][0]]
    bboxes = bboxes.reshape((bboxes.size // 7, 7))
    bboxes = bboxes[bboxes[:, 2] > 0.8][:, 3:7]

为了更好地理解此流程，让我们举个例子。假设 :code:`np.array(in_nn.getFirstLayerFp16())` 返回以下数组

.. code-block:: bash

  [0, 15, 0.99023438, 0.45556641, 0.34399414  0.88037109, 0.9921875, 0, 15, 0.98828125, 0.03076172, 0.23388672, 0.60205078, 1.0078125, -1, 0, 0, 0, ...]

第一个操作，从数组中删除尾随零，所以现在bbox数组将如下所示 :code:`bboxes[:np.where(bboxes == -1)[0][0]]`

.. code-block:: bash

  [0, 15, 0.99023438, 0.45556641, 0.34399414  0.88037109, 0.9921875, 0, 15, 0.98828125, 0.03076172, 0.23388672, 0.60205078, 1.0078125]

第二个-将1D数组重塑为2D数组-其中每一行都是单独的结果 :code:`bboxes.reshape((bboxes.size // 7, 7))`

.. code-block:: bash

  [
    [0, 15, 0.99023438, 0.45556641, 0.34399414  0.88037109, 0.9921875],
    [0, 15, 0.98828125, 0.03076172, 0.23388672, 0.60205078, 1.0078125]
  ]

最后一个 :code:`bboxes = bboxes[bboxes[:, 2] > 0.8][:, 3:7]` -将根据置信度列（第三个，索引为 :code:`2` ）高于定义的阈值（ :code:`0.8` ）过滤结果-从这些结果中，仅将最后4列作为边界框。由于我们的两个结果都具有很高的置信度（ :code:`0.99023438` 和 :code:`0.98828125` ），因此不会对其进行过滤，最终数组将如下所示

.. code-block:: bash

  [
    [0.45556641, 0.34399414  0.88037109, 0.9921875],
    [0.03076172, 0.23388672, 0.60205078, 1.0078125]
  ]

显示结果
####################

到此为止，我们已经从DepthaI设备消耗了所有结果，剩下的就是实际显示它们。

.. code-block:: python

  if frame is not None:
    for raw_bbox in bboxes:
        bbox = frame_norm(frame, raw_bbox)
        cv2.rectangle(frame, (bbox[0], bbox[1]), (bbox[2], bbox[3]), (255, 0, 0), 2)
    cv2.imshow("preview", frame)

您可以在此处看到 :code:`frame_norm` 我们先前定义的用于边界框坐标归一化的用法。通过使用 :code:`cv2.rectangle` 我们在rgb框架上绘制一个矩形作为指示人脸位置的指示器，然后使用 :code:`cv2.imshow`

最后，我们添加了一种终止程序的方法（因为它在无限循环内运行）。我们将使用 :code:`cv2.waitKey`  的方法，即一键等待用户按下-在我们的例子中，我们要打破循环出来的时候用户按下 :code:`q` 键

运行示例
###########################

放在一起，剩下要做的就是运行我们在本教程中准备的文件并查看结果

.. code-block:: bash

  python3 hello_world.py

你已经上手了！ 你可以在 GitHub 上找到 `本教程的完整代码 <https://gitee.com/oakchina/depthai-tutorials/blob/master/1-hello-world/hello_world.py>`__ 。

.. include::  /pages/includes/footer-short.rst
