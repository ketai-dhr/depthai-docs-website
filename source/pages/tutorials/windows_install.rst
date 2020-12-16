在 Windows 上安装需求
===============================

本教程提供了在 Windows 10 系统上安装 DepthAI 的步骤

#. 从 Microsoft Store 安装 Python 3.8 。

    .. image:: /_static/images/tutorials/windows/store.png
      :alt: Windows store

    安装完成后，您需要同时禁用 **python.exe** 和 **python3.exe** 的 **应用程序执行别名** 。

    这可以通过开始并键入 :code:`Manage app` 并选择 :code:`Manage app execution aliases` 条目来完成。

    .. image:: /_static/images/tutorials/windows/execution_search.png
      :alt: Search execution

    从那里关闭 **python.exe** 和 **python3.exe** 的 :code:`App Installer` 别名。

    .. image:: /_static/images/tutorials/windows/execution_aliases.png
      :alt: Execution aliases

#. 安装 Git 。 您可以在 `这里 <https://git-scm.com/download/win>`__ 找到最新版本。 

    .. image:: /_static/images/tutorials/windows/git.png
      :alt: Git install

#. 安装 CMake。 您可以在 `这里 <https://cmake.org/download/#latest>`__ 找到最新版本。 

    .. image:: /_static/images/tutorials/windows/cmake.png
      :alt: CMake install

#. 安装 Microsoft Visual Studio 2017（`此处 <https://download.visualstudio.microsoft.com/download/pr/c5c75dfa-1b29-4419-80f8-bd39aed6bcd9/7ed8fa27575648163e07548ff5667b55b95663a2323e2b2a5f87b16284e481e6/vs_Community.exe>`__ 直接下载链接）

    从 PyPi 构建 OpenCV Python 软件包需要 2017 版本

    请注意，您只需要 **Windows 10 SDK**，**Visual C++ for CMake** 和 **VC++ 2017**，而不需要整个软件包（请参见下图）

    .. image:: /_static/images/tutorials/windows/vsstudio.png
      :alt: Visual Studio install


#. 打开 Winodws PowerShell

    您可以通过在搜索栏中输入 **PowerShell** 来实现

    .. image:: /_static/images/tutorials/windows/powershell.png
      :alt: Open PowerShell

#. 更新 Pip

    需要最新的 pip 版本才能正确安装 PyPi 需求。

    要升级 pip，请在 Powershell 中键入以下命令

    .. code-block:: bash

      python3.exe -m pip install -U pip

    .. image:: /_static/images/tutorials/windows/pip.png
      :alt: Update Pip

    成功安装后，您应该会看到 pip 已安装最新版本

    .. image:: /_static/images/tutorials/windows/pip_success.png
      :alt: Update Pip success

#. 运行 DepthAI 演示代码

    有关详细信息，请参阅 :ref:`验证安装 <验证安装>` 

.. include::  /pages/includes/footer-short.rst