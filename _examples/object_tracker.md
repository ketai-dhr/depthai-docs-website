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
