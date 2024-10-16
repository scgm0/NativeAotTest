# 在 Linux 通过 wine 编译能在 Windows 使用的 C# Native Aot

使用的 Linux 发行版: [`Arch Linux`](https://archlinux.org/)

- [在 Linux 通过 wine 编译能在 Windows 使用的 C# Native Aot](#在linux通过wine编译能在windows使用的c-native-aot)
  - [方案一](#方案一)
  - [方案二（推荐）](#方案二推荐)

## 方案一

- 1.使用 AUR 助手[`yay`](https://github.com/Jguer/yay)或[`paru`](https://github.com/Morganamilo/paru)安装[`wine`](https://www.winehq.org/)和[`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine)

- 2.在[`.Net`](https://dotnet.microsoft.com)官网下载[`dotnet8`](https://dotnet.microsoft.com/zh-cn/download/dotnet/8.0)的`exe`安装程序，使用`wine`运行并安装

- 3.运行`wine regedit`修改注册表`HKEY_CURRENT_USER\Environment`添加以下环境变量：
  ```
  `/opt/msvc`是aur中`msvc-wine-git`的默认安装路径，如果安装在了其他路径请自行替换
  PATH: /opt/msvc/VC/Tools/MSVC/14.41.34120/bin/Hostx64/x64
  LIB: /opt/msvc/Windows Kits/10/Lib/10.0.22621.0/um/x64;Z:/opt/msvc/Windows Kits/10/Lib/10.0.22621.0/ucrt/x64;/opt/msvc/VC/Tools/MSVC/14.41.34120/lib/x64
  ```

- 4.设置项目属性`<IlcUseEnvironmentalTools>true</IlcUseEnvironmentalTools>`

- 5.在项目根目录运行`wine dotnet publish ./ -r win-x64 -c Release`触发编译

- 6.运行编译结果`wine ./bin/Release/net8.0/win-x64/publish/NativeAotTest.exe`，成功打印`Hello, World!`

## 方案二（推荐）
- 1.使用 AUR 助手[`yay`](https://github.com/Jguer/yay)或[`paru`](https://github.com/Morganamilo/paru)安装[`wine`](https://www.winehq.org/)和[`msvc-wine-git`](https://github.com/mstorsjo/msvc-wine)

- 2.在项目中引入`PublishAotCross.targets`(复制到其他项目中时，`PublishAotCross.targets`和`Crosscompile.targets`应该一起复制)
  ```
  (可选)
  如果`msvc-wine-git`安装目录不是`/opt/msvc`，请在项目中添加`MSVCWineBinPath`属性
  默认为<MSVCWineBinPath>/opt/msvc/bin</MSVCWineBinPath>
  ```

- 3.在项目根目录运行`dotnet publish ./ -r win-x64 -c Release`触发编译

- 4.运行编译结果`wine ./bin/Release/net8.0/win-x64/publish/NativeAotTest.exe`，成功打印`Hello, World!`