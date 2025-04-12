# u-boot-aarch64-none-elf
My howto to build a working u-boot
## Preamble
Yes, you might ask yourself why you fall again on another build of u-boot. Well, in my case i want the most custom framework possible to make and run bare metal application or OS for different boards. I followed a lot of tutorials only to realized they don't work for my case.<br><br>So, here is my reminder for myself and everyone finding it.<br><br>For this repo, you will need the help of the [cross-compiler](https://github.com/Tiloup9291/aarch64-none-elf-cross-compiler) of my other repository.<br><br>My article will cover a build for a Raspberry Pi 3B+ and an Orange Pi R1 Plus LTS.

---
# U-boot for Raspberry Pi 3 B+
## Building u-boot
To build u-boot, you will need to get the source first : `wget https://github.com/u-boot/u-boot/archive/refs/tags/v2025.01.tar.gz`<br>
Once done, extract the tar ball and change directory to the root of the sources files (u-boot-2025-01).<br>
You will need to generate a makefile for the board. Thanks to the u-boot team, we already have a definition file pour this board.<br>
So run : `make rpi_3_b_plus_defconfig`<br>
Once done, start the build process with : `make CROSS_COMPILE=aarch64-none-elf-`<br>
When the build process is complete, we will use u-boot.bin for our project.<br>
## Setting the microsd card
We will need to partition and format our microsd card for the board to recognize it.<br>
Plug in your card and run : `sudo fdisk -l` to list all the /dev/sdX available and locate your card. Mine was /dev/sdb.<br>
I start by wiping all the card with zeros : `sudo dd if=/dev/zero of=/dev/sdb bs=10M status=progress conv=fdatasync`<br>
Once complete, we will create the partition : `sudo fdisk /dev/sdb`<br>
here is all the choice for a bootable fat32 partition :<br>
```
n (create a new partition)
p (for a primary partition)
1 (for the partition number)
<enter> (accept the default start sector)
<enter> (accept the default end sector, so all the card will be partition)
t (to change the type of the partition)
c ( for a type : W95 FAT32 (LBA))
a (set the boot flag on)
w (write the partition)
```
Now the partition made, lets put a filesystem on it : `sudo mkfs.vfat -F 32 /dev/sdb1`<br>
## Firmware and u-boot
We will need to get the official firmware : `wget https://github.com/raspberrypi/firmware/releases/download/1.20250326/raspi-firmware_1.20250326.orig.tar.xz`<br>
Extract the tar ball. The files we want are : bootcode.bin, fixup.dat, start.elf.<br>
We will also need the device tree blob of the board : `wget https://github.com/raspberrypi/firmware/raw/refs/heads/master/boot/bcm2710-rpi-3-b-plus.dtb`<br>
You will also need to create a text file called `config.txt`<br>
Inside the config.txt file, you will need to add these instruction :
```
enable_uart=1
kernel=u-boot.bin
arm_64bit=1
core_freq=250
device_tree=bcm2710-rpi-3-b-plus.dtb
```
Now, put all the necessary files on your microsd card. Here is the list : `bootcode.bin, fixup.dat, start.elf, u-boot.bin, config.txt, bcm2710-rpi-3-b-plus.dtb`<br><br>
From now on, just put the microsd card in the board and start it. u-boot will output through the uart port. You can use putty with an adapter usb-to-uart. That's it, congrat!<br>

---
# U-boot for Orange Pi R1 Plus LTS
work in progress.

---
# Ending
And now, what should we do? An OS, a bare metal app? Stay tune. ;)
