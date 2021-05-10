Python API安装详解
=================================

请参考下表，为您的平台安装必要的 :ref:`依赖项 <支持的平台>`。安装后，您可以 :ref:`安装DepthAI库 <从 PyPi 安装>`。

我们一直在努力改善发行软件的方式，以与无数平台和多种打包方式保持同步。如果您没有看到下面列出的特定平台或软件包格式，请通过 `Gitee <https://gitee.com/oakchina/depthai>`_ 与我们联系。

支持的平台
#################################

我们为以下平台保留最新的，预编译的库。请注意，新的变化是，对于Jetson / Xavier系列，Ubuntu现在也可以保持不变：

======================== ============================================== ================================================================================
平台                      指示                                            技术支持
======================== ============================================== ================================================================================
Windows 10               :ref:`Platform dependencies <Windows>`         `Discord <https://discord.com/channels/790680891252932659/798284448323731456>`__
macOS                    :ref:`Platform dependencies <macOS>`           `Discord <https://discord.com/channels/790680891252932659/798283911989690368>`__
Ubuntu & Jetson/Xavier   :ref:`Platform dependencies <Ubuntu>`          `Discord <https://discord.com/channels/790680891252932659/798302162160451594>`__
Raspberry Pi OS          :ref:`Platform dependencies <Raspberry Pi OS>` `Discord <https://discord.com/channels/790680891252932659/798302708070350859>`__
======================== ============================================== ================================================================================

社区和Luxonis的组合也支持以下平台。

====================== ===================================================== ================================================================================
平台                    指示                                                   技术支持
====================== ===================================================== ================================================================================
Fedora                                                                       `Discord <https://discord.com/channels/790680891252932659/798592589905264650>`__
Robot Operating System                                                       `Discord <https://discord.com/channels/790680891252932659/795749142793420861>`__
Windows 7              :ref:`WinUSB driver <Windows 7>`                      `Discord <https://discord.com/channels/790680891252932659/798284448323731456>`__
Docker                 :ref:`Pull and run official images <Docker>`          `Discord <https://discord.com/channels/790680891252932659/796794747275837520>`__
Kernel Virtual Machine :ref:`Run on KVM <KVM>`                               `Discord <https://discord.com/channels/790680891252932659/819663531003346994>`__
VMware                 :ref:`Run on VMware <vmware>`                         `Discord <https://discord.com/channels/790680891252932659/819663531003346994>`__
====================== ===================================================== ================================================================================

安装系统依赖
##############################

运行 DepthAI 库需要几个基本的系统依赖。
它们中的大多数应该已经安装在大多数系统中，但是如果没有安装，
我们准备了一个 :download:`安装脚本 </_static/install_dependencies.sh>`
，这将确保安装了所有依赖项以及方便的开发/编程工具。还有适用于 macOS ( `此处 <https://www.bilibili.com/video/BV1Vy4y1m7qG?from=search&seid=17057089443751489307>`__ ),Raspberry Pi ( `此处 <https://youtu.be/BpUMT-xqwqE>`__ ), Ubuntu( `此处 <https://www.bilibili.com/video/BV1TT4y1u7Fv?from=search&seid=17057089443751489307>`__ )和 Windows 10( `此处 <https://www.bilibili.com/video/BV1uA411s7Ly?from=search&seid=17057089443751489307>`__ )的视频指南。

macOS
******************************

.. code-block:: bash

  bash -c "$(curl -fL https://cdn.jsdelivr.net/gh/OAKChina/depthai-docs-website@develop/source/_static/install_dependencies.sh)"

执行此命令后，关闭并重新打开终端窗口。

该脚本还可以在 M1 Mac 上运行，并且在 Rosetta 2 下安装了 Homebrew，因为某些 Python 软件包仍缺少对M1的原生支持。如果您已经在本地安装了 Homebrew 并且无法正常工作，请参见 `此处 <https://github.com/luxonis/depthai/issues/299#issuecomment-757110966>`__ 以了解其他一些疑难解答步骤。

请注意，如果未出现视频流窗口，请考虑运行以下命令：

.. code-block:: bash

  python3 -m pip install opencv-python --force-reinstall --no-cache-dir -i https://pypi.tuna.tsinghua.edu.cn/simple

