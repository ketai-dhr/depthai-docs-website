测试设备连接
===================

我们已经编译好了一个测试程序，您可以下载直接使用。

如果您的设备是POE版本的，可以通过此程序看到更多信息，有助于判断问题所在。

在Windows上运行
#################

.. raw:: html

    <div class="download">
        <a href="/_static/download/list_devices.rar">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-box-arrow-down" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M3.5 10a.5.5 0 0 1-.5-.5v-8a.5.5 0 0 1 .5-.5h9a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 0 0 1h2A1.5 1.5 0 0 0 14 9.5v-8A1.5 1.5 0 0 0 12.5 0h-9A1.5 1.5 0 0 0 2 1.5v8A1.5 1.5 0 0 0 3.5 11h2a.5.5 0 0 0 0-1h-2z"/>
                <path fill-rule="evenodd" d="M7.646 15.854a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 14.293V5.5a.5.5 0 0 0-1 0v8.793l-2.146-2.147a.5.5 0 0 0-.708.708l3 3z"/>
            </svg>
            点击此处下载程序
        </a>
    </div>

解压后双击运行bat文件即可

.. image:: /_static/images/tutorials/list_devices/windows.png

在Ubuntu上运行
#################

.. raw:: html

    <div class="download">
        <a href="/_static/download/list_devices.tar.xz">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-box-arrow-down" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M3.5 10a.5.5 0 0 1-.5-.5v-8a.5.5 0 0 1 .5-.5h9a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 0 0 1h2A1.5 1.5 0 0 0 14 9.5v-8A1.5 1.5 0 0 0 12.5 0h-9A1.5 1.5 0 0 0 2 1.5v8A1.5 1.5 0 0 0 3.5 11h2a.5.5 0 0 0 0-1h-2z"/>
                <path fill-rule="evenodd" d="M7.646 15.854a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 14.293V5.5a.5.5 0 0 0-1 0v8.793l-2.146-2.147a.5.5 0 0 0-.708.708l3 3z"/>
            </svg>
            点击此处下载程序
        </a>
    </div>

解压后执行:

.. code-block:: bash
    
    ./list_devices

手动编译
#################

.. raw:: html

    <div class="download">
        <a href="/_static/download/XLink-4c149080d22c35a17ce285f5bca99f2b2fe05e46.zip">
            <svg xmlns="http://www.w3.org/2000/svg" width="16" height="16" fill="currentColor" class="bi bi-box-arrow-down" viewBox="0 0 16 16">
                <path fill-rule="evenodd" d="M3.5 10a.5.5 0 0 1-.5-.5v-8a.5.5 0 0 1 .5-.5h9a.5.5 0 0 1 .5.5v8a.5.5 0 0 1-.5.5h-2a.5.5 0 0 0 0 1h2A1.5 1.5 0 0 0 14 9.5v-8A1.5 1.5 0 0 0 12.5 0h-9A1.5 1.5 0 0 0 2 1.5v8A1.5 1.5 0 0 0 3.5 11h2a.5.5 0 0 0 0-1h-2z"/>
                <path fill-rule="evenodd" d="M7.646 15.854a.5.5 0 0 0 .708 0l3-3a.5.5 0 0 0-.708-.708L8.5 14.293V5.5a.5.5 0 0 0-1 0v8.793l-2.146-2.147a.5.5 0 0 0-.708.708l3 3z"/>
            </svg>
            点击此处下载源码包
        </a>
    </div>

编译
********

解压后进入文件夹

.. code-block:: bash

    cd XLink-4c149080d22c35a17ce285f5bca99f2b2fe05e46

    mkdir build && cd build

    cmake -D XLINK_BUILD_EXAMPLES=ON ..

    make

可以在进入examples文件夹中找到编译好的list_devices程序，如下图所示：

.. image:: /_static/images/tutorials/list_devices/make_list_devices.png

运行
********

.. code-block:: bash

    cd examples

    ./list_devices

.. image:: /_static/images/tutorials/list_devices/run.png

上图可以看到，如果找到了设备，会在最后一行将设备信息显示出来。

.. include::  /pages/includes/footer-short.rst
