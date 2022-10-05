---
title: 'ArchLinux Installation and Configuration'
date: 2022-10-05
permalink: /posts/arch_recipe/
tags:
  - Linux
---

This post is about my own archlinux installation and configuration process from archiso on.
Updating...

## Important commands

Here are some pre-utils to use:

- `ip link` for look up wlan interface configuration and status
- `wpa_passphrase $SSID $PWD > X.conf` for generate corresponding network connection configuration
- `wpa_supplicant -c X.conf -i wlan0 -B` for connect corresponding wifi in the background
- `dhcpcd &` for configure wifi ip address if the network doesn't work
- `fdisk -l` for list disk partition
- `mkfs.ext4` for make format for linux partition
- `mkswap` for make format for swap
- `mkfs.fat` for make format for EFI partition
 
Here are six important successive commands: 
 
- `pacstrap /mnt base linux linux-firmware` for install linux into mounted linux main partition
- `genfstab -U /mnt >> /mnt/etc/fstab` for system config
- `arch-chroot /mnt` for change root into new system
- `pacman -S grub efibootmgr intel-ucode os-prober` for important system essentials
- `grub-mkconfig > /boot/grub/grub.cfg` for grub configuration generation
- `grub-install --target=x86_64-efi --efi-directory=/boot` for grub installtion
 
Here are some other useful commands for daily use:
- `timedatectl -set-local-rtc 1 --adjust-system-clock` for fix windows&linux timezone conflict(localtime for windows, utc for linux)
- `sudo pacman -R $(pacman -Qtdq)` for delete some not useful dependency packages
- [This link has some useful commands for clean pacman cache](https://zhongguo.eskere.club/%E5%A6%82%E4%BD%95%E6%B8%85%E7%90%86-arch-linux-%E4%B8%AD%E7%9A%84%E5%8C%85%E7%BC%93%E5%AD%98/2021-09-03/)
 
## Efficient packages or apps

- `flameshot` for screenshot
- `ranger` for file-related manipulation
- `zsh` for well-performed command-line use
- `lazygit` for convenient git operation
- `neovim` for better vim
- `gvim` for some great experience compared with conventional vim(here especially for archlinux copyboard sharing between vim and system)
- `spacevim` for easy-use vim
- `dwm` for the best window manager
- `alacritty` for well-looking terminal client
- `kitty` for previewing pictures and visual parts
- `rofi` for weel-looking app launcher
- `neofetch` for printing current system configuration with great appearance
- `yay` for aur package installer
- `qv2ray & v2ray` for good vpn client
- `nerd-font-complete(aur)` for whole nerd-font support
- `fcitx & google-pinyin` for chinese input
- `sddm & kde` for kde desktop environment
- `blueman` for bluetooth connection
- `pulseaudio` for audio manipulation
- `spotify` for some free music
- `networkmanager & network-manager-applet` for visual wifi connection
- `conda` for virtual python environment
- `feh` for automative wallpaper change
- `zotero` for paper managment
- `screenkey` for keyboard monitor
- `picom` for great apperance
- `mpv` for great video player
