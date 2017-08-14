Ubuntu Base Minimal Image (Xenial Xerus 16.04.3)
------------------------------------------------

This are my annotations on how to build Ubuntu Xenial Base Minimal Image (barebone minimal image) for the Nano Pi M3 (HOWTO).

|                        |     footprint     |
|------------------------|-------------------|
|RAM Memory usage        |  43 /  (*) 62 MB  |
|Rootfs size             | 391 / (*) 735 MB  |
|(*) with network-manager|                   |


For this instructions we need a chroot environment and kernel 4.11.6 pre-built.
I have a ready kernel 4.11.6 built with **GCC 7.1** but you can use any kernel or build your own.
These instructions works with Ubuntu Xenial (linux box) but can be adapted to any Distro and can be used with other boards if you have a Kernel ready.

#When you use this instructions for different board, chose the Kernel and bootloader target to that board.

* Ubuntu Xenial 16.04 (Ubuntu Base)
* 62 MB of RAM in use
* No Desktop, just networking
* No debootstrap
* This instructions were created on Ubuntu Xenial 16.04 Box (64 bit)

For this we are going to use Ubuntu Base rootfs with some pre-built packages made by the Ubuntu Team.

System Requirements
-------------------

* Ubuntu Xenial 16.04 (Host PC)
* Chroot environment, wget, git, qemu, etc...
* Kernel pre-built for your board
* SD CARD ( 2 GB will fit, but better use >= 8 GB )
* USB reader/writer (get a good one)

**Important**

* Get a good SD CARD brand and a good USB reader/writer or you will get the final SD CARD with bad data, don't blame me*


Instructions
------------

**1. Download this instructions**

	git clone https://github.com/avafinger/nanopi-m3-ubuntu-base-minimal
	cd nanopi-m3-ubuntu-base-minimal

or

	Use [Clone or download] green button
	uncompress
	cd nanopi-m3-ubuntu-base-minimal-master


**2. Set up your chroot environment**

We need the chroot tools to access the ARM64 Ubuntu Base rootfs from the X86_64 (HOST PC) and add some packages


**3. Get the Ubuntu Base files**

	wget http://cdimage.ubuntu.com/ubuntu-base/xenial/daily/current/xenial-base-arm64.tar.gz
 

**4. Find your SD CARD**

Insert the **SD CARD** into USB card reader/writer and check:

	dmesg|tail
	[  181.158588] sd 6:0:0:0: [sdc] 30547968 512-byte logical blocks: (15.6 GB/14.6 GiB)
	[  181.159831] sd 6:0:0:0: [sdc] Write Protect is off
	[  181.159835] sd 6:0:0:0: [sdc] Mode Sense: 03 00 00 00
	[  181.160836] sd 6:0:0:0: [sdc] No Caching mode page found
	[  181.160841] sd 6:0:0:0: [sdc] Assuming drive cache: write through
	[  181.167092]  sdc: sdc1 sdc2
	[  181.170832] sd 6:0:0:0: [sdc] Attached SCSI removable disk
	[  182.194865] EXT4-fs (sdc1): mounted filesystem with ordered data mode. Opts: (null)
	[  182.202709] EXT4-fs (sdc2): mounted filesystem without journal. Opts: (null)
	

The SD CARD is in the format /dev/sdX where X is a letter (b,c,....), in our case is c (from above)

	sd card is /dev/sdc


**5. Format *SD CARD* with specific geometry**

	sudo ./format_sd.sh /dev/sdc


**6. Prepare the *SD CARD* with Ubuntu Base Minimal Rootfs**

Change the SDCARD=/dev/sdX (our sd card device) to your /dev/sdX (X is your device letter)

	sudo su
	export SDCARD=/dev/sdc
	mkdir -p rootfs
	mount $SDCARD"2" rootfs


**7. Decompress the rootfs**

	tar -xvpzf ./xenial-base-arm64.tar.gz -C ./rootfs --numeric-ow
	sync


**8. Prepare the SD CARD to chroot**

	cp /usr/bin/qemu-aarch64-static ./rootfs/usr/bin/
	sync


**9. Prepare chroot to add some needed packages**

	cp -fv /etc/resolv.conf ./etc/resolv.conf
	cp -rvf ./etc/* ./rootfs/etc
	sync


**10. Update our SD CARD with the pre-built kernel**

	mkdir -p ./rootfs/lib/modules
	sudo tar -xvpzf kernel.tar.gz -C ./rootfs/lib/modules --numeric-ow
	sync
	mkdir -p ./rootfs/lib/firmware
	sudo tar -xvpzf firmware.tar.gz -C ./rootfs/lib/firmware --numeric-ow
	sync


*Chroot to SD CARD and add networking*

	chroot ./rootfs /bin/bash
	(*) apt-get install network-manager (add a ton of packages you may not need)
	apt-get install ifupdown
	apt-get install net-tools
	sync
	exit (*Exit from chroot*)


**11. Edit with your preferred editor the file ./rootfs/etc/passwd**

Change the line from

	root:x:0:0:root:/root:/bin/bash

to

	root::0:0:root:/root:/bin/bash

and save


**12. Unmount the partition**

	umount ./rootfs
	sleep 1
	rm -rf ./rootfs


**13. Prepare the Boot**

	mkdir -p boot
	mount $SDCARD"1" boot
	sleep 1
	sudo tar -xvpzf boot.tar.gz -C ./boot --numeric-ow
	sync
	sleep 1
	umount ./boot
	sleep 1
	rm -rf ./boot


*Prepare the booloader*

	sudo dd if=bl1.bin of=/dev/sdc seek=1
	sync
	sudo dd if=u-boot.bin of=/dev/sdc seek=64
	sync


Exit from **su** (root)

	exit

**14. We have now our Ubuntu Base Minimal Image on SD CARD**

	Just make sure the SD CARD is unmounted and remove it from the USB reader/writer

You can boot the board with this new SD CARD, default to DHCP.
You will be asked the user: root (without the password)
	

**15. If you read this, you have suceeded and from now on you should:**

* apt-get update and apt-get dist-upgrade and add packages
* Add user, add sudoers, locales, timezone, keyboard layout, ssh, services, etc.... and restore the line we changed in the passwd file


**Enjoy your Ubuntu Xenial Base Minimal Image!**



Credits
-------
* Special thanks to @rafaello7 for the Kernel 4.11.6
