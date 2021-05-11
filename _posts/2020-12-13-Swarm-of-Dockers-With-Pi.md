---
layout: post
title: Swarm-of-Dockers-With-Pi
---

## Pi Based Docker Swarm

I have 3 RaspberryPi 3B+ that were intended for...something...but they can run docker swarm! I did this with raspberrypios. It might get rebuilt with Ubuntu for consistency as my RockPi Xs, Odroids, and Intel NUC are all Ubuntu. Both are debian based so...maybe 2 flavoirs will work.

Raspberryos on RPI

- Install the 32bit image using Balena/Raspberyy Pi Imager/Whatever (Pi3s don't need 64bit. Maybe if I get 4s)
  - Mount the boot volume and touch the ssh file to make sure ssh gets turned on.

Docker Swarm take 1

https://howchoo.com/g/njy4zdm3mwy/how-to-run-a-raspberry-pi-cluster-with-docker-swarm
https://blog.ruanbekker.com/blog/2019/05/10/running-a-ha-mysql-galera-cluster-on-docker-swarm/

- raspi-config

   - Set name, locale, TZ, upgrade, expand disk, predictable network names

- sudo -s

- exit

- sudo raspi-config

- sudo -s

- curl -sSL https://get.docker.com | sh

- sudo usermod -aG docker pi

- docker swarm init --advertise-addr 192.168.0.220

   Swarm initialized: current node (n2ps3j4mbi0lzvjawwhwb7e38) is now a manager.

To add a worker to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0yf1ozk4x9xn3f4fb0crl8muutmfahf9s79say9t92r9p735au-as3p11umnb526w3z1mawfb8ci 192.168.0.220:2377

To add a manager to this swarm, run 'docker swarm join-token manager' and follow the instructions.

$ docker swarm join-token manager
To add a manager to this swarm, run the following command:

    docker swarm join --token SWMTKN-1-0yf1ozk4x9xn3f4fb0crl8muutmfahf9s79say9t92r9p735au-825xvsd7wm9gthohkes87dxcr 192.168.0.220:2377

pi@rpi01:~ $ docker swarm join --token SWMTKN-1-0yf1ozk4x9xn3f4fb0crl8muutmfahf9s79say9t92r9p735au-825xvsd7wm9gthohkes87dxcr 192.168.0.220:2377
This node joined a swarm as a manager.

pi@rpi02:~ $ docker swarm join --token SWMTKN-1-0yf1ozk4x9xn3f4fb0crl8muutmfahf9s79say9t92r9p735au-as3p11umnb526w3z1mawfb8ci 192.168.0.220:2377
This node joined a swarm as a worker.

pi@rpi03:~ $ docker swarm join --token SWMTKN-1-0yf1ozk4x9xn3f4fb0crl8muutmfahf9s79say9t92r9p735au-as3p11umnb526w3z1mawfb8ci 192.168.0.220:2377
This node joined a swarm as a worker.

ssh pi@docker1
sudo docker service create \
        --name viz \
        --publish 8080:8080/tcp \
        --constraint node.role==manager \
        --mount type=bind,src=/var/run/docker.sock,dst=/var/run/docker.sock \
        alexellis2/visualizer-arm:latest

Put a passwd_file in /etc/samba/ with the shared storage mount password

pi@rpi03:~ $ sudo sudo mkdir /mnt/swarm01
pi@rpi03:~ $ sudo vi /etc/fstab
pi@rpi03:~ $ cat /etc/fstab
proc            /proc           proc    defaults          0       0
PARTUUID=a714761d-01  /boot           vfat    defaults          0       2
PARTUUID=a714761d-02  /               ext4    defaults,noatime  0       1

# a swapfile is not a swap partition, no line here
#   use  dphys-swapfile swap[on|off]  for that
#NFS
//192.168.0.176/sto/docker/swarm01      /mnt/swarm01    cifs    credentials=/etc/samba/passwd_file,rw,domain=WINERY,uid=1000,gid=1000,iocharset=utf8    0       0
###EOF

pi@rpi03:~ $ sudo sudo mount /mnt/swarm01
pi@rpi03:~ $ df -h
Filesystem                          Size  Used Avail Use% Mounted on
/dev/root                            30G  1.7G   27G   6% /
devtmpfs                            431M     0  431M   0% /dev
tmpfs                               463M     0  463M   0% /dev/shm
tmpfs                               463M  7.1M  456M   2% /run
tmpfs                               5.0M  4.0K  5.0M   1% /run/lock
tmpfs                               463M     0  463M   0% /sys/fs/cgroup
/dev/mmcblk0p1                      253M   55M  198M  22% /boot
tmpfs                                93M     0   93M   0% /run/user/1000
//192.168.0.176/sto/docker/swarm01   64T   14T   51T  22% /mnt/swarm01

$ curl -L https://portainer.io/download/portainer-agent-stack.yml -o portainer-agent-stack.yml
$ vi portainer-agent-stack.yml

	-     volumes:
	  - /mnt/swarm01/appdata/portainer/data:/data
	- Remove unneeded Volumes section at bottom
$ docker stack deploy --compose-file=portainer-agent-stack.yml portainer

$ vi pihole-stack.yml

	- An amalgamation from various posts

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

  - Install vim (come on kid, everyone's doing it)

  - Run `sudo raspi-config` and set some things

    - Change Hostname
    - Autologin Text console, automatically logged in as 'pi' user
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
      - `//192.168.XXX.XXX/storage/Pictures	/mnt/pictures	cifs	credentials=/etc/samba/passwd_file,rw,domain=DOMAIN,uid=1000,gid=1000,iocharset=utf8	0	0`
    - Create a mountpoint - mkdir /mnt/pictures
    - Mount it with `mount -a`

  - Display Images

    - Impressive
      - sudo apt-get install impressive
      - Pick your directory full of pictures 
    - pi

