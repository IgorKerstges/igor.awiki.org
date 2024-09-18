# Installing Workstation from minimal iso

In this document, I will have a go on installing Alma linux 9.4 (current at the time of this writing) with KDE Plasma. In my prior trials, I found I was not able to setup plymouth (Plymouth is a graphical boot system and logger for Linux) properly, so during startup and close-down, the console would be flooded with kernel messages. To get this configured properly, I will be installing Alma Workstation (based on GNOME) first, which will take care of the proper plymouth config) and then replace GNOME with KDE Plasma.

## Installation

Download the minimal iso from Alma's website and start the installation: [AlmaLinux OS 9.4 Minimal ISO](https://repo.almalinux.org/almalinux/9.4/isos/x86_64/AlmaLinux-9.4-x86_64-minimal.iso)

I first use GParted to remove any existing partitions from the harddisk I'm installing to. In the installer choices, I choose:
- install Workstation
- use automatic partitioning
- enter root password and allow root to login
- create user 'igor' with password and root-user rights

After installation, I'm greeted with the plain GNOME desktop.

## Pre-requisites

1. Some household chores first:

edit dnf.conf to improve performance, add below lines:

```
sudo tee -a /etc/dnf/dnf.conf <<EOF
fastestmirror=True
max_parallel_downloads=10
defaultyes=True
EOF
```

also take care of pesky sudo password questions:

```
sudo EDITOR=nano visudo
```

Edit/activate line: %wheel	ALL=(ALL)	NOPASSWD:	ALL

2. Enable 'crb' (CodeReady Builder, formerly 'PowerTools') repository:

```
sudo dnf config-manager --set-enabled crb
```

3. enable EPEL (Extra Packages for Enterprise Linux) repository:

```
sudo dnf install epel-release
```

## Install KDE Plasma

1. First, run command to print all available package groups:

```
sudo dnf group list
```
2. Now install the packagegroup for KDE Plasma:

```
sudo dnf groupinstall 'KDE Plasma Workspaces'
```

3. Make sure the system will boot to graphical mode:

```
sudo systemctl set-default graphical.target
```

4. Switch display manager from GDM to SDDM:

```
sudo systemctl disable gdm.service
sudo systemctl enable sddm.service
sudo reboot now
```

> [!TIP|style:flat|label:GRUB]
> If GRUB shows several entries during next boot, then login to KDE Plasma and change the SDDM login screen from 'Breeze Fedora' (Default from GNOME) to 'Breeze'. This can be found in 'System Settings' --> 'Startup and Shutdown' --> 'Login Screen (SDDM)'.

## Remove GNOME

Once I'm satified with the plain setup of KDE Plasma, it is time to remove GNOME:

```
sudo dnf groupremove 'Workstation'
```

After a reboot, we now have a working KDE Plasma setup on Alma Linux 9.4 with working Plymouth graphical boot system and logger. The next steps are all about configuring it to performance, functionality and taste..

## Functional changes

### Regional Settings

'System Settings' --> 'Regional Settings'. (As I see fit)

### Bluetooth Moouse/Keyboard

'System Settings' --> 'Bluetooth'. Choose to add new device and follow steps on-screen.

### Adding Flathub sources

Open 'Discover - Software Center' --> 'Settings' and in the section 'Flatpak', check the tick-box for adding Flathub repositories.

### Konsole

Create directory `~/.ssh` and copy the ssh private key files for any remote locations to this directory. Then set proper ownership and permissions:

```
chown -R $USER:$USER ~/.ssh
chmod 700 ~/.ssh
chmod 600 ~/.ssh/*
```

### Energy saving options

'System Settings' --> 'Power Management', adjust settings to the 'mood of the day'..

Also don't forget to give the Screen Locking settings a look-over:

'System Settings' --> 'Workspace Behavior' --> 'Screen Locking' (option: 'Lock screen automatically')

### Bootsplash (for fun)

On my Dell laptop, the bootsplash shows the boring Dell logo with a small spinner below. To change this, we need to update the Plymouth graphical boot system and logger:

```
sudo dnf install plymouth plymouth-plugin* plymouth-theme* plymouth-scripts
sudo dracut -f
```

## Software

I like installing software mostly from flathub. Following packages come to mind:
- Brave Browser
- VSCodium
- Podman Desktop
...

