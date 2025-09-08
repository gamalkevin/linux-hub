# 🔪🐧 Deleting Installed Linux System 

## 🔄 System Diagram 🗺️
```
┌───────────────────────────────┐
│ Disk 1: /dev/sda (HDD, 1TB)  │  <-- Linux disk
├───────────────┬───────────────┤
│ sda1 (NTFS)   │ Game (Windows data)
│ sda2 (ext4)   │ Manjaro /boot (GRUB & EFI loader)
│ sda3 (ext4)   │ Manjaro / (root filesystem)
│ sda4 (vfat)   │ EFI System Partition (shared) 
│               │ ├─ boot/          (generic EFI bootloader)
│               │ ├─ Parrot/       (Parrot bootloader) ← will be deleted
│               │ └─ Garuda/       (Garuda bootloader) ← will be deleted too
│ sda5 (NTFS)   │ Work & Media (Windows data)
│ sda6 (ext4)   │ Parrot root (deleted, now unallocated)
└───────────────┴───────────────┘

┌───────────────────────────────┐
│ Disk 2: /dev/sdb (SSD, 119GB) │  <-- Windows disk
├───────────────┬───────────────┤
│ sdb1 (NTFS)   │ Recovery
│ sdb2 (vfat)   │ EFI System Partition (Windows bootloader)
│ sdb3 (16M)    │ Microsoft reserved
│ sdb4 (NTFS)   │ Windows root / data
│ sdb5 (NTFS)   │ Windows recovery
└───────────────┴───────────────┘

```
I am running a triple boot setup:
1. Windows 11 as a previous daily-drive,
2. Manjaro XFCE as my main daily now, and
3. Parrot Security 6.4 / Parrot OS as a testing ground.

Win 11 is installed on M.2 SATA SSD, and the Linux systems are on HDD, sharing the same partition for bootloader.

Later on, I decided to change Parrot OS to **Kali Linux** instead, as it seems to be the *de facto* distro for pentesting, something I planned to study and learn much more in-depth. Many courses and exams mandates the usage of Kali Linux, so let's start by installing it.

We'll start with deleting Parrot OS' partition, as well as cleanly removing its EFI boot entries.

---

## ❌ Deleting Parrot OS

- First things first, we'll remove Parrot from the system. 
	- For this, I simply headed to `gparted`, and deleted the Parrot OS' partition. Quick and easy.

---

## 🌬️ Removing boot entry

Up next, we'll remove Parrot OS' bootloader entry. Let's list the entries first.

###   Listing the Entries
```
[kev@Randrake ~]$ sudo efibootmgr
[sudo] password for kev: 
BootCurrent: 0001
Timeout: 0 seconds
BootOrder: 2001,0001,0006,0003,0007,0000,2002,2003
Boot0000* HDD0: ST1000LM035-1RK172	PciRoot(0x0)/Pci(0x8,0x2)/Pci(0x0,0x0)/Sata(0,0,0)/HD(4,GPT,175e1851-ce00-4e51-9c95-9292ac9675e7,0x3a2ed800,0x96000)RC
Boot0001* manjaro	HD(2,GPT,b0f4c535-07ce-4b03-9026-346b377c1236,0xfa000,0x32000)/\EFI\manjaro\grubx64.efi
Boot0003* Garuda	HD(6,GPT,175e1851-ce00-4e51-9c95-9292ac9675e7,0x3a2ed800,0x96000)/\EFI\Garuda\grubx64.efi
Boot0004* HDD0: ST1000LM035-1RK172	PciRoot(0x0)/Pci(0x8,0x2)/Pci(0x0,0x0)/Sata(0,0,0)/HD(6,GPT,175e1851-ce00-4e51-9c95-9292ac9675e7,0x3a2ed800,0x96000)RC
Boot0005* EFI Network	RC
Boot0006* Parrot	HD(6,GPT,175e1851-ce00-4e51-9c95-9292ac9675e7,0x3a2ed800,0x96000)/\EFI\Parrot\grubx64.efi
Boot0007* Windows Boot Manager	HD(2,GPT,b0f4c535-07ce-4b03-9026-346b377c1236,0xfa000,0x32000)/\EFI\Microsoft\Boot\bootmgfw.efi57494e444f5753000100000088000000780000004200430044004f0042004a004500430054003d007b00390064006500610038003600320063002d0035006300640064002d0034006500370030002d0061006300630031002d006600330032006200330034003400640034003700390035007d00000031000100000010000000040000007fff0400
Boot2001* EFI USB Device	RC
Boot2002* EFI DVD/CDROM	RC
Boot2003* EFI Network	RC
```
It seems kinda messy, especially with the long strings from **Windows Boot Manager**. We'll focus on the entries from Linux:
```
Boot0000* HDD0: ST1000LM035-1RK172
Boot0001* manjaro
Boot0003* Garuda
Boot0004* HDD0: ST1000LM035-1RK172
Boot0005* EFI Network	RC
Boot0006* Parrot
```

Turns out, there seems to be remnants of previous distro testing too, which is `Boot0003* Garuda`. We'll remove that as well:
```
# For Parrot
sudo efibootmgr -b 0006 -B

# For Garuda 
sudo efibootmgr -b 0003 -B
```
####   What are the flags for this `efibootmgr` command?
- `-b 0006`
	+ `-b` = **bootnum**
	+ Tells `efibootmgr` which entry we want to act on.
	+ Here, `0006` refers to `Boot0006`, which is Parrot.

- -B
	+ Uppercase **B = delete bootnum**
	+ Removes the entry from UEFI firmware altogether.
	+ Once run, `Boot0006` will no longer exist in the list.
	
*Note: Apparently, this only cleans the firmware’s boot menu. It doesn’t delete the actual files in /boot/efi/EFI/Parrot/.* 

So let's continue by checking the directory...

---

## 🩺 Checking EFI Partition for Leftovers

###   Mounting the EFI Partition
```
sudo mkdir /mnt/esp
sudo mount /dev/sda4 /mnt/esp
```

###   Listing the EFI contents
```
sudo eza -lah --only-dirs /mnt/esp/EFI
Permissions Size User Date Modified Name
drwx------     - root 17 Aug 07:39  boot
drwx------     - root 17 Aug 07:39  Garuda
drwx------     - root 17 Aug 18:00  Parrot
```
####   What are the flags for this `eza` command?
- `-l`
	+ **Long format listing**
	+ Shows permissions, owner, group, size, modified time, and name.
	+ Basically the same as `ls -l`.

- `-a`
	+ **All entries, including hidden ones**
	+ Displays files and directories starting with `.` (dotfiles).
	+ Similar to `ls -a`.

- `-h`
	+ **Human-readable sizes**
	+ Converts raw byte counts into KB, MB, GB.
	+ So instead of `4096`, we might see `4.0K`.

- Combined `-lah`
	+ `-l` → detailed listing
	+ `-a` → includes hidden files
	+ `-h` → readable sizes
	+ Shows _everything_ in `/mnt/esp/EFI`, with detailed info, including any hidden EFI directories, in a human-friendly format.

*Note that I'm using `eza`, since it has much better QoL improvements compared to just `ls`. This is a personal preference, so there's nothing wrong if you just use `ls`.*

###   Removing the old entries
We can safely clear Parrot & Garuda's leftovers:
```
sudo rm -r /mnt/esp/EFI/Parrot
sudo rm -r /mnt/esp/EFI/Garuda

```

## 🤝 Last thing to do
With the entry removed and leftovers cleaned, we should update grub:
```
sudo update-grub
```

All done! Off to installing Kali~

Thanks for reading.