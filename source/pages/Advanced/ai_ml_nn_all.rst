模型性能
#################

您可以借助下表估计模型的性能。它包含依赖于 FLOP 和参数的 OAK 设备上模型的 FPS 估计。

.. image:: /_static/images/fps_models.png

`您可以在此表 <https://docs.google.com/spreadsheets/u/8/d/e/2PACX-1vQ_tVk2PhOhnFeJrL5t2rtncxHeDVYX8j1o52vdZozRzXJ5C3gq8EngVvx32suCPasIelIwU5Ny6tLE/pubhtml?gid=41416082&single=true>`__ 中找到更详细的通用模型 FPS评估。

AI视觉任务
###############

我们为许多不同的AI视觉任务提供开源示例和演示，例如：

- **Object detection** 模型提供所有检测到的对象的边界框、置信度和标签。 示例: `MobileNet <https://docs.luxonis.com/projects/api/en/latest/samples/MobileNet/rgb_mobilenet/#rgb-mobilenetssd>`__, `Yolo <https://docs.luxonis.com/projects/api/en/latest/samples/Yolo/tiny_yolo/#rgb-tiny-yolo>`__, `EfficientDet <https://github.com/luxonis/depthai-experiments/tree/master/gen2-efficientDet>`__, `Palm detection <https://github.com/luxonis/depthai-experiments/tree/master/gen2-palm-detection>`__.
- **Landmark detection** 模型提供对象的特征/关键点。 示例: `Human pose <https://github.com/geaxgx/depthai_blazepose#blazepose-tracking-with-depthai>`__, `hand landmarks <https://github.com/geaxgx/depthai_hand_tracker#hand-tracking-with-depthai>`__, and `facial landmarks <https://github.com/luxonis/depthai-experiments/tree/master/gen2-facemesh#gen2-facial-landmarks-on-depthai>`__.
- **Semantic segmentation** 模型为每个像素提供标签/类别。 示例: `Person segmentation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-deeplabv3_depth>`__, `multiclass segmentation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-deeplabv3_multiclass#gen2-deeplabv3-multiclass-on-depthai>`__, `road segmentation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-road-segmentation#gen2-road-segmentation-on-depthai>`__.
- **Classification** 模型提供分类标签和对该标签的置信度。 示例: `EfficientNet <https://github.com/luxonis/depthai-experiments/tree/master/gen2-efficientnet-classification#efficientnet-b0>`__, `Tensorflow classification <https://github.com/luxonis/depthai-experiments/tree/master/gen2-tf-image-classification#demo>`__, `fire classification <https://github.com/luxonis/depthai-experiments/tree/master/gen2-fire-detection>`__, `emotions classification <https://github.com/luxonis/depthai-experiments/tree/master/gen2-emotion-recognition>`__.
- **Recognition** 模型提供可用于识别或识别特征本身的字节数组。 示例: `Face recognition <https://github.com/luxonis/depthai-experiments/tree/master/gen2-face-recognition#demo>`__, `person identification <https://github.com/luxonis/depthai-experiments/tree/master/gen2-pedestrian-reidentification>`__, `OCR <https://github.com/luxonis/depthai-experiments/tree/master/gen2-ocr#how-to-run>`__, `license plate recognition <https://github.com/luxonis/depthai-experiments/tree/master/gen2-license-plate-recognition>`__.

还有许多其他 AI 视觉任务不属于上述任何类别， 例如 `crowd counting <https://github.com/luxonis/depthai-experiments/tree/master/gen2-crowdcounting#gen2-crowd-counting-with-density-maps-on-depthai>`__,
`monocular depth estimation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-depth-mbnv2>`__, `gaze estimation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-gaze-estimation>`__, 或
`age/gender estimation <https://github.com/luxonis/depthai-experiments/tree/master/gen2-age-gender>`__.

**上面的所有演示都在彩色/灰度帧上运行**。 其中许多视觉任务可以与深度感知(在OAK相机本身上) **融合** , **从而释放** :ref:`Spatial AI <spatialai>` 的力量。