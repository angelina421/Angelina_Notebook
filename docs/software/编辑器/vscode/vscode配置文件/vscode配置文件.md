# .vscode配置文件

在 Visual Studio Code (VSCode) 中，`launch.json`、`tasks.json` 和 `settings.json` 是配置文件，允许用户定制编辑器的行为、调试设置、任务管理等。这些文件位于 `.vscode` 文件夹内，并可以根据需求进行修改和扩展。

## `launch.json` — 调试配置

`launch.json` 用于配置调试器的启动方式，包含了你如何调试应用程序的信息。你可以在这个文件中配置调试器的运行时环境、程序参数、工作目录等。

### 主要内容

- #### 1. **`version`**

  - **类型**：字符串
  - **描述**：指定 `launch.json` 文件的版本。当前应该始终为 `"0.2.0"`。

  #### 2. **`configurations`**

  - **类型**：数组
  - **描述**：该数组包含多个调试配置，每个配置都是一个对象。每个配置描述了如何启动一个调试会话。

  #### 每个调试配置的关键参数

  #### 3. **`name`**

  - **类型**：字符串
  - **描述**：调试配置的名称。它将显示在调试配置列表中，用户可以选择不同的配置进行调试。

  #### 4. **`type`**

  - **类型**：字符串

  - 描述

    ：调试器类型，用于指定所使用的调试器。例如：

    - `cppdbg`：C++ 调试（使用 `GDB` 或 `LLDB`）。
    - `node`：Node.js 调试。
    - `python`：Python 调试。
    - `coreclr`：.NET Core 调试。
    - `chrome`：用于调试 Chrome 或其他浏览器中的 JavaScript。

    其他调试器类型可以通过 VSCode 扩展安装。

  #### 5. **`request`**

  - **类型**：字符串

  - 描述

    ：指定调试请求类型。通常有两种：

    - `launch`：启动一个新调试会话（调试新进程）。
    - `attach`：连接到一个已运行的进程。

  #### 6. **`program`**

  - **类型**：字符串

  - **描述**：要调试的程序的路径。对于 C++ 项目，通常是生成的可执行文件路径。

    例如：

    ```json
    "program": "${workspaceFolder}/a.out"
    ```

    这里的 `${workspaceFolder}` 是 VSCode 的预定义变量，指代当前工作区文件夹。

  #### 7. **`args`**

  - **类型**：数组（字符串）

  - **描述**：传递给程序的命令行参数。它允许你在调试时给程序传递输入参数。

    例如：

    ```json
    "args": ["arg1", "arg2"]
    ```

    将在运行时将 `"arg1"` 和 `"arg2"` 作为参数传递给程序。

  #### 8. **`stopAtEntry`**

  - **类型**：布尔值

  - **描述**：如果为 `true`，调试器会在程序开始执行时暂停。这通常用于检查程序的初始状态。

    例如：

    ```json
    "stopAtEntry": true
    ```

  #### 9. **`cwd`**

  - **类型**：字符串

  - **描述**：指定调试时的工作目录。默认情况下是程序的路径，通常不需要设置，但在某些情况下可能需要修改。

    例如：

    ```json
    "cwd": "${workspaceFolder}"
    ```

  #### 10. **`env`**

  - **类型**：对象

  - **描述**：指定调试时的环境变量。你可以为程序添加或修改环境变量。

    例如：

    ```json
    "env": {
      "MY_VAR": "value"
    }
    ```

    这将在调试时为程序设置 `MY_VAR` 环境变量为 `value`。

  #### 11. **`externalConsole`**

  - **类型**：布尔值

  - **描述**：是否启动外部控制台。默认值为 `false`，表示使用 VSCode 的内置终端。如果为 `true`，将启动一个新的外部终端窗口来运行程序。

    例如：

    ```json
    "externalConsole": true
    ```

  #### 12. **`MIMode`**

  - **类型**：字符串
  - 描述：指定使用的调试模式，通常与 GDB 或 LLDB 配合使用，常见的值为：
    - `gdb`：使用 GDB 调试器（GNU 调试器）。
    - `lldb`：使用 LLDB 调试器（适用于 macOS 和 Linux）。

  #### 13. **`miDebuggerPath`**

  - **类型**：字符串

  - **描述**：指定调试器（如 GDB）的路径。如果没有安装 GDB 或 LLDB，或者它不在默认路径中，需要手动设置路径。

    例如：

    ```json
    "miDebuggerPath": "/usr/bin/gdb"
    ```

  #### 14. **`preLaunchTask`**

  - **类型**：字符串

  - **描述**：指定在启动调试之前要执行的任务。这通常用于编译程序或执行其他准备工作。需要确保 `tasks.json` 文件中有对应的任务。

    例如：

    ```json
    "preLaunchTask": "build"
    ```

  #### 15. **`postDebugTask`**

  - **类型**：字符串

  - **描述**：指定在调试会话结束后要执行的任务。这通常用于清理操作或其他后处理工作。

    例如：

    ```json
    "postDebugTask": "clean-up"
    ```

  #### 16. **`setupCommands`**

  - **类型**：数组（对象）

  - **描述**：指定调试器启动时要执行的设置命令。例如，在 GDB 调试时，可以启用 GDB 的一些设置。

    例如：

    ```json
    "setupCommands": [
      {
        "description": "Enable pretty-printing for gdb",
        "text": "-enable-pretty-printing",
        "ignoreFailures": false
      }
    ]
    ```

  #### 17. **`miDebuggerArgs`**

  - **类型**：数组（字符串）

  - **描述**：指定传递给 GDB 调试器的额外参数。

    例如：

    ```json
    "miDebuggerArgs": ["--args"]
    ```

