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

also take care of annoying sudo password questions:

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

