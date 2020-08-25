<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
  使用树莓派计算机模组版本([BW1097](https://docs.luxonis.com/products/bw1097/)) 或者 USB3板载相机版本（OAK-D）([BW1098OBC](https://docs.luxonis.com/products/bw1098obc/))? <strong>你的产品已经预校正过了。</strong><br/>
</div>

OAK-D上的双目相机在出厂之间就是校正好的 - 但您可能希望在安装过程中重新校准以获得更好的质量，也有可能在你多次使用处理后校准质量变差需要重新校准。

下面是一个演示如何校正的简单视频，用到了我们的树莓派计算模组版本。

在观看下面的视频之后，您将可以了解到校准您自己的DepthAI所需的步骤。关于校准选项的更多信息/细节，请参见下面的步骤，以及`./calibrate.py --help`，它将打印出所有的校准选项。

[![DepthAI Calibration Example](https://i.imgur.com/oJm0s8o.jpg)](https://www.youtube.com/watch?v=lF01f0p1oZM "DepthAI Calibration")

<h3 class="step" data-toc-title="安装 Python API" id="calibrate_install_api"><span></span> 下载[depthai](https://github.com/luxonis/depthai)代码库.</h3>

<div class="alert alert-primary" role="alert">
<i class="material-icons">
error
</i>
  已经安装了`depthai`? <strong>请跳过该步骤</strong><br/>
</div>

```
git clone https://github.com/luxonis/depthai.git
cd depthai
```

<h3 class="step" data-toc-title="打印棋盘格" id="print_chessboard"><span></span> 打印棋盘校准图像。</h3>

要么将校准棋盘格打印到一个平面上，要么将棋盘格显示在一个平面（非曲面的）显示器上。 请注意，如果您打印校准目标，请注意确保它附着在一个平面上，并且是平的，没有皱纹或者起伏。

通常情况下，使用显示器来显示校准目标更快更容易。

[![Print this chessboard calibration image](https://raw.githubusercontent.com/luxonis/depthai/master/resources/patternnew.png)](https://raw.githubusercontent.com/luxonis/depthai/master/resources/patternnew.png)

请通过合适的缩放来把这个棋盘格铺满整张纸。 而如果在显示器上显示，则以白色背景全屏显示图像。

<h3 class="step" data-toc-title="执行校正脚本" id="start_calibration_script"><span></span> 执行校正脚本</h3>

用实际值替代方括号内的参数:

```
python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd [BOARD]
```

参数参考:

* `-s SQUARE_SIZE_IN_CM`, `--square_size_cm SQUARE_SIZE_IN_CM`: 测量印刷棋盘格的正方形尺寸，单位为厘米。
* `-brd BOARD`, `--board BOARD`: BW1097, BW1098OBC - 来自资源/板子/的板子类型（不区分大小写）。或自定义.json板型配置的路径。与[-fv -b -w]互斥，允许手动指定视场、基线和相机方向（交换或不交换）。

通过用尺子或卡尺测量校正目标上方格大小的数值，并输入该数字（以厘米为单位）来代替[SQUARE_SIZE_IN_CM]。  

如果方格为2.35厘米宽，1098OBC的参数如下。
```
python3 calibrate.py -s 2.35 -brd bw1098obc
```
并且注意，在校准时，镜像显示往往是有用的（这样运动方向就不会显得相反）。 当看到自己的时候，我们习惯看到相反的运动方向（因为我们在镜子里看到的就是这个样子），所以这样做，使用`-ih`选项，如下图。
```
python3 calibrate.py -s 2.35 -brd bw1098obc -ih
```

因此，当我们在内部运行校准时，我们几乎总是使用`-ih`选项，所以我们将在以下所有的示例命令中包含这个选项。

#### BW1098OBC (USB3板载相机版本/OAK-D)):
```
python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd bw1098obc -ih
```

{: #modular_cameras }
#### BW1098FFC（USB3模块化摄像机版）或BW1094（树莓派HAT）:
使用[这里](https://github.com/luxonis/depthai/tree/master/resources/boards)的一个板子`*.json`文件来定义双目相机之间的基线，以及左边摄像机和彩色摄像机之间的基线，替换下面括号内的项目。

* 左右互换（即摄像机朝向哪个方向，设置为`true`或`false`）。
* 左/右灰度相机之间的 ``BASELINE`，单位为厘米。
* `左`灰度相机和彩色相机之间的距离 `RGBLEFT`，以厘米为单位。

```
{
    "board_config":
    {
        "name": "ACME01",
        "revision": "V1.2",
        "swap_left_and_right_cameras": [true | false],
        "left_fov_deg": 71.86,
        "rgb_fov_deg": 68.7938,
        "left_to_right_distance_cm": [BASELINE],
        "left_to_rgb_distance_cm": [RGBLEFT]
    }
}
```
所以，比如说如果你把你的OAK-D的立体基线设置为20cm，彩色摄像机正好在两台灰度摄像机之间，而且摄像机的方向和BW1097一样，使用以下JSON。

```
{
    "board_config":
    {
        "name": "ACME01",
        "revision": "V1.2",
        "swap_left_and_right_cameras": true,
        "left_fov_deg": 71.86,
        "rgb_fov_deg": 68.7938,
        "left_to_right_distance_cm": 20,
        "left_to_rgb_distance_cm": 10
    }
}
```
Then, run calibration with this board name:
```
python3 calibrate.py -s [SQUARE_SIZE_IN_CM] -brd ACME01 -ih
```

Run `python3 calibrate.py -h` (or `-h`) for a full list of arguments and usage examples.

<h3 class="step" data-toc-title="捕捉图像" id="capture_images"><span></span> 定位棋盘格并捕捉图像</h3>

显示左右两个相机的视频流，每个视频流上都会叠加一个多边形。

举起打印好的棋盘格（或屏幕上显示图像的笔记本电脑），使整个棋盘显示在两个视频流中。

将棋盘叠加的多边形的方向匹配起来，按[空格键]捕捉图像。棋盘图案不需要与多边形完全匹配，更重要的是用多边形来指导棋盘和相机的相对角度和位置。有13个必要的多边形位置。

捕获所有多边形位置的图像后，将开始校准图像处理步骤。如果成功，将在`depthai/resources/depthai.calib`处创建一个校准文件。该文件默认通过`consts/resource_paths.py`中的`calib_fpath`变量加载。

<h3 class="step" id="test_depth"><span></span> 测试深度</h3>

我们将查看深度流，以确保相机的校准正确。

1. 启动一个终端会话。
2. 访问您本地的`depthai`副本。
    ```
    cd [depthai repo]
    ```
3. 运行 `python3 test.py -s depth_raw -o`。<br/>
    该脚本启动一个窗口，启动相机，并显示深度视频流：

    ![object localization demo](/images/depth.png)

    在上面的截屏中，手离相机更近.
    
<h3 class="step" id="write_to_eeprom"><span></span> 将校准和板卡参数写入板载eeprom</h3>

如果您对上述深度质量感到满意，您可以将其写入DephtAI的板载eeprom中，以便校准与DepthAI保持一致（所有支持立体深度的设计都有用于此目的的板载eeprom）。

要将校准和相关板卡信息写入DepthAI的EEPROM中，请使用以下命令：

```
python3 test.py -brd [BOARD] -e
```
其中`[BOARD]`是`BW1097`(Raspberry Pi计算模块版)、`BW1098OBC`(OAK-D)或自定义板卡文件(如[这里](#modular_cameras))，都不区分大小写。

例如，要将（更新的）校准和板卡信息写入BW1098OBC，请使用以下命令。
```
python3 test.py -brd bw1098obc -e
```

而为了验证你的DepthAI上写到EEPROM的内容，用以下的命令，你可以看到每当运行DetphAI时检查输出。
```
python3 test.py
```
并在运行上述命令后，在终端的打印输出中查找`EEPROM数据`:
```
EEPROM data: valid (v2)
  Board name     : BW1098OBC
  Board rev      : R0M0E0
  HFOV L/R       : 71.86 deg
  HFOV RGB       : 68.7938 deg
  L-R   distance : 7.5 cm
  L-RGB distance : 3.75 cm
  L/R swapped    : yes
  L/R crop region: top
  Calibration homography:
    1.002324,   -0.004016,   -0.552212,
    0.001249,    0.993829,   -1.710247,
    0.000008,   -0.000010,    1.000000,
```

如果有任何看起来不正确的地方，您可以再次校准或者更改电路板信息，并使用上述`-brd`和`-e`标志覆盖存储的eeprom信息和校准数据。
