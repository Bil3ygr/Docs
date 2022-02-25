# 常用软件和下载地址

## 开发

|Name|Download Url|
|-|-|
|Visual Studio Code|[Win64下载地址](https://code.visualstudio.com/sha/download?build=stable&os=win32-x64)，或[点此](https://code.visualstudio.com/download#)自行选择版本|
|Visual Studio 2019|[Visual Studio Installer](https://aka.ms/vs/17/release/vs_community.exe)|
|Python|[下载地址](https://www.python.org/downloads/)|
|Git|[下载地址](https://git-scm.com/download/win)|
|Svn|[下载地址](https://tortoisesvn.net/downloads.html)|

## 工具

|Name|Download Url|
|-|-|
|StEx|[Github Release](https://github.com/stefankueng/tools/releases)|
|grepWin|[Github Release](https://github.com/stefankueng/grepWin/releases)|
|Everything|[Win64下载地址](https://www.voidtools.com/Everything-1.4.1.1009.x64-Setup.exe)，或[点此](https://www.voidtools.com/zh-cn/downloads/)自行选择版本|
|GnuWin32|[sourceforge](https://sourceforge.net/projects/getgnuwin32/files/)|
|DeskPins|[下载地址](https://efotinis.neocities.org/downloads/DeskPins-1.32-setup.exe)|
|7-Zip|[下载地址](https://www.7-zip.org/)|

## Windows

|Name|Download Url|
|-|-|
|TranslucentTB|[Github](https://github.com/TranslucentTB/TranslucentTB/releases)|
|WindowsTerminal|[Github](https://github.com/microsoft/terminal/releases)|
|PowerToys|[Github](https://github.com/microsoft/PowerToys/releases)|

## 其它

|Name|Download Url|
|-|-|
|Oracle VM VirtualBox|[下载地址](https://www.virtualbox.org/wiki/Downloads)|
|VNC Viewer|[下载地址](https://www.realvnc.com/en/connect/download/viewer/)|
|Wechat|[下载地址](https://windows.weixin.qq.com/)|
|Typora|[Windows](https://typora.io/#windows)，或[点此](https://typora.io/#)选择其它版本（1.0版本开始收费，可下载试用版）|
|OBS Studio|[下载地址](https://obsproject.com/download)|

## 操作系统

Windows11安装
1. 虚拟机安装可能会出现硬件不支持
  1. 在安装界面按`shift+F10`弹出命令行，输入`regedit`打开注册表
  2. 打开路径`HKEY_LOCAL_MACHINE\SYSTEM\Setup`，右键`Setup`，选新建**项**，输入`LabConfig`
  3. 选中`LabConfig`，在右边空白处新增两个`DWORD(32位)`，名字分别是`BypassTPMCheck`和`BypassSecureBootCheck`，值改成1，关闭注册表，不要重启，继续安装流程
2. 激活
  1. 管理员方式运行命令行
  2. `slmgr -skms kms.03k.org`，更改kms服务器地址
  3. `slmgr -upk`，卸载激活密钥
  4. `slmgr -ipk W269N-WFGWX-YVC9B-4J6C9-T83GX`，注册激活密钥
  5. `slmgr -ato`，激活