有关更多信息，请参见我们论坛上的 `macOS 视频预览窗口未能出现 <https://discuss.luxonis.com/d/95-video-preview-window-fails-to-appear-on-macos>`__ 话题讨论。

Raspberry Pi OS
*********************************

.. code-block:: bash

  sudo curl -fL https://cdn.jsdelivr.net/gh/OAKChina/depthai-docs-website@develop/source/_static/install_dependencies.sh | bash

Ubuntu
*********************************

.. code-block:: bash

  sudo wget -qO- https://cdn.jsdelivr.net/gh/OAKChina/depthai-docs-website@develop/source/_static/install_dependencies.sh | bash

openSUSE
********************************

对于 openSUSE,在这篇 `官方文章 <https://en.opensuse.org/SDB:Install_OAK_AI_Kit.>`__ 中提供了如何在 openSUSE 平台上安装 OAK 设备。

Windows
********************************

- 右键单击开始按钮
- 选择 Windows PowerShell (管理员)
- 安装 Chocolatey 软件包管理器(类似于 macOS 的 Homebrew)：

.. code-block:: bash

  Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

- 关闭 PowerShell ，然后通过重复前两个步骤重新打开另一个 PowerShell (管理员)。
- 安装 Python 和 PyCharm

.. code-block:: bash

  choco install cmake git python pycharm-community -y

Windows 7
**************************

尽管我们不正式支持Windows 7, 但是我们的社区成员 `已经成功 <https://discuss.luxonis.com/d/105-run-on-win7-sp1-x64-manual-instal-usb-driver>`__ 使用 `Zadig
<https://zadig.akeo.ie/>`__ 手动安装WinUSB . 连接DepthAI设备后，寻找具有 :code:`USB ID:03E7 2485` 的设备并选择WinUSB（v6.1.7600.16385）安装WinUSB驱动程序，然后安装WCID驱动程序。

Docker
**********************

我们 在Docker Hub上的 `luxonis/depthai-library <https://hub.docker.com/r/luxonis/depthai-library>`_ 存储库中维护了一个包含DepthAI及其依赖关系和有用工具的Docker映像。它建立在基于
`luxonis/depthai <https://hub.docker.com/r/luxonis/depthai-base>`_ 的图像上。

:code:`01_rgb_preview.py` 在Linux主机(带有X11窗口系统)的Docker容器内运行示例：

.. code-block:: bash

   docker pull luxonis/depthai-library
   docker run --rm \
       --privileged \
       -v /dev/bus/usb:/dev/bus/usb \
       --device-cgroup-rule='c 189:* rmw' \
       -e DISPLAY=$DISPLAY \
       -v /tmp/.X11-unix:/tmp/.X11-unix \
       luxonis/depthai-library:latest \
       python3 /depthai-python/examples/01_rgb_preview.py

要允许容器更新X11，您可能需要在主机上运行。:code:`xhost local:root`

KVM
******************

要访问 `内核虚拟机 <https://www.linux-kvm.org/page/Main_Page>`_ 中的OAK-D摄像机，需要在主机检测到USB总线中的更改时即时连接和分离USB设备。

当DepthAI API使用OAK-D相机时，它会更改USB设备的类型。当本机使用相机时，这种情况会在背景中发生。但是，当在虚拟环境中使用相机时，情况就不同了。

在您的主机上，使用以下代码：

.. code-block:: bash

  SUBSYSTEM=="usb", ACTION=="bind", ENV{ID_VENDOR_ID}=="03e7", MODE="0666", RUN+="/usr/local/bin/movidius_usb_hotplug.sh depthai-vm"
  SUBSYSTEM=="usb", ACTION=="remove", ENV{PRODUCT}=="3e7/2485/1", ENV{DEVTYPE}=="usb_device", MODE="0666", RUN+="/usr/local/bin/movidius_usb_hotplug.sh depthai-vm"
  SUBSYSTEM=="usb", ACTION=="remove", ENV{PRODUCT}=="3e7/f63b/100", ENV{DEVTYPE}=="usb_device", MODE="0666", RUN+="/usr/local/bin/movidius_usb_hotplug.sh depthai-vm"

