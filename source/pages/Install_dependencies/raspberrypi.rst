Raspberry Pi
======================

下载OAK树莓派镜像

**百度网盘**:

    =============  =============================================== ======
    平台            链接                                             密码
    =============  =============================================== ======
    Raspberry Pi   https://pan.baidu.com/s/1c84oDpEXT7VqAwfz4UstLQ  surj
    =============  =============================================== ======

**sourceforge**: 

    地址：https://sourceforge.net/projects/oak-rpi-image/

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

树莓派常见问题请参考 :ref:`此处 <Raspberry Pi出现 "realloc(): invalid pointern Aborted">`