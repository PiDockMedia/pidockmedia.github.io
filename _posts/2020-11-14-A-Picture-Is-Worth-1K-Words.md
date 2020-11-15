---
layout: post
title: A-Picture-Is-Worth-1K-Words
---

## DYI Picture Frame with RaspBerry Pi

I have Monitor. I have a Pi Zero W . . . Picture Frame! (use any Pi honestly)

WARNING: I am comfortable with working around electricity. It has been years since I shocked myself inadvertently. Be VERY careful going in to the guts of your exposed electronics.

- Things I need.

  - Capton Tape (2 inches wide is cheap and useful)
  - Tiny Computer (RaspberryPi Zero W)
  - A Monitor (1080p 20in ASUS)
  - Mini HDMI to HDMI Cable
  - HDMI 90deg adapter
  - Micro USB to USB Cable
  - Short C15 to 3 prong power

- Monitor

  - Shuck the monitor.
  - Get the Controller Board, Power Board, control buttons, and any other electronics out.
  - Capton Tape them logically to the back of the screen so everything reconnects. Put plastic sheets under them to insulate from the screen metal. Make sure you have the power control successfully included. USB ports may be akipped but could be useful for powering the board (I did not have these).
  - Plug them all back in. Make sure the monitor powers on

- Pi

  - Load Raspbian Lite on a Micro SD Card

  - Mount the micro SD card on your PC/Mac//Linux/Other Pi/Whatever

  - In the boot partition put an empty file named SSH (to enable ssh).

  - Add a file called wpa_supplicant.conf and add your wifi info to it. The example below assumes the county is US, SSID is AWESOME_WIRELESS, and password is SECRET_PASSWORD.

    `ctrl_interface=DIR=/var/run/wpa_supplicant GROUP=netdev
    update_config=1
    country=US

    network={
     ssid="AWESOME_WIRELESS"
     psk="SECRET_PASSWORD"
    }`

  - Stick that in your Pi and boot it up

  - Find it on your network and log in as the pi user/default password

  - Change the pi user password (passwd)

  - Run `sudo raspi-config` and set some things

    - Change Hostname
    - Wait for network at boot - Probably No
    - Set your Locale & TimeZone
    - Expand Filesystem
    - Enable Overscan
    - Memory Split to 128 (or 256 on Pi4) for more GPU oomph)
    - GL driver set to G1 Legacy
    - Finish & Reboot

  - Update - `sudo apt-get update -y && apt-get dist-upgrade -y`

  - Reboot

  - My pictures sit on a NAS

    - cifs-utils should be installed
    - `sudo mkdir -p /etc/samba;sudo touch /etc/samba/passwd_file; sudo chmod 600 /etc/samba/passwd_file;sudo vi /etc/samba/passwd_file`
    - Add 2 lines to that file and save it. (and learn basic vi if you need to...or replace vi with nano...find your happiness)
      - `username=<NAS Username>`
      - `password=<NAS Password>`
    - Add a line to /etc/fstab to mount this
      - `//192.168.XXX.XXX/storage/Pictures	/mnt/Pictures	cifs	credentials=/etc/samba/passwd_file,rw,domain=DOMAIN,uid=1000,gid=1000,iocharset=utf8	0	0`
    - Mount it with `mount -a`

  - Display Images

    - Impressive

