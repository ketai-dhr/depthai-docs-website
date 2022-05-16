本地 OpenVINO 模型转换
===============================

在本教程中，您将学习如何将 OpenVINO 中间表示文件(IR)转换为在DepthAI上运行所需的格式，即使是在低功耗的Raspberry Pi上也能使用。 我将向您介绍OpenVINO工具集、Open Model Zoo(我们将在那里下载
`face-detection-retail-0004 <https://github.com/opencv/open_model_zoo/blob/2019_R3/models/intel/face-detection-retail-0004/description/face-detection-retail-0004.md>`__
模型), 并向您展示如何生成在OAK板上运行模型推理所需的.blob文件。

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=591665234&bvid=BV1fq4y1z792&cid=440909525&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"> </iframe>
    </div>
    <br/>

.. note::
  除了本地模型转换(更耗时)， 您还可以使用 :ref:`Google colab <使用 Google Colab>`, 我们的 :ref:`online converter <使用在线转换器>` 或 :ref:`blobconverter package <使用 blobconverter 库>`.

还没听说过 OpenVINO 或者 Open Model Zoo? 我先简单介绍一下为什么我们需要这些工具。

什么是OpenVINO?
#################

在底层，OAK使用英特尔技术来执行高速模型推断。 然而，要获得高性能，不只是把神经网络塞到芯片中这么简单。 这个时候，我们就需要
`OpenVINO <https://docs.openvinotoolkit.org/>`__ 了. OpenVINO是一个免费工具包，可将深度学习模型转换为可在Intel硬件上运行的格式。 转换模型后，通常可以看到每秒帧数（FPS）提高25倍或更多。 几个小步骤是否值得将FPS提高25倍？ 通常，答案是肯定的！

查看 `OpenVINO 工具包网站  <https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit.html>`__ 以获取安装说明。

什么是Open Model Zoo?
###########################

`Open Model Zoo <https://github.com/opencv/open_model_zoo>`__ 是一个免费的预训练模型库。
Zoo还包含用于将这些模型下载为可编译格式以便在DepthAI上运行的脚本。

DepthAI 能够运行 Zoo 中的许多对象检测模型。其中一些模型包含在 `DepthAI Github repositoy <https://github.com/luxonis/depthai/tree/master/resources/nn/>`__ 中。


安装 OpenVINO
################

.. note::
  DepthAI会在OpenVINO新版本发布后的几天内增加对OpenVINO新版本的支持， 所以 **建议您使用最新的OpenVINO版本** ，以下教程分2022.1 LTS和2021.4 LTS

2022.1 LTS
*************

