上手案例
================

实现逻辑
################

.. code-block::

                    OAK积木搭建流程
    ———————————————————————————————————————————————
    |   ----------------                          |
    |   | 初始化OAK管道 |                          |
    |   |   |-----------       --------------     |
    |   |   |            ◄———— | 加载OAK节点 |     |
    |   |   |-----------       --------------     |
    |   |              |                          |
    |   ----------------                          |
    |          |                                  |
    |          |                                  |
    |          ▼                                  |
    |   ---------------                           |
    |   | 开启图像预览 |                           |
    |   ---------------                           |
    |          |                                  |
    |          |                                  |
    |          ▼                                  |
    |   ---------------                           |
    |   | 循环执行绘制 |        -----------------  |
    |   |   |----------        |       ------- |  |
    |   |   |            ◄———— | 绘制  | 数据 | |  |
    |   |   |----------        |       ------- |  |
    |   |             |        -----------------  |
    |   ---------------                           |
    |                                             |
    ———————————————————————————————————————————————

案例
##############

1.人体姿态估计

.. image:: /_static/images/scratch/pose_case.gif

.. image:: /_static/images/scratch/pose_case.jpeg

2.口罩识别

.. image:: /_static/images/scratch/mask_case.gif

.. image:: /_static/images/scratch/mask_case.jpeg

3.物体识别

.. image:: /_static/images/scratch/object_case.gif

.. image:: /_static/images/scratch/object_case.jpeg

4.深度信息人脸检测

.. image:: /_static/images/scratch/spatial_face_case.gif

.. image:: /_static/images/scratch/spatial_face_case.jpeg

5.人脸检测

.. image:: /_static/images/scratch/face_case.gif

.. image:: /_static/images/scratch/face_case.jpeg

6.情绪识别

.. image:: /_static/images/scratch/estimation_case.gif

.. image:: /_static/images/scratch/estimation_case.jpeg

7.人脸网格

.. image:: /_static/images/scratch/facemesh_case.gif

.. image:: /_static/images/scratch/facemesh_case.jpeg

8.人脸识别

.. image:: /_static/images/scratch/recognition_case.gif

.. image:: /_static/images/scratch/recognition_case.jpeg

9.手掌识别

.. image:: /_static/images/scratch/hand_case.gif

.. image:: /_static/images/scratch/hand_case.jpeg

10.深度信息物体检测

.. image:: /_static/images/scratch/spatial_object_case.gif

.. image:: /_static/images/scratch/spatial_object_case.jpeg

.. include::  /pages/includes/footer-long.rst