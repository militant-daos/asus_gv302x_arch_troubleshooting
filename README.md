# Arch Linux installation and troubleshooting for Asus Rog Flow x13 (GV302X)

System and environment installation
Disclaimer: I have dual boot with Windows 11 (vendor pre-installed). Previously Ubuuntu was installed as a secondary OS.
- Turn off secure boot in BIOS settings. May be left turned on but I didn't check this option.
- Turn off BitLocker to provide access to the EFI partition.
- Install Arch using a live USB stick image using this guide: `https://wiki.archlinux.org/title/Installation_guide`
- On the bootloader installation step use rEFInd: `https://wiki.archlinux.org/title/REFInd`.
- In case the installed system does not boot check whether the kernel parameters string is correct. At the moment of writing rEFInd passed an incorrect path to the root partition. To fix that boot from the live USB stick image, do `arch-chroot` to the root (in case of a single=partition layout) or `/boot`, locate `refind_linux.conf`, replace wrong path for each boot menu item with the following `rw root=/dev/XYZ` where `XYZ` - the root partition.
- Install `sudo` package.
- Add your user to `wheel` group.
- Generate locales and set up timesync:
  ```
  sudo vim /etc/locale.gen
  sudo vim /etc/locale.gen
  sudo locale-gen
  sudo systemctl enable systemd-timesyncd
  sudo vim /etc/locale.conf  # Add your preferred locale, like LANG=en_US.UTF-8
  ```
- Install `mesa`, `X11` and NVidia proprietary drivers.
  - x11:
    ```
    sudo pacman -S xorg xorg-xinit xorg-server
    ``` 
  - Fix X11 config by adding the following:
    - AMD eGPU: `/etc/X11/xorg.conf.d/20-amdgpu.conf`
    ```
    Section "OutputClass"
	    Identifier "AMD"
	    MatchDriver "amdgpu"
	    Driver "amdgpu"
    EndSection
    ```
  - When installing NVidia proprietary driver DO NOT allow it rewriting the current X11 config! X11 won't start with it.
  - Edit `/etc/X11/Xwrapper.config` and add the following line: `allowed_users = anybody`.
  - Install `asusctl` and `supergfxctl` using this guide: `https://asus-linux.org/guides/arch-guide/`.
  - Fix GPG keys fetching issues:
    - Create `~/.gnupg/dirmngr.conf` with the following content:
    ```
    standard-resolver
    keyserver hkp://keyserver.ubuntu.com:80
    ```
    - Fix names resoludion for `dirmngr`: `ln -sf /run/systemd/resolved/resolv.conf /etc/resolv.conf`
    - Make sure that `resolved` is ENABLED and start it if it isn't running (see `https://bbs.archlinux.org/viewtopic.php?id=261267`).
  - Launch:
    ```
    sudo systemctl enable --now supergfxd
    sudo systemctl enable --now switcheroo-control
    ```
- Fix sound issues using this guide: `https://asus-linux.org/guides/cirrus-amps/`
  - At the time of writing this Arch uses 6.10 kernel, so no extra patches are needed, the Cirrus amp stuff should be already there. 
  - GV302X has I2C-connected amp so install `alsa`, `alsa-tools`, `alsa-firmware`, crete `etc/modprobe.d/alsa-base.conf` and add the following: `options snd-hda-intel model=1043:17f3` and reboot.
  - Install `jack2`, `jack-utils`, `pulseaudio`, `pavucontrol` and reboot. Speakers and headphones output should work.
- Install `xfce4` & required plugins.
  - Install the following:
    ```
    sudo pacman -S networkmanager
    sudo pacman -S network-manager-applet
    ```
  - Edit `.xinitrc`:
    ```
    exec startxfce4
    exec nm-applet
    ```
    
TBC

Developer tools installation
- Visual Studio code
  - Install OSS code package: `sudo pacman -S code`
  - Pull & install Microsoft marketplace AUR via `makepkg`: `https://aur.archlinux.org/packages/code-marketplace`. See `https://wiki.archlinux.org/title/Makepkg#` for details.
- nRF Connect Extension
  - Install `nRF Connect Extension Pack` in VSCode.
  - Fis missing shared objects (relevant for Aug 2024): `sudo ln -s /usr/lib/libunistring.so.5 /usr/lib/libunistring.so.2` and `sudo ln -s /usr/lib/libcrypt.so.2 /usr/lib/libcrypt.so.1`
  - Install toolchain, CLI tools & SDK.
  - nrfjprog & mergehex should be symlink-ed to /usr/local/bin. Shared objects should be there as well. JLink binaries should reside in /opt/SEGGER/JLink.
