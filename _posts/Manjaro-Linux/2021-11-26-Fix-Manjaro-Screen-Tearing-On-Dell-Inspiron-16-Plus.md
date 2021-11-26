---	
layout:     post	
title:      『Manjaro Linux』 Fix Manjaro Screen Tearing On Dell Inspiron 16 Plus	
subtitle:   『Manjaro Linux』 在戴尔灵越16Plus上修复Manjaro的屏幕撕裂    
date:       2021-11-26	   
author:     Coekjan 
header-img: img/post-bg-MJ.jpg	
catalog:    true	
katex:    false    
tags:	
    - Manjaro Linux  
---

本文记录笔者在戴尔灵越16Plus上解决Manjaro屏幕撕裂的方法。

## Manjaro版本与配置信息

```shell
~$ screenfetch

 ██████████████████  ████████     coekjan@Inspiron-Manjaro
 ██████████████████  ████████     OS: Manjaro 21.2.0 Qonos
 ██████████████████  ████████     Kernel: x86_64 Linux 5.13.19-2-MANJARO
 ██████████████████  ████████     Uptime: 16m
 ████████            ████████     Packages: 1423
 ████████  ████████  ████████     Shell: zsh 5.8
 ████████  ████████  ████████     Resolution: 3072x1920
 ████████  ████████  ████████     DE: KDE 5.88.0 / Plasma 5.23.3
 ████████  ████████  ████████     WM: KWin
 ████████  ████████  ████████     GTK Theme: Mcata-dark-alt [GTK2/3]
 ████████  ████████  ████████     Icon Theme: McMojave-circle-dark
 ████████  ████████  ████████     Disk: 67G / 322G (22%)
 ████████  ████████  ████████     CPU: 11th Gen Intel Core i7-11800H @ 16x 4.6GHz [52.0°C]
 ████████  ████████  ████████     GPU: NVIDIA GeForce RTX 3060 Laptop GPU
                                  RAM: 3746MiB / 15746MiB

~$ inxi -Fazy
System:
  Kernel: 5.13.19-2-MANJARO x86_64 bits: 64 compiler: gcc v: 11.1.0
  parameters: BOOT_IMAGE=/boot/vmlinuz-5.13-x86_64
  root=UUID=f452a375-9b39-4c50-b40b-e77daecd1209 rw quiet apparmor=1
  security=apparmor udev.log_priority=3
  Desktop: KDE Plasma 5.23.3 tk: Qt 5.15.2 info: latte-dock wm: kwin_x11 vt: 1
  dm: SDDM Distro: Manjaro Linux base: Arch Linux
Machine:
  Type: Laptop System: Dell product: Inspiron 16 7610 v: N/A
  serial: <superuser required> Chassis: type: 10 serial: <superuser required>
  Mobo: Dell model: 09FDV3 v: A01 serial: <superuser required> UEFI: Dell
  v: 1.1.3 date: 09/24/2021
Battery:
  ID-1: BAT0 charge: 84.3 Wh (100.0%) condition: 84.3/84.3 Wh (100.0%)
  volts: 13.0 min: 11.4 model: BYD DELL M59JH18 type: Li-poly serial: <filter>
  status: Full
  Device-1: hidpp_battery_0 model: Logitech M585/M590 Multi-Device Mouse
  serial: <filter> charge: 10% (should be ignored) rechargeable: yes
  status: Discharging
CPU:
  Info: 8-Core model: 11th Gen Intel Core i7-11800H bits: 64 type: MT MCP
  arch: Tiger Lake family: 6 model-id: 8D (141) stepping: 1 microcode: 34
  cache: L1: 640 KiB L2: 10 MiB L3: 24 MiB
  flags: avx avx2 ht lm nx pae sse sse2 sse3 sse4_1 sse4_2 ssse3 vmx
  bogomips: 73744
  Speed: 1040 MHz min/max: 800/4600 MHz Core speeds (MHz): 1: 860 2: 1022
  3: 1071 4: 1173 5: 1024 6: 1103 7: 1044 8: 1056 9: 1087 10: 1056 11: 977
  12: 800 13: 966 14: 813 15: 937 16: 931
  Vulnerabilities: Type: itlb_multihit status: Not affected
  Type: l1tf status: Not affected
  Type: mds status: Not affected
  Type: meltdown status: Not affected
  Type: spec_store_bypass
  mitigation: Speculative Store Bypass disabled via prctl and seccomp
  Type: spectre_v1
  mitigation: usercopy/swapgs barriers and __user pointer sanitization
  Type: spectre_v2 mitigation: Enhanced IBRS, IBPB: conditional, RSB filling
  Type: srbds status: Not affected
  Type: tsx_async_abort status: Not affected
Graphics:
  Device-1: Intel TigerLake-H GT1 [UHD Graphics] vendor: Dell driver: i915
  v: kernel bus-ID: 0000:00:02.0 chip-ID: 8086:9a60 class-ID: 0300
  Device-2: NVIDIA GA106M [GeForce RTX 3060 Mobile / Max-Q] vendor: Dell
  driver: nvidia v: 495.44 alternate: nouveau,nvidia_drm bus-ID: 0000:01:00.0
  chip-ID: 10de:2520 class-ID: 0300
  Device-3: Microdia Integrated_Webcam_HD type: USB driver: uvcvideo
  bus-ID: 3-5:4 chip-ID: 0c45:6a10 class-ID: 0e02
  Display: x11 server: X.Org 1.21.1.1 compositor: kwin_x11 driver:
  loaded: intel unloaded: modesetting display-ID: :0 screens: 1
  Screen-1: 0 s-res: 3072x1920 s-dpi: 192 s-size: 406x254mm (16.0x10.0")
  s-diag: 479mm (18.9")
  Monitor-1: eDP1 res: 3072x1920 hz: 60 dpi: 227 size: 344x215mm (13.5x8.5")
  diag: 406mm (16")
  OpenGL: renderer: Mesa Intel UHD Graphics (TGL GT1) v: 4.6 Mesa 21.2.5
  direct render: Yes
Audio:
  Device-1: Intel Tiger Lake-H HD Audio vendor: Dell
  driver: sof-audio-pci-intel-tgl
  alternate: snd_hda_intel,snd_sof_pci_intel_tgl bus-ID: 0000:00:1f.3
  chip-ID: 8086:43c8 class-ID: 0401
  Device-2: NVIDIA vendor: Dell driver: snd_hda_intel v: kernel
  bus-ID: 0000:01:00.1 chip-ID: 10de:228e class-ID: 0403
  Sound Server-1: ALSA v: k5.13.19-2-MANJARO running: yes
  Sound Server-2: JACK v: 1.9.19 running: no
  Sound Server-3: PulseAudio v: 15.0 running: yes
  Sound Server-4: PipeWire v: 0.3.40 running: yes
Network:
  Device-1: Intel Tiger Lake PCH CNVi WiFi driver: iwlwifi v: kernel
  bus-ID: 0000:00:14.3 chip-ID: 8086:43f0 class-ID: 0280
  IF: wlp0s20f3 state: up mac: <filter>
Bluetooth:
  Device-1: Intel AX201 Bluetooth type: USB driver: btusb v: 0.8
  bus-ID: 3-14:5 chip-ID: 8087:0026 class-ID: e001
  Report: rfkill ID: hci0 rfk-id: 1 state: up address: see --recommends
RAID:
  Hardware-1: Intel Volume Management Device NVMe RAID Controller driver: vmd
  v: 0.6 port: N/A bus-ID: 0000:00:0e.0 chip-ID: 8086:9a0b rev: class-ID: 0104
Drives:
  Local Storage: total: 476.94 GiB used: 66.9 GiB (14.0%)
  SMART Message: Unable to run smartctl. Root privileges required.
  ID-1: /dev/nvme0n1 maj-min: 259:0 vendor: Toshiba
  model: KBG40ZNS512G NVMe KIOXIA 512GB size: 476.94 GiB block-size:
  physical: 512 B logical: 512 B speed: 31.6 Gb/s lanes: 4 type: SSD
  serial: <filter> rev: 10410106 temp: 54.9 C scheme: GPT
Partition:
  ID-1: / raw-size: 320 GiB size: 313.91 GiB (98.10%) used: 66.82 GiB (21.3%)
  fs: ext4 dev: /dev/nvme0n1p7 maj-min: 259:7
  ID-2: /boot/efi raw-size: 150 MiB size: 146 MiB (97.33%)
  used: 84.3 MiB (57.7%) fs: vfat dev: /dev/nvme0n1p1 maj-min: 259:1
Swap:
  Alert: No swap data was found.
Sensors:
  System Temperatures: cpu: 49.0 C mobo: N/A
  Fan Speeds (RPM): N/A
Info:
  Processes: 353 Uptime: 17m wakeups: 3 Memory: 15.38 GiB
  used: 3.7 GiB (24.1%) Init: systemd v: 249 tool: systemctl Compilers:
  gcc: 11.1.0 Packages: pacman: 1423 lib: 425 flatpak: 0 Shell: Zsh v: 5.8
  default: Bash v: 5.1.8 running-in: yakuake inxi: 3.3.09
```

## 屏幕撕裂解决方案

首先安装[xf86-video-intel](https://archlinux.org/packages/?name=xf86-video-intel)，直接采用yay来安装此软件：

```shell
~$ yay -S xf86-video-intel
```

随后按下述步骤，创建并打开20-intel.conf文件：

```shell
~$ cd /etc/X11/xorg.conf.d
~$ sudo vim 20-intel.conf
```

文件中填入：

```plaintext
Section "Device"
    Identifier  "Intel Graphics"
    Driver      "intel"
    Option      "AccelMethod"     "uxa"
    Option      "TearFree"        "true"
    Option      "SwapbuffersWait" "true"
    Option      "TripleBuffer"    "true"
    Option      "DRI"             "true"
EndSection
```

随后重启即可：

```shell
~$ reboot
```