# 在 Linux 通过 lld 与 msvc-wine 编译 Windows C# Native AOT

本文档介绍了如何在 Arch Linux 环境下，使用 [`lld`](https://lld.llvm.org) 和 [`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine) 交叉编译 C# Native AOT 项目，使其能够在 Windows 上运行。

**使用的 Linux 发行版:** [Arch Linux](https://archlinux.org/)

## 具体步骤

### 1. 安装依赖

使用 AUR 助手 (例如 [`yay`](https://github.com/Jguer/yay) 或 [`paru`](https://github.com/Morganamilo/paru)) 安装 [`lld`](https://lld.llvm.org) 和 [`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine)。这一步是为了确保系统中存在 `lld-link` 命令以及 `Windows SDK`。

```bash
# 使用 yay
yay -S lld msvc-wine-git

# 或者使用 paru
paru -S lld msvc-wine-git
```

> **注意**
>
> `msvc-wine-git` 的默认安装目录是 `/opt/msvc`。如果您的安装目录不同，请在 `.csproj` 项目文件中添加 `MSVCWineBinPath` 属性来指定正确的路径，例如：
> ```xml
> <PropertyGroup>
>   <MSVCWineBinPath>/path/to/your/msvc/bin</MSVCWineBinPath>
> </PropertyGroup>
> ```

### 2. 在项目中引入构建脚本

在 `.csproj` 文件中引入 [`PublishAotCross.LinuxToWin`](https://www.nuget.org/packages/PublishAotCross.LinuxToWin) 以启用交叉编译功能。

### 3. 执行编译

在您的项目根目录打开终端，运行以下命令来为 Windows x64 平台发布 Release 版本的应用：

```bash
dotnet publish ./ -r win-x64 -c Release
```

### 4. 验证编译结果

编译成功后，可以使用 [`wine`](https://www.winehq.org) 运行生成的可执行文件，验证程序是否能正常工作。

```bash
wine ./bin/Release/net8.0/win-x64/publish/YourProjectName.exe
```

如果一切顺利，您应该能看到程序成功打印出 `Hello, World!` 或其他预期的输出。


# Compiling Windows C# Native AOT on Linux using lld and msvc-wine

This document describes how to cross-compile a C# Native AOT project on Arch Linux using [`lld`](https://lld.llvm.org) and [`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine) to run on Windows.

**Linux Distribution Used:** [Arch Linux](https://archlinux.org/)

## Steps

### 1. Install Dependencies

Use an AUR helper (like [`yay`](https://github.com/Jguer/yay) or [`paru`](https://github.com/Morganamilo/paru)) to install [`lld`](https://lld.llvm.org) and [`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine). This step ensures that the `lld-link` command and the `Windows SDK` are available on your system.

```bash
# Using yay
yay -S lld msvc-wine-git

# Or using paru
paru -S lld msvc-wine-git
```

> **Note**
>
> The default installation directory for `msvc-wine-git` is `/opt/msvc`. If your installation path is different, please add the `MSVCWineBinPath` property in your `.csproj` project file to specify the correct path, for example:
> ```xml
> <PropertyGroup>
>   <MSVCWineBinPath>/path/to/your/msvc/bin</MSVCWineBinPath>
> </PropertyGroup>
> ```

### 2. Include Build Scripts in Your Project

Import [`PublishAotCross.LinuxToWin`](https://www.nuget.org/packages/PublishAotCross.LinuxToWin) in your `.csproj` file to enable the cross-compilation functionality.

### 3. Compile the Project

Open a terminal in your project's root directory and run the following command to publish a Release version of your application for the Windows x64 platform:

```bash
dotnet publish ./ -r win-x64 -c Release
```

### 4. Verify the Compilation Result

After a successful compilation, you can use [`wine`](https://www.winehq.org) to run the generated executable and verify that the program works correctly.

```bash
wine ./bin/Release/net8.0/win-x64/publish/YourProjectName.exe
```

If everything is successful, you should see the program print `Hello, World!` or other expected output.