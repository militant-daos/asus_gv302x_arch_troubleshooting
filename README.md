# Arch Linux installation and troubleshooting for Asus Rog Flow x13 (GV302X)

System and environment installation
Disclaimer: I have dual boot with Windows 11 (vendor pre-installed). Previously Ubuuntu was installed as a secondary OS.
- Turn off secure boot in BIOS settings. May be left turned on but I didn't check this option.
- Turn off BitLocker to provide access to the EFI partition.
- Install Arch using a live USB stick image using this guide: `https://wiki.archlinux.org/title/Installation_guide`
- On the bootloader installation step use rEFInd: `https://wiki.archlinux.org/title/REFInd`.
- In case the installed system does not boot check whether the kernel parameters string is correct. At the moment of writing rEFInd passed an incorrect path to the root partition. To fix that boot from the live USB stick image, do `arch-chroot` to the root (in case of a single=partition layout) or `/boot`, locate `refind_linux.conf`, replace wrong path for each boot menu item with the following `rw root=/dev/XYZ` where `XYZ` - the root partition.
- TBC   

Developer tools installation
- Visual Studio code
  - Install OSS code package: `sudo pacman -S code`
  - Pull & install Microsoft marketplace AUR via `makepkg`: `https://aur.archlinux.org/packages/code-marketplace`. See `https://wiki.archlinux.org/title/Makepkg#` for details.
- nRF Connect Extension
  - Install `nRF Connect Extension Pack` in VSCode.
  - Fis missing shared objects (relevant for Aug 2024): `sudo ln -s /usr/lib/libunistring.so.5 /usr/lib/libunistring.so.2` and `sudo ln -s /usr/lib/libcrypt.so.2 /usr/lib/libcrypt.so.1`
  - Install toolchain, CLI tools & SDK. 
