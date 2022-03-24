[![Actions Status](https://github.com/filipdutescu/modern-cpp-template/workflows/MacOS/badge.svg)](https://github.com/filipdutescu/modern-cpp-template/actions)
[![Actions Status](https://github.com/filipdutescu/modern-cpp-template/workflows/Windows/badge.svg)](https://github.com/filipdutescu/modern-cpp-template/actions)
[![Actions Status](https://github.com/filipdutescu/modern-cpp-template/workflows/Ubuntu/badge.svg)](https://github.com/filipdutescu/modern-cpp-template/actions)
[![codecov](https://codecov.io/gh/filipdutescu/modern-cpp-template/branch/master/graph/badge.svg)](https://codecov.io/gh/filipdutescu/modern-cpp-template)
[![GitHub release (latest by date)](https://img.shields.io/github/v/release/filipdutescu/modern-cpp-template)](https://github.com/filipdutescu/modern-cpp-template/releases)



这是官方readme.md的中文概括，带有个人理解，而且不完整。

若要查看完整的请看原工程：https://github.com/filipdutescu/modern-cpp-template

# Modern C++ Template

这是一个**现代C++的工程模板**，帮助我们快速的启动一个项目的开发。



## 特性(Features)

* 使用现代CMake配置与工程
* Clang-Format配置示例
* 集成了静态分析器，包括Clang-Tidy（默认）与Cppcheck
* 支持Doxygen，通过`ENABLE_DOXYGEN`打开
* 支持单元测试，包括GoogleTest（默认，并带有启用`GoogleMock`的选项）或Catch2
* 支持Code coverage，通过`ENABLE_CODE_COVERAGE`启用
* 包管理器，包括Conan与Vcpkg
* 使用GitHub Actions的Windows、Linux与MacOS的CI工作流，利于缓存功能，以确保最短的运行时间
* `.md`模板，包含自述文件、贡献指南、问题与拉取请求
* 允许你很方便的提供Permissive license，该模板在[Unlicense](https://unlicense.org/)下获得许可
* 既可构建为静态库，又可构建为动态库，甚至可构建为仅头文件的库，还可以构建为可执行文件
* 集成了Ccache，用于加速rebuild的时间



## 开始(Getting Started)

- CMake v3.15+
- C++ Compiler - needs to support at least the C++17 standard, i.e. MSVC, GCC, Clang



### 先决条件(Prerequisites)

- CMake v3.15+
- C++ Compiler - needs to support at least the C++17 standard, i.e. MSVC, GCC, Clang



### 初始化(Installing)

主要工作就是改工程名，工程名散乱在各个角落，你需要一个一个修改

1. 拉取工程：`git clone https://github.com/filipdutescu/modern-cpp-template/`

2. 在`include/`目录下创建一个新文件夹，如`include/<你的工程名>`

3. 编辑`cmake/SourcesAndHeaders.cmake`添加你的文件

4. 对`cmake/ProjectConfig.cmake.in`进行重命名，如`cmake/<你的工程名>Config.cmake.in`

5. 修改Github工作流，特别是`.github/workflows/ubuntu.yml`
   1. 将CMake选项`-DProject_ENABLE_CODE_COVERAGE=1`替换为`-D<你的工程名>_ENABLE_CODE_COVERAGE=1`
   
6. 最后，修改`CMakeLists.txt`
   1. 将`project("Project" ...)`改成`project(<你的工程名> ...)`
   
   

### 构建项目(Building the project)

```bash
mkdir build && cd build
cmake .. -DCMAKE_INSTALL_PREFIX=D:/install
cmake --build . --target install
```

注意：如果您希望安装在 [默认安装位置](https://cmake.org/cmake/help/latest/module/GNUInstallDirs.html)，可以省略自定义。CMAKE_INSTALL_PREFIX



更多选项，可以在文件[cmake/StandardSettings.cmake](https://github.com/filipdutescu/modern-cpp-template/blob/master/cmake/StandardSettings.cmake)中进行设置。对于某些选项，可能需要在其各自的`*cmake`文件中进行配置

- 如，conan的相关设置在`cmake/Conan.cmake`中。需要`CONAN_REQUIRES`并且可能需要`CONAN_OPTIONS`设置才能使其正常工作



### 生成文档

为了为项目生成文档，您需要将构建配置为使用Doxygen

```bash
mkdir build/ && cd build/
cmake .. -D<project_name>_ENABLE_DOXYGEN=1 -DCMAKE_INSTALL_PREFIX=/absolute/path/to/custom/install/directory
cmake --build . --target doxygen-docs
```

注意： 这将在项目的根目录中生成一个`docs/`目录



### 运行测试

默认情况下使用Google Test进行单元测试。如要使用构建目录中的CTest，传递运行测试所需的配置

```bash
cd build          # if not in the build directory already
ctest -C Release  # or `ctest -C Debug` or any other configuration you wish to test

# you can also run tests with the `-VV` flag for a more verbose output (i.e.
#GoogleTest output as well)
```



如何关闭单元测试？

- `ENABLE_UNIT_TESTING`设置为false（在[cmake/StandardSettings.cmake](https://github.com/filipdutescu/modern-cpp-template/blob/master/cmake/StandardSettings.cmake)中）



### 安装

若要安装已构建的项目，您需要`install`，如

```bash
cmake --build build --target install --config Release

# a more general syntax for that command is:
cmake --build <build_directory> --target install --config <desired_config>
```


# 修改记录

根据自身需要修改项目。

1. 启用conan，弃用Vcpkg
2. 使用conan下载google test
3. 为示例代码适配Windows
4. 为测试工程加Visual Studio中的折叠目录


# 使用示例

```
mkdir build & cd build
cmake .. -G "Visual Studio 16" -DCMAKE_INSTALL_PREFIX=D:/install/project
```