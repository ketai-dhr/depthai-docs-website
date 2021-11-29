初识积木
=================

OAK设备结合可拖拽积木，让OAK编程变得简单有趣。

积木种类
################

.. image:: /_static/images/scratch/pageBlocks.png
    :alt: "积木总览"

- 内置积木：这类积木，可以操作精灵，舞台等
- OAK拓展积木：操作OAK设备

OAK积木介绍
################

1. 初始化OAK管道

.. image:: /_static/images/scratch/pipelineBlocks.png
    :alt: "初始化管道积木"

OAK设备启动必需的积木，在此基础上，和“加载节点积木”积木结合，实现各种模型的预处理过程。

2. 加载()节点

.. image:: /_static/images/scratch/nodeBlocks.png
    :alt: "加载节点积木"

各种模型节点
********************

3. 人脸检测

- 人脸数据：所有人脸坐标

.. image:: /_static/images/scratch/face_detection_data.png

- 人脸编号数据：获取某编号的人脸坐标数据

.. image:: /_static/images/scratch/faceID.png

- 人脸坐标：获取某个人脸数据的x,y坐标

.. image:: /_static/images/scratch/facePoint.png

- 绘制人脸框

.. image:: /_static/images/scratch/drawFace.png

4. 人脸识别

- 人脸识别数据

.. image:: /_static/images/scratch/face_recognition_data.png

- 添加标记：为识别数据添加名称

.. image:: /_static/images/scratch/add_face_mark.png

- 删除标记：删除已标记名

.. image:: /_static/images/scratch/remove_face_mark.png

- 绘制人脸识别结果

.. image:: /_static/images/scratch/draw_face_recognition.png

5. 口罩检测

- 罩检测坐标数据

.. image:: /_static/images/scratch/mask_data.png

- 绘制口罩

.. image:: /_static/images/scratch/draw_mask.png

6. 物体识别

- 物体识别名称/坐标：获取识别物体的名称或坐标

.. image:: /_static/images/scratch/object_recognition_data.png

- 绘制物体检测

.. image:: /_static/images/scratch/draw_object.png

7. 表情数据

- 表情数据：识别的表情happy，angry等

.. image:: /_static/images/scratch/draw_gaze_estimation.png

- 绘制人脸识别结果

.. image:: /_static/images/scratch/gaze_estimation_data.png

8. 人脸网格数据

- 人脸网格坐标

.. image:: /_static/images/scratch/face_facemesh_data.png

- 绘制人脸网格

.. image:: /_static/images/scratch/draw_facemesh.png

9. 人体关节检测

- 人体关节坐标

.. image:: /_static/images/scratch/pose_estimation_data.png

- 绘制人体关节

.. image:: /_static/images/scratch/draw_pose_estimation.png

10. 指关节检测

- 人体关节坐标

.. image:: /_static/images/scratch/hand_landmarks_data.png

- 绘制人体关节图

.. image:: /_static/images/scratch/draw_hand_landmarks.png

11. 开启/关闭预览

.. image:: /_static/images/scratch/preview.png

开启预览后，可以在舞台区预览画面


.. include::  /pages/includes/footer-long.rst