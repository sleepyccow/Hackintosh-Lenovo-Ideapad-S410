## Hackintosh-Lenovo-Ideapad-S410

⚠️ 不熟悉的朋友，请先熟读 [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/) 然后再动手。



## S410硬件配置

| 项目 | 明细 |
| :------:| :------| 
|CPU|Intel Core i5-4210U @ 1.70GHz|
|芯片组| Intel Lynx Point-LP, Intel Haswell |
|核显| Intel HD Graphics 4400 |
|音频| Realtek ALC233 |
|内存| DDR3 1600 MHz 4GB|
|硬盘| 杂牌 SSD 256G|
|有线网络| Realtek RTL8139 百兆|
|无线网络| COMFAST USB 无线网卡（板载无线有问题）|




## 软件配置

* 引导: OpenCore 0.7.8
* 黑果系统: macOS Catalina 10.15.7 (MacBookAir6,2)


## 正常工作的功能
* 声卡 Realtek ALC233：VoodooHDA 不用patch了，直接驱动，支持耳机
* 核显 HD4400：HD4400仿冒HD4600
* 有线网卡 Realtek RTL8139：OC中配置好，即完美可用
* 无线 COMFAST USB网卡：下载并安装驱动即可
* UEFI：通过 Clover/OC 启动
* 内置键盘以及数字键盘
* 原生USB3.0/USB2.0：最好定制下
* 内置摄像头
* 原生电源管理
* 电池状态
* 背光控制
* CPU变频
* 触控板 （全系支持全手势）





## 不能正常工作的功能
* HDMI：为处理登录卡顿的问题，视频端口屏蔽除内置显示接口外的所有接口
* 板载无线蓝牙：虽然可以驱动，但是速度只有1m，顾放弃
* 睡眠唤醒：用不到所以没配置
* iMessage/FaceTime：没有免驱蓝牙，搞不了


## 具体配置详情

### 核显 Intel HD 4400
首先是仿冒HD4600，其次就是屏蔽不用的接口，防止开机卡顿。

具体参数如下：

| 项目 | 内容 |数据类型|
| :------:| :------:| :------:|
|AAPL,ig-platform-id|0600260A|DATA|
|device-id| 12040000 |DATA|
|disable-external-gpu| 01000000 |DATA|
|framebuffer-con1-enable| 01000000 |DATA|
|framebuffer-con1-index|FFFFFFFF|DATA|
|framebuffer-con2-enable| 01000000 |DATA|
|framebuffer-con2-index|FFFFFFFF|DATA|
|framebuffer-fbmem|00009000|DATA|
|framebuffer-patch-enable|01000000|DATA|
|framebuffer-portcount|01000000|DATA|
|framebuffer-stolenmem|00000004|DATA|
|framebuffer-unifiedmem|00000060|DATA|


### 声卡

配置参数:

* 必要驱动 ：VoodooHDA.kext

OC开机正常加载就好，没有任何问题。





### 有线网卡
 

配置参数：

* 必要驱动：RealtekRTL8100.kext

OC开机正常加载就好，没有任何问题。


### 无线网卡和蓝牙

因为板载的WIFI安装后速度不够理想（只有1M用毛），蓝牙也是间歇性抽风。故放弃。
网购COMFAST的USB无线，安装国外大大的驱动即可。` Wireless USB Adapter Driver.pkg `


### USB定制

因为系统版本是10.15.7，所以USB可以不用定制。
当然定制也是极好的。






### EFI

#### SSDTs

可以从这里找 [Dortania's ACPI Guide](https://dortania.github.io/Getting-Started-With-ACPI/),可以直接用编译好的。

* SSDT-ALS0.aml
* SSDT-EC-LAPTOP.aml
* SSDT-PLUG-DRTNIA.aml
* SSDT-PMCR.aml
* SSDT-PNLF.aml
* SSDT-XOSI.aml


#### Kexts

* Lilu.kext `1.6.2`
* VirtualSMC.kext `1.3.0`
* SMCBatteryManager.kext `1.3.0`
* SMCLightSensor.kext `1.3.0`
* SMCProcessor.kext `1.2.9`
* SMCSuperIO.kext `1.2.9`
* BrightnessKeys.kext `1.0.2`
* ApplePS2SmartTouchPad.kext `202206`
* WhateverGreen.kext `1.5.8`
* VoodooHDA.kext `3.0.1`
* RealtekRTL8100.kext `2.1.0`
* RealtekCardReader.kext `0.9.5`
* RealtekCardReaderFriend.kext `0.9.5`
* USBMap_mba62.kext #(安装前在 Windows 下定制好USB, MacbookAir6,2)



## 参考资料

* [OpenCorePkg](https://github.com/acidanthera/OpenCorePkg)
* [OpenCore Install Guide](https://dortania.github.io/OpenCore-Install-Guide/)