然后，udev规则正在调用的脚本(movidius_usb_hotplug.sh)应该将USB设备连接/分离到虚拟机。在这种情况下，我们需要调用 :code:`virsh` 命令。例如，脚本可以执行以下操作：

.. code-block::

  #!/bin/bash
  # Abort script execution on errors
  set -e
  if [ "${ACTION}" == 'bind' ]; then
    COMMAND='attach-device'
  elif [ "${ACTION}" == 'remove' ]; then
    COMMAND='detach-device'
    if [ "${PRODUCT}" == '3e7/2485/1' ]; then
      ID_VENDOR_ID=03e7
      ID_MODEL_ID=2485
    fi
    if [ "${PRODUCT}" == '3e7/f63b/100' ]; then
      ID_VENDOR_ID=03e7
      ID_MODEL_ID=f63b
    fi
  else
    echo "Invalid udev ACTION: ${ACTION}" >&2
    exit 1
  fi
  echo "Running virsh ${COMMAND} ${DOMAIN} for ${ID_VENDOR}." >&2
  virsh "${COMMAND}" "${DOMAIN}" /dev/stdin <<END
  <hostdev mode='subsystem' type='usb'>
    <source>
      <vendor id='0x${ID_VENDOR_ID}'/>
      <product id='0x${ID_MODEL_ID}'/>
    </source>
  </hostdev>
  END
  exit 0

请注意，当设备从USB总线断开连接时，某些udev环境变量不可用（ :code:`ID_VENDOR_ID` 或 :code:`ID_MODEL_ID` ），这就是为什么您需要使用 :code:`PRODUCT` 环境变量来标识哪个设备已断开连接的原因。

运行DepthAI API应用程序的虚拟机应定义一个udev规则来标识OAK-D摄像机。udev规则 `在这里描述 <https://docs.luxonis.com/en/latest/pages/faq/#does-depthai-work-on-the-nvidia-jetson-series>`_

`Manuel Segarra-Abad <https://github.com/maseabunikie>`__ 提供的解决方案。

VMware
***************

在VMware中使用OAK-D设备需要一些额外的一次性设置，才能使其正常工作。

首先，确保将USB控制器从USB2切换到USB3。转到 :code:`Virtual Machine Settings -> USB Controller -> USB compatibility` 并更改为USB 3.1（或对于较早的VMware版本，为USB 3.0，如有）。

根据设备的状态，可能会出现两个设备，并且都需要路由到虚拟机。这些可以在以下位置看到 :code:`Player -> Removable Devices` :

*英特尔Movidius MyriadX
*英特尔VSC环回设备或英特尔Luxonis设备

在Linux OS中，运行以下命令为普通用户提供USB权限：

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

如果虚拟机未检测到设备，请尝试以下操作：查找并选择 *Forget connection rule* 选项（针对两个设备），然后尝试在VM内再次运行DepthAI示例。选择路由到VM并选择 *not ask again* （这很重要，因为存在超时，并且如果主机在几秒钟内未开始通信，则设备监视程序可能会被触发）。您可能需要重复运行该脚本几次，直到为VMware正确设置了所有脚本。

启用 USB 设备（仅在 Linux 上）
#######################################

由于 DepthAI 是 USB 设备，因此为了在使用 :code:`udev` 工具的系统上与之通信， 
您需要添加 udev 规则以使设备可访问。

以下命令将向您的系统添加新的 udev 规则

.. warning::
    提示：
        第一次使用一定要配置此规则！

        第一次使用一定要配置此规则！
        
        第一次使用一定要配置此规则！

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

从 PyPi 安装
#################

我们的软件包是 `通过 PyPi <https://pypi.org/project/depthai/>`__ 分发的，为了可以在您的环境中安装，请使用

.. code-block:: bash

  python3 -m pip install depthai -i https://pypi.tuna.tsinghua.edu.cn/simple 

有关其他安装选项，请参阅 :ref:`其他安装选项 <其他安装方式>` 。

镜像加速
########

pypi 镜像使用帮助
*****************

临时使用
--------

.. code:: bash

    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package 

注意，\ ``simple`` 不能少, 是 ``https`` 而不是 ``http``

设为默认
--------

-  使用命令行

升级 pip 到最新的版本 (>=10.0.0) 后进行配置：

