Ubuntu Base Minimal Image (Xenial Xerus 16.04.3)
------------------------------------------------

This are my annotations on how to build Ubuntu Xenial Base Minimal Image (barebone minimal image) for the Nano Pi M3 (HOWTO).

|                      |footprint |
|----------------------|----------|
|RAM Memory usage      |  62 MB   |
|Rootfs size           | 735 MB   |


For this instructions we need a chroot environment and kernel 4.11.6 pre-built.
I have a ready kernel 4.11.6 built with GCC 7.1 but you can use any kernel or build your own.
These instructions works with Ubuntu Xenial (linux box) but can be adapted to any Distro.

* Ubuntu Xenial 16.04 (Ubuntu Base)
* 62 MB of RAM in use
* No Desktop, just networking
* No debootstrap
* This instructions were created on Ubuntu Xenial 16.04 Box (64 bit)

This is a WiP and is to be completed...


Credits
-------
* Special thanks to @rafaello7 for the Kernel 4.11.6
