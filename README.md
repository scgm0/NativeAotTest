# 在Linux通过wine编译能在Windows使用的C# Native Aot


使用的Linux发行版: [`Arch Linux`](https://archlinux.org/)

1.使用AUR助手[`yay`](https://github.com/Jguer/yay)或[`paru`](https://github.com/Morganamilo/paru)安装[`wine`](https://www.winehq.org/)和[`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine)

2.在[`.Net`](https://dotnet.microsoft.com)官网下载[`dotnet9`](https://dotnet.microsoft.com/zh-cn/download/dotnet/9.0)的`exe`安装程序，使用`wine`运行并安装

3.运行`wine regedit`修改注册表`HKEY_CURRENT_USER\Environment`添加以下环境变量：
```
PATH: /opt/msvc/VC/Tools/MSVC/14.41.34120/bin/Hostx64/x64
LIB: /opt/msvc/Windows Kits/10/Lib/10.0.22621.0/um/x64;Z:/opt/msvc/Windows Kits/10/Lib/10.0.22621.0/ucrt/x64;/opt/msvc/VC/Tools/MSVC/14.41.34120/lib/x64
```
4.在项目根目录运行`wine dotnet publish ./ -r win-x64`触发编译

5.运行编译结果`wine ./bin/Release/net9.0/win-x64/publish/NativeAotTest.exe`，成功打印`Hello, World!`