示例：调试 C++ 程序的 `launch.json`

```json
{
    "version": "0.2.0",
    "configurations": [
        
        {
            "name": "Debug C++ Program",  
            "type": "cppdbg",             
            "request": "launch",          
            "program": "${workspaceFolder}/a.out",  
            "args": [],                   
            "stopAtEntry": false,         
            "cwd": "${workspaceFolder}",  
            "environment": [],            
            "externalConsole": true,      
            "MIMode": "gdb",              
            "miDebuggerPath": "/usr/bin/gdb",  
            "setupCommands": [
              {
                "description": "Enable pretty-printing for gdb",
                "text": "-enable-pretty-printing",  
                "ignoreFailures": false
              }
            ],
            "preLaunchTask": "build",      
            "postDebugTask": "clean-up",   
          }
        
    ]
}
```

## `tasks.json` — 任务配置

`tasks.json` 用于在 `VSCode` 中配置任务（如构建、运行、测试等），可以手动或自动执行这些任务。任务可以是编译任务、清理任务、测试任务等等。

### 主要内容

- #### 1. **`version`**

  - **类型**：字符串
  - **描述**：指定 `tasks.json` 文件的版本。目前应该始终为 `"2.0.0"`。

  #### 2. **`tasks`**

  - **类型**：数组
  - **描述**：该数组包含一个或多个任务，每个任务是一个对象。你可以在这里定义多个不同的任务。

  #### 每个任务的关键字段

  #### 3. **`label`**

  - **类型**：字符串
  - **描述**：任务的名称，显示在 VSCode 的任务列表中。你可以通过任务名称在命令面板或快捷键中选择该任务。

  #### 4. **`type`**

  - **类型**：字符串

  - 描述

    ：任务类型。常见的任务类型有：

    - `shell`：执行 Shell 命令或脚本。
    - `process`：执行外部进程。
    - `npm`：执行 NPM 命令（适用于 Node.js 项目）。
    - `gulp`、`grunt`：执行 Gulp 或 Grunt 任务。

  #### 5. **`command`**

  - **类型**：字符串
  - **描述**：要执行的命令。如果是 Shell 类型任务，通常是一个命令或脚本的路径。例如，编译命令 `g++`，或者执行某个脚本文件的路径。

  #### 6. **`args`**

  - **类型**：数组（字符串）

  - **描述**：传递给命令的参数。例如，编译时传递给 `g++` 的源代码路径和编译选项。

    示例：

    ```json
    "args": ["-g", "${workspaceFolder}/main.cpp", "-o", "${workspaceFolder}/a.out"]
    ```

    这里的 `${workspaceFolder}` 是一个 VSCode 预定义变量，指代当前工作区的根目录。

  #### 7. **`group`**

  - **类型**：对象

  - **描述**：任务的分组。VSCode 支持将任务分为不同的组（如构建、测试）。常见的分组类型有：

    - `build`：表示该任务是构建任务。
    - `test`：表示该任务是测试任务。
    - `none`：表示该任务没有分组。

    示例：

    ```json
    "group": {
      "kind": "build",          // 任务类型：构建任务
      "isDefault": true         // 设置为默认构建任务
    }
    ```

    其中，`isDefault` 设置为 `true` 时，表示这是默认的构建任务。VSCode 会在按下 `Ctrl+Shift+B` 时执行该任务。

  #### 8. **`problemMatcher`**

  - **类型**：数组（字符串）

  - **描述**：用于匹配任务输出中的问题（如编译错误、警告等）。`problemMatcher` 定义了如何从任务输出中提取错误或警告，并将其显示在 `Problems` 面板中。

    常用的匹配器：

    - `$gcc`：用于匹配 `g++` 编译器的输出。
    - `$tsc`：用于匹配 TypeScript 编译器的输出。
    - `$eslint-compact`：用于匹配 ESLint 的输出。

    示例：

    ```json
    "problemMatcher": ["$gcc"]
    ```

  #### 9. **`detail`**

  - **类型**：字符串
  - **描述**：任务的描述文本，显示在任务列表中，帮助用户了解该任务的目的或功能。

  #### 10. **`options`**

  - **类型**：对象

  - **描述**：指定任务运行时的一些额外选项。例如，你可以指定工作目录、环境变量等。

    例如，设置任务的工作目录：

    ```json
    "options": {
      "cwd": "${workspaceFolder}/src"
    }
    ```

  #### 11. **`problemMatcher`**

  - **类型**：数组（字符串）

  - **描述**：用于识别任务输出中的问题，通常用于编译任务。它将自动从任务输出中提取错误、警告等，并显示在 `Problems` 面板中。

    常见问题匹配器：

    - `$gcc`：适用于 `gcc` 或 `g++` 编译器。
    - `$tsc`：适用于 TypeScript 编译器。
    - `$eslint-compact`：适用于 ESLint 检查。

  #### 12. **`isBackground`**

  - **类型**：布尔值

  - **描述**：如果设置为 `true`，表示该任务在后台执行，不会阻塞 VSCode 的其他操作。对于长时间运行的任务，建议设置为 `true`。

    示例：

    ```json
    "isBackground": true
    ```

  #### 13. **`runInBackground`**

  - **类型**：布尔值

  - **描述**：表示是否在后台运行任务。如果为 `true`，任务会在后台运行，不会阻塞 VSCode。

    示例：

    ```json
    "runInBackground": true
    ```
  
  #### 14. **`dependsOn`**
  
  - **类型**：数组（字符串）

  - **描述**：指定此任务依赖的其他任务。在执行当前任务之前，VSCode 会首先运行依赖的任务。

    示例：

    ```json
    "dependsOn": ["build", "lint"]
    ```

