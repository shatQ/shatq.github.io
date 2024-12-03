---
{"dg-publish":true,"permalink":"/garden-notes/install-alpine-linux-on-oracle-cloud/","tags":["note","seedling"],"created":"2024-11-18T09:19","updated":"2024-11-30T19:57"}
---

# Install Alpine Linux on Oracle Cloud

1. Create x86_64 instance, e.g. CentOS
2. Download the "virtual" type aarch64 ISO file from [https://www.alpinelinux.org/downloads/](https://www.alpinelinux.org/downloads/) with wget
3. Execute `sudo dd if=alpine.iso of=/dev/sda`
4. On the Oracle Cloud panel, setup a console connection and connect to the serial console.
5. Execute `sudo reboot`
6. When Alpine is launched and you are logged in as root, execute these commands in the serial console:
	```
	mkdir /media/setup
	cp -a /media/sda/* /media/setup
	mkdir /lib/setup
	cp -a /.modloop/* /lib/setup
	/etc/init.d/modloop stop
	umount /dev/sda
	mv /media/setup/* /media/sda/
	mv /lib/setup/* /.modloop/
	```
1. Complete the setup with `setup-alpine`

---
- https://gist.github.com/unixfox/05d661094e646947c4b303f19f9bae11
- https://gist.github.com/B83C/1cf528b176f171bc16d1404920093fff
