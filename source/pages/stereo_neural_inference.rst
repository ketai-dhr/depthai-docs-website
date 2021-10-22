立体神经推理
#######################

在这种模式下，神经推理（物体检测，标志检测等）上运行的左 **和** 右摄像机，以产生立体声推理结果。与融合立体深度的单眼神经推理不同 - 没有最大视差搜索限制 - 所以最小距离纯粹受 (a) 立体相机本身的水​​平视野 (HFOV) 和 (b) 超焦距中的较大者限制的相机。

全局快门同步立体对的超焦距为 19.6cm。因此，距离小于 19.6 厘米的物体会显得失焦。这有效地用于该操作模式的最小距离，因为在大多数情况下(除用于与非常宽的立体声基线 `OAK-FFC-3P-OG <https://docs.luxonis.com/projects/hardware/en/latest/pages/BW1098FFC.html>`__),
这 **有效** 最小距离大于较高 **实际** 最小距离为的结果立体相机视野。 例如，当距离 `OAK-D <https://docs.luxonis.com/projects/hardware/en/latest/pages/BW1098OAK.html>`__ 小于 `5.25cm
<https://www.google.com/search?ei=GapBX-y3BsuxtQa3-YaQBw&q=%3Dtan%28%2890-71%2F2%29*pi%2F180%29*7.5%2F2&oq=%3Dtan%28%2890-71%2F2%29*pi%2F180%29*7.5%2F2&gs_lcp=CgZwc3ktYWIQAzoECAAQR1DZkwxYmaAMYPilDGgAcAF4AIABS4gB1AKSAQE1mAEAoAEBqgEHZ3dzLXdpesABAQ&sclient=psy-ab&ved=0ahUKEwisqPat-6_rAhXLWM0KHbe8AXIQ4dUDCAw&uact=5>`__
(下图中标记为 **M**)时, 物体将完全脱离两个灰度相机的视野，但比灰度相机的超焦距更近 (即 19.6cm，标记为 **Y**),
所以实际的最小深度就是这个超焦距。

.. image:: https://user-images.githubusercontent.com/59799831/132251763-02d7a767-a057-43e9-8704-e8cb1ec5f497.jpeg
  :alt: Minimum perceiving distance

因此，要计算此操作模式的最小距离，请使用以下公式：

.. code-block:: python

  min_distance = max(tan((90 - HFOV/2) * pi/2) * base_line_dist/2, 19.6)

该公式实现了 HFOV 施加的最小距离的最大值，以及 19.6cm，即超焦距施加的最小距离。

Demo
****

.. image:: https://user-images.githubusercontent.com/59799831/132098832-70a2d0b9-1a30-4994-8dad-dc880a803fb3.gif
  :target: https://github.com/luxonis/depthai-experiments/tree/master/gen2-triangulation
  :alt: Triangulation Demo

有关更多信息，请查看 执行立体神经接口的 `gen2-triangulation 演示 <https://github.com/luxonis/depthai-experiments/tree/master/gen2-triangulation>`__ 。

.. include::  /pages/includes/footer-short.rst