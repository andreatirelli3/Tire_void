# Arch Linux guide
---
## Pre Requirements
First of all get the latest **Arch Linux** ISO image, you can download it [here](https://archlinux.org/download/). It is necessary a bootable USB:
- at least 4GB
- optimal 8GB

To make a bootable USB you can:
- [Rufus](https://rufus.ie/it/) (Windows)
- [Impression](https://apps.gnome.org/it/Impression/) (Linux)

Based on your system, enter in the boot menu, some common keys:
- DEL
- F1
- F8
- F11
- F12

But check your motherboard and BIOS specification of it

> Warning, to boot inside the Arch Linux USB it is necessary to disable **Secure boot**.

Once inside the media installation, just:
1. Update the package manager server
``` bash
pacman -Syy
```
2. Install the latest version of these packages:
	- [archlinux-keyring](https://archlinux.org/packages/core/any/archlinux-keyring/)
	- [archinstall](https://wiki.archlinux.org/title/archinstall)
``` bash
pacman -S archlinux-keyring archinstall
```

Wifi problem? It is easy, use **iwctl** tool to connect to your wifi network.
- [iwctl(1) - Arch manual pages](https://man.archlinux.org/man/iwctl.1)

Use **archinstall** package tool to install the system.
``` bash
archinstall
```
### Easy installation via SSH
For an easy installation of the system via your prefer host machine, configure SSH.
1. Set a password for root
``` bash
passwd
<insert_new_root_pwd>
```
2. Check the IP of the machine
``` bash
ip address
```
3. Connect to it (this in your prefer host machine)
``` bash
ssh root@<ip>
```

> By default the **sshd** service should be running and the port 22 open. If this isn't the case do the following.

``` bash
systemctl start sshd.service
```

- [How to setup SSH access to Arch Linux ISO (livecd) booted computer?](https://unix.stackexchange.com/questions/352139/how-to-setup-ssh-access-to-arch-linux-iso-livecd-booted-computer)
## System configuration via chroot
Now configure the following packages/services based on your preferences.
Start by install an **AUR Helper**:
- [paru](https://github.com/Morganamilo/paru)
- [yay](https://github.com/Jguer/yay)

> For more details base on which AUR Helper choose, follow their guide shown in their repository. Following it is shown the paru installation.

``` bash
git clone https://aur.archlinux.org/paru.git
cd paru
makepkg -si

# Update the package manager and AUR helper servers
sudo pacman -Syy && paru
```

Next proceed to configure the [Chaotic AUR](https://aur.chaotic.cx/docs) repository.

``` bash
sudo pacman-key --recv-key 3056513887B78AEB --keyserver keyserver.ubuntu.com
sudo pacman-key --lsign-key 3056513887B78AEB
sudo pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-keyring.pkg.tar.zst'
sudo pacman -U 'https://cdn-mirror.chaotic.cx/chaotic-aur/chaotic-mirrorlist.pkg.tar.zst'
```

Add the repository to the package manager list.

```bash
etc/pacman.conf
---------------------------------------------------------------------------------
[chaotic-aur]  
Include = /etc/pacman.d/chaotic-mirrorlist
```

Install the package [pacman-contrib](https://github.com/archlinux/pacman-contrib) for enable the package cache cleaning.

``` bash
paru -S pacman-contrib

# Enable the service that time 7 days to clean the cache
sudo systemctl enable paccache.timer
```

- [Cleaning the package cache - Arch wiki](https://wiki.archlinux.org/title/Pacman#Cleaning_the_package_cache)

Lastly edit the file `/etc/pacman.conf` to enable the tag **Color** e **ILoveCandy**.

``` bash
/etc/pacman.conf
---------------------------------------------------------------------------------
...
Color
ILoveCandy
...
```

For better mirrorlist server install [reflector](https://wiki.archlinux.org/title/reflector).

``` bash
paru -S reflector rsync curl

# Backup the current mirrorlist
sudo cp /etc/pacman.d/mirrorlist /etc/pacman.d/mirrorlist.bak
# Generate the new one with reflector
sudo reflector --latest 20 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

Create the file `/etc/xdg/reflector/reflector.conf` with the configuration that reflector service will use to update periodically the system mirrorlist.

``` bash
/etc/xdg/reflector/reflector.conf
---------------------------------------------------------------------------------
# Reflector configuration file for the systemd service.
#
# Empty lines and lines beginning with "#" are ignored. All other lines should
# contain valid reflector command-line arguments. The lines are parsed with
# Python's shlex modules so standard shell syntax should work. All arguments are
# collected into a single argument list.
#
# See "reflector --help" for details.

# Recommended Options

# Set the output path where the mirrorlist will be saved (--save).
--save /etc/pacman.d/mirrorlist

# Select the transfer protocol (--protocol).
--protocol https

# Select the country (--country).
# Consult the list of available countries with "reflector --list-countries" and
# select the countries nearest to you or the ones that you trust. For example:
--country Italy,Germany,France

# Use only the most recently synchronized mirrors (--latest).
--latest 20

# Sort the mirrors by download speed (--sort).
--sort rate
```

Activate the `reflector.service` in the system.

``` bash
sudo systemctl enable reflector.service
```

Now activate the desired system service that want to be started every time that the system boot inside of it.
- **SSH**
- **Bluetooth**

``` bash
sudo systemctl enable sshd

paru -S bluez bluez-utils
sudo systemctl enable bluetooth
```

Also it is possible to enable **experimental features** and **dual controller-mode** to better compatibility with Apple device and BT headphones.
- [Bluetooth - Arch wiki](https://wiki.archlinux.org/title/bluetooth)

``` bash
/etc/bluetooth/main.conf
---------------------------------------------------------------------------------
[General]
...
ControllerMode = dual
...
# Enables D-Bus experimental interfaces
# Possible values: true or false
Experimental = true

# Enables kernel experimental features, alternatively a list of UUIDs
# can be given.
# Possible values: true,false,<UUID List>
# Possible UUIDS:
...
# Defaults to false.
KernelExperimental = true
```
### Additional steps
If the system is a laptop, might be useful to have different types of power profile to increase or decrease battery usage. For that install:
- [power-profiles-daemon](https://archlinux.org/packages/extra/x86_64/power-profiles-daemon/) - native support by GNOME
- [TPL](https://wiki.archlinux.org/title/TLP)

> The following command show how to enable power management via **power-profiles-daemon**.

``` bash
paru -S power-profiles-daemon
sudo systemctl enable power-profiles-daemon.service
```

If in the system is installed a NVIDIA graphic card, it is necessary to load the correct kernel module and GRUB flags to make it work properly.
``` bash
/etc/mkinitcpio.conf
---------------------------------------------------------------------------------
# Add the kernel nvidia modules
MODULES=(... nvidia nvidia_modeset nvidia_uvm nvidia_drm)
```

``` bash
/etc/default/grub
---------------------------------------------------------------------------------
# Add "nvidia-drm.modeset=1" 
GRUB_CMDLINE_LINUX_DEFAULT="loglevel=3 quiet nvidia_drm.modeset=1"
```

Now make the system available to use the **NVidia ENCoder - (NVENC)**.

``` bash
/etc/udev/rules.d/70-nvidia.rules
---------------------------------------------------------------------------------
ACTION=="add", DEVPATH=="/bus/pci/drivers/nvidia", RUN+="/usr/bin/nvidia-modprobe -c 0 -u"
```

If the system run in a GNOME desktop environment, so the login manager is **GDM**, need to for it to show **Wayland** entry even if in the system run with a NVIDIA card.

``` bash
# Disable GDM udev rules which force the use of Xorg 
ln -s /dev/null /etc/udev/rules.d/61-gdm.rules
```

``` bash
/etc/gdm/custom.conf
---------------------------------------------------------------------------------
"WaylandEnable=true"
```

``` bash
/etc/modprobe.d/nvidia-power-mgmt.conf
---------------------------------------------------------------------------------
options nvidia NVreg_PreserveVideoMemoryAllocations=1
```

Finally regenerate all the necessary configuration, kernel and GRUB.

``` bash
# Update the kernel moduels 
mkinitcpio -P
# Generate the GRUB configuration
grub-mkconfig -o /boot/grub/grub.cfg
```

In a dual boot system, with a recent Windows installation, which require [secure boot](https://wiki.archlinux.org/title/Unified_Extensible_Firmware_Interface/Secure_Boot), it is necessary to configure it and make [GRUB](https://wiki.archlinux.org/title/GRUB) support it.
- [GRUB Tips and tricks - Arch wiki](https://wiki.archlinux.org/title/GRUB/Tips_and_tricks)
- [My easy method for setting up Secure Boot with GRUB - Reddit](https://www.reddit.com/r/archlinux/comments/10pq74e/my_easy_method_for_setting_up_secure_boot_with/)

``` bash
paru -S sbctl os-prober ntfs-3g

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
```

To be more fast, copy the 1line command:

``` bash
sbctl sign -s /boot/EFI/GRUB/grubx64.efi && sbctl sign -s /boot/grub/x86_64-efi/core.efi && sbctl sign -s /boot/grub/x86_64-efi/grub.efi && sbctl sign -s /boot/vmlinuz-linux*
```

> By doing those command, in the efi table will remain the previous entry of the grub boot loader that didn't support TPM, to clean the entry follow the next commands. 

> Warning, be careful with these command, might broke the system, if you are not sure or insecure, please read this documentation.

- [efibootmgr - Arch manual pages](https://man.archlinux.org/man/efibootmgr.8.en)
- [Use Linux efibootmgr Command to Manage UEFI Boot Menu - LinuxBabe](https://www.linuxbabe.com/command-line/how-to-use-linux-efibootmgr-examples)

First enter as `root` and in the `/boot/EFI` and remove the directory called `BOOT`. 
``` bash
# Remove the old boot entry 
sudo efibootmgr -b X -B
```
