# debian-bookworm-zfs-root
Installs Debian GNU/Linux 12 Bookworm to a native ZFS root filesystem using a [Debian Bookworm Live CD](https://www.debian.org/CD/live/). The resulting system is a fully updateable debian system with no quirks or workarounds.

## Usage

1. Boot [Bookworm Live CD](https://www.debian.org/CD/live/)
1. Login (user: `user`, password: `live`) and become root
1. Setup network and export `http_proxy` environment variable (if needed)
1. Run [this script](https://raw.githubusercontent.com/hn/debian-stretch-zfs-root/master/debian-bookworm-zfs-root.sh)
1. User interface: Select disks and RAID level
1. User interface: Decide if you want Legacy BIOS or EFI boot (only if your hardware supports EFI)
1. Let the installer do the work
1. User interface: install grub to *all* disks participating in the array (only if you're using Legacy BIOS boot)
1. User interface: enter root password and select timezone
1. Reboot

## Fixes included

* grub (v2.02, included in Debian 9), especially `grub-probe`, [does not support](https://github.com/zfsonlinux/grub/issues/19) [all ZFS features](http://savannah.gnu.org/bugs/?42861) and subsequently [refuses to install](https://bugs.launchpad.net/ubuntu/+source/grub2/+bug/1451476). This script disables `feature@hole_birth` and `feature@embedded_data` (and you _must_ _not_ enable those features after installation).
* Some mountpoints, notably `/var`, need to be mounted via fstab as the ZFS mount script runs too late during boot.
* The EFI System Partition (ESP) is a single point of failure on one disk, [this is arguably a mis-design in the UEFI specification](https://wiki.debian.org/UEFI#RAID_for_the_EFI_System_Partition). This script installs an ESP partition to every RAID disk, accompanied with a corresponding EFI boot menu.

## Bugs

* `grub-install` sometimes mysteriously fails for disk numbers >= 4 (`grub-install: error: cannot find a GRUB drive for /dev/disk/by-id/...`).

## Credits

* https://github.com/hn/debian-jessie-zfs-root
* https://github.com/zfsonlinux/zfs/wiki/Ubuntu-16.04-Root-on-ZFS
* https://janvrany.github.io/2016/10/fun-with-zfs-part-1-installing-debian-jessie-on-zfs-root.html

