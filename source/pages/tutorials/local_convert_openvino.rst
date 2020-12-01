教程-本地OpenVINO模型转换
===============================

在本教程中，您将学习如何将OpenVINO 中间表示文件（IR）转换为在DepthAI上运行所需的格式，即使是在低功耗的Raspberry Pi上也能使用。 我将向您介绍OpenVINO工具集、Open Model Zoo(我们将在那里下载
`face-detection-retail-0004 <https://github.com/opencv/open_model_zoo/blob/2019_R3/models/intel/face-detection-retail-0004/description/face-detection-retail-0004.md>`__
模型), 并向您展示如何生成在DepthAI上运行模型推理所需的文件。

.. image:: /_static/images/tutorials/local_convert_openvino/face.png
  :alt: face

还没听说过OpenVINO或者Open Model Zoo? 我先简单介绍一下为什么我们需要这些工具。

什么是OpenVINO?
#################

在后台，DepthAI使用英特尔技术来执行高速模型推断。 然而，要获得高性能，不只是把神经网络塞到芯片中这么简单。 这个时候，我们就需要
`OpenVINO <https://docs.openvinotoolkit.org/>`__ 了. OpenVINO是一个免费工具包，可将深度学习模型转换为可在Intel硬件上运行的格式。 转换模型后，通常可以看到每秒帧数（FPS）提高25倍或更多。 几个小步骤是否值得将FPS提高25倍？ 通常，答案是肯定的！

什么是Open Model Zoo?
###########################

`Open Model Zoo <https://github.com/opencv/open_model_zoo>`__ 是一个免费的预训练模型库。
Zoo还包含用于将这些模型下载为可编译格式以便在DepthAI上运行的脚本。

DepthAI能够运行Zoo中的许多对象检测模型。

安装 OpenVINO
################

.. warning::

  如果您已经安装过了OpenVINO或者想按照
  `官方教程 <https://software.intel.com/content/www/us/en/develop/tools/openvino-toolkit.html>`__ 操作, **请跳过这个步骤**。

  请注意，以下安装说明适用于 **Ubuntu 18.04** 操作系统, 如果您打算使用其他操作系统，请按照官方的OpenVINO安装说明进行安装。


DepthAI需要OpenVINO :code:`2020.1` 版本来运行。  让我们为操作系统获取一个软件包，并使用以下命令满足该版本：

.. code-block:: bash

  apt-get update
  apt-get install -y software-properties-common
  add-apt-repository -y ppa:deadsnakes/ppa
  apt-get update
  apt-get install -y wget pciutils python3.8 libpng-dev libcairo2-dev libpango1.0-dev libglib2.0-dev libgtk2.0-dev libswscale-dev libavcodec-dev libavformat-dev
  cd
  mkdir openvino_install && cd openvino_install
  wget http://registrationcenter-download.intel.com/akdlm/irc_nas/16345/l_openvino_toolkit_p_2020.1.023.tgz
  tar --strip-components=1 -zxvf l_openvino_toolkit_p_2020.1.023.tgz
  ./install_openvino_dependencies.sh
  ./install.sh # 完成后，您可以继续执行 "rm -r ~/openvino_install"

现在，我们要使用的第一个屏幕是EULA， 按 :code:`Enter`, 滚动后输入 :code:`accept` 即可。

下一个是同意英特尔软件改进计划，这没有关系，所以你可以选择是否同意 (:code:`1`) 或不同意 (:code:`2`) 。

接下来，您可能会看到“缺少必要文件”的画面，显示 :code:`Intel® Graphics Compute Runtime for OpenCL™ Driver is missing` - 你可以继续并忽略这个警告。

最后，我们会看到安装总结–请确认它指出的位置是否正确 - :code:`/opt/intel`。
如果一切看起来都很好，继续进行 (:code:`1`). 如果再次出现缺少必要文件的提示，请随意跳过。

让我们验证一下你的主机上是否安装了正确的版本。通过在终端会话中运行以下内容来检查你的版本:

.. code-block:: bash

  cat /opt/intel/openvino/inference_engine/version.txt

你可能看到类似如下的输出:

.. code-block::

  Thu Jan 23 19:14:14 MSK 2020
  d349c3ba4a2508be72f413fa4dee92cc0e4bc0e1
  releases_2020_1_InferenceEngine_37988

确认您在输出中看到 :code:`releases_2020_1` 。 如果你看到了，继续前进。 如果您使用的是不同版本，
请转到 `OpenVINO 网站 <https://docs.openvinotoolkit.org/2019_R3/index.html>`__ 并为您的操作系统下载2020.1版本：

.. image:: /_static/images/tutorials/local_convert_openvino/openvino_version.png
  :alt: face

检查是否安装了模型下载器
##########################################

在安装OpenVINO时，您可以选择执行一个较小的安装以节省磁盘空间。这个自定义安装可能不包括模型下载器脚本。让我们检查下载器是否已经安装。在终端会话中，键入以下内容:

