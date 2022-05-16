如何最大化FOV
=====================

默认情况下, 当您使用 :ref:`ColorCamera <Gen2 彩色相机Preview流>` 的 :code:`preview` 输出时, DepthAI 将裁剪帧以获得所需的纵横比。例如，如果您使用的是 Mobilenet-SSD 模型，则需要:code:`300x300` 分辨率的图像。 OAK会将1080P帧裁剪为 :code:`1080x1080` 分辨率的图像，然后将它们调整为 :code:`300x300` 的分辨率。 这意味着您将丢失图像的某些部分。

如果您想最大化图像的 FOV，您可以：

#. 更改纵横比（拉伸图像）
#. 对图像使用Letterboxing

更改纵横比
*******************

使用 :code:`camRgb.setPreviewKeepAspectRatio(False)`.  这意味着不会保留纵横比并且图像将被"拉伸"。 对于某些现成的 NN 模型，这可能会出现问题，因此可能需要对其进行模型微调。
`此处是用法示例 <https://gitee.com/oakchina/depthai-python/blob/main/examples/MobileNet/rgb_mobilenet_4k.py>`__.

Letterboxing
************

对图像进行 `Letterboxing <https://en.wikipedia.org/wiki/Letterboxing_%28filming%29>`__ 。此方法将减小图像的大小并在图像上方和下方填充"黑条"，因此纵横比得以保留。 您可以通过将 :code:`ImageManip` 和 :code:`manip.setResizeThumbnail(x,y)` (对于Mobilenet :code:`x=300,y=300`) 一起使用来实现此目的。
使用这种方法的缺点是您的实际图像会更小，因此可能无法保留某些特征，这可能意味着 NN 精度可能会降低。
`此处是用法示例 <https://gitee.com/oakchina/depthai-python/blob/main/examples/ObjectTracker/object_tracker_video.py>`__.

.. image:: /_static/images/tutorials/fov.jpeg

.. include::  /pages/includes/footer-long.rst