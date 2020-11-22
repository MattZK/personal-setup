# Install stuff

## Shred
shred 3 times and overwrite with zeros
`shred -n 3 -z /dev/nvme0n1`

## Partitions
`cfdisk /dev/nvme0n1`

GPT partition table

1. EFI System Partition 256M (EFI System type)
2. boot 512M (Linux System)
3. root FREE SPACE (Linux System)

## LUKS Encryption
`cryptsetup luksFormat <params> /dev/nvme0n1p3`

`cryptsetup open /dev/nvme0n1p3 <mapper>`

## Formatting
```
mkfs.vfat -F32 -n "EFI System Partition" /dev/nvme0n1p1
mkfs.ext4 -L boot /dev/nvme0n1p2
mkfs.ext4 -L root /dev/mapper/<mapper>
```

## Mounting
```
mount /dev/mapper/<mapper> /mnt
cd /mnt
mkdir boot
mount /dev/nvme0n1p2 boot
mkdir boot/efi
mount /dev/nvme0n1p1 boot/efi
```

## Installing
`pacstrap -i /mnt linux linux-firmware base base-devel`

## FSTab w/ UUIDs
`genfstab -U /mnt > /mnt/etc/fstab`

## Chroot
`arch-chroot /mnt`

## Bootloader
Install GRUB
```pacman -Syu efibootmgr grub```

Edit `/etc/default/grub`:
```
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet mem_sleep_default=deep"
GRUB_CMDLINE_LINUX="cryptdevice=UUID=<UUID>:<mapper> root=/dev/mapper/<mapper>"
```

Edit `/etc/mkinitcpio.conf` add `encrypt` hook after block and build `mkinitcpio -p linux`

Install bootloader and build config
```
grub-install --boot-directory=/boot --efi-directory=/boot/efi
grub-mkconfig -o /boot/grub/grub.cfg
```