.. code-block:: bash

  find /opt/intel/ -iname downloader.py

**如果看到了如下的输出，请继续**:

.. code-block:: bash

  /opt/intel/openvino_2020.1.023/deployment_tools/open_model_zoo/tools/downloader/downloader.py

**没看到任何输出？** 如果没有找到 :code:`downloader.py` 也不要着急。 我们将在下面安装这个。

安装 Open Model Zoo 下载器
*********************************

如果没有找到下载工具， 我们将通过克隆
`Open Model Zoo Repo <https://github.com/openvinotoolkit/open_model_zoo/blob/2020.1/tools/downloader/README.md>`__ 来安装工具，并安装工具依赖关系。

启动一个终端会话，并在终端中运行以下命令:

.. code-block:: bash

  apt-get install -y git curl
  curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
  python3 get-pip.py
  rm get-pip.py
  cd ~
  git clone https://github.com/opencv/open_model_zoo.git
  cd open_model_zoo
  git checkout tags/2020.1
  cd tools/downloader
  python3 -m pip install --user -r ./requirements.in

这部分操作将把代码库克隆到 :code:`~/open_model_zoo` 目录下，拷贝所需的 :code:`2020.1` 版本，并安装下载程序的dependencies.zh。

创建一个 OPEN_MODEL_DOWNLOADER 环境变量
####################################################

输入 :code:`downloader.py` 的完整路径可能会使用大量的按键。为了延长你的键盘寿命，让我们把这个脚本的路径存储在一个环境变量中。

在你的终端中运行以下内容:

.. code-block:: bash

  export OPEN_MODEL_DOWNLOADER='INSERT PATH TO YOUR downloader.py SCRIPT'

其中 :code:`INSERT PATH TO YOUR downloader.py SCRIPT` 可以通过以下方式找到:

.. code-block:: bash

  find /opt/intel/ -iname downloader.py
  find ~ -iname downloader.py

例如，如果你自己安装了 :code:`open_model_zoo` :

.. code-block:: bash

  export OPEN_MODEL_DOWNLOADER="$HOME/open_model_zoo/tools/downloader/downloader.py"

下载 face-detection-retail-0004 模型
#############################################

我们已经安装了从Open Model Zoo下载模型所需的所有东西！现在我们将使用
`模型下载器 <https://github.com/opencv/open_model_zoo/blob/2019_R3/tools/downloader/README.md>`__ 来下载
:code:`face-detection-retail-0004` 模型文件. 在您的终端运行以下内容:

.. code-block:: bash

  $OPEN_MODEL_DOWNLOADER --name face-detection-retail-0004 --output_dir ~/open_model_zoo_downloads/

这将会把模型文件下载到 :code:`~/open_model_zoo_downloads/` 。 具体来说，我们所需要的模型文件位于:

.. code-block:: bash

  ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16

你会在目录中看到两个文件:

.. code-block:: bash

  $ ls -lh
  total 1.3M
  -rw-r--r-- 1 root root 1.2M Jul 28 12:40 face-detection-retail-0004.bin
  -rw-r--r-- 1 root root 100K Jul 28 12:40 face-detection-retail-0004.xml

该模型采用OpenVINO中间表示（IR）格式：

- :code:`face-detection-retail-0004.xml` - Describes the network topology
- :code:`face-detection-retail-0004.bin` - Contains the weights and biases binary data.

这意味着我们已准备好为MyriadX编译模型！

编译模型
#################

我们的DepthAI板上使用的MyriadX芯片不直接使用IR格式文件。 相反，我们需要生成两个文件:

* :code:`face-detection-retail-0004.blob` - 我们将用 :code:`myriad_compile` 命令创建这个文件。
* :code:`face-detection-retail-0004.json` - JSON格式的 :code:`blob_file_config` 文件。 它描述了输出 tensors 的格式。 
你可以在 :ref:`这里 <Creating Blob configuration file>` 阅读更多关于这个文件结构和例子。

我们将从创建 :code:`blob` 文件开始。

定位 myriad_compile
*********************

让我们来寻找 :code:`myriad_compile` 的位置。在你的终端，运行:

.. code-block:: bash

  find /opt/intel/ -iname myriad_compile

您应该看到类似于以下的输出：

.. code-block:: bash

  find /opt/intel/ -iname myriad_compile
  /opt/intel/openvino_2020.1.023/deployment_tools/inference_engine/lib/intel64/myriad_compile

由于路径很长， 让我们把 :code:`myriad_compile` 可执行文件存储在一个环境变量中 (就像
:code:`OPEN_MODEL_DOWNLOADER`)一样:

.. code-block:: bash

  export MYRIAD_COMPILE=$(find /opt/intel/ -iname myriad_compile)

激活 OpenVINO 环境
*****************************

为了使用 :code:`myriad_compile` 工具, 我们需要激活我们的OpenVINO环境。

