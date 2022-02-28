# 在Visual Studio Code中使用CMake开发STM32

> 提示：使用该模板需要开发者对编译流程有一定了解

## 工具链准备

本模板默认使用以下工具：
1. [gcc-arm-none-eabi](https://developer.arm.com/tools-and-software/open-source-software/developer-tools/gnu-toolchain/gnu-rm/downloads)：编译器
2. [CMake](https://cmake.org/download/)、[Ninja](https://ninja-build.org)：构建工具
3. [OpenOCD](https://openocd.org/pages/getting-openocd.html)：烧录器、调试器

点击上述各个链接即可跳转至其下载页面；如果使用Linux系统请自行搜索选择安装方法。

此外，将其上各个工具的二进制文件所在目录加入系统环境。可用下述命令测试：

```bash
arm-none-eabi-gcc -v
cmake --version
ninja --version
openocd -v
```

## 扩展安装

本模板推荐使用以下扩展：
1. ms-vscode.cpptools：C/C++语言支持
2. twxs.cmake：CMakeLists语言支持
3. ms-vscode.cmake-tools：CMake工具支持
4. marus25.cortex-debug：调试工具

## 使用方法

1. 修改CMakeLists.txt中源文件、头文件路径以及链接文件和启动文件
2. 修改CMakeLists.txt中全局宏定义
3. 修改.vscode/launch.json中OpenOCD的配置文件
4. 在ST官网上下载对应芯片型号的svd文件，并修改.vscode/launch.json中的svd文件名

## 使用注意

1. cmake-tools暂不支持嵌入式设备的烧录和调试，因此仅使用其方便编译，烧录和调试使用Cortex-Debug扩展
2. 可使用Release等模式编译以优化代码、减小二进制文件体积；但是请充分测试，不保证嵌入式平台上的编译器优化完全无误
3. 当修改CMakeLists.txt中目标名称时，需要同时修改.vscode/launch.json中Cortex-Debug的可执行文件路径
