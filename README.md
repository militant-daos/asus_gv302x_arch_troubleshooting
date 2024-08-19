# Arch Linux installation and troubleshooting for Asus Rog Flow x13 (GV302X)


Developer tools installation
- Visual Studio code
  - Install OSS code package: `sudo pacman -S code`
  - Pull & install Microsoft marketplace AUR via `makepkg`: `https://aur.archlinux.org/packages/code-marketplace`. See `https://wiki.archlinux.org/title/Makepkg#` for details.
- nRF Connect Extension
  - Install `nRF Connect Extension Pack` in VSCode.
  - Fis missing shared objects (relevant for Aug 2024): `sudo ln -s /usr/lib/libunistring.so.5 /usr/lib/libunistring.so.2` and `sudo ln -s /usr/lib/libcrypt.so.2 /usr/lib/libcrypt.so.1`
  - Install toolchain, CLI tools & SDK. 
