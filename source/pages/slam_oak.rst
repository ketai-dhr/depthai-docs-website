OAK 与 SLAM
====================

可以使用我们的OAK设备执行 `SLAM <https://en.wikipedia.org/wiki/Simultaneous_localization_and_mapping>`__ (同时定位和映射)。

我们计划创建我们自己的视觉惯性 SLAM 项目，该项目将使用设备上的特征跟踪(使用 `FeatureTracker <https://github.com/luxonis/depthai-python/blob/develop/examples/feature_tracker.py>`__ 节点)和间隙数据(使用 `IMU <https://docs.luxonis.com/projects/api/en/latest/components/nodes/imu/>`__ 节点)，如果设备有板载 IMU 传感器。

- `ArduCam Visual SLAM tutorial <https://www.arducam.com/docs/opencv-ai-kit-oak/performing-location-with-visual-slam/>`__
- `OAKD_ORBSLAM3 <https://github.com/duncanrhamill/oakd_orbslam3>`__
- `DepthAI-SLAM <https://github.com/bharath5673/depthai-slam>`__

您还可以 :code:`#slam` 在我们的 `Discord server <https://discord.gg/EPsZHkg9Nx>`__ ,服务器上查看我们的频道，那里有大量关于如何在 OAK 设备上执行 SLAM 的有用信息。

OAK与ROS
****************

请查看 `此文档 <https://github.com/luxonis/depthai-ros>`__

.. include::  /pages/includes/footer-short.rst