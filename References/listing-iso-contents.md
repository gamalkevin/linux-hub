# Listing a content of an ISO
In my example, I am going to peek inside Slackware-based Slax.

Here's the command that I use:
```
$ bsdtar -tf slax.iso | tree --fromfile
.
├── .
├── readme.txt
└── slax
    ├── boot
    │   ├── bootinst.bat
    │   ├── bootinst.sh
    │   ├── bootlogo.png
    │   ├── EFI
    │   │   └── Boot
    │   │       ├── bootx64.efi
    │   │       ├── ldlinux.e64
    │   │       ├── libcom32.c32
    │   │       ├── libutil.c32
    │   │       ├── menu.c32
    │   │       ├── syslinux.cfg
    │   │       ├── syslinux.efi
    │   │       └── vesamenu.c32
    │   ├── extlinux.x32
    │   ├── extlinux.x64
    │   ├── help.txt
    │   ├── initrfs.img
    │   ├── isolinux.bin
    │   ├── isolinux.boot
    │   ├── isolinux.cfg
    │   ├── ldlinux.c32
    │   ├── libcom32.c32
    │   ├── libutil.c32
    │   ├── mbr.bin
    │   ├── pxelinux.0
    │   ├── runadmin.vbs
    │   ├── samedisk.vbs
    │   ├── syslinux.cfg
    │   ├── syslinux.com
    │   ├── syslinux.exe
    │   ├── vesamenu.c32
    │   ├── vmlinuz
    │   └── zblack.png
    ├── changes
    └── modules
        ├── 01-core.sb
        ├── 01-firmware.sb
        ├── 02-xorg.sb
        ├── 03-desktop.sb
        ├── 04-apps.sb
        ├── 05-chromium.sb
        └── 06-devel.sb
```

## Understanding the commands
### `bsdtar -tf ...`
- `bsdtar` → a `tar`-compatible tool from libarchive, but supports many formats beyond `.tar`, including `.iso`.
- `-t` → **list** the archive’s contents (table of contents).
- `-f slax-64bit-slackware-15.0.4.iso` → use this file as the archive input.

👉 `bsdtar -tf slax.iso` prints all file paths inside the ISO, one per line.

---

### `tree --fromfile`
- Normally, `tree` scans a directory on disk.  
- `--fromfile` makes it read **a list of paths from stdin** instead of walking the filesystem.  
- When you pipe the `bsdtar` output, `tree` interprets those paths as a virtual directory tree.

---

### Combined effect
- `bsdtar` extracts the list of paths from the ISO.  
- `tree --fromfile` reconstructs those paths as if you had mounted the ISO.  

**Result:** You get a nice directory tree view of the ISO contents *without mounting it*.

---

### Extra tip
Add `-v` for details like permissions and sizes:
```bash
bsdtar -tvf slax.iso | less