示例：构建 C++ 程序的 `tasks.json`

```json
{
    "version": "2.0.0",
    "tasks": [
        {
            "type": "cppbuild",
            "label": "C/C++: g++.exe 生成活动文件",
            "command": "C:\\path\\c_c++\\mingw64\\bin\\g++.exe",
            "args": [
                "-fdiagnostics-color=always",
                "-g",
                "${file}",
                "-o",
                "${fileDirname}\\${fileBasenameNoExtension}.exe"
            ],
            "options": {
                "cwd": "${fileDirname}"
            },
            "problemMatcher": [
                "$gcc"
            ],
            "group": {
                "kind": "build",
                "isDefault": true
            },
            "detail": "调试器生成的任务。"
        }
    ]
}
```

## `settings.json` — 设置配置

`settings.json` 用于配置工作区或全局的 VSCode 设置。它控制了编辑器、调试、主题、代码风格等的行为。`settings.json` 可以在全局和工作区层级设置。

### 主要内容

- **`editor.\*`**：控制编辑器行为（如字体、主题、缩进等）。
- **`files.\*`**：文件管理设置（如文件编码、自动保存等）。
- **`terminal.\*`**：终端设置（如字体、终端类型等）。
- **`debug.\*`**：调试设置（如调试窗口的行为）。

### 示例：`settings.json` 文件

```json
{
    "editor.fontSize": 14,
    "editor.tabSize": 4,
    "files.autoSave": "onWindowChange",
    "files.eol": "\n",
    "terminal.integrated.shell.windows": "C:\\Program Files\\Git\\bin\\bash.exe",
    "C_Cpp.intelliSenseEngine": "Default",
    "C_Cpp.autocomplete": "Default",
    "C_Cpp.errorSquiggles": "Enabled"
}
```

- `editor.fontSize`：设置编辑器的字体大小。
- `files.autoSave`：自动保存设置，`onWindowChange` 表示当窗口失去焦点时自动保存。
- `terminal.integrated.shell.windows`：指定 Windows 下集成终端使用的 shell。
- `C_Cpp.intelliSenseEngine`：设置 C++ IntelliSense 引擎（如默认的或 Clang）。
- `C_Cpp.errorSquiggles`：启用错误波浪线（如编译错误）。

## 总结

- **`launch.json`**：用于配置调试器的行为和调试会话，包括调试目标、传递给程序的参数等。
- **`tasks.json`**：用于配置执行任务（如编译、构建、清理等）的自动化流程。
- **`settings.json`**：用于配置 VSCode 编辑器的全局或工作区设置，如编辑器的行为、终端设置、代码风格等。
