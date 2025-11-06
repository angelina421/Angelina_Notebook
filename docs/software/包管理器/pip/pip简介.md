# pip简介

## 什么是pip？

`pip` 是 **Python 的官方包管理工具**，全称通常理解为 “**Pip Installs Packages**”。它负责在你的 Python 环境中 **安装、升级、卸载和管理第三方库**。

Python 自带了标准库（像 `math`、`os`、`json` 等），但大多数功能需要额外包（例如 `requests`、`numpy`、`pandas` 等）。`pip` 就是帮你管理这些额外包的工具。

## 如何使用pip？

* **安装包**
  * 从 PyPI（Python Package Index）下载并安装包。

    ```bash
    pip install requests
    ```

  * 从 requirements.txt 安装
  
    ```bash
    pip install -r requirements.txt
    ```
  
  * 指定 PyPI 镜像
  
    ```bash
    pip install -i https://pypi.tuna.tsinghua.edu.cn/simple requests
    ```
  
* **卸载包**

    ```bash
    pip uninstall requests
    ```

* **查看已安装包**

    ```bash
    pip list
    ```

* **生成依赖列表**
  * 用于记录项目依赖，以便其他人或服务器复现相同环境。

    ```bash
    pip freeze > requirements.txt
    ```

* **检查依赖兼容性**

    ```bash
    pip check
    ```

* **升级包**

    ```bash
    pip install --upgrade requests
    ```

