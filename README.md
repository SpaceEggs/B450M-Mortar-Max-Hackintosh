可以直接通过此 EFI 升级到 MacOS Big Sur

![About This Mac](https://github.com/SpaceEggs/B450M-Mortar-Max-Hackintosh/blob/master/Pic/AboutThisMac.png?raw=true)

## 配置

CPU: Ryzen 3700X

MotherBoard: MSI B450M Mortar MAX

GPU: AMD RX580

## 版本

| Package                    | Version |
| -------------------------- | ------- |
| OpenCore                   | 0.6.7   |
| Lilu                       | 1.5.1   |
| VirtualSMC                 | 1.2.1   |
| WhateverGreen              | 1.4.8   |
| AppleALC                   | 1.5.8   |
| RealtekRTL8111             | 2.4.0   |
| NVMeFix                    | 1.0.5   |
| AMDRyzenCPUPowerManagement | 0.6.6   |
| SMCAMDProcessor            | 0.6.4   |

## 准备工作

- U盘制作：[Making the installer in Windows](https://dortania.github.io/OpenCore-Install-Guide/installer-guide/winblows-install.html#downloading-macos-modern)
- 编辑 plist 工具: [ProperTree](https://github.com/corpnewt/ProperTree)
- 生成序列号: [GenSMBIOS](https://github.com/corpnewt/GenSMBIOS)

## 食用指南

制作完 Recovery U盘后，替换目录下的 EFI 目录

使用 ProperTree 打开 EFI/OC 目录下的 config.plist 文件。

打开 GenSMBIOS，先输入 1，再输入 3，根据以下配置输入型号可得序列号：

- iMacPro1,1: AMD RX Polaris 及以上显卡
- MacPro7,1: AMD RX Polaris 架构及以上显卡（该型号只能运行在 catalina 及以上版本）
- MacPro6,1: AMD R5/R7/R9 及以下显卡
- iMac14,2: Nvidia Kepler 及以上显卡

![GenSMBIOS](https://raw.githubusercontent.com/SpaceEggs/B450M-Mortar-Max-Hackintosh/master/Pic/GenSMBIOS.png)

将返回的信息与 plist 中的 PlatformInfo 项对应：

| 序列号        | 对应项                        |
| ------------- | ----------------------------- |
| Type          | Generic -> SystemProductName |
| Serial        | Generic -> SystemSerialNumber |
| Board Serial  | Generic -> MLB                |
| SmUUID        | Generic -> SystemUUID         |
| 网卡 Mac 地址 | Generic -> ROM                |

### GPU 设置

如显卡非 AMD RX Polaris 架构，需要在 NVRAM -> 7C436110-AB2A-4BBB-A880-FE41995C9F82 -> boot args 添加下列参数：

| boot-args      | Description                             |
| -------------- | --------------------------------------- |
| agdpmod=pikera | RX5000 系列显卡添加                     |
| nvda_drv_vrl=1 | Nvidia Maxwell 和 Pascal 架构的显卡使用 |

![ADDBootArgs](https://raw.githubusercontent.com/SpaceEggs/B450M-Mortar-Max-Hackintosh/master/Pic/AddBootArgs.png)

> 如无法加载 OC 请尝试将主板 BIOS 更新到最新

## BIOS 设置

### 关闭

- Fast Boot
- Secure Boot
- Serial/COM Port
- Parallel Port
- Compatibility Support Module (CSM)
- Above 4G decoding

### 开启

- EHCI/XHCI Hand-off
- OS type: Windows 8.1/10 UEFI Mode
- SATA Mode: AHCI