安装OpenVINO Runtime
""""""""""""""""""""""

您可以从他们的 `下载页面 <https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/download.html>`__ 选择不同的平台下载OpenVINO Runtime，我们将使用最新版本-在撰写本文时为2022.1。

.. image:: /_static/images/tutorials/local_convert_openvino/OpenvinoRuntimeDownload.jpg
    :alt: OpenvinoRuntimeDownload

以下适用Linux APT 下载

.. code-block:: bash

  wget https://apt.repos.intel.com/intel-gpg-keys/GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
  sudo apt-key add GPG-PUB-KEY-INTEL-SW-PRODUCTS.PUB
  echo "deb https://apt.repos.intel.com/openvino/2022 focal main" | sudo tee /etc/apt/sources.list.d/intel-openvino-2022.list
  sudo apt update
  sudo apt install openvino

安装OpenVINO Development Tools
""""""""""""""""""""""""""""""""""""""""""""
您可以从他们的 `下载页面 <https://www.intel.com/content/www/us/en/developer/tools/openvino-toolkit/download.html>`__ 选择不同的平台下载OpenVINO Development Tools，我们将使用最新版本-在撰写本文时为2022.1。

.. image:: /_static/images/tutorials/local_convert_openvino/OpenvinoDevDownload.jpg
    :alt: OpenvinoDevDownload

以下适用Linux PIP 下载

.. code-block:: bash

  # 构建虚拟环境，如果不需要直接执行最后一步
  python3 -m venv openvino_env 
  source openvino_env/bin/activate
  python -m pip install --upgrade pip
  # 例子：TensorFlow 2.x and ONNX，可以参考下载页面下载自己需要的模块
  pip install openvino-dev[tensorflow2,onnx] 

apt安装，默认安装路径是 :code:`/opt/intel/openvino_2022` (默认位置), 下面2022.1版本我们将使用此路径。

2021.4 LTS
*************

您可以从他们的 `下载页面 <https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit/download.html>`__ 下载2021.4 版本的OpenVINO工具包安装程序。

.. image:: /_static/images/tutorials/local_convert_openvino/OpenvinoDownload.jpg
    :alt: download

下载并解压压缩文件夹后，我们可以运行安装:

.. code-block:: bash

  cd ~/Downloads/l_openvino_toolkit_p_2021.4.752
  $ sudo ./install_GUI.sh

我们需要的所有组件都将默认安装。我们的安装路径将是 :code:`~/intel/openvino_2021` (默认位置), 下面2021.4版本我们将使用此路径。

下载 face-detection-retail-0004 模型
#############################################

现在我们已经安装了 OpenVINO，我们可以使用模型下载器:

.. code-block:: bash

  # 2022.1 LTS
  cd ~
  omz_downloader -h # 查看帮助
  omz_downloader --print_all # 可以查看能够下载模型的名字
  omz_downloader --name face-detection-0004

  # 2021.4 LTS
  cd ~/intel/openvino_2021/deployment_tools/tools/model_downloader
  python3 -mpip install -r requirements.in
  python3 downloader.py --name face-detection-retail-0004 --output_dir ~/

这将会把模型文件下载到 :code:`~/intel/` 。 具体来说，我们所需要的模型文件位于:

.. code-block:: bash

  ~/intel/face-detection-retail-0004/FP16

我们将进入这个文件夹，以便我们稍后可以将此模型编译为所需的 **.blob** 。

你会在目录中看到两个文件:

.. code-block:: bash

  $ ls -lh
  total 1.3M
  -rw-r--r-- 1 root root 1.2M Jul 28 12:40 face-detection-retail-0004.bin
  -rw-r--r-- 1 root root 100K Jul 28 12:40 face-detection-retail-0004.xml

该模型采用 OpenVINO 中间表示(IR)格式：

- :code:`face-detection-retail-0004.xml` - 描述网络拓扑结构
- :code:`face-detection-retail-0004.bin` - 包含权重和偏差二进制数据。

这意味着我们已准备好为 MyriadX 编译模型！

编译模型
#################

.. note::
  在OpenVINO 2022.1 LTS发布后， **您应该在编译模型时连接OAK** ，2022.1 LTS APT默认安装目录为 **/opt/intel**


我们的 DepthAI 板上使用的 MyriadX 芯片不直接使用 IR 格式文件。 相反，我们需要 :code:`face-detection-retail-0004.blob` 使用 :code:`compile_tool` 工具。

激活 OpenVINO 环境
*****************************

为了使用 :code:`compile_tool` 工具, 我们需要激活我们的OpenVINO环境。

首先，让我们找到 :code:`setupvars.sh` 文件。

.. code-block:: bash
  
  # 2022.1 LTS
  find /opt/intel/ -name "setupvars.sh"
  /opt/intel/openvino_2022.1.0.643/setupvars.sh

  # 2021.4 LTS
  find ~/intel/ -name "setupvars.sh"
  /home/root/intel/openvino_2021.4.752/data_processing/dl_streamer/bin/setupvars.sh
  /home/root/intel/openvino_2021.4.752/opencv/setupvars.sh
  /home/root/intel/openvino_2021.4.752/bin/setupvars.sh

我们对 :code:`bin/setupvars.sh` 文件感兴趣, 所以让我们继续用它来激活环境:

.. code-block:: bash

  # 2022.1 LTS
  source /opt/intel/openvino_2022.1.0.643/setupvars.sh
  [setupvars.sh] OpenVINO environment initialized

  # 2021.4 LTS
  source /home/root/intel/openvino_2021.4.752/bin/setupvars.sh
  [setupvars.sh] OpenVINO environment initialized

如果你看到 :code:`[setupvars.sh] OpenVINO environment initialized` 那么你的环境应该是正确初始化的。

找到 compile_tool
*******************

让我们找出 :code:`compile_tool` 位置。在您的终端中,运行:

.. code-block:: bash

  # 2022.1 LTS
  find /opt/intel/ -iname compile_tool
  /opt/intel/openvino_2022.1.0.643/tools/compile_tool/compile_tool

  # 2021.4 LTS
  find ~/intel/ -iname compile_tool
  /home/root/intel/openvino_2021.4.752/deployment_tools/tools/compile_tool/compile_tool

保存此路径，因为您在下一步运行 :code:`compile_tool` 时需要它。

运行 compile_tool
*********************

从 :code:`~/intel/face-detection-retail-0004/FP16` 我们现在调用 :code:`compile_tool` 将模型编译到 **face-detection-retail-0004.blob**

.. code-block:: bash

  # 2022.1 LTS
  /opt/intel/openvino_2022.1.0.643/tools/compile_tool/compile_tool -m face-detection-retail-0004.xml -ip U8 -d MYRIAD -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4

  # 2021.4 LTS
  ~/intel/openvino_2021.4.752/deployment_tools/tools/compile_tool/compile_tool -m face-detection-retail-0004.xml -ip U8 -d MYRIAD -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4

你应该看到：

.. code-block:: bash

  Inference Engine:
    IE version ......... 2021.4.0
    Build ........... 2021.4.0-3839-cd81789d294-releases/2021/4

  Network inputs:
      data : U8 / NCHW
  Network outputs:
      detection_out : FP16 / NCHW
  [Warning][VPU][Config] Deprecated option was used : VPU_MYRIAD_PLATFORM
  Done. LoadNetwork time elapsed: 1760 ms

blob 文件在哪里？它位于当前文件夹中

.. code-block:: bash

  /intel/face-detection-retail-0004/FP16$ ls -l
  total 2.6M
  -rw-rw-r-- 1 root root 1176544 jul 28 19:32 face-detection-retail-0004.bin
  -rw-rw-r-- 1 root root 1344256 jul 28 19:51 face-detection-retail-0004.blob
  -rw-rw-r-- 1 root root  106171 jul 28 19:32 face-detection-retail-0004.xml

运行并显示模型输出
################################

有了神经网络 :code:`.blob` 文件, 我们就可以开始了！
为了验证模型是否正确运行，让我们稍微修改一下我们在 :ref:`Hello World` 中创建的程序。

具体的说，让我们修改 :code:`setBlobPath` 调用来加载我们的模型 **记得将路径替换为你所拥有的正确路径！**

.. code-block:: diff

  - detection_nn.setBlobPath(str(blobconverter.from_zoo(name='mobilenet-ssd', shaves=6)))
  + detection_nn.setBlobPath("/path/to/face-detection-retail-0004.blob")

以上就是全部了。

你应该能看到一个带注释的输出，跟下面这个类似:

.. image:: /_static/images/tutorials/local_convert_openvino/face.png
  :alt: face

审查流程
##################

我们走过的流程适用于 Open Model Zoo 的其他预训练对象检测模型:

#. 下载模型:

    .. code-block:: bash

      python3 downloader.py --name face-detection-retail-0004 --output_dir ~/

#. 创建 MyriadX blob 文件:

    .. code-block:: bash

      ./compile_tool -m [INSERT PATH TO MODEL XML FILE] -ip U8 -d MYRIAD -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4
    
    `这里 <https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#run_the_compile_tool>`__ **可以看到compile_tool支持的所有参数**.

#. 在您的脚本中使用此模型

你已经上手了! 你可以在 GitHub 上找到这个教程的 `完整代码 <https://github.com/luxonis/depthai-tutorials/blob/master/2-face-detection-retail/face-detection-retail-0004.py>`__

.. note::
    您还应该查看 :ref:`使用预训练的 OpenVINO 模型` 教程，其中此过程得到了显着简化。

.. include::  /pages/includes/footer-short.rst


