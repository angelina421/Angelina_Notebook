# `CMake` 简介

## 什么是 CMake

​	CMake 是一个 **跨平台的自动化构建系统**（build system generator）。它本身不直接编译代码，而是 **生成编译系统**——比如生成能被 **Makefile**、**Ninja**、**Visual Studio 工程文件** 等工具识别的构建配置。

![img](https://www.runoob.com/wp-content/uploads/2024/08/Single_Source_Build-cmake.png)

## 如何使用 CMake

### 下载 CMake

* **在官网下载 `cmake` 的安装包**

  ```bash
  wget https://github.com/Kitware/CMake/releases/download/v3.26.0-rc4/cmake-3.26.0-rc4-linux-x86_64.sh
  ```

* **找到下载的 sh 文件，并使用 bash 来执行 sh 脚本**

  ```bash
  bash cmake-3.26.0-rc4-linux-x86_64.sh 
  ```

* **将 `CMake` 添加到 `PATH`**

  ```bash
  nano ~/.bashrc
  export PATH=/home/angelina/cmake-3.26.0-rc4-linux-x86_64/bin:$PATH
  ```

* **测试是否安装完成**

  ```bash
  cmake --version
  ```

### CMake 语法

#### 常见命令

* **最小版本要求**

  * 告诉 CMake：这个项目至少要用哪个版本的 CMake 来构建。

  ```cmake
  cmake_minimum_required(VERSION 3.10)
  ```

* **定义项目**

  * 声明项目名、版本、使用的语言（C、CXX、CUDA 等）

  ```cmake
  project(ProjectName VERSION 1.0 LANGUAGES CXX)
  ```

* **添加可执行文件**

  * 生成一个叫 `my_app` 的可执行文件

  ```cmake
  add_executable(my_app main.cpp utils.cpp)
  ```

* **添加库**

  * `STATIC` 表示静态库，`SHARED` 表示动态库

  ```cmake
  add_library(my_lib STATIC math.cpp io.cpp)
  ```

* **链接库**

  * 让可执行文件链接上一个库

  ```cmake
  target_link_libraries(my_app PRIVATE my_lib)
  ```

* **包含头文件路径**

  * 让编译器知道去哪里找头文件

  ```cmake
  target_include_directories(my_app PRIVATE ${PROJECT_SOURCE_DIR}/include)
  ```


#### 变量与控制结构

* **设置变量**

  ```cmake
  set(MY_NAME "Angelica")
  message("Hello ${MY_NAME}")
  ```

* **条件判断**

  ```cmake
  if(WIN32)
      message("Building on Windows")
  elseif(UNIX)
      message("Building on Linux/Mac")
  endif()
  ```

* **循环**

  ```cmake
  foreach(file IN LISTS SRC_LIST)
      message("Source file: ${file}")
  endforeach()
  ```

* **路径变量**

  | 变量名                     | 含义                         |
  | -------------------------- | ---------------------------- |
  | `PROJECT_SOURCE_DIR`       | 项目根目录                   |
  | `CMAKE_CURRENT_SOURCE_DIR` | 当前 CMakeLists 所在目录     |
  | `CMAKE_BINARY_DIR`         | 构建输出目录（build 文件夹） |

### 使用 CMake 构建工程

* **已有的项目结构**

  * `src/`：主程序和源码
  * `lib/`：自定义库
  * `include/`：公共头文件

  ```css
  my_project/
  ├── CMakeLists.txt
  ├── src/
  │   ├── main.cpp
  │   ├── utils.cpp
  │   └── utils.hpp
  ├── lib/
  │   └── mathlib.cpp
  │   └── mathlib.hpp
  └── include/
      └── common.hpp
  ```

* **编写 `CMakeLists.txt`**

  ```cmake
  cmake_minimum_required(VERSION 3.10)
  project(MyApp VERSION 1.0 LANGUAGES CXX)
  
  # 指定 C++ 标准
  set(CMAKE_CXX_STANDARD 17)
  set(CMAKE_CXX_STANDARD_REQUIRED True)
  
  # 添加头文件搜索路径
  include_directories(${PROJECT_SOURCE_DIR}/include)
  
  # 添加自定义库
  add_library(mathlib STATIC lib/mathlib.cpp)
  
  # 添加主程序
  add_executable(my_app src/main.cpp src/utils.cpp)
  
  # 可执行文件自己的头文件路径
  target_include_directories(my_app PRIVATE ${PROJECT_SOURCE_DIR}/include ${PROJECT_SOURCE_DIR}/lib)
  
  # 链接库到主程序
  target_link_libraries(my_app PRIVATE mathlib)
  ```

* **创建 build 目录**

  ```bash
  mkdir build
  cd build
  ```

* **生成 `Makefile`**

  ```bash
  cmake ..
  ```

* **`make` 构建**

  ```bash
  make
  ```

  > 如果想在 Windows 上用 **MinGW** 配置 CMake 项目
  >
  > * **生成 `Makefile`**
  >
  >   ```bash
  >   cmake -G "MinGW Makefiles" .. 
  >   ```
  >
  > * **`make` 构建**
  >
  >   ```bash
  >   mingw32-make
  >   ```
  >





