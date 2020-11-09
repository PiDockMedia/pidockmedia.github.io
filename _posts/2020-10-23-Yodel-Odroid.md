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
  - adduser pi
  - usermod -aG sudo pi
  - su - pi
  - sudo ls -lah /root
  - exit
  - exit
  - ssh-copy-id pi@*ip* #and log in as your new sudo enabled user
- Disable ssh root logins
  - sudo vi /etc/ssh/sshd_config and change PermitRootLogoin to no
  - sudo systemctl restart sshd
  - Go ahead and test a root ssh
- Set Hostname
  - sudo hostnamectl set-hostname odr00
  - vi /etc/hosts and change as well
- Static IP
  - I set this up later in the process but this is best done here.
  - nmicli and get the name of the network device
  - Change the name of the connection to something sensical. I am old school
    - nmcli con edit id "Wired connection 1"
    - set connection.id eth0
    - save
    - quit
  - Set your static
    - nmcli con mod "eth0" ipv4.addresses 192.168.0.31/24
    - nmcli con mod "eth0" ipv4.gateway 192.168.0.1
    - nmcli con mod "eth0" ipv4.dns "192.168.0.1"
    - nmcli con mod "eth0" ipv4.method manual
    - nmcli con up "eth0"
- Good time to reboot and login and check hostname
- Install the Avahi daemon
  - sudo apt install avahi-daemon
- Let's go ahead and get those upgrades done
  - sudo apt update
  - sudo apt dist-upgrade
  - sudo reboot
- Set up big disk
  - apt-get install gdisk (not necessary but cgdisk is fast and easy)
  - sudo blkid
  - sudo cgdisk /dev/sda
  - sudo mkfs -t ext4 /dev/sda1
  - sudo mkdir /mnt/brick01
  - Run sudo blkid again, note the UUID of your /dev/sda1 partition and add it into /etc/fstab (make a backup of fstab by installing etckeeper - this file is important): UUID="b4c93..."  /mnt/brick01  ext4  defaults  0  2
- Gluster
  - Install the packages you need
    - sudo apt-get update
    - sudo apt-get install glusterfs-server attr
  - From odr00 create a trustedpool of GlusterFS nodes
