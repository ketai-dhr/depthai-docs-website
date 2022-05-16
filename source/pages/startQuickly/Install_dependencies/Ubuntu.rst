Ubuntu
======================

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
