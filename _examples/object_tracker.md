---
layout: default
title: 示例 - DepthAI的物体追踪示例
toc_title: 物体追踪
description: 使用object_tracker流的示例 
order: 4
---
# {{ page.title }}

## 演示

<video muted autoplay controls>
    <source src="/images/samples/tracking.mp4" type="video/mp4">
</video>

## 源代码

```python
import consts.resource_paths
import cv2
import depthai

if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
    raise RuntimeError("Error initializing device. Try to reset it.")

p = depthai.create_pipeline(config={
    "streams": ["previewout", "object_tracker"],
    "ai": {
        "blob_file": consts.resource_paths.blob_fpath,
        "blob_file_config": consts.resource_paths.blob_config_fpath,
    },
    'ot': {
        'max_tracklets': 20,
        'confidence_threshold': 0.5,
    },
})

if p is None:
    raise RuntimeError("Error initializing pipelne")

entries_prev = []
labels = ["background", "aeroplane", "bicycle", "bird", "boat", "bottle", "bus", "car", "cat", "chair", "cow", "diningtable", "dog", "horse", "motorbike", "person", "pottedplant", "sheep", "sofa", "train", "tvmonitor"]
tracklets = None

while True:
    for packet in p.get_available_data_packets():
        if packet.stream_name == 'object_tracker':
            tracklets = packet.getObjectTracker()
        elif packet.stream_name == 'previewout':
            data = packet.getData()
            data0 = data[0, :, :]
            data1 = data[1, :, :]
            data2 = data[2, :, :]
            frame = cv2.merge([data0, data1, data2])

            traklets_nr = tracklets.getNrTracklets() if tracklets is not None else 0

            for i in range(traklets_nr):
                tracklet = tracklets.getTracklet(i)
                left = tracklet.getLeftCoord()
                top = tracklet.getTopCoord()
                right = tracklet.getRightCoord()
                bottom = tracklet.getBottomCoord()
                tracklet_label = labels[tracklet.getLabel()]

                cv2.rectangle(frame, (left, top), (right, bottom), (255, 0, 0))

                middle_pt = (int(left + (right - left) / 2), int(top + (bottom - top) / 2))
                cv2.circle(frame, middle_pt, 0, (255, 0, 0), -1)
                cv2.putText(frame, f"ID {tracklet.getId()}", middle_pt, cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 0, 0), 2)

                cv2.putText(frame, tracklet_label, (left, bottom - 40), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 0, 0), 2)
                cv2.putText(frame, tracklet.getStatus(), (left, bottom - 20), cv2.FONT_HERSHEY_SIMPLEX, 0.5, (255, 0, 0), 2)

            cv2.imshow('previewout', frame)

    if cv2.waitKey(1) == ord('q'):
        break

del p
depthai.deinit_device()
```

## 解释
<div class="alert alert-primary" role="alert">
<i class="material-icons info">
contact_support
</i>
  <strong>刚开始使用DepthAI?</strong><br/>
  <span class="small">DepthAI的基础知识在[DepthAI工作的最短代码](/examples/minimal_working_example/#explanation)和[OAK套件的Hello World](/tutorials/hello_world/)中有解释。</span>
</div>

DepthAI能够从设备本身进行物体追钟，所以你不必为此编写自己的自定义代码。

首先，我们改变从管道中检索数据包的方法，因为我们没有使用`metaout`流。

```python
    for packet in p.get_available_data_packets():
```

接下来，如果数据包来自流`object_tracker`，我们使用一个特殊的方法，只有在来自这个流的数据包中才能获得tracklets对象--如果在其他流上使用这个方法，会抛出一个错误。

```python
        if packet.stream_name == 'object_tracker':
            tracklets = packet.getObjectTracker()
``` 

接下来，我们要获取所有tracklet的信息，以便进一步处理。

```python
                tracklet = tracklets.getTracklet(i)
                left = tracklet.getLeftCoord()
                top = tracklet.getTopCoord()
                right = tracklet.getRightCoord()
                bottom = tracklet.getBottomCoord()
                tracklet_label = labels[tracklet.getLabel()]
```

而其余的处理只是为了直观地表现跟踪对象，如果你想以不同的方式使用物体追踪器，可以跳过这部分。
