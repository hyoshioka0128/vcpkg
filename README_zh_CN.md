# Vcpkg: 总览

[English Overview](README.md)

Vcpkg 可帮助您在 Windows、 Linux 和 MacOS 上管理 C 和 C++ 库。
这个工具和生态链正在不断发展，我们一直期待您的贡献！

若您从未使用过vcpkg，或者您正在尝试了解如何使用vcpkg，请查阅 [入门](#入门) 章节。

如需获取有关可用命令的简短描述，请在编译vcpkg后执行 `vcpkg help` 或执行 `vcpkg help [command]` 来获取具体的帮助信息。

* GitHub: [https://github.com/microsoft/vcpkg](https://github.com/microsoft/vcpkg)
* Slack: [https://cppalliance.org/slack/](https://cppalliance.org/slack/)， #vcpkg 频道
* Discord: [\#include \<C++\>](https://www.includecpp.org)， #🌏vcpkg 频道
* 文档: [Documentation](docs/README.md)

[![当前生成状态](https://dev.azure.com/vcpkg/public/_apis/build/status/microsoft.vcpkg.ci?branchName=master)](https://dev.azure.com/vcpkg/public/_build/latest?definitionId=29&branchName=master)

# 目录

- [Vcpkg: 总览](#vcpkg-总览)
- [目录](#目录)
- [入门](#入门)
  - [快速开始: Windows](#快速开始-windows)
  - [快速开始: Unix](#快速开始-unix)
  - [安装 Linux Developer Tools](#安装-linux-developer-tools)
  - [安装 macOS Developer Tools](#安装-macos-developer-tools)
  - [在 CMake 中使用 vcpkg](#在-cmake-中使用-vcpkg)
    - [Visual Studio Code 中的 CMake Tools](#visual-studio-code-中的-cmake-tools)
    - [Visual Studio CMake 工程中使用 vcpkg](#visual-studio-cmake-工程中使用-vcpkg)
    - [CLion 中使用 vcpkg](#clion-中使用-vcpkg)
    - [将 vcpkg 作为一个子模块](#将-vcpkg-作为一个子模块)
- [Tab补全/自动补全](#tab补全自动补全)
  - [示例](#示例)
  - [贡献](#贡献)
- [开源协议](#开源协议)
- [数据收集](#数据收集)

# 入门

首先，请阅读以下任一快速入门指南：
[Windows](#快速开始-windows) 或 [macOS和Linux](#快速开始-unix)，
这取决于您使用的是什么平台。

有关更多信息，请参见 [安装和使用软件包][getting-started:using-a-package]。
如果vcpkg目录中没有您需要的库，
您可以 [在GitHub上打开问题][contributing:submit-issue]。
vcpkg团队和贡献者可以看到它的地方，
并可能将这个库添加到vcpkg。

安装并运行vcpkg后，
您可能希望将 [TAB补全](#tab补全自动补全) 添加到您的Shell中。

最后，如果您对vcpkg的未来感兴趣，请查看 [清单][getting-started:manifest-spec]！
这是一项实验性功能，可能会出现错误。
因此，请尝试一下并[打开所有问题][contributing:submit-issue]!

## 快速开始: Windows

前置条件:
- Windows 7 或更新的版本
- [Git][getting-started:git]
- [Visual Studio 2015 Update 3][getting-started:visual-studio] 或更新的版本（**包含英文语言包**）

首先，**请使用git clone vcpkg** 并执行 bootstrap.bat 脚本。
您可以将vcpkg安装在任何地方，但是通常我们建议您使用 vcpkg 作为 CMake 项目的子模块，并将其全局安装到 Visual Studio 项目中。
我们建议您使用例如 `C:\src\vcpkg` 或 `C:\dev\vcpkg` 的安装目录，否则您可能遇到某些库构建系统的路径问题。

```cmd
> git clone https://github.com/microsoft/vcpkg
> .\vcpkg\bootstrap-vcpkg.bat
```

使用以下命令安装您的项目所需要的库：

```cmd
> .\vcpkg\vcpkg install [packages to install]
```

请注意: vcpkg在Windows中默认编译并安装x86版本的库。 若要编译并安装x64版本，请执行:

```cmd
> .\vcpkg\vcpkg install [package name]:x64-windows
```

或

```cmd
> .\vcpkg\vcpkg install [packages to install] --triplet=x64-windows
```

您也可以使用 `search` 子命令来查找vcpkg中集成的库:

```cmd
> .\vcpkg\vcpkg search [search term]
```

若您希望在 Visual Studio 中使用vcpkg，请运行以下命令 (可能需要管理员权限)

```cmd
> .\vcpkg\vcpkg integrate install
```

在此之后，您可以创建一个非cmake项目 (或打开已有的项目)。
在您的项目中，所有已安装的库均可立即使用 `#include` 包含您需使用的库的头文件且无需额外配置。

若您在 Visual Studio 中使用cmake工程，请查阅[这里](#visual-studio-cmake-工程中使用-vcpkg)。

为了在IDE以外在cmake中使用vcpkg，您需要使用以下工具链文件:

```cmd
> cmake -B [build directory] -S . "-DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake"
> cmake --build [build directory]
```

在cmake中，您仍需通过 `find_package` 来使用vcpkg中已安装的库。
请查阅 [CMake 章节](#在-cmake-中使用-vcpkg) 获取更多信息，其中包含了在IDE中使用cmake的内容。

对于其他工具 (包括Visual Studio Code)，请查阅 [集成指南][getting-started:integration]。

## 快速开始: Unix

Linux平台前置条件:
- [Git][getting-started:git]
- [g++][getting-started:linux-gcc] >= 6

macOS平台前置条件:
- [Apple Developer Tools][getting-started:macos-dev-tools]

首先，**请使用git clone vcpkg** 并执行 bootstrap.sh 脚本。
我们建议您将vcpkg作为cmake项目的子模块使用。

```sh
$ git clone https://github.com/microsoft/vcpkg
$ ./vcpkg/bootstrap-vcpkg.sh
```

使用以下命令安装任意包：

```sh
$ ./vcpkg/vcpkg install [packages to install]
```

您也可以使用 `search` 子命令来查找vcpkg中已集成的库:

```sh
$ ./vcpkg/vcpkg search [search term]
```

为了在cmake中使用vcpkg，您需要使用以下工具链文件:

```sh
$ cmake -B [build directory] -S . "-DCMAKE_TOOLCHAIN_FILE=[path to vcpkg]/scripts/buildsystems/vcpkg.cmake"
$ cmake --build [build directory]
```

在cmake中，您仍需通过 `find_package` 来使用vcpkg中已安装的库。
为了您更好的在cmake或 VSCode CMake Tools 中使用vcpkg，
请查阅 [CMake 章节](#在-cmake-中使用-vcpkg) 获取更多信息，
其中包含了在IDE中使用cmake的内容。

对于其他工具，请查阅 [集成指南][getting-started:integration]。

## 安装 Linux Developer Tools

在Linux的不同发行版中，您需要安装不同的工具包:

- Debian，Ubuntu，popOS或其他基于 Debian 的发行版:

```sh
$ sudo apt-get update
$ sudo apt-get install build-essential tar curl zip unzip
```

- CentOS

```sh
$ sudo yum install centos-release-scl
$ sudo yum install devtoolset-7
$ scl enable devtoolset-7 bash
```

对于其他的发行版，请确保已安装 g++ 6 或更新的版本。
若您希望添加特定发行版的说明，[请提交一个 PR][contributing:submit-pr]!

## 安装 macOS Developer Tools

在 macOS 中，您唯一需要做的是在终端中运行以下命令:

```sh
$ xcode-select --install
```

然后按照出现的窗口中的提示进行操作。
此时，您就可以使用 bootstrap.sh 编译vcpkg了。 请参阅 [快速开始](#快速开始-unix)

## 在 CMake 中使用 vcpkg

若您希望在CMake中使用vcpkg，以下内容可能帮助您：

### Visual Studio Code 中的 CMake Tools

将以下内容添加到您的工作区的 `settings.json` 中将使CMake Tools自动使用vcpkg中的第三方库:

```json
{
  "cmake.configureSettings": {
    "CMAKE_TOOLCHAIN_FILE": "[vcpkg root]/scripts/buildsystems/vcpkg.cmake"
  }
}
```

### Visual Studio CMake 工程中使用 vcpkg

打开CMake设置选项，将 vcpkg toolchain 文件路径在 `CMake toolchain file` 中：

```
[vcpkg root]/scripts/buildsystems/vcpkg.cmake
```

### CLion 中使用 vcpkg

打开 Toolchains 设置
(File > Settings on Windows and Linux, CLion > Preferences on macOS)，
并打开 CMake 设置 (Build, Execution, Deployment > CMake)。
最后在 `CMake options` 中添加以下行:

```
-DCMAKE_TOOLCHAIN_FILE=[vcpkg root]/scripts/buildsystems/vcpkg.cmake
```

遗憾的是，您必须手动将此选项加入每个项目配置文件中。

### 将 vcpkg 作为一个子模块

当您希望将vcpkg作为一个子模块加入到您的工程中时，
您可以在第一个 `project()` 调用之前将以下内容添加到 CMakeLists.txt 中，
而无需将 `CMAKE_TOOLCHAIN_FILE` 传递给cmake调用。

```cmake
set(CMAKE_TOOLCHAIN_FILE "${CMAKE_CURRENT_SOURCE_DIR}/vcpkg/scripts/buildsystems/vcpkg.cmake"
  CACHE STRING "Vcpkg toolchain file")
```

使用此种方式可无需设置 `CMAKE_TOOLCHAIN_FILE` 即可使用vcpkg，且更容易完成配置工作。

[getting-started:using-a-package]: docs/examples/installing-and-using-packages.md
[getting-started:integration]: docs/users/buildsystems/integration.md
[getting-started:git]: https://git-scm.com/downloads
[getting-started:cmake-tools]: https://marketplace.visualstudio.com/items?itemName=ms-vscode.cmake-tools
[getting-started:linux-gcc]: #installing-linux-developer-tools
[getting-started:macos-dev-tools]: #installing-macos-developer-tools
[getting-started:macos-brew]: #installing-gcc-on-macos
[getting-started:macos-gcc]: #installing-gcc-on-macos
[getting-started:visual-studio]: https://visualstudio.microsoft.com/
[getting-started:manifest-spec]: docs/specifications/manifests.md

# Tab补全/自动补全

`vcpkg` 支持命令，包名称，以及 Powershell 和 Bash 中的选项。
若您需要在指定的 shell 中启用Tab补全功能，请依据您使用的shell运行：

```pwsh
> .\vcpkg integrate powershell
```

或

```sh
$ ./vcpkg integrate bash # 或 zsh
```

然后重新启动控制台。

## 示例

请查看 [文档](docs/README.md) 获取具体示例，
其包含 [安装并使用包](docs/examples/installing-and-using-packages.md)，
[使用压缩文件添加包](docs/examples/packaging-zipfiles.md)
和 [从GitHub源中添加一个包](docs/examples/packaging-github-repos.md)。

我们的文档现在也可以从 [vcpkg.io](https://vcpkg.io/) 在线获取。
我们真诚的希望您向我们提出关于此网站的任何建议! 请在[这里](https://github.com/vcpkg/vcpkg.github.io/issues) 打开issue.

观看4分钟 [demo视频](https://www.youtube.com/watch?v=y41WFKbQFTw)。

## 贡献

Vcpkg是一个开源项目，并通过您的贡献不断发展。
下面是一些您可以贡献的方式:

* [提交一个关于vcpkg或已支持包的新issue][contributing:submit-issue]
* [提交修复PR和创建新包][contributing:submit-pr]

请参阅我们的 [贡献准则](CONTRIBUTING_zh.md) 了解更多详细信息。

该项目采用了 [Microsoft开源行为准则][contributing:coc]。
获取更多信息请查看 [行为准则FAQ][contributing:coc-faq] 或联系 [opencode@microsoft.com](mailto:opencode@microsoft.com) 提出其他问题或意见。

[contributing:submit-issue]: https://github.com/microsoft/vcpkg/issues/new/choose
[contributing:submit-pr]: https://github.com/microsoft/vcpkg/pulls
[contributing:coc]: https://opensource.microsoft.com/codeofconduct/
[contributing:coc-faq]: https://opensource.microsoft.com/codeofconduct/

# 开源协议

在此存储库中使用的代码均遵循 [MIT License](LICENSE.txt)。

# 数据收集

vcpkg会收集使用情况数据，以帮助我们改善您的体验。
Microsoft收集的数据是匿名的。
您也可以通过以下步骤禁用数据收集：
- 将选项 `-disableMetrics` 传递给 bootstrap-vcpkg 脚本并重新运行此脚本
- 向 vcpkg 命令传递选项 `--disable-metrics`
- 设置环境变量 `VCPKG_DISABLE_METRICS`

请在 [privacy.md](docs/about/privacy.md) 中了解有关 vcpkg 数据收集的更多信息。
