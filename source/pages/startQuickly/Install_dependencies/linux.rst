Linux
======================

Linux平台我们建议使用git直接拉取depthai仓库。

- depthai

.. code-block:: bash

    git clone https://gitee.com/oakchina/depthai.git
    
- depthai-python

.. code-block:: bash

    git clone https://gitee.com/oakchina/depthai-python.git

- depthai-experiments

.. code-block:: bash

    git clone https://gitee.com/oakchina/depthai-experiments.git

.. warning::

    在Linux平台并且第一次使用OAK需要配置udev规则 - :ref:`详情 <启用 USB 设备（仅在 Linux 上）>`

安装依赖:

.. code-block:: python

    python3 -m pip install -r depthai/requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

执行以下命令:

.. code-block:: python

    python3 depthai/depthai_dome.py

效果如下:

.. image:: /_static/images/GetStartedQuickly/linux_show.png
    :alt: show