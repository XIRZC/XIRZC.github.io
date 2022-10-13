---
title: 'ArchLinux Installation and Configuration'
date: 2022-10-05
permalink: /posts/arch_recipe/
tags:
  - Linux
---

This post is about my own archlinux installation and configuration process from archiso on.

**Updating...**

## Installation Procedure

1. Download Newest [Archlinux GUI Distribution ISO](https://archlinuxgui.in/)
2. Prepare an U Disk for [Ventoy](https://www.ventoy.net/cn/index.html) installation
3. Place all your OS iso files and PE into the available ventoy partition
4. Reboot and press corresponding mainboard keys to enter boot option interface
5. Select U Disk boot option and select your OS option
6. Just install by the `Install Arch Linux` software

## Configuration Steps after Installation

1. Run `sudo pacman -Syyu` for update all packages to avoid some possible errors
2. Run `sudo vim /etc/pacman.conf` for insert [archlinuxcn](https://github.com/archlinuxcn/mirrorlist-repo) and uncomment multitest source
3. Open [Archlinux Mirrorlist](https://archlinux.org/mirrorlist/) for selelcting Chinese mirrorlist and place the fastest in the first place, then run `sudo pacman -Syyu && paru` for updating all packages
4. Run `sudo pacman -S vim clash chezmoi` for facilitating successive steps
5. Jump [V2Free](https://w1.v2free.net/) for getting clash configuration
6. Run `clash` and this will automatically generating essential dbs and config file for using, and `vim ~/.config/clash/clash.yaml`, paste copied configuration, and then modify the socks port to 1089, at last, run clash at any terminal
7. Run `git config --global http.proxy 127.0.0.1:7890 && git config --global https.proxy 127.0.0.1:7890`
8. Run `chezmoi init --apply https://github.com/XIRZC/dotfiles.git`, and open [Github personal access tokens](https://github.com/settings/tokens), login and generate a token as password
9. Run `sudo pacman -S fish zsh qtile ranger feh neovim rofi picom alacritty kitty` for preliminary configuration
10. Open [DistroTube Gitlab: Shell Color Scripts](https://gitlab.com/dwt1/shell-color-scripts) and follow the README guide to install colorscript in the bin, specifically by `cd ~/Downloads && git clone https://gitlab.com/dwt1/shell-color-scripts.git && cd shell-color-scripts && sudo make install`
11. Run `paru -S nerd-font-complete` for nerdfont support
12. Run `sudo pacman -S flameshot lazygit neofetch lolcat figlet cowsay blueman screenkey mpv pulseaudio zotero sioyek networkmanager network-manager-applet fcitx fcitx-configtool fcitx-googlepinyin code`
13. Run `cd ~/Downloads/ && git clone https://github.com/vinceliuice/grub2-themes && cd grub2-themes && sudo ./install.sh -t whitesur -s 2k -b && grub-install --target=x86_64-efi --efi-directory=/boot/efi` for grub theme beatify([grub2-themes link](https://github.com/vinceliuice/grub2-themes))
14. Run `cd ~ && rm -rf Pictures && git clone https:/github.com/XIRZC/mywp.git Pictures` for feh directory setting
15. Run `git clone https://github.com/cdump/ranger-devicons2 ~/.config/ranger/plugins/devicons2` for ranger icon

## Original Archlinux Live ISO useful commands

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
 