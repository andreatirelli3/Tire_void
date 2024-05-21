# Archlinux 
---
## Checklist
- [ ] (optional) [[#Connect via ssh]]
	- [ ] (optional) [[#Wifi connection with iwctl]]
- [ ] [[#Update the archlinux key-ring and installation script]]
- [ ] [[#Archlinux installation configuration]]
- [ ] [[#Mirrorlist]]
- [ ] [[#Bluetooth]]
- [ ] [[#SSH]]
- [ ] [[#Pacman chill]]
- [ ] [[#AUR helper - PARU]]
- [ ] [[#Flatpak]]
- [ ] [[#Chaotic AUR repository servers]]
- [ ] [[#Package manager cache cleaner]]
- [ ] (optional) [[#Power plan]]
- [ ] (optional) [[#NVIDIA Driver]]
- [ ] (optional) [[#Windows dual-boot with TPM]]
## Helper
### Connect via ssh
Set the password for the **root** user in che archiso installer and display the current IP.
``` bash
passwd
ip address
```
Connect to the archiso installation through **ssh**.
``` bash
ssh root@<ip>
```
*Useful links*:
- https://unix.stackexchange.com/questions/352139/how-to-setup-ssh-access-to-arch-linux-iso-livecd-booted-computer
#### Wifi connection with iwctl
> TO DO: this sections is in building ...

*Useful link*:
- https://man.archlinux.org/man/iwctl.1
### Update the archlinux key-ring and installation script
Update the package manager server and download the latest version of the packages.
``` bash
pacman -Syy && pacman -S archlinux-keyring archinstall
```
### Archlinux installation configuration
Feel free to customize and adapt the installation parameters based on the system.
> Tips: for NVIDIA driver use the proprietary one and for the extra packages install:
``` bash
archinstall
# Setup the installation script ...
base-devel git vim
```
> Attention: after the installation, the script ad if we want to configure the  new system by accessing it with **chroot**, is important to say *yes*, to perform the next sections.
### Mirrorlist
Backup the old mirrorlist file and generate a new one with **reflector**.
> Before doing the next step, if you didn't already update the package manager server, do it with the classic **pacman -Syy**.
``` bash
# Install the packages
pacman -S reflector rsync curl
# Backup the current mirrorlist
cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
# Generate the new one with reflector
reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```
### Bluetooth
Install the necessary packages and enable via *systemd*.
``` bash
pacman -S bluez bluez-utils
systemctl enable bluetooth
```
### SSH
Enable the ssh daemon.
``` bash
systemctl enable sshd
```
### Pacman chill
Let's personalize the arch package manager output and it's downloading bar.
Need to edit the **pacman.conf** file and enable two tags.
``` bash
vim /etc/pacman.conf
```
1. Un-comment line 33, "Color";
2. Add the line 34 with the tag "ILoveCandy".
To check if work correctly just do:
``` bash
pacman -Syy
```
### AUR helper - PARU
The AUR helper of choice is [paru](https://github.com/Morganamilo/paru), an AUR helper written in rust. But if you don't like this one, there is always available the classic [yay](https://github.com/Jguer/yay).
> Important: to install this AUR helper, log with a non-root user.
``` bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si
# Update the package manager and AUR helper servers
sudo pacman -Syy && paru
```
### Flatpak
For all other application that want to be run in a sandbox environment, install **flatpak**.
``` bash
sudo pacman -S flatpak
```
### Chaotic AUR repository servers
Add this repository for more packages that come already pre-compiled for us.
> Important: execute all the command with the **root** user.
``` bash
pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com
pacman-key --lsign-key 3056513887B78AEB
pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst'
pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'

# Add the repository servers to the package manager list
vim /etc/pacman.conf
# Add the following lines to the EOF
# 
# [chaotic-aur]  
# Include = /etc/pacman.d/chaotic-mirrorlist

# Update package manager repos
pacman -Syy
```
### Package manager cache cleaner
It is important to clean regularly the package manager cache, to help us in this task install **pacman-contrib** that come with a set off tools for:
- check regularly packages update;
- clean cache with cronjob;
- and more.
``` bash
pacman -S pacman-contrib
# Enable the cronjob that time 7 days to clean the cache
systemctl enable paccache.timer
```
*Useful links*:
- https://wiki.archlinux.org/title/Pacman#Cleaning_the_package_cache
### Power plan
``` bash
# Install the power daemom
pacman -S power-profiles-daemon
# Enable it at startup
systemctl enable power-profiles-daemon.service
```
*Useful links*:
- https://www.reddit.com/r/gnome/comments/11pkz2j/power_profiles_what_exactly_do_they_change/
- https://gitlab.freedesktop.org/upower/power-profiles-daemon/-/blob/main/README.md
### NVIDIA Driver
For the NVIDIA proprietary driver, it is necessary to load some kernel modules.
``` bash
# Edit the mkinitcpio configuration
vim /etc/mkinitcpio.conf
# Load the following nvidia modules
"MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm)"

# Kernel flag for GRUB - also feel free to do more GRUB configuration like timer and submenus
vim /etc/default/grub

# Add "nvidia-drm.modeset=1" 
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet nvidia_drm.modeset=1"
```
If the system is on **KDE** or other expect **GNOME** and **gdm**, regenerate the new configuration with the following commands:
``` bash
# Update the kernel moduels 
mkinitcpio -P
# Generate the GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg
```
*Useful links*:
- https://wiki.archlinux.org/title/NVIDIA#Hardware_accelerated_video_encoding_with_NVENC
#### NVENC
Enable **nvidia_uvm** to use its encoding:
``` bash
vim /etc/udev/rules.d/70-nvidia.rules
# Paste this lines => ACTION=="add", DEVPATH=="/bus/pci/drivers/nvidia", RUN+="/usr/bin/nvidia-modprobe -c 0 -u"
```
#### GDM + NVIDIA
If the system run with gdm, there are further action to do:
- Eliminate the gdm rule for nvidia;
- Force Wayland support;
- Fix nvidia suspension.
``` bash
# Disable GDM udev rules which force the use of Xorg 
ln -s /dev/null /etc/udev/rules.d/61-gdm.rules

# Edit GDM configuration
vim /etc/gdm/custom.conf 
# Edit this string "#WaylandEnable=false" => "WaylandEnable=true"

# Power managment on suspension
vim /etc/modprobe.d/nvidia-power-mgmt.conf
# Add this lines => options nvidia NVreg_PreserveVideoMemoryAllocations=1
```
If you are here, remember to regenerate the kernel configurations:
``` bash
# Update the kernel moduels 
mkinitcpio -P
# Generate the GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg
```
### Windows dual-boot with TPM
If the system run in a dual boot environment it is necessary to make grub to support TPM and sign the kernel images with key generated with microsoft cert.
``` bash
# Install the packages
pacman -S sbctl os-prober ntfs-3g

# Install GRUB with module TPM and disable Shim
grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB --modules="tpm" --disable-shim-lock
# Generate GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg

# Check if sbctl is in Setup mode
sbctl status
# sbctl key creation and generation
sbctl create-keys && sbctl enroll-keys -m
# Sign the necessary bootloader and kernel images (Check manualty in the /boot mount point)
sbctl sign -s /boot/EFI/GRUB/grubx64.efi
sbctl sign -s /boot/grub/x86_64-efi/core.efi
sbctl sign -s /boot/grub/x86_64-efi/grub.efi
sbctl sign -s /boot/vmlinuz-linux*

# (1 line command)
sbctl sign -s /boot/EFI/GRUB/grubx64.efi && sbctl sign -s /boot/grub/x86_64-efi/core.efi && sbctl sign -s /boot/grub/x86_64-efi/grub.efi && sbctl sign -s /boot/vmlinuz-linux*
```
After the chroot setup and login in the DE, do this
``` bash
# Remove the old boot entry 
sudo efibootmgr -b X -B
```
Where X is the numeric ID of the boot entry in **efibootmgr**.
*Useful links*:
- https://www.reddit.com/r/archlinux/comments/10pq74e/my_easy_method_for_setting_up_secure_boot_with/
# GNOME
---
## Checklist
- [ ] [[#Audio step]]
- [ ] [[#Debloat]]
- [ ] [[#General GNOME customization]]
- [ ] [[#Default application]]
- [ ] [[#Extension]]
- [ ] [[#Shortcut]]
## Helper
### Audio step
Modify the audio step by 2 steps.
``` bash
gsettings set org.gnome.settings-daemon.plugins.media-keys volume-step 2
```
### Debloat
Uninstall useless application that come pre installed. A example list:
```
totem yelp gnome-software gnome-tour gnome-music epiphany gnome-maps gnome-contacts gnome-logs gnome-font-viewer simple-scan orca gnome-system-monitor gnome-connections gnome-characters snapshot baobab gnome-disk-utility gnome-text-editor gnome-remote-desktop gnome-console loupe gnome-calculator
```
After remove the un-wanted desktop link in the application launcher.
``` bash
# Access with root user
su -
# Go in the .desktop directory
cd /usr/share/applications
# Do your stuff
```
### General GNOME customization
Now customize the GNOME DE to enjoy a more linear and equal experience to adapt every application to the *libadwaita* theming and fix the gtk4 vs gtk3 application layout.
#### MoreWaita
Set of extended icon in **adwaita** theme:
``` bash
paru -S morewaita
gsettings set org.gnome.desktop.interface icon-theme 'MoreWaita'
```
=> https://github.com/somepaulo/MoreWaita
#### adw-gtk3
Fix the old gtk3 theming layout.
``` bash
paru -S adw-gtk3
flatpak install org.gtk.Gtk3theme.adw-gtk3 org.gtk.Gtk3theme.adw-gtk3-dark
gsettings set org.gnome.desktop.interface gtk-theme 'adw-gtk3' && gsettings set org.gnome.desktop.interface color-scheme 'default'

```
=> https://github.com/lassekongo83/adw-gtk3
#### Bibata cursor
Cool cursor theme.
``` bash
paru -S bibata-cursor-theme-bin
```
=> https://github.com/ful1e5/Bibata_Cursor
#### Restore emoji and nerd fonts
``` bash
sudo pacman -S noto-fonts-emoji nerd-fonts
```
### Default application
Now install the *personal* default application for the workflow:
- Terminal: paru -S blackbox-terminal
- Browser => paru -S brave-bin
- Video => flatpak install flathub com.github.rafostar.Clapper
- Email => flatpak install flathub org.mozilla.Thunderbird
	- Follow this [project](https://github.com/rafaelmardojai/thunderbird-gnome-theme) to adapt into **libadwaita**.
- Document: => flatpak install flathub org.libreoffice.LibreOffice
- Note => paru -S obsidian
- Recorder => flatpak install flathub com.obsproject.Studio
- Image => flatpak install flathub org.gnome.Loupe
- Extension: paru -S extension-manager
### Extension
- Caffeine
- Clipboard indicator
- Light style
- Night theme switcher
- Removable driver menu
- Weather o'clock
- Window title is back
- Logo menu
- OSD Volume number
- Gnome 4x ui
- Blur my shell
- IP Finder
- Use avatar in quick settings
- Pop shell: https://github.com/pop-os/shell
- Just perfection
- Just another search bar
- ?battery??
- Quick setting tweaker
- ArchLinux update
- Extension list
- custom accent color
- TopHat
- Rounded window corner: https://github.com/flexagoon/rounded-window-corners
- Kstatus
### Shortcut
- Terminal: super + T
- Browser: super + F
- File: super + E
- Settings: super + I
- Obsidian: super + N
- Workspace 1: super + 1
- Workspace 2: super + 2
- Workspace 3: super + 3
- Workspace 4: super + 4
- Move to the rx workspace: super + tab
- Move to the lx workspace: super + shift + tab
- Move to the last workspace: super + 0
- Move a window to the N workspace: super + shift + 'N'
- Close application: super + Q
# Development
---
## Checklist
- [ ] [[#SSH Key]]
- [ ] [[#ZSH]]
	- [ ] [[#Eza]]
- [ ] [[#Docker]]
- [ ] [[#Pyenv and Pyvirtualenv]]
- [ ] [[#QEMU and KVM]]
## Helper
### SSH Key
> Tip: Just follow the github guide to replicate each step and be up to date to the latest authentication method.
> => https://docs.github.com/en/authentication/connecting-to-github-with-ssh/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent
### ZSH
Change the default shell into **zsh**.
``` bash
# Install zsh
sudo pacman -S zsh
# Run zsh
zsh
# Do the configuration process

# Install oh-my-zsh
sh -c "$(wget -O- https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"
```
Now copy the **.zshrc** file in the *.config* directory, to make the resource file work correctly, install this deps:
- zsh-syntax-highlighting: git clone https://github.com/zsh-users/zsh-syntax-highlighting.git ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-syntax-highlighting
- **zsh-autosuggestions**: git clone [https://github.com/zsh-users/zsh-autosuggestions](https://github.com/zsh-users/zsh-autosuggestions) ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions
- **zsh-Z**: git clone https://github.com/agkozak/zsh-z $ZSH_CUSTOM/plugins/zsh-z
- **fzf**: git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf && sudo pacman -S fd && ~/.fzf/install
After all, if you want to personalize more, install [powerlevel10k](https://github.com/romkatv/powerlevel10k)
#### Eza
Install eza for colorfull and cool *ls* command.
``` bash
# Install the package
sudo pacman -S eza

# Override ls with alias
alias  l='eza -lh  --icons=auto' # long list
alias ls='eza -1   --icons=auto' # short list
alias ll='eza -lha --icons=auto --sort=name --group-directories-first' # long list all
alias ld='eza -lhD --icons=auto' # long list dirs
```
> Note: the **alias** come already in the source file (**.zshrc**).
### Docker
Install [**docker**](https://wiki.archlinux.org/title/Docker), it allow to create container and work with save development environment.
``` bash
# Install docker
sudo pacman -S docker
# Enable the services
systemctl enable docker.service
systemctl enable docker.socket
# Add the usergroup
usermod -aG docker $USER
```
### Pyenv and Pyvirtualenv
Install the **pyenv** and **pyenv-virtualenv**(AUR) packages.
``` bash
paru pyenv

# Export the dirs in the source file
echo 'export PYENV_ROOT="$HOME/.pyenv"' >> ~/.zshrc
echo '[[ -d $PYENV_ROOT/bin ]] && export PATH="$PYENV_ROOT/bin:$PATH"' >> ~/.zshrc
echo 'eval "$(pyenv init -)"' >> ~/.zshrc

# Enable pyenv-virtualenv activation
$ echo 'eval "$(pyenv virtualenv-init -)"' >> ~/.zshrc
```
> Note: this line are already added and configured in the source file if completed the [[#ZSH]] section. Only need to install the packages.
### QEMU and KVM
Better machine virtualization to allow architecture emulation. (Useful for ROP Challenges)
``` bash
# Install the requirment packages
sudo pacman -S qemu-full virt-manager virt-viewer dnsmasq vde2 bridge-utils openbsd-netcat

# Start the service
systemctl enable libvirtd

# Edit the service configuration file to grant access
sudo vim /etc/libvirt/libvirtd.conf
# Search for:
#
# "unix_sock_group = libvirt" and uncomment
# "unix_sock_ro_perms = 0777" and uncomment
# "unix_sock_rw_perms = 0770" and uncomment

# Add the group to the user
usermod -aG libvirt $USER
```