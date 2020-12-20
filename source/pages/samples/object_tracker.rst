物体追踪
==============

演示
####

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//www.youtube.com/embed/70rA7T_vJV0" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"></iframe>
    </div>

源代码
###########

.. code-block:: python

  import cv2
  import depthai

  device = depthai.Device('', False)

  p = device.create_pipeline(config={
      "streams": ["previewout", "object_tracker"],
      "ai": {
          #blob compiled for maximum 12 shaves
          "blob_file": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.blob.sh12cmx12NCE1",
          "blob_file_config": "/path/to/depthai/resources/nn/mobilenet-ssd/mobilenet-ssd.json",
          "shaves" : 12,
          "cmx_slices" : 12,
          "NN_engines" : 1,
      },
      'ot': {
          'max_tracklets': 20,
          'confidence_threshold': 0.5,
      },
  })

  if p is None:
      raise RuntimeError("Error initializing pipelne")

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
  del device

解释
###########

.. warning::

  **刚开始使用 DepthAI ?**

  DepthAI 的基础知识在 :ref:`示例-访问 DepthAI 相机所需的最少代码` 和 :ref:`Hello World` 中有解释。


DepthAI 能够从设备本身进行物体追踪，所以你不必为此编写自己的自定义代码。

首先，我们改变从管道中检索数据包的方法，因为我们没有使用 :code:`metaout` 流

.. code-block:: python

  for packet in p.get_available_data_packets():

接下来，如果数据包来自流 :code:`object_tracker` ，我们使用一个特殊的方法，只有在来自这个流的数据包中才能获得 tracklets 对象 - 如果在其他流上使用这个方法，会抛出一个错误

.. code-block:: python

  if packet.stream_name == 'object_tracker':
      tracklets = packet.getObjectTracker()

接下来，我们要获取所有 tracklet 的信息，以便进一步处理。

.. code-block:: python

  tracklet = tracklets.getTracklet(i)
  left = tracklet.getLeftCoord()
  top = tracklet.getTopCoord()
  right = tracklet.getRightCoord()
  bottom = tracklet.getBottomCoord()
  tracklet_label = labels[tracklet.getLabel()]

而其余的处理只是为了直观地表现跟踪对象，如果你想以不同的方式使用物体追踪器，可以跳过这部分。

.. include::  /pages/includes/footer-short.rst
