帮助文件  - 把 DepthAI 作为一个类使用
==========================================

如果你想把 DepthAI 集成到你的项目中，这个例子可能会对你有用，
因为它把 “**如何** 获得结果”, 和 “用它们 **做什么** ”分开了。

这个例子将管道初始化和实际使用分离开来。

如果你想为它创建更多的子类 （例如 *MonoDepthAI* ，用来包含黑白相机的配置），
或者在一个子进程中运行 DepthAI 代码（例如 :code:`Process(target=DepthAI().run).start()`）

代码
####

.. code-block:: python

  from pathlib import Path

  import cv2
  import depthai


  class DepthAI:
      def __init__(self):
          self.device = depthai.Device('', False)

          self.p = self.device.create_pipeline(config={
              "streams": ["metaout", "previewout"],
              "ai": {
                  "blob_file": "/path/to/model.blob",
                  "blob_file_config": "/path/to/config.json"
              }
          })

          self.detections = []

      def run(self):
          while True:
              nnet_packets, data_packets = self.p.get_available_nnet_and_data_packets()

              for nnet_packet in nnet_packets:
                  self.detections = list(nnet_packet.getDetectedObjects())

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

                      for detection in self.detections:
                          pt1 = int(detection.x_min * img_w), int(detection.y_min * img_h)
                          pt2 = int(detection.x_max * img_w), int(detection.y_max * img_h)

                          cv2.rectangle(frame, pt1, pt2, (0, 0, 255), 2)

                      cv2.imshow('previewout', frame)

              if cv2.waitKey(1) == ord('q'):
                  break

          del self.p
          del self.device


  DepthAI().run()


.. include::  /pages/includes/footer-short.rst
