# [WIP] System .ISOs with unusual structure
I was having a hard time booting Slackware-based [Slax](https://www.slax.org/) through `Ventoy`. 

It always says something to the tune of `"can't find the `EFI` file, so perhaps the file is not UEFI?"` while it definitely supports EFI, since I'm using the latest ISO.

After some digging, it seems like the culprit is Slax' unconventional structuring:
- Apparently some Slackware-based ISOs like Slax don’t follow the typical hybrid ISO layout (with `/EFI/BOOT/bootx64.efi` at the root). 
- Instead, Slax has a custom folder arrangement (`/slax/boot/`), and its bootloader assumes it’s being booted directly from a CD/DVD or written raw to a USB, not mounted virtually by Ventoy.