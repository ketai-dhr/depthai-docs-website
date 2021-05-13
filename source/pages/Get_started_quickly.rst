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

    Python37\python.exe depthai\depthai_dome.py

效果如下:

.. image:: /_static/images/GetStartedQuickly/windows_show.png
    :alt: show

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

另外，我们还准备了单纯的depthai示例包，里面包含了depthai_demo.py能运行的所有神经网络模型，可以直接运行depthai_demo.py。还有许多depthai API示例。

.. image:: /_static/images/GetStartedQuickly/demo.png
    :alt: demo

.. include::  /pages/includes/footer-short.rst