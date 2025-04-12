PREFIX = $(CURDIR)
.PHONY: start restart
start:
	@{ \
	echo $(PREFIX) ;\
	##Build tree ;\
	mkdir raspiBoot raspiBoot/uboot raspiBoot/firmware raspiBoot/fileForCard ;\
	##Get u-boot ;\
	cd raspiBoot/uboot ;\
	wget https://github.com/u-boot/u-boot/archive/refs/tags/v2025.01.tar.gz ;\
	tar -xzvf v2025.01.tar.gz ;\
	cd u-boot-2025.01 ;\
	##Build u-boot ;\
	make rpi_3_b_plus_defconfig ;\
	make CROSS_COMPILE=aarch64-none-elf- ;\
	cp u-boot.bin ../../fileForCard/ ;\
	##Get firmware ;\
	cd ../../firmware ;\
	wget https://github.com/raspberrypi/firmware/releases/download/1.20250326/raspi-firmware_1.20250326.orig.tar.xz ;\
	tar -xvf raspi-firmware_1.20250326.orig.tar.xz ;\
	cd raspi-firmware-1.20250326/boot ;\
	wget https://github.com/raspberrypi/firmware/raw/refs/heads/master/boot/bcm2710-rpi-3-b-plus.dtb ;\
	cp bootcode.bin ../../../fileForCard/ ;\
	cp fixup.dat ../../../fileForCard/ ;\
	cp start.elf ../../../fileForCard/ ;\
	cp bcm2710-rpi-3-b-plus.dtb ../../../fileForCard ;\
	##Create config file ;\
	cd ../../../fileForCard ;\
	echo -e 'enable_uart=1\nkernel=u-boot.bin\narm_64bit=1\ncore_freq=250\ndevice_tree=bcm2710-rpi-3-b-plus.dtb' > config.txt ;\
	echo "**************************************ATTENTION*************************************************" ;\
	echo "Your files are in raspiBoot/fileForCard" ;\
	echo "************************************************************************************************" ;\
	}
restart:
	rm -rvf raspiBoot
