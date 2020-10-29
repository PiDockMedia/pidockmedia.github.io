---
layout: post
title: Yodel-Odroid!
---

## Odroid HC2 for the Robot Army

Drobo frustrating. Odroid Good. One day I might insert some commentary here but really this is about my setup.

- Need an Odroid, a 12v 2amp capable adapter, a network connection, a micro SD, and a sata disk.
  - check, check from my shucked drive, lots of ports 2 ft away, 32GB $7 win, prime day shuckable 12GB WD Easy Store works with no mods
- Get the latest Ubuntu minimal image from Odroid
- Put the image on the Micro SD Card
  - Balena Etcher is my goto
- Micro SD then Network, then Power
- Find the device named odroid
- ssh in to the IP as root pw odroid
- Change the root password
  - passwd
- Create a sudo enabled user other than root and test
  - adduser pgtips
  - usermod -aG sudo pgtips
  - su - pgtips
  - sudo ls -lah /root
  - exit 
  - ssh-copy-id pgtips@*ip* #and log in as your new sudo enabled user
- Disable ssh root logins
  - vi /etc/ssh/sshd_config and change PermitRootLogoin to no
  - systemctl restart sshd
  - Go ahead and test a root ssh
- Set Hostname
  - sudo hostnamectl set-hostname odr00
  - vi /etc/hosts and change as well
- Good time to reboot and login and check hostname
- Install the Avahi daemon
  - sudo apt install avahi-daemon
- Let's go ahead and get those upgrades done
  - sudo apt update
  - sudo apt dist-upgrade
  - sudo reboot
- Set up cockpit (maybe)
  - sudo apt install cockpit
