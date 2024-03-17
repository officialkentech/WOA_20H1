# WOA_20H1 - Windows 10 for ARM Version 20H1 on QEMU

## Why do this?
To run Windows on ARM on an emulator without finding a real hardware :)

<br> There are some pages show how to do this on the Internet. But the problem is, you will have to download the iso and install yourself
and this takes <b>long time</b> to install on QEMU.

## What have been modified?
- Disable Pagefile
- Disable Search Index
- Disable Printer Service
- Fixed EFI

## Download link
- <a href="https://www.mediafire.com/file/9oja45cjad3ogo8/WOA_20H1.7z/file">7z compressed version</a> (Size: 2.5GB)

## How to use
Step 1: Install QEMU: Goto <a href="https:\\qemu.org">qemu.org</a> and install QEMU if you haven't installed it yet. On Linux just run ```sudo apt install qemu-system-aarch64```
        
Step 2: Download the script: Goto <a href="https://github.com/raspiduino/waq/releases">Release</a> and download it. Or you can copy the script here:
- For Windows:
```bat
@echo off
title WOA - 20H1
qemu-system-aarch64 ^
-name "Windows 10 20H1 on ARM64" ^
-M virt ^
-cpu cortex-a76 ^
-smp 4 ^
--accel tcg,thread=multi ^
-m 4096 ^
-pflash QEMU_EFI.img ^
-pflash QEMU_VARS.img ^
-device VGA ^
-device nec-usb-xhci ^
-device usb-kbd ^
-device usb-tablet ^
-device usb-storage,drive=boot ^
-drive if=none,id=boot,file="WOA_20H1.vhdx" ^
-device usb-storage,drive=drivercdrom ^
-drive file="virtio-win-0.1.248.iso",media=cdrom,if=none,id=drivercdrom
```
- For Linux/MacOS:
```bash
#/bin/bash
qemu-system-aarch64 -name "Windows 10 on ARM64" -M virt -cpu cortex-a72 -smp 3 --accel tcg,thread=multi -m 2048 -pflash QEMU_EFI.img -pflash QEMU_VARS.img -device VGA -device nec-usb-xhci -device usb-kbd -device usb-mouse -device usb-storage,drive=boot -drive if=none,id=boot,file="woa_17134.img" -device usb-storage,drive=drivercdrom -drive file="virtio-win-0.1.185.iso",media=cdrom,if=none,id=drivercdrom
```

Step 3: (Optional) Install Virtio driver: Download the driver iso <a href="https://fedorapeople.org/groups/virt/virtio-win/direct-downloads/stable-virtio/virtio-win.iso">here</a> and put it to the vm directory.
<br><b>IMPORTANT:</b> If you don't want to install Virtio driver or you finished install it, you can delete it (if you have downloaded it) and comment out the 2 last line in the script. For Linux/MacOS script, just remove ```-device usb-storage,drive=drivercdrom -drive file="virtio-win-0.1.185.iso",media=cdrom,if=none,id=drivercdrom``` from arm.sh

Step 4: Start the script
<br>Just like normal script, ```./arm.sh``` for Linux/MacOS and ```arm.sh``` for Windows

## On boot
When it boot to EFI shell, enter ```exit```. Then it will come to a list of options, select 'Boot Manager', then select 'UEFI QEMU QEMU HARDDRIVE ...3' or 'UEFI QEMU QEMU HARDDRIVE ...4.1'. If this does not work and you return to that menu, please try another boot device in that list. That should work.

## Notes:
- The emulator may be so lag when you boot it. After a while when the desktop loaded, you can use it normally.
- This image file can be used to flash the real rpi
- You can use ```--enable-kvm``` when you are on a real ARM cpu with <i>virtualization</i>. I'm about to run this on my phone [here](https://github.com/raspiduino/sm-a600g-kvm)

## Todo
- Add saved state image so you can just restore it.
