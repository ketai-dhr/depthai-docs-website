安装depthai
==================

支持的平台
#################################

我们为以下平台保留最新的，预编译的库。对于Windows用户我们直接提供了安装包。请注意，新的变化是对于Jetson / Xavier系列，Ubuntu现在也可以保持不变：

    ======================== ==============================================
    平台                      项目直达                                          
    ======================== ==============================================
    Windows                  :ref:`点击进入 <windows10>`        
    macOS                    :ref:`点击进入 <macOS>`           
    Ubuntu                   :ref:`点击进入 <Ubuntu>`         
    Raspberry Pi OS          :ref:`点击进入 <Raspberry Pi>`
    Jetson/Xavier            :ref:`点击进入 <Jetson平台>`         
    ======================== ==============================================


depthai项目链接
#####################

下面是我们放在gitee上的存储库，里面有丰富的例子可供初学者使用学习。

    ===================  ===============================================
    项目                  链接
    ===================  ===============================================
    depthai               https://gitee.com/oakchina/depthai
    depthai-python        https://gitee.com/oakchina/depthai-python
    depthai-experiments   https://gitee.com/oakchina/depthai-experiments
    ===================  ===============================================


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

.. toctree::
    :glob:

    ./Install_dependencies/Windows10.rst
    ./Install_dependencies/linux.rst
    ./Install_dependencies/raspberrypi.rst
    ./Install_dependencies/Ubuntu.rst
    ./Install_dependencies/Jetson.rst
    ./Install_dependencies/macOS.rst

镜像加速
=======================

pypi 镜像使用帮助
#######################

临时使用
**********************

.. code:: bash

    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple some-package 

注意，\ ``simple`` 不能少, 是 ``https`` 而不是 ``http``

设为默认
***********

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

使用Pycharm打开项目
=======================

1、下载安装后打开pycharm界面

.. image:: /_static/images/GetStartedQuickly/pycharm1.png

2、打开我们打包好的工程文件

点击Open，打开Open Fire or Project 文件夹，路径设置为下载的百度云文件(开发环境安装包)解压后的位置

.. image:: /_static/images/GetStartedQuickly/pycharm2.png

3、设置python解释器为我们开发环境安装包内自带的python开发环境

右下角点击Interpreter Settings

.. image:: /_static/images/GetStartedQuickly/pycharm3.png

4、将python解释器改为我们开发环境安装包内的python.exe

.. image:: /_static/images/GetStartedQuickly/pycharm4.png

其他编辑器跟PyCharm一样，首先需要加载压缩包中的python开发环境


.. include::  /pages/includes/footer-long.rst

