# FreeBSD On Linode - 11.1-RELEASE x64

#### Table of Contents

1. [What is this?](#what-is-this)
2. [Configuration](#configuration)
3. [Verification and Integrity Checking](#verification-and-integrity-checking)
4. [Notes](#notes)
5. [Installation](#installation)

## What is this?

This is a FreeBSD 11.1-RELEASE image installed from the 11.1-RELEASE
mini-memstick installer. It is meant to be used with a paravirtual 
KVM Linode from http://linode.com

Filesize is 1536 Mb

The Installer is now an IMG file in this repo delivered by git-lfs

https://git-lfs.github.com/

Certain changes made to this image will cause some freatures to not
work right off the bat with a Full Virtualization Linode.

THIS IMAGE WILL NOT BOOT ON A Xen LINODE.

## Configuration

### The following changes have been made:

`/boot/loader.conf` contains these lines

	boot_multicons="YES"
	boot_serial="YES"
	comconsole_speed="115200"
	console="comconsole,vidconsole"

Hostname is set to "freebsd111"

ZFS is enabled. To keep the image size down, we have kept the image to 1.5GB
The `autoexpand=on` zpool property has been set on the root zpool, `zroot`.

This was done by running `zpool set autoexpand=on zroot`

Once you've dd'd it to the disk you want and made sure the disk is the dize
you want, reboot your Linode into your FreeBSD installation,
log in, and run these commands:

	gpart recover da0
	gpart resize -i 2 da0
	zpool online -e zroot /dev/da0p2

This will expand your zpool to your full disk size.

Timezone is set to UTC

###	/!\ /!\ /!\ SSH IS DISABLED BY DEFAULT. PLEASE LOG IN VIA LISH /!\ /!\ /!\
###	/!\ /!\ /!\  OR GLISH  FROM THE REMOTE ACCESS TAB /!\ /!\ /!\
	
### Default root password: 
	changeme

	
### ZFS Configuration:
	Single Disk
	Pool Name: zroot
	Force 4K Sectors?: YES
	Encrypt Disks?: NO
	Partition Scheme: GPT
	Swap Size: 0 (See Below)
	Mirror Swap?: NO
	Encrypt Swap?: NO
	
### Swap: Please enable your own swap. Consider a second disk mounted as /dev/da1
You can enable it by running:
	
	swapon /dev/da1

Then add your swap disk to /etc/fstab according to https://www.freebsd.org/doc/handbook/adding-swap-space.html

Default, if you use a swap disk created in the Linode Dashboard, would be:

	/dev/da1	none	swap	sw	0	0

### Datasets:
	Base
	lib32
	src

### Keymap:
	Default en-US

### Networking:
- Interface is vtnet0
- IPv4 is provided by DHCP
- IPv4 DNS is set to 8.8.8.8
- IPv6 is assigned via SLAAC
- IPv6 DNS is set to 2600:3c03::5 (Newark)

### Packages:
`pkg` has been bootstrapped. No database has been created and no other ports installed.

## Verification and Integrity Checking
	
You can verify integrity of the image by downloading `freebsd111-linode.sig`
from either my github or the fileserver you found this on.

Grab my key (0x7D1919E3) and run `gpg --verify freebsd111-linode.sig freebsd111-linode.img`

The file was signed by running:
`gpg --output freebsd111-linode.sig --detach-sig freebsd111-linode.img`

## Notes

Please note that this file was signed by my work key, as opposed to previous releases.
This key was signed by my personal key, and should be considered to be as trusted as my personal key. 
(How much you trust that, well, that's up to you.)

## Installation

Installation is as simple as:
1. Create a new disk.
2. Boot into rescue mode
3. From Rescue Mode, run the following:

	`apt-get update`

	`apt-get install -y ca-certificates`
	
	`wget -O - https://media.githubusercontent.com/media/RainbowHackerHorse/FreeBSD-On-Linode/master/freebsd111-linode.img  | dd of=/dev/sda`

It'll take a little while to run, as GitHub's LFS server can be a bit slow to initially retrive the file.
I may move to other storage for future releases.
