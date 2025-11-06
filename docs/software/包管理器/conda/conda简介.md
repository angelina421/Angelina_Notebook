# Conda 简介

## Conda 是什么？

Conda 是一个功能非常强大的 **包管理器 + 环境管理器**，它最初是为 Anaconda 发行版设计的，但也可以单独安装（Miniconda）。它的核心优势是可以同时管理 **Python 版本**、**依赖包** 和 **虚拟环境**，尤其在数据科学和科学计算场景下非常方便。

## Conda 有什么功能？

| 功能                     | 说明                                      | 举例                                     |
| ------------------------ | ----------------------------------------- | ---------------------------------------- |
| **安装软件包**           | 安装 numpy、pandas、opencv 等包（含依赖） | `conda install numpy`                    |
| **更新/卸载包**          | 方便地升级或卸载                          | `conda update pandas`                    |
| **创建隔离环境**         | 避免不同项目间的依赖冲突                  | `conda create -n proj1 python=3.10`      |
| **跨平台支持**           | Windows/Linux/Mac 都可用                  | -                                        |
| **管理多个 Python 版本** | 不同环境中可用不同版本                    | 比如一个项目用 Python 3.8，另一个用 3.12 |

## 如何下载Conda?

### **Anaconda**

- 含 **Python、Conda 以及大量常用科学计算库**（numpy、pandas、matplotlib、scipy 等）。
- 体积大（几百 MB）。
- 适合希望一次性装好科学计算环境的人。

下载链接：
 <https://www.anaconda.com/products/distribution>

安装步骤：

1. 下载适合你操作系统的安装程序（Windows / macOS / Linux）。
2. 运行安装程序，选择默认路径。
3. 勾选“Add Anaconda to PATH”（可选，但推荐用 Anaconda Prompt）。
4. 安装完成后，用 **Anaconda Prompt** 或命令行测试：

```bash
conda --version
```

------

### **Miniconda**

- 只有 **Python + Conda**，没有预装库。
- 体积小（几十 MB），适合轻量环境和自定义安装。
- 适合你想用 Conda 自己管理包、避免安装过多不必要的库。

下载链接：
 <https://docs.conda.io/en/latest/miniconda.html>

安装步骤类似 Anaconda，安装后用命令行测试：

```bash
conda --version
```

## 如何使用 Conda？

- **安装包**

  - 从 Conda 仓库安装包：

    ```bash
    conda install numpy
    ```

  - 从特定 channel 安装包（如 conda-forge）：

    ```bash
    conda install -c conda-forge numpy
    ```

  - 安装指定版本：

    ```bash
    conda install numpy=1.26
    ```

- **卸载包**

  ```bash
  conda remove numpy
  ```

- **查看已安装包**

  ```bash
  conda list
  ```

- **创建虚拟环境**

  ```bash
  conda create -n myenv python=3.12
  ```

- **激活虚拟环境**

    ```bash
    conda activate myenv
    ```

- **退出虚拟环境**

    ```bash
    conda deactivate
    ```

-  **查看已有虚拟环境**

    ```bash
    conda env list
    ```

- **删除虚拟环境**

    ```bash
    conda remove -n myenv --all
    ```

- **查看可用环境**

    ```bash
    conda env list
    ```

- **导出环境依赖**

    ```bash
    conda env export > environment.yml
    ```

- **加载环境**

    ```bash
    conda env create -f environment.yml
    ```
