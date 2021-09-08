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

---

# VirtualBox安装Mac OS Monterey

## 开始前

准备好所需文件

- **VirtualBox**和**Extension Pack**，[下载地址](https://www.virtualbox.org/wiki/Downloads)
- **macOS Monterey.iso**，[下载地址](https://nodeninjas.net/download-monterey-iso/)
- VirtualBox Command File

iso下载回来可能是多个文件，需要用如下命令合并成一个
```bat
copy /b filename1+filename2+filename3+... filename
```

## 开始

1. 安装**VirtualBox**和**Extension Pack**
2. 新建虚拟机
   1. 选择高级模式
   2. 输入名称，如 **`macOS Monterey`**，记作 **Name** ，方便后续使用
   3. 类型选 **Mac OS X**，版本选 **Mac OS X (64-bit)**
   4. 内存最少选择**8G**，上不封顶
   5. 选择创建虚拟硬盘，点击创建
   6. 文件保存位置用默认，文件大小拉到**150GB**
   7. 文件类型选**VDI**，并动态分配，点击创建
3. 配置刚创建好的虚拟机
   1. 点击设置
   2. 选择**系统**，取消**软驱**勾选，并保证**扩展特性**全部勾选
   3. 切到**处理器**页签，**处理器数量**拉到绿色封顶
   4. 选择**显示**，**显存大小**拉满
   5. 选择**存储**，选中**没有盘片**，右侧点击**光盘图标**，选择准备好的**macOS Monterey.iso**
   6. 选择**USB设备**，选中**USB3.0 (xHCI) 控制器**
   7. 点击OK保存并关闭设置
4. 关闭VirtualBox，运行VirtualBox命令，其中`VB_PATH`是VirtualBox安装路径，`VMDK_NAME`是新建虚拟机流程时的 **Name**
```bat
@echo off
REM VirtualBox路径
set VB_PATH="C:\Program Files\Oracle\VirtualBox"
REM 虚拟机名称
set VMDK_NAME="macOS Monterey"
REM 分辨率
set RESOLUTION=1600x900
pushd %VB_PATH%
VBoxManage.exe modifyvm %VMDK_NAME% --cpuidset 00000001 000106e5 00100800 0098e3fd bfebfbff
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/efi/0/Config/DmiSystemProduct" "MacBookPro15,1"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/efi/0/Config/DmiBoardProduct" "Mac-551B86E5744E2388"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/smc/0/Config/DeviceKey" "ourhardworkbythesewordsguardedpleasedontsteal(c)AppleComputerInc"
VBoxManage setextradata %VMDK_NAME% "VBoxInternal/Devices/smc/0/Config/GetKeyFromRealSMC" 1
VBoxManage setextradata %VMDK_NAME% "VBoxInternal2/EfiGraphicsResolution" %RESOLUTION%
REM 如果是AMD CPU，需要取消下面这行的注释
REM VBoxManage modifyvm %VMDK_NAME% --cpu-profile "Intel Core i7-6700K"
popd
```
5. 启动虚拟机，开始安装
   1. 选择语言
   2. 选择**硬盘工具**，点击**继续**，选中**VBOX HARDDISK Media**，点击右上角**抹掉**
   3. 名称随意，格式选择**Mac OS扩展 (日志式)**，点击**抹掉**
   4. 完成后退出**硬盘工具**，选择**Install macOS 12 Beta**，点击**继续**
   5. 一路继续，完成安装