.. code:: bash

    pip install pip -U   
    pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple

如果您的 pip 默认源的网络连接较差，临时使用镜像站来升级 pip：

.. code:: bash

    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple pip -U

-  文本编辑

Pip 的配置文件为用户根目录下的：\ ``~/.pip/pip.conf``\ （Windows
路径为：\ ``C:\Users\<UserName>\pip\pip.ini``\ ）, 您可以配置如下内容：

.. code:: text
    
    [global]   
    index-url = https://pypi.tuna.tsinghua.edu.cn/simple   
    trusted-host = pypi.tuna.tsinghua.edu.cn   
    timeout = 120

Ubuntu 镜像使用帮助
*****************************


备份配置文件：
---------------

.. code:: bash

    sudo cp -a /etc/apt/sources.list /etc/apt/sources.list.bak

修改 ``sources.list`` 文件
----------------------------

.. code:: bash

    sudo sed -i "s@http://.*archive.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list
    sudo sed -i "s@http://.*security.ubuntu.com@https://mirrors.tuna.tsinghua.edu.cn@g" /etc/apt/sources.list

更新索引
-----------

.. code:: bash

    sudo apt-get update

GitHub 镜像使用帮助
*******************

    `FastGit <https://fastgit.org/>`__ 是一个适用于 GitHub
    的加速服务，\ `fgit <https://github.com/fastgh/fgit>`__
    一个在克隆项目时自动把 GitHub 链接替换为 FastGit 链接的工具。

如何使用？
----------

-  Linux

  .. code:: bash

      sudo curl -L https://github.com/fastgh/fgit/releases/download/v1.0.0/fgit.linux -o /usr/local/bin/fgit   
      sudo chmod +x /usr/local/bin/fgit

-  Mac

  .. code:: bash

      sudo curl -L https://github.com/fastgh/fgit/releases/download/v1.0.0/fgit.darwin -o /usr/local/bin/fgit   
      sudo chmod +x /usr/local/bin/fgit

-  Windows

https://github.com/fastgh/fgit/releases/download/v1.0.0/fgit.exe
下载后把它加入系统路径环境变量

Homebrew 镜像使用帮助
*********************

    注：该镜像是 Homebrew 源程序以及 formula/cask 索引的镜像（即
    ``brew update`` 时所更新内容）。

首次安装 Homebrew
-----------------

-  首先，需要确保系统中安装了 bash、git 和 curl，对于 macOS
   用户需额外要求安装 Command Line Tools (CLT) for Xcode。

  .. code:: bash

      # 在命令行输入 xcode-select --install 安装 CLT for Xcode 即可
      xcode-select --install 

-  接着，在终端输入以下两行命令设置环境变量：

  .. code:: bash

      # macOS 用户，请使用以下两句命令
      export OMEBREW_CORE_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git"

      export HOMEBREW_BOTTLE_DOMAIN="https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles"

-  最后，在终端运行以下命令以安装 Homebrew / Linuxbrew：

  .. code:: bash

      # 从镜像下载安装脚本，修改其中的仓库源并安装 Homebrew
      git clone --depth=1 https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/install.git brew-install

      /bin/bash -c "$(sed 's|^HOMEBREW_BREW_GIT_REMOTE=.*$|HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"|g' brew-install/install.sh)"

      rm -rf brew-install

      # 也可从 GitHub 获取官方安装脚本，修改其中的仓库源，运行以安装 Homebrew / Linuxbrew
      /bin/bash -c "$( curl -fsSL https://github.com/Homebrew/install/raw/master/install.sh | sed 's|^HOMEBREW_BREW_GIT_REMOTE=.*$|HOMEBREW_BREW_GIT_REMOTE="https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git"|g' )" 

这样在首次安装的时候也可以使用镜像。

替换现有仓库上游
----------------

替换 brew 程序本身的源：

.. code:: bash

    git -C "$(brew --repo)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/brew.git 

以下针对 macOS 系统上的 Homebrew：

.. code:: bash

    # 手动设置
    git -C "$(brew --repo homebrew/core)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-core.git
    git -C "$(brew --repo homebrew/cask)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask.git
    git -C "$(brew --repo homebrew/cask-fonts)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-fonts.git
    git -C "$(brew --repo homebrew/cask-drivers)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-drivers.git
    git -C "$(brew --repo homebrew/cask-versions)" remote set-url origin https://mirrors.tuna.tsinghua.edu.cn/git/homebrew/homebrew-cask-versions.git

    # 使用脚本
    bash -c "$(curl -fL https://cdn.jsdelivr.net/gh/OAKChina/depthai-docs-website@develop/source/_static/replace_existing_upstream.sh)"

更换上游后需重新设置 git 仓库 HEAD：

.. code-block:: bash

    brew update-reset

Homebrew-bottles 镜像使用帮助
*****************************

    注:该镜像是 Homebrew 二进制预编译包的镜像。

临时使用
--------

.. code:: bash

    export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles

设为默认
--------

-  如果你使用 bash:

  .. code:: bash

      echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.bashrc 
      source ~/.bashrc


-  如果你使用 zsh:

  .. code:: bash

      echo 'export HOMEBREW_BOTTLE_DOMAIN=https://mirrors.tuna.tsinghua.edu.cn/homebrew-bottles' >> ~/.zshrc 
      source ~/.zshrc




测试安装
#################

我们在 GitHub 上有 `depthai <https://github.com/luxonis/depthai>`__ 存储库，
其中包含许多有用的示例和准备好的神经网络，
您可以使用它们来加快原型制作速度。它还包括由我们的贡献者维护的测试脚本，
该脚本应有助于您验证设置是否正确。

首先，克隆 `depthai <https://github.com/luxonis/depthai>`__ 存储库，并将目录更改为该目录：

.. code-block:: bash

  git clone https://github.com/luxonis/depthai.git
  cd depthai

接下来是安装此仓库的要求。请注意，我们建议将依赖项安装在虚拟环境中，以免它们干扰系统上的其他Python工具/环境。

- 对于 Mac / Windows / Ubuntu 等开发工具，我们建议使用 PyCharm IDE，因为它会自动为您创建和管理虚拟环境，以及许多其他好处。可替代的有： `conda` , `pipenv` 或者 `virtualenv` 可以直接使用(或用您的首选IDE)。
- 有关资源受限的系统，如树莓派或其他小型Linux系统的安装，我们建议 `conda` , `pipenv` 或者 `virtualenv` 中的一个。要使用 `virtualenv` 设置虚拟环境，请运行 `virtualenv venv && source venv/bin/activate` 。

使用虚拟环境(或者如果需要在整个系统范围内)，运行以下命令以安装此示例存储库的依赖：

.. code-block:: bash

  python3 install_requirements.py

现在，从 DepthAI 内部运行演示脚本，以确保一切正常：

.. code-block:: bash

  python3 depthai_demo.py

如果一切顺利的话，会弹出一个小视频窗口。
如果画面中的物体属于 `物体检测示例 20 类 <https://github.com/luxonis/depthai/blob/master/resources/nn/mobilenet-ssd/mobilenet-ssd.json#L22>`__ 中的某一类，画面上会叠加该物体的信息。

运行其他示例
##########################################

运行此演示之后，您可以运行 :code:`python3 depthai_demo.py -h` 来查看默认情况下可以运行的其他神经网络。

检查完之后，继续执行以下操作：

- 我们的教程，如何使用 OpenVINO 的预训练模型， `此处 <https://docs.luxonis.com/en/latest/pages/tutorials/pretrained_openvino/>`__
- 我们的实验性示例，在 `这里 <https://github.com/luxonis/depthai-experiments>`__ 可以学习更多使用 DepthAI 的方法。

您还可以在下面继续学习如何转换自己的神经网络以在DepthAI上运行。

另外，我们还在下面提供了在线模型训练，该演示向您展示了如何为 DepthAI 训练和转换模型。

- 在线机器学习训练和模型转换： `此处 <https://github.com/luxonis/depthai-ml-training/tree/master/colab-notebooks>`__

准备 MyriadX blob 文件和它的配置文件
###########################################

正如你在 :ref:`本例 <例如>` 中所看到的，:func:`Device.create_pipeline()` 方法的基本用法包括指定所需的输出流和 AI 部分，在其中指定 MyriadX blob 及其配置。

在本节中，我们将介绍如何同时获取 :code:`blob_file` 和 :code:`blob_file_config` 。

获取 MyriadX Blob 
**********************

由于我们使用的是 MyriadX VPU，
您的模型需要被编译(或准确地进行优化和转换)为 MyriadX Blob 文件，然后将其发送到设备并执行。

最简单的方法是使用我们的在线 `BlobConverter应用程序 <http://69.164.214.171:8083/>`__ 来获取这个 blob 文件。
它有编译所需的所有工具，
所以你不需要设置任何东西–你甚至可以从 `OpenVINO模型Zoo <https://github.com/openvinotoolkit/open_model_zoo>`__ 下载一个模型的 blob。

如果你愿意，你也可以自己编译 blob。
你需要安装 `OpenVINO 工具包 <https://docs.openvinotoolkit.org/latest/index.html>`__，
然后使用 `Model Optimizer  <https://docs.openvinotoolkit.org/latest/openvino_docs_MO_DG_Deep_Learning_Model_Optimizer_DevGuide.html>`__ 
和 `Myriad Compiler <https://docs.openvinotoolkit.org/latest/openvino_inference_engine_tools_compile_tool_README.html#myriad_platform_option>`__ 来获得 MyriadX blob。 
我们已经在 `这里 <https://github.com/luxonis/depthai#conversion-of-existing-trained-models-into-intel-movidius-binary-format>`__ 记录了这些编译器的使用实例。

创建 Blob 配置文件
********************************

如果未提供配置文件或将 :code:`output_format` 设置为 :code:`raw`，
则设备上不会进行解码，用户必须在主机端手动进行解码。

当前支持在设备上对基于 :code:`Mobilenet-SSD` 和 :code:`(tiny-)YOLO-v3` 的网络进行解码。
对于该配置文件，需要使用网络特定的参数。

`tiny-yolo-v3` 网络示例：

.. code-block:: json

  {
      "NN_config":
      {
          "output_format" : "detection",
          "NN_family" : "YOLO",
          "NN_specific_metadata" :
          {
              "classes" : 80,
              "coordinates" : 4,
              "anchors" : [10,14, 23,27, 37,58, 81,82, 135,169, 344,319],
              "anchor_masks" :
              {
                  "side26" : [1,2,3],
                  "side13" : [3,4,5]
              },
              "iou_threshold" : 0.5,
              "confidence_threshold" : 0.5
          }
      },
      "mappings":
      {
          "labels":
          [
              "person",
              "bicycle",
              "car",
              "..."
          ]
      }
  }


* :code:`NN_config` - 网络配置
    * :code:`output_format`
        * :code:`"detection"` - 设备上完成解码， 接收到的数据包为 :class:`Detections` 格式
        * :code:`"raw"` - 在主机上完成解码
    * :code:`NN_family` - `"YOLO"` 或 `"mobilenet"`
    * :code:`NN_specific_metadata` - 仅用于 `"YOLO"`
        * :code:`classes` - classes 数量
        * :code:`coordinates` - coordinates 数量
        * :code:`anchors` - YOLO 网络的锚点
        * :code:`anchor_masks` - 每个输出层的锚定遮罩 : :code:`26x26`, :code:`13x13` (+ `52x52` for full YOLO-v3)
        * :code:`iou_threshold` - 检测到的对象的联合阈值交集
        * :code:`confidence_threshold` - 检测物体的得分置信度阈值
* :code:`mappings.labels` - 使用 :code:`depthai_demo.py` 脚本来解码ID的标签

将 :code:`output_format` 设置为 :code:`detection` 时的解码示例：

.. code-block:: python

  nnet_packets, data_packets = p.get_available_nnet_and_data_packets()

  for nnet_packet in nnet_packets:
    in_layers = nnet_packet.getInputLayersInfo()

    input_width  = in_layers[0].get_dimension(depthai.TensorInfo.Dimension.W)
    input_height = in_layers[0].get_dimension(depthai.TensorInfo.Dimension.H)

    detections = nnet_packet.getDetectedObjects()
    objects = list()

    for detection in detections:
        detection_dict = detection.get_dict()
        # 将标准化坐标缩放为图像坐标
        detection_dict["x_min"] = int(detection_dict["x_min"] * input_width)
        detection_dict["y_min"] = int(detection_dict["y_min"] * input_height)
        detection_dict["x_max"] = int(detection_dict["x_max"] * input_width)
        detection_dict["y_max"] = int(detection_dict["y_max"] * input_height)
        objects.append(detection_dict)

  print(objects)

主机和设备上基于完整 :code:`yolo-v3` 和 :code:`tiny-yolo-v3` 的解码示例在 `此处 <https://github.com/luxonis/depthai/blob/develop/depthai_helpers/tiny_yolo_v3_handler.py>`__


主机和设备上基于 :code:`mobilenet` 的网络的解码示例在 `此处 <https://github.com/luxonis/depthai/blob/develop/depthai_helpers/mobilenet_ssd_handler.py>`__


其他安装方式
##########################

要从我们的源代码中获取最新但尚未发布的功能，您可以继续手动编译 DepthAI 软件包。

从源构建的依赖项
*********************************

- CMake > 3.2.0
- 生成工具 (Ninja, make, ...)
- C/C++ 编译器
- libusb1 开发包

.. _raspbian:

Ubuntu, Raspberry Pi OS, ... (基于 Debian 的系统)
---------------------------------------------------

在基于 Debian 的系统 (Raspberyy Pi OS, Ubuntu, ...)上，可以通过运行以下命令获取安装依赖：

.. code-block:: bash

  sudo apt-get -y install cmake libusb-1.0-0-dev build-essential

macOS (Mac OS X)
----------------

假设安装了 Mac OS X , 则 `depthai-python <https://github.com/luxonis/depthai-python>`__ 库需要以下依赖项

- HomeBrew (如果尚未安装)

  .. code-block:: bash

    /bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install.sh)"

- Python, libusb, CMake, wget

  .. code-block:: bash

      brew install coreutils python3 cmake libusb wget

现在你已经准备好克隆 `depthai-python <https://github.com/luxonis/depthai-python>`__ ，并在 Mac OSX 上构建。

使用 GitHub commit 进行安装
***************************

Pip 允许用户从特定的 commit 安装软件包，即使它们尚未在 PyPi 上发布。


为此，请使用以下命令 - 并确保使用正确的 `commit hash <https://github.com/luxonis/depthai-python/commits>`__ 替换 :code:`<commit_sha>` 

.. code-block:: bash

    python3 -m pip install git+https://github.com/luxonis/depthai-python.git@<commit_sha>

使用/测试特定的 分支/PR
**********************************

有时，使用特定分支可能会引起您的注意。 
例如，这可能是因为我们已经听取了您的功能要求并在分支中实现。
或者可能是出于稳定性目的，在合并到 :code:`main` 中之前，在 :code:`develop` 中实现。

因此，当在 `depthai <https://github.com/luxonis/depthai>`__ 存储库中工作时, 可以通过以下命令来使用分支。
在此示例中， 我们将尝试使用 :code:`develop` 分支
(这是在将新功能合并到 :code:`main` 之前我们用来吸收新功能的分支)：

在运行以下命令之前， 您可以独立克隆存储库
(以免覆盖任何本地更改) 也可以先执行 :code:`git pull` 。

.. code-block:: bash

  git checkout develop
  python3 install_requirements.py

从源安装
*******************

如果需要，您还可以从源代码本身安装该软件包 
- 它将允许您对 API 进行更改，并看到它们的实际操作。

为此，请先下载存储库，然后在开发模式下将该包添加到您的 python 解释器中

.. code-block:: bash

  git clone https://github.com/luxonis/depthai-python.git
  cd depthai-python
  git submodule update --init --recursive
  python3 setup.py develop  # 如果使用系统解释器而不是虚拟环境，则可能需要添加sudo

如果您要使用默认(:code:`main`)以外的其他分支(e.g. :code:`develop`)， 可以通过键入

.. code-block:: bash

  git checkout develop  # replace the "develop" with a desired branch name
  git submodule update --recursive
  python3 setup.py develop

或者，如果您要使用特定的 commit，请键入

.. code-block:: bash

  git checkout <commit_sha>
  git submodule update --recursive
  python3 setup.py develop

.. include::  /pages/includes/footer-short.rst