This is a FreeBSD 11.1-RELEASE image installed from the 11.1-RELEASE
mini-memstick installer. It is meant to be used with a paravirtual 
KVM Linode from http://linode.com
Filesize is 1536 Mb
The Installer is now an IMG file in this repo delivered by git-lfs
https://git-lfs.github.com/
Certain changes made to this image will cause some freatures to not
work right off the bat with a Full Virtualization Linode.
THIS IMAGE WILL NOT BOOT ON A Xen LINODE.

The following changes have been made:
	/boot/loader.conf contains these lines
		`boot_multicons="YES"`
		`boot_serial="YES"`
		`comconsole_speed="115200"`
		`console="comconsole,vidconsole"`
	hostname is set to freebsd111
	ZFS is enabled. To keep the image size down, we have kept the image to 1.5GB
	The autoexpand=on zpool property has been set on the root zpool, zroot.
	This was done by running `zpool set autoexpand=on zroot`
	Once you've dd'd it to the disk you want and made sure the disk is the dize
	you want, log in and run these commands:
		`gpart recover da0`
		`gpart resize -i 2 da0`
		`zpool online -e zroot /dev/da0p2`
	This will expand your zpool to your full disk size.
	Clock is set to localtime
	Timezone is set to UTC
	
##	/!\ /!\ /!\ SSH IS DISABLED BY DEFAULT. PLEASE LOG IN VIA LISH /!\ /!\ /!\
##	/!\ /!\ /!\  OR GLISH  FROM THE REMOTE ACCESS TAB /!\ /!\ /!\
	
Default root password: changeme

	
ZFS Configuration:
	Single Disk
	Pool Name: zroot
	Force 4K Sectors?: YES
	Encrypt Disks?: NO
	Partition Scheme: GPT
	Swap Size: 0 (See Below)
	Mirror Swap?: NO
	Encrypt Swap?: NO
	
Swap: Please enable your own swap. Consider a second disk mounted as /dev/da1
	You can enable it by running:
		`swapon /dev/da1`
	Then add your swap disk to /etc/fstab according to https://www.freebsd.org/doc/handbook/adding-swap-space.html
	Default would be:
	`/dev/da1	none	swap	sw	0	0`

The following datasets are installed:
	Base
	lib32
	src
	
The following keymap is set:
	Default en-US
	
Networking:
	Interface is vtnet0
	IPv4 is provided by DHCP
	IPv4 DNS is set to 8.8.8.8
	IPv6 is assigned via SLAAC
	IPv6 DNS is set to 2600:3c03::5 (Newark)

Packages:
	`pkg` has been bootstrapped. No database has been created and no other ports installed.
	
You can verify integrity of the image by downloading freebsd111-linode.sig
from either my github or the fileserver you found this on.
Grab my key (0x5F94763A) and run `gpg --verify freebsd111-linode.sig freebsd111-linode.img`
The file was signed by running:
`gpg --output freebsd111-linode.sig --detach-sig freebsd111-linode.img`

Installation is as simple as:
1. Create a new disk.
2. Boot into rescue mode
3. Run
