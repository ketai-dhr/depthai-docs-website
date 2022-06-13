windows
==========================

windows10
##########################

我们准备了windows平台下的python开发环境，以及depthai示例。

并且python环境中已为您安装好了运行depthai示例所需的所有依赖。做到开箱即用。

.. raw:: html

    <div style="position: relative; padding-bottom: 56.25%; height: 0; overflow: hidden; max-width: 100%; height: auto;">
        <iframe src="//player.bilibili.com/player.html?aid=421575163&bvid=BV1c341187zr&cid=438680227&page=1" frameborder="0" allowfullscreen style="position: absolute; top: 0; left: 0; width: 100%; height: 100%;"> </iframe>
    </div>
    <br/>


下载安装程序

**百度网盘**:

    =======  =============================================== ======
    平台      链接                                             密码
    =======  =============================================== ======
    Windows  https://pan.baidu.com/s/1nCxw9pk4Y4LrSgWozMkUxA  0vww
    =======  =============================================== ======

**sourceforge**: 

    地址：https://sourceforge.net/projects/depthai/

    此处可以选择不同平台下载:

    .. image:: /_static/images/GetStartedQuickly/sourceforge.png
        :alt: sourceforge

.. note:: 

    这个安装包的depthai版本版本号是2.15.1.0，更新时间2022-03-24。部分示例可能会在gitee上不定期更新，最新示例请在 `此处 <https://gitee.com/oakchina/depthai-experiments>`_ 查看。

安装程序下载好后，双击安装。

.. image:: /_static/images/GetStartedQuickly/OAKEnvironmentalSetup.png

选择安装目录

.. image:: /_static/images/GetStartedQuickly/selectDir.png

选择菜单目录

建议不要安装在C盘。

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

如果您的电脑已安装过python，则可以通过 :code:`%DEPTHAI_HOME%` 变量调用我们安装好环境的python。

.. code-block:: bash

    "%DEPTHAI_HOME%\python\python" -V

运行depthai_demo.py程序:

另外，安装包带有depthai，不需要再去GitHub拉取。

双击桌面的OAK Demo即可运行depthai_demo.py。

.. image:: /_static/images/GetStartedQuickly/oak_demo.png

也可以通过以下命令直接运行Demo:

由于depthai_demo.py需要特殊版本的depthai库，我们为depthai_demo.py单独制作了一个Python环境。

.. code-block:: bash

    cd /d %DEPTHAI_HOME%\depthai

    depthai_demo_python\python.exe depthai_demo.py

.. image:: /_static/images/GetStartedQuickly/depthaiDemoGui.png

depthai_demo.py程序默认为GUI显示，不过目前GUI的demo还不是很稳定，可以使用 :code:`-gt` 参数设置opencv显示。

.. code-block:: bash

    depthai_demo_python\python.exe depthai_demo.py -gt cv

.. image:: /_static/images/GetStartedQuickly/depthaiDemoCV.png

.. warning::

    **如果系统用户名是中文** 并出现下图错误：

    .. image:: /_static/images/GetStartedQuickly/modeError.png

    您可以在depthai-demo.py文件中添加以下代码：

    .. code-block:: python

        import blobconverter

        blobconverter.set_defaults(output_dir="<指定模型文件下载路径>")

    如果下载太慢，您可以将用户目录下的.cache文件夹中blobconverter文件夹复制到上面代码中所指定的文件夹中。

运行校准程序:

在DEPTHAI_HOME中，我们还准备了为OAK-D校准的bat程序。

.. image:: /_static/images/GetStartedQuickly/calibrate_bat.png

如果您想要校准其他OAK设备，可以修改depthai_calibrate.bat文件。

.. image:: /_static/images/GetStartedQuickly/modify_bat.png

也可以在命令行运行校准程序。

.. code-block:: bash

    depthai_demo_python\python.exe calibrate.py -s 2.5 -db -brd BW1098OBC

运行API示例:

.. code-block:: bash

    python depthai_API_examples\ColorCamera\rgb_preview.py

我们还准备了许多应用示例在depthai-experiments文件夹中。

.. image:: /_static/images/GetStartedQuickly/depthaiExperiments.png

Windows 7
########################

尽管我们不正式支持Windows 7, 但是我们的社区成员 `已经成功 <https://discuss.luxonis.com/d/105-run-on-win7-sp1-x64-manual-instal-usb-driver>`__ 使用 `Zadig
<https://zadig.akeo.ie/>`__ 手动安装WinUSB . 连接DepthAI设备后，寻找具有 :code:`USB ID:03E7 2485` 的设备并选择WinUSB（v6.1.7600.16385）安装WinUSB驱动程序，然后安装WCID驱动程序。
