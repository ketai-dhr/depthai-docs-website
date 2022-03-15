开发者上手指南
==================

depthai项目链接
#####################

    ===================  ===============================================
    项目                  链接
    ===================  ===============================================
    depthai               https://gitee.com/oakchina/depthai
    depthai-python        https://gitee.com/oakchina/depthai-python
    depthai-experiments   https://gitee.com/oakchina/depthai-experiments
    ===================  ===============================================

支持的平台
#################################

我们为以下平台保留最新的，预编译的库。请注意，新的变化是，对于Jetson / Xavier系列，Ubuntu现在也可以保持不变：

    ======================== ==============================================
    平台                      指示                                          
    ======================== ==============================================
    Windows 10               :ref:`Platform dependencies <Windows10>`        
    macOS                    :ref:`Platform dependencies <macOS>`           
    Ubuntu                   :ref:`Platform dependencies <Ubuntu>`         
    Raspberry Pi OS          :ref:`Platform dependencies <Raspberry Pi OS>`
    Jetson/Xavier            :ref:`Platform dependencies <Jetson>`         
    ======================== ==============================================

启用 USB 设备（仅在 Linux 上）
#######################################

由于OAK是USB设备，因此为了在使用 :code:`udev` 工具的系统上与之通信， 
您需要添加udev规则以使设备可访问。

以下命令将向您的系统添加新的udev规则

.. warning::
    提示：
        第一次使用一定要配置此规则！

        第一次使用一定要配置此规则！
        
        第一次使用一定要配置此规则！

.. code-block:: bash

  echo 'SUBSYSTEM=="usb", ATTRS{idVendor}=="03e7", MODE="0666"' | sudo tee /etc/udev/rules.d/80-movidius.rules
  sudo udevadm control --reload-rules && sudo udevadm trigger

安装依赖
###################

Windows10
********************************

请参考 :ref:`此处 <Windows>` ,下载OAK开发环境安装程序。

Windows 7
**************************

尽管我们不正式支持Windows 7, 但是我们的社区成员 `已经成功 <https://discuss.luxonis.com/d/105-run-on-win7-sp1-x64-manual-instal-usb-driver>`__ 使用 `Zadig
<https://zadig.akeo.ie/>`__ 手动安装WinUSB . 连接DepthAI设备后，寻找具有 :code:`USB ID:03E7 2485` 的设备并选择WinUSB（v6.1.7600.16385）安装WinUSB驱动程序，然后安装WCID驱动程序。

macOS
****************

- 拉取项目

.. code-block:: bash

    git clone https://gitee.com/oakchina/depthai.git
    
- 安装依赖

.. code-block:: bash

    cd depthai
    python3 install_requirements.py

如果安装依赖遇到网络问题可以查看此处换成 :ref:`国内镜像源 <镜像加速>` 

- 运行Demo

.. code-block:: bash

    python3 depthai-demo.py

Ubuntu
****************

- 拉取项目

.. code-block:: bash

    git clone https://gitee.com/oakchina/depthai.git
    
- 安装依赖

.. code-block:: bash

    cd depthai
    python3 install_requirements.py

如果安装依赖遇到网络问题可以查看此处换成 :ref:`国内镜像源 <镜像加速>` 

- 运行Demo

.. code-block:: bash

    python3 depthai-demo.py

**注意!** 如果从 PyPi 安装后 opencv 失败并显示非法指令，请添加：

.. code-block:: bash

  echo "export OPENBLAS_CORETYPE=ARMV8" >> ~/.bashrc
  source ~/.bashrc

Jetson平台
********************************

.. warning::

    在Jetson上千万别直接去运行depthai包里自带的安装依赖脚本。
    
    它会安装OpenCV，就会把原先的OpenCV覆盖了，普通的OpenCV是无法在Jetson上运行的。
    
    可以按照下面的方法创建一个虚拟环境。

要在 `Jetson Nano <https://developer.nvidia.com/embedded/jetson-nano-developer-kit>`__ 或 `Jetson Xavier <https://developer.nvidia.com/embedded/jetson-xavier-nx-devkit>`__ 上安装 DepthAI ，请在完成全新安装和设置后执行以下步骤。在第一次登录时，不要立即运行更新。

第一步是可选的：更新升级软件包并删除您可能不会使用的应用程序或软件。

.. code-block:: bash

    sudo apt update && sudo apt upgrade
    sudo reboot now

更改SWAP的大小。这些说明来自Nvidia的 `Jetson上的AI入门 <https://developer.nvidia.com/embedded/learn/jetson-ai-certification-programs>`__

.. code-block:: bash

    # 禁用 ZRAM:
    sudo systemctl disable nvzramconfig
    # 创建 4GB swap文件
    sudo fallocate -l 4G /mnt/4GB.swap
    sudo chmod 600 /mnt/4GB.swap
    sudo mkswap /mnt/4GB.swap

如果您对最终命令有疑问，可以尝试以下操作：

.. code-block:: bash

    sudo vi /etc/fstab

    # 在文件底部添加这一行
    /mnt/4GB.swap swap swap defaults 0 0

    # 重启 
    sudo reboot now

下一步是安装 :code:`pip` 和 :code:`python3` :

.. code-block:: bash
  
    sudo -H apt install -y python3-pip

之后，安装并设置虚拟环境：

.. code-block:: bash

    sudo -H pip3 install virtualenv virtualenvwrapper

将以下行添加到 bash 脚本中：

.. code-block:: bash

    sudo vi ~/.bashrc

    # 虚拟环境包装器配置
    export WORKON_HOME=$HOME/.virtualenvs
    export VIRTUALENVWRAPPER_PYTHON=/usr/bin/python3
    source /usr/local/bin/virtualenvwrapper.sh

通过运行 :code:`source ~/.bashrc` 命令保存并重新加载脚本。然后创建一个虚拟环境(在本例中称为 :code:`depthAI` )。

.. code-block:: bash

    mkvirtualenv depthAI -p python3

**注意!** 在安装 :code:`depthai` 之前，请确保您处于虚拟环境中。

.. code-block:: bash

    #下载并安装依赖包
    sudo wget -qO- http://docs.luxonis.com/_static/install_dependencies.sh | bash

    #克隆 github 仓库
    git clone https://github.com/luxonis/depthai-python.git
    cd depthai-python

最后一步是 :code:`.bashrc` 使用以下行进行编辑：

.. code-block:: bash

    echo "export OPENBLAS_CORETYPE=ARMV8" >> ~/.bashrc

进入 :code:`depthai` 示例文件夹, 运行 :code:`python install_requirements.py` , 然后运行 :code:`python rgb_preview.py`.

`iacisme <https://github.com/iacisme>`__ 通过 `Discord <https://discord.com/channels/790680891252932659/795742008119132250>`__ 提供的解决方案.

Raspberry Pi OS
*********************************

请参考 :ref:`此处 <Raspberry Pi>` ,下载OAK树莓派系统镜像。

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

如果您的pip默认源的网络连接较差，临时使用镜像站来升级pip：

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

.. include::  /pages/includes/footer-long.rst