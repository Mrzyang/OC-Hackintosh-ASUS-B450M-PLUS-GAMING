## OpenCore for B450M-PLUS-GAMING mainboard



Support MacOS 10.15.7 Catalina, newer version not tested

online download macos image

edit time: 2022-09-26



### 固件版本 / Firmware version
- [x] Mainboard bios Version: 3802, updated on 2022/05/12, 10.81 MBytes 
- [x] MacOS 版本 / macOS Version
    - [x] 10.15.7 Catalina 
- [x] OpenCore Version: 0.6.7
### 完美驱动 / Perfectly Drived


- [x] CPU
    - [x] Ryzen r5 2600x(锐龙cpu理论上不限制，后续有升级到R5 5600x的打算)
- [x] 显卡 / GPU
    - [x] NVIDIA GT710 2G 铭瑄
- [x] 声卡 / Audio Card
    - [x] Realtek ALC887
- [x] 睡眠&唤醒 / Sleep&Wake Up
- [x] 有线网卡 / Ethernet
    - [x] Intel(R) Wi-Fi 6 AX200 160MHz
- [x] 所有 USB 插口 / All of USB


### 未正常驱动 / Not Drived
- [x] 蓝牙 /  Bluetooth
	 -  [x]  Intel(R) Wi-Fi 6 AX200 160MHz自带的(
IntelBluetoothFirmware无效，极低概率可传输文件成功)

### 引导安装U盘目录结构 / USB Installer Directory Structure
<pre>
├─com.apple.recovery.boot
│      BaseSystem.chunklist
│      BaseSystem.dmg
└─EFI
    ├─BOOT
    │      BOOTx64.efi
    └─OC
        │  config.plist
        │  OpenCore.efi
        ├─ACPI
        ├─Drivers
        └─Kexts
</pre>


### win&mac双系统单硬盘引导ESP（FAT32）分区目录结构
<pre>
└─EFI
    ├─BOOT
    │      BOOTx64.efi
    │
    ├─Microsoft
    │  ├─Boot
    │  └─Recovery
    └─OC
        │  config.plist
        │  OpenCore.efi
        ├─ACPI
        ├─Drivers
        └─Kexts
</pre>

### 采坑
1. U盘启动及OC顺利引导至macos安装界面后，若机箱后面网卡灯亮了，代表有线网卡驱动成功了，如果安装过程中在线下载镜像报无法连接到服务器等网络错误，极大可能是时间错误导致的，苹果默认用UTC时间，北京时间（东八区UTC+8）减8小时即得UTC时间。所以调起终端，date命令修改时间即可。 格式  date MMddhhmmYY
2. 开源驱动并不是最新版本的就最好，我的ALC887声卡在AppleALC 1.60以上版本就没法驱动
3. 最好备两块固态硬盘，win10和macos各用一块
4. 如果只有一块硬盘，建议先把硬盘重新分区（必须再磁盘头部建立至少300M的ESP分区用来放EFI引导文件，分区格式为FAT32），清除掉所有数据（如果不重新分区，可能在安装第二个系统时就会报各种奇怪的错误，无论是windows后装还是macos后装，报错原因都是硬盘或分区数据不干净，这个问题我排查了一天，终于解决，人都麻了，因为回想到2017年用clover引导在笔记本上装黑苹果也出现同样的错误，不过当时挺可悲，最后把固态和主板都报废了）。一定得是win10先装，macos后装，因为win10后装会在开机设备初始化后报错卡住，导致装不上
5. Intel AX200 WIFI成功驱动了，附带的蓝牙没法驱动，换了几个版本IntelBluetoothFirmware都没法成功，目前最新版v2.2.0打上后，开机还会卡苹果logo，奇了怪了，所以，我放弃了，蓝牙对台式机作用不大
6. win10&macos双系统时间不同步解决办法，win10上以管理员命令行执行 Reg add HKLM\SYSTEM\CurrentControlSet\Control\TimeZoneInformation /v RealTimeIsUniversal /t REG_DWORD /d 1， 重启电脑

### 提示
1. 双系统引导需要添加两条EFI记录，分别是OpenCore和windows boot manager（win10装好一般就存在了，所以只需要手动添加一条OC的），推荐用easyEFI或者BOOTICE，分别指向\EFI\OC\OpenCore.efi和\EFI\Microsoft\Boot\bootmgfw.efi， 并把OC引导放前面（优先）
2. opencore_macos.CMO与opencore_macos_setting.txt是该主板调整好的bios设置，开机按F2然后加载就完了
### 参考链接 / Reference Link
- [Dortania's OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/prerequisites.html)
- [
MSI-B450m-MORTAR-Hackintosh](https://github.com/heyxiaobai/MSI-B450m-MORTAR-Hackintosh)

