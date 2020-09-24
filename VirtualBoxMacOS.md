# VirtualBox安装Mac OS Catalina

## 安装前准备

### Virtual Box

[下载页](https://www.virtualbox.org/wiki/Downloads)

### macOS Catalina 虚拟硬盘文件

[macOS Catalina Final by Geekrar](https://drive.google.com/drive/folders/1Odu1gu4cdMU0HNw8QztrR_fvYlmcNSLt)

### Boot Image 虚拟硬盘文件

[VirtualBox Boot Image by Geekrar](https://drive.google.com/drive/folders/1xcTmJOBc9dZQMySYgW_TNpjd2YY6WV0s)

> password: `Geekrar.com`

## 安装步骤

1. 新建虚拟机，版本选`Mac OS X (64-bit)`（没有10.15，直接选通用即可），
   内存8G，名称任意（记住这个名称，后面会用到），选择**使用已有的虚拟硬盘文件**，
   使用下载好的[macOS Catalina](#macos-catalina-虚拟硬盘文件)，点击创建
2. 设置虚拟机配置
   1. 系统->主板->芯片组，选择`PIIX3`
   2. 系统->处理器->处理器数量，选当前CPU核心数量的50%以上
   3. 显示->屏幕->显存大小，拉满
   4. 存储->存储介质->控制器：SATA->添加虚拟硬盘，选择[Boot Image](#boot-image-虚拟硬盘文件)
3. 关闭Virtual Box，新建bat，把下面代码复制到bat中，根据实际情况和需求修改3个变量，保存并双击运行
```bat
@echo off
REM VirtualBox路径
set VB_PATH="C:\Program Files\Oracle\VirtualBox"
REM 虚拟机名称
set VMDK_NAME="macOS Catalina"
REM 分辨率
set RESOLUTION=1600x900
pushd %VB_PATH%
VBoxManage.exe modifyvm %VMDK_NAME% --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "iMac11,3"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/efi/0/Config/DmiSystemVersion" "1.0"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Iloveapple"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
VBoxManage setextradata %VMDK_NAME% VBoxInternal2/EfiGraphicsResolution %RESOLUTION%
popd
pause
```
4. 打开虚拟机，可能报错打不开，尝试输入`install.nsh`并执行，之后即可正常进入系统

## 其它

1. 10.14及以上版本无法安装增强功能
2. `xcode-select install`安装失败，去[这里](https://developer.apple.com/downloads/index.action?name=for%20Xcode)下载自行安装
