---
layout: post
title: PiDockMedia - The Robot Swarm!
---

## Odroid HC2 for the Robot Army

Drobo is frustrating. Pi tastes good and has it's place in computing but not in serving some media.

- The solution today
  - Cloud based virtual with the magic gee-sweet unlimited storage with rclone (Oh no, this may be going to prohibitively expensive!)
  - Intel NUC i7 with docker, gee-sweet with rclone
  - Drobo with storage
- The new plan
  - Network
  - Distributed Storage
    - Hardware
      - Odroid H2 Disk sleds (x6)
      - WD shucked "Cheap NAS" disks (x6)
      - Network switch
        - Currently sharing Netgear 16 port
        - Might move to discrete 8 port Trendnet I have on hand
        - 6 ports for Odroid, one to backup device(s), one upstream
      - Software
        - OS - Ubuntu 20.04LTS
        - Format - Standard Linux
        - Local FS - XFS
        - Gluster
          - Dispersed Volume?
        - File access
          - NFS
          - SMB
          - GlusterFS
  - Distributed X86
  - Distributed Raspberry Pi
  - Disaster protection
    - Local
      - Disk Based for performance
      - Hardware
        - Old Silverstone case with 8 sata bays and power supply
        - 2x Raspberry Pi 3B+
        - 2x 4 sata port Hats
        - 8x 4TB Drives (have on hand from initial Drobo build)
      - Software (TBD)
