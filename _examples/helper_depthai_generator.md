---
layout: default
title: 把DepthAI作为发生器使用
toc_title: 帮助文件 - 把DepthAI作为发生器使用
description: 把DepthAI代码作为发生器使用的示例
order: 6
---

# {{ page.title }}

如果你想把DepthAI集成到你的项目中，这个例子可能会对你有用，因为它把 "__如何__ 获得结果 "和 "用它们 __做什么__ "分开了。

这个例子使用`yield`关键字将结果发送到for循环，而for循环调用了这个方法。

如果你想在你的自定义代码中进一步处理接收到的帧，就能用得上这个例子。

## Code

```python
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
                    data0 = data[0, :, :]
                    data1 = data[1, :, :]
                    data2 = data[2, :, :]
                    frame = cv2.merge([data0, data1, data2])

                    img_h = frame.shape[0]
                    img_w = frame.shape[1]

                    results = []
                    for e in self.entries_prev:
                        pt1 = int(e['x_min'] * img_w), int(e['y_min'] * img_h)
                        pt2 = int(e['x_max'] * img_w), int(e['y_max'] * img_h)
                        results.append((pt1, pt2))
                    yield frame, results

    def __del__(self):
        del self.p
        depthai.deinit_device()


d = DepthAI()

for frame, results in d.run():
    for pt1, pt2 in results:
        cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)
    cv2.imshow('previewout', frame)

    if cv2.waitKey(1) == ord('q'):
        break


del d
```
