Jetson平台
======================

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

