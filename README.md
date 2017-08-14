Ubuntu Base Minimal Image (Xenial Xerus 16.04.3)
------------------------------------------------

This are my annotations on how to build Ubuntu Xenial Base Minimal Image (barebone minimal image) for the Nano Pi M3 (HOWTO).

|                      |footprint |
|----------------------|----------|
|RAM Memory usage      |  62 MB   |
|Rootfs size           | 735 MB   |


For this instructions we need a chroot environment and kernel 4.11.6 pre-built.
I have a ready kernel 4.11.6 built with GCC 7.1 but you can use any kernel or build your own.
These instructions works with Ubuntu Xenial (linux box) but can be adapted to any Distro and can be used with others boards if you have a Kernel ready.

* Ubuntu Xenial 16.04 (Ubuntu Base)
* 62 MB of RAM in use
* No Desktop, just networking
* No debootstrap
* This instructions were created on Ubuntu Xenial 16.04 Box (64 bit)

For this we are going to use Ubuntu Base rootfs with some pre-built packages made by the Ubuntu Team.

System Requirements
-------------------

* Ubuntu Xenial 16.04 (Host PC)
* Chroot environment, wget, git
* Kernel pre-built for your board
* SD CARD ( 2 GB will fit, but better use >= 8 GB )
* USB reader/writer (get a good one)

#Important

* Get a good SD CARD brand and a good USB reader/writer or you will get the final SD CARD with bad data, don't blame me*


Instructions
------------

#1. Download this instructions

	git clone https://github.com/avafinger/nanopi-m3-ubuntu-base-minimal
	cd nanopi-m3-ubuntu-base-minimal

or

	Use *Clone or download* green button
	uncompress
	cd nanopi-m3-ubuntu-base-minimal-master


#1. Set up your chroot environment

We need the chroot tools to access the ARM64 Ubuntu Base rootfs from the X86_64 (HOST PC) and add some packages


#2. Get the Ubuntu Base files

	wget http://cdimage.ubuntu.com/ubuntu-base/xenial/daily/current/xenial-base-arm64.tar.gz
 

#3. Find your SD CARD


This is a WiP and is to be completed...


Credits
-------
* Special thanks to @rafaello7 for the Kernel 4.11.6
