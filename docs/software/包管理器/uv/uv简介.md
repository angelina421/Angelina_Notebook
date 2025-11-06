# uv 简介

## 什么是 uv？

**uv** 是由 **Astral（也就是打造 `ruff` 的团队）** 开发的一个新一代 Python 工具，用于 **超快的包管理和虚拟环境管理**。
它用 **Rust** 编写，旨在统一并取代 Python 生态中常用的多个工具，如：

> `pip`、`pip-tools`、`virtualenv`、部分 `poetry` 功能。

它的口号可以理解为：“Python 的 `cargo`”。

## 如何下载 uv?

- 用 pip 安装

```bash
pip install uv
```

- 用 curl 安装

```bash
curl -LsSf https://astral.sh/uv/install.sh | sh
```

- 验证安装

```bash
uv --version
```

## 如何使用 uv？

- **初始化项目**

  ```bash
  uv init
  ```

- **创建虚拟环境**

  ```bash
  uv venv .venv
  ```

- **激活环境**

  ```bash
  source .venv/bin/activate      # Linux/Mac
  .venv\Scripts\activate         # Windows
  ```

- **安装包**

  - 从 PyPI（Python Package Index）下载并安装包。

    ```bash
    uv add requests
    ```

  - 从 requirements.txt 安装

    ```bash
    uv pip install -r requirements.txt
    ```

  - 指定 PyPI 镜像（例如清华源）

    ```bash
    uv pip install -i https://pypi.tuna.tsinghua.edu.cn/simple requests
    ```

- **卸载包**

  ```bash
  uv remove requests
  ```

- **查看已安装包**

  ```bash
  uv pip list
  ```

- **生成依赖列表**

  ```bash
  uv pip freeze > requirements.txt
  ```

- **检查依赖兼容性**

  ```bash
  uv pip check
  ```

- **升级包**

  ```bash
  uv pip install --upgrade requests
  ```

- **运行 Python 程序（自动使用虚拟环境）**

  ```bash
  uv run main.py
  ```

- **生成锁文件**

  ```bash
  uv lock
  ```

- **同步**

  ```bash
  uv sync
  ```
