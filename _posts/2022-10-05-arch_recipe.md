---
title: 'ArchLinux Installation and Configuration'
date: 2022-10-05
permalink: /posts/arch_recipe/
tags:
  - Linux
---

This post is about my own archlinux installation and configuration process from archiso on.

**Updating...**

![](https://xirzc.github.io/images/banner.png)

## Installation Procedure

1. Download Newest [Archlinux GUI Distribution ISO](https://archlinuxgui.in/)
2. Prepare an U Disk for [Ventoy](https://www.ventoy.net/cn/index.html) installation
3. Place all your OS iso files and PE into the available ventoy partition
4. Reboot and press corresponding mainboard keys to enter boot option interface
5. Select U Disk boot option and select your OS option
6. Just install by the `Install Arch Linux` software

## Configuration Steps after Installation

**Pacman Mirror Preparation:**

- Run `sudo vim /etc/pacman.conf` for insert [archlinuxcn](https://github.com/archlinuxcn/mirrorlist-repo) and uncomment multitest source
- Open [Archlinux Mirrorlist](https://archlinux.org/mirrorlist/) for selelcting Chinese mirrorlist and place the fastest in the first place, then run `sudo pacman -Syyu && sudo pacman -S archlinux-keyring archlinuxcn-keyring && sudo pacman -Syyu` for updating all packages
- Run `sudo Pacman -Rdd mutter` to solve mutter and mutter-performance signature conflict problem by archlinuxgui iso mirrors

**VPN Proxy Preparation:**

- Run `sudo pacman -S vim clash chezmoi` for facilitating successive steps
- Open [V2Free](https://w1.v2free.net/) for getting clash configuration
- Run `clash` and this will automatically generating essential dbs and config file for using, and `vim ~/.config/clash/clash.yaml`, paste copied configuration, and then modify the socks port to 1089, at last, run clash at any terminal
- Run `paru` for paru updating packages
- Run `git config --global http.proxy 127.0.0.1:7890 && git config --global https.proxy 127.0.0.1:7890`

**Chezmoi Dotfiles Pulling and Applying:**

- Run `chezmoi init --apply https://github.com/XIRZC/dotfiles.git`, and open [Github personal access tokens](https://github.com/settings/tokens), login and generate a token as password

**Softwares and Packages Installation:**

- Run `sudo pacman -S qtile alacritty kitty fish zsh ranger feh neovim rofi picom starship yarn npm xclip python3 ruby perl python-pip && pip install pynvim psutil && gem install neovim` for preliminary configuration
- Run `sudo pacman -S htop exa bat dust duf procs ripgrep httpie kdiff3 neofetch lolcat figlet toilet cowsay blueman pulseaudio pavucontrol pamixer brightnessctl udiskie ntfs-3g volumeicon cbatticon libnotify notification-daemon networkmanager network-manager-applet fcitx fcitx-configtool fcitx-googlepinyin tmux screen axel lazygit flameshot screenkey typespeed nnn zotero sioyek thunar dolphin code zathura mpv vlc gimp filezilla && paru -S boxes` for all kinds of useful packages

**Beautify:**

- Run `cd ~/Downloads/ && git clone https://github.com/vinceliuice/grub2-themes && cd grub2-themes && sudo ./install.sh -t whitesur -s 2k -b && grub-install --target=x86_64-efi --efi-directory=/boot/efi` for grub theme beatify([grub2-themes link](https://github.com/vinceliuice/grub2-themes))
- Run `paru -S nerd-fonts-complete && sudo pacman -S wqy-microhei wqy-zenhei adobe-source-han-sans-cn-fonts adobe-source-han-serif-cn-fonts` for nerdfont and chinese font support([chinese font archlinux wiki page](https://wiki.archlinux.org/title/Localization/Simplified_Chinese))
- Open [DistroTube Gitlab: Shell Color Scripts](https://gitlab.com/dwt1/shell-color-scripts) and follow the README guide to install colorscript in the bin, specifically by `cd ~/Downloads && git clone https://gitlab.com/dwt1/shell-color-scripts.git && cd shell-color-scripts && sudo make install`
- Run `cd ~ && rm -rf Pictures && git clone https://ghproxy.com/https:/github.com/XIRZC/mywp.git Pictures` for feh directory setting
- Run `git clone https://ghproxy.com/https://github.com/cdump/ranger-devicons2 ~/.config/ranger/plugins/devicons2` for ranger icon

**Fish Configuration:**

- Run `curl https://ghproxy.com/https://raw.githubusercontent.com/oh-my-fish/oh-my-fish/master/bin/install | fish` for oh-my-fish installation
- Run `starship preset pastel-powerline > ~/.config/starship.toml` or `starship preset nerd-font-symbols > ~/.config/starship.toml` for starship command line prompt configuration([starship link](https://starship.rs/presets/))

**Cuda, Conda and PyTorch Installation:**

- Run `cd ~/Downloads && axel -n 10 https://developer.download.nvidia.com/compute/cuda/11.3.0/local_installers/cuda_11.3.0_465.19.01_linux.run && chmod +x cuda_11.3.0_465.19.01_linux.run && sudo pacman -R gcc && sudo pacman -S gcc10 nvidia nvidia-settings nvidia-utils nvtop && sudo ln -s /usr/bin/gcc10 /usr/bin/gcc && sudo ./cuda_11.3.0_465.19.01_linux.run` for cuda 11.3 installation without selecting driver within run file([cuda 11.3 download link](https://developer.nvidia.com/cuda-11.3.0-download-archive?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=20.04&target_type=runfile_local), [gcc compatible version with cuda version](https://stackoverflow.com/questions/6622454/cuda-incompatible-with-my-gcc-version))
- Run `cd ~/Downloads && axel -n 10 https://repo.anaconda.com/miniconda/Miniconda3-py37_4.12.0-Linux-x86_64.sh && chmod +x Miniconda3-py37_4.12.0-Linux-x86_64.sh && ./Miniconda3-py37_4.12.0-Linux-x86_64.sh` for miniconda installation([minconda download link](https://conda.io/projects/conda/en/latest/user-guide/install/linux.html))
- Follow the instruction in [Tsinghua Conda](https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/) and [Tsinghua Pip](https://mirrors.tuna.tsinghua.edu.cn/help/pypi/) for conda and pip acceleration
- Run `conda create -n com python=3.7 && conda activate com && conda install pytorch==1.8.0 torchvision==0.9.0 torchaudio==0.8.0 cudatoolkit=10.2 -c pytorch` for configure a common `python=3.7, pytorch=1.8, torchvision=0.9, torchaudio=0.8, cudatoolkit=10.2` python virtual environment([pytorch previous version link](https://pytorch.org/get-started/previous-versions/#v180))

**DualBoot Time Fixing:**

- Run `sudo timedatectl set-local-rtc true set-ntp true` for fix windows&linux timezone conflict(localtime for windows, utc for linux)

**Zotero Preparation:**

- Config Zotero by validate by webdav `dav.jianguoyun.com/dav`, and then opening [Jianguoyun setting link](https://www.jianguoyun.com/d/home#/safety) for query password

**Remote SSH Connecting Configuration:**

- Run `sudo pacman -S openssh && sudo systemctl start sshd && sudo systemctl enable sshd`, and if you want to change the binding port, just run `sudo vi /etc/ssh/sshd_config` and uncomment the Port line and change this value

**Docker and Nvidia Container Installation and Configuration:**

- Run `sudo pacman -S docker docker-compose nvidia-container-toolkit && paru -S nvidia-container-runtime && sudo cp ~/.config/docker/daemon.json /etc/docker/daemon.json && sudo systemctl enable docker && sudo systemctl start docker` for install and config docker

**Remote VNC Installation and Configuration:**

- Run `sudo pacman -S tigervnc && vncpasswd`, then run `echo 1:mrxir >> /etc/tigervnc/vncserver.users && echo session=lxqt \n geometry=1920x1080 \n localhost \n alwaysshared > ~/.vnc/config && sudo systemctl start vncserver@1.service && sudo systemctl enable vncserver@1.service`, and you just need to open `vncviewer` to enter your `ip addr` ipv4 address follow by `:1` such as `vncviewer 10.31.164.163:1`([tigervnc archwiki](https://wiki.archlinux.org/title/TigerVNC))

> Reference: https://github.com/antoniosarosi/dotfiles

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
- `sudo pacman -R $(pacman -Qtdq)` for delete some not useful dependency packages
- [This link has some useful commands for clean pacman cache](https://zhongguo.eskere.club/%E5%A6%82%E4%BD%95%E6%B8%85%E7%90%86-arch-linux-%E4%B8%AD%E7%9A%84%E5%8C%85%E7%BC%93%E5%AD%98/2021-09-03/)
 