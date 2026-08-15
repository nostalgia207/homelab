Windows already installed on a separate SATA SSD (sda). Target disk for Arch is a second NVMe SSD (nvme0n1), used entirely for Arch (no separate games partition on this disk).

> [!warning]
> Confirm the correct disk before touching anything - `lsblk` should show Windows-style partitions (EFI, MSR, large NTFS partition, recovery) on the disk NOT being touched. In this setup, sda = Windows, nvme0n1 = target.

#### 1 - Wipe existing partition table on the target disk

	wipefs -a /dev/nvme0n1

#### 2 - Partition with fdisk (GPT)

	fdisk /dev/nvme0n1

Inside fdisk:

	g              # create fresh GPT table

	n              # new partition - EFI
	[Enter]        # default partition number
	[Enter]        # default first sector
	+1G
	t
	1
	1              # EFI System

	n              # new partition - swap
	[Enter]
	[Enter]
	+4G
	t
	2
	19             # Linux swap

	n              # new partition - root
	[Enter]
	[Enter]
	[Enter]        # rest of disk

	w              # write

#### 3 - Format

	mkfs.fat -F32 /dev/nvme0n1p1
	mkswap /dev/nvme0n1p2
	swapon /dev/nvme0n1p2
	mkfs.ext4 /dev/nvme0n1p3

#### 4 - Mount

	mount /dev/nvme0n1p3 /mnt
	mkdir -p /mnt/boot
	mount /dev/nvme0n1p1 /mnt/boot

Result:

![](Partitions.png)