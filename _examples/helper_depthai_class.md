---
layout: default
title: 将DepthAI作为一个类使用
toc_title: 帮助文件 - 将DepthAI作为一个类使用
description: 将DepthAI代码作为一个类使用的示例
order: 6
---

# {{ page.title }}

如果你想把DepthAI集成到你的项目中，这个例子可能会对你有用，因为它把 "__如何__ 获得结果 "和 "用它们 __做什么__ "分开了。

这个例子将管道初始化和实际使用分离开来。

如果你想为它创建更多的子类(例如 _MonoDepthAI_，用来包含黑白相机的配置)，或者在一个子进程中运行DepthAI代码(例如 `Process(target=DepthAI().run).start()`)，那么这个例子就很有用。

## 代码

```python
from pathlib import Path

import consts.resource_paths
import cv2
import depthai


class DepthAI:
    def __init__(self):
        if not depthai.init_device(consts.resource_paths.device_cmd_fpath):
            raise RuntimeError("Error initializing device. Try to reset it.")

        self.p = depthai.create_pipeline(config={
            "streams": ["metaout", "previewout"],
            "ai": {
                "blob_file": "/path/to/model.blob",
                "blob_file_config": "/path/to/config.json"
            }
        })

        self.entries_prev = []

    def run(self):
        while True:
            nnet_packets, data_packets = self.p.get_available_nnet_and_data_packets()

            for _, nnet_packet in enumerate(nnet_packets):
                self.entries_prev = []
                for _, e in enumerate(nnet_packet.entries()):
                    if e[0]['image_id'] == -1.0 or e[0]['conf'] == 0.0:
                        break
                    if e[0]['conf'] > 0.5:
                        self.entries_prev.append(e[0])

            for packet in data_packets:
                if packet.stream_name == 'previewout':
                    data = packet.getData()
                    if data is None:
                        continue
                    data0 = data[0, :, :]
                    data1 = data[1, :, :]
                    data2 = data[2, :, :]
                    frame = cv2.merge([data0, data1, data2])

                    img_h = frame.shape[0]
                    img_w = frame.shape[1]

                    for e in self.entries_prev:
                        pt1 = int(e['x_min'] * img_w), int(e['y_min'] * img_h)
                        pt2 = int(e['x_max'] * img_w), int(e['y_max'] * img_h)

                        cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

                    cv2.imshow('previewout', frame)

            if cv2.waitKey(1) == ord('q'):
                break

        del self.p
        depthai.deinit_device()


DepthAI().run()
```