首先，让我们找到 :code:`setupvars.sh` 文件。

.. code-block:: bash

  find /opt/intel/ -name "setupvars.sh"
  /opt/intel/openvino_2020.1.023/opencv/setupvars.sh
  /opt/intel/openvino_2020.1.023/bin/setupvars.sh

我们对 :code:`bin/setupvars.sh` 文件感兴趣, 所以让我们继续用它来激活环境:

.. code-block:: bash

  source /opt/intel/openvino_2020.1.023/bin/setupvars.sh
  [setupvars.sh] OpenVINO environment initialized

如果你看到 :code:`[setupvars.sh] OpenVINO environment initialized` then your environment should be initialized correctly

运行 myriad_compile
******************

.. code-block:: bash

  $MYRIAD_COMPILE -m ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.xml -ip U8 -VPU_MYRIAD_PLATFORM VPU_MYRIAD_2480 -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4

你将看到:

.. code-block:: bash

  Inference Engine:
    API version ............ 2.1
    Build .................. 37988
    Description ....... API
  Done

Blob文件在哪里？它和 :code:`face-detection-retail-0004.xml`  在同一文件夹里:

.. code-block:: bash

  ls -lh ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/
  total 2.6M
  -rw-r--r-- 1 root root 1.2M Jul 28 12:40 face-detection-retail-0004.bin
  -rw-r--r-- 1 root root 1.3M Jul 28 12:50 face-detection-retail-0004.blob
  -rw-r--r-- 1 root root 100K Jul 28 12:40 face-detection-retail-0004.xml

创建blob配置文件
###########################

MyriadX需要一个 :code:`blob` 文件 (我们刚刚创建了一个) 和一个JSON格式的 `blob_file_config` 。
我们将手动创建此配置文件。 在您的终端中请输入:

.. code-block:: bash

  cd ~/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/
  touch face-detection-retail-0004.json

把下面的代码复制粘贴到 :code:`face-detection-retail-0004.json` :

.. code-block:: json

  {
      "NN_config": {
          "output_format" : "detection",
          "NN_family" : "mobilenet",
          "confidence_threshold" : 0.5
      }
  }

这些值是什么意思？

- :code:`output_format` - 是 :code:`detection` 还是 :code:`raw`. Detection 告诉 DepthAI 对 NN 进行解析并根据它们创建一个检测对象， 您可以通过运行 :code:`getDetectedObjects` 方法来访问该对象。 Raw 表示 "不解析, 我将在主机上处理解析", 要求你解析原始张量。
- :code:`NN_family` - 仅在检测到 :code:`output_format` 是 :code:`detection` 时才需要。 目前支持两个NN系列 :code:`mobilenet` 和 :code:`YOLO`
- :code:`confidence_threshold` - DepthAI将仅返回置信度高于阈值的结果。

运行并显示模型输出
################################

有了 :code:`blob` 和 a :code:`json` blob配置文件, 我们就可以开始了！
为了验证模型是否正确运行，让我们稍微修改一下我们在 :ref:`Hello World` 中创建的程序。

具体的说，让我们修改 :code:`create_pipeline` 调用来加载我们的模型 **记得将路径替换为你所拥有的正确路径！**

.. code-block:: diff

  pipeline = device.create_pipeline(config={
      'streams': ['previewout', 'metaout'],
      'ai': {
  -        'blob_file': "/path/to/mobilenet-ssd.blob",
  +        'blob_file': "/path/to/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.blob",
  -        'blob_file_config': "/path/to/mobilenet-ssd.json"
  +        'blob_file_config': "/path/to/open_model_zoo_downloads/intel/face-detection-retail-0004/FP16/face-detection-retail-0004.json"
      }
  })

以上就是全部了。

你应该能看到一个带注释的输出，跟下面这个类似:

.. image:: /_static/images/tutorials/local_convert_openvino/face.png
  :alt: face

审查流程
##################

我们走过的流程适用于Open Model Zoo的其他预训练对象检测模型:

#. 下载模型:

    .. code-block:: bash

      $OPEN_MODEL_DOWNLOADER --name [INSERT MODEL NAME] --output_dir ~/open_model_zoo_downloads/

#. 创建 MyriadX blob 文件:

    .. code-block:: bash

      $MYRIAD_COMPILE -m [INSERT PATH TO MODEL XML FILE] -ip U8 -VPU_MYRIAD_PLATFORM VPU_MYRIAD_2480 -VPU_NUMBER_OF_SHAVES 4 -VPU_NUMBER_OF_CMX_SLICES 4

#. 根据模型的输出 :ref:`创建blob配置文件` 。
#. 在你的脚本中使用这个模型。

你已经上手了! 你可以在Github上找到这个教程的 `完整代码 <https://github.com/luxonis/depthai-tutorials/blob/master/2-face-detection-retail/face-detection-retail-0004.py>`__

.. include::  /pages/includes/footer-short.rst


