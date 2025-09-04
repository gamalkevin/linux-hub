# Old kernel(s) Cleanup Script
I have a persistent problem that's starting to get annoying: full boot partition whenever upgrading to a new kernel.

Installing new kernel is fine and I can boot to a new one with no problem. However, when a new update comes (especially the one touching boot partition), it will say that boot partition is full, thus abruptly ending the upgrade. The boot partition is not terribly small though (500 MB), so I won't be resizing the partition for now.

Instead, I opted to use a script that will:
- Delete old kernel
- Clean the boot partition of the old kernel's fallback images.

---

## The Script

Here's my current script to clean remnants from old kernels:
```  
#!/bin/bash
# Safe kernel cleanup script for Manjaro/Arch

set -euo pipefail

current_kernel="$(uname -r | cut -d'-' -f1)"  # e.g., "6.16.4"
current_pkg="linux${current_kernel%%.*}"      # e.g., "linux616"

echo "🧠 Current running kernel: $current_kernel ($current_pkg)"
echo

# List installed kernel packages
installed_kernels=$(pacman -Qq | grep -E '^linux[0-9]+$' || true)

echo "📦 Installed kernels:"
echo "$installed_kernels"
echo

# Clean up old kernels safely
for k in $installed_kernels; do
    if [[ "$k" == "$current_pkg" ]]; then
        echo "🔒 Keeping $k (current kernel)"
        continue
    fi

    echo "🗑️  Removing $k..."
    sudo pacman -Rns --noconfirm "$k"

    preset="/etc/mkinitcpio.d/${k}.preset"
    if [[ -f "$preset" ]]; then
        echo "   ➤ Removing leftover preset $preset"
        sudo rm -f "$preset"
    fi
done

# Rebuild initramfs for safety
echo
echo "🔧 Rebuilding initramfs..."
sudo mkinitcpio -P

echo
echo "✅ Kernel cleanup done safely!"

```
What this script does:
- Detects the current kernel package and skips it.
- Uses pacman -Q to confirm what’s actually installed before deleting anything.
- Cleans leftover .preset files only if the package was removed.
- Rebuilds initramfs at the end so everything stays consistent.

---

## Note
Previous version of this script was quite problematic: 
- It was too 'destructive' that it also removed the running kernel's image
	+ In my case, it removed `/boot/vmlinuz-6.16-x86_64`, 6.16 being the running kernel
	+ It's failing the mkinitcpio step post-update, and that's not good.