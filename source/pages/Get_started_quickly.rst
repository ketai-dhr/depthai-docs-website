快速上手
=============================

我们准备了windows平台下的python开发环境，以及depthai示例。

并且python环境中已为您安装好了运行depthai示例所需的所有依赖。做到开箱即用。

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=421575163&bvid=BV1c341187zr&cid=438680227&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"> </iframe>
    </div>
    <br/>

下载地址
*************

**百度网盘**:

    ===================================  =============================================== ======
    平台                                  链接                                             密码
    ===================================  =============================================== ======
    :ref:`Linux <Linux>`                 https://pan.baidu.com/s/1TVX03XxOj8ngN_zjFCiL8g  r2hs
    :ref:`Windows <Windows>`             https://pan.baidu.com/s/10A3bVZlFgnmooRQ3B6XjGA  h3rq
    :ref:`Raspberry Pi <Raspberry Pi>`   https://pan.baidu.com/s/1cOaa-A5CzMJGQA2ofcehBA  84u3
    ===================================  =============================================== ======

**sourceforge**: 

    地址：https://sourceforge.net/projects/depthai/

    此处可以选择不同平台下载:

    .. image:: /_static/images/GetStartedQuickly/sourceforge.png
        :alt: sourceforge

Windows
*****************

安装程序下载好后，双击安装。

.. image:: /_static/images/GetStartedQuickly/OAKEnvironmentalSetup.png

选择安装目录

.. image:: /_static/images/GetStartedQuickly/selectDir.png

选择菜单目录

.. image:: /_static/images/GetStartedQuickly/meunDir.png

选择是否添加环境变量

.. image:: /_static/images/GetStartedQuickly/inputPath.png

开始安装

.. image:: /_static/images/GetStartedQuickly/install.png

安装成功

.. image:: /_static/images/GetStartedQuickly/success.png

此时可以看到桌面有一个bat的快捷方式，双击它可以直接运行depthai_demo.py程序。

.. image:: /_static/images/GetStartedQuickly/depthaiDemoShow.png

打开命令提示符，如图所示：

.. image:: /_static/images/GetStartedQuickly/cmd.png

如果您选择添加环境变量，则可以执行以下命令：

进入刚才选择的安装目录

.. code-block:: bash

    cd /d %DEPTHAI_HOME%

查看python版本

.. code-block:: bash

    python -V

查看pip版本及路径

.. code-block:: bash

    pip -V

查看已安装的库

.. code-block:: bash

    pip list

如果您的电脑已安装过python，则可以通过%DEPTHAI_HOME%变量调用我们安装好环境的python。

.. code-block:: bash

    "%DEPTHAI_HOME%\python\python" -V

另外，安装包带有depthai，不需要再去GitHub拉取。

通过命令可直接运行Demo:

.. code-block:: bash

    cd /d %DEPTHAI_HOME%

    python depthai\depthai_demo.py

运行API示例:

.. code-block:: bash

    python depthai_API_examples\ColorCamera\rgb_preview.py

我们还准备了许多应用示例在depthai-experiments文件夹中。

.. image:: /_static/images/GetStartedQuickly/depthaiExperiments.png

Linux
*****************

.. warning::

    因为Linux版本众多并且自带python，所以我们将不再打包python环境。

    在Linux平台并且第一次使用OAK需要配置udev规则 - :ref:`详情 <启用 USB 设备（仅在 Linux 上）>`

下载好压缩包后，将其解压。

命令行进入文件夹内:

.. code-block:: bash

    cd python_linux

.. image:: /_static/images/GetStartedQuickly/linux_terminal.png
    :alt: terminal

安装依赖:

.. code-block:: python

    python3 -m pip install -r depthai/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

执行以下命令:

.. code-block:: python

    python3 depthai/depthai_dome.py


效果如下:

.. image:: /_static/images/GetStartedQuickly/linux_show.png
    :alt: show

运行depthai API 示例，运行以下命令:

.. code-block:: python

    python3 depthai_API_examples\(您想要运行的程序).py

另外，我们还准备了单纯的depthai示例包，里面包含了depthai_demo.py能运行的所有神经网络模型，可以直接运行depthai_demo.py。还有许多depthai API示例。

.. image:: /_static/images/GetStartedQuickly/demo.png
    :alt: demo

Raspberry Pi
*********************

我们准备了一个安装好OAK依赖的树莓派镜像。烧录完成后插入树莓派启动即可。

`下载树莓派官方烧录工具 <https://www.raspberrypi.com/software/>`__ 

.. image:: /_static/images/GetStartedQuickly/download_imager.png
    :alt: download_imager

打开软件，选择我们的oak树莓派镜像和您要烧录进的内存卡，即可开始烧录。

.. image:: /_static/images/GetStartedQuickly/imager_app.png
    :alt: imager_app

启动成功后，打开命令行执行以下命令就可以运行oak。

.. code-block:: bash

    cd depthai
    python3 depthai_demo.py

使用Pycharm打开项目
**************************

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

.. include::  /pages/includes/footer-short.rst