快速上手
=============================

我们准备了windows平台下的python开发环境，以及depthai示例。

并且python环境中已为您安装好了运行depthai示例所需的所有依赖。做到开箱即用。

下载地址
*************

**百度网盘**:

    =======  =============================================== ======
    平台      链接                                             密码
    =======  =============================================== ======
    Linux    https://pan.baidu.com/s/1NcXYmCd0Ek3gIV4aHNjCcw  gy6m
    windows  https://pan.baidu.com/s/13ylkyATQUkb8LJ34eBR2bA  k31m
    =======  =============================================== ======

**sourceforge**: 

    地址：https://sourceforge.net/projects/depthai/

    此处可以选择不同平台下载:

    .. image:: /_static/images/GetStartedQuickly/sourceforge.png
        :alt: sourceforge

Windows
*****************

下载好压缩包后，将其解压。

进入文件夹内，在文件夹地址输入cmd，调出命令提示符。

.. image:: /_static/images/GetStartedQuickly/windows_cmd.jpg
    :alt: cmd1

.. image:: /_static/images/GetStartedQuickly/windows_cmd2.jpg
    :alt: cmd2

执行以下命令:

.. code-block:: python

    Python37\python.exe depthai\depthai_demo.py

效果如下:

.. image:: /_static/images/GetStartedQuickly/windows_show.png
    :alt: show

运行depthai API 示例，运行以下命令:

.. code-block:: python

    Python37\python.exe depthai_API_examples\(您想要运行的程序).py

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