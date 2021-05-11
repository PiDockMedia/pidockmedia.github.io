---
layout: post
title: Eddie-LizardFS!
---

## A Robot looks at a Lizard(FS)

LizardFS might have more flexability than Gluster. Thisn is my journey towards giving it a try.

https://lizardfs-docs.readthedocs.io/en/latest/adminguide/installation.html

- It has been marginally maintained the last few years so this might be an exercise without a good end.

- The URLs for the binaries are incorrect in the documentation

  - They say - http://packages.lizardfs.com/
  - But it's - https://dev.lizardfs.com/old-packages/

- ```
  wget -O - https://dev.lizardfs.com/old-packages/lizardfs.key | apt-key add -
  ```

- ```
  echo "deb https://dev.lizardfs.com/old-packages/ubuntu/$(lsb_release -sc) $(lsb_release -sc) main" > /etc/apt/sources.list.d/lizardfs.list
  echo "deb-src https://dev.lizardfs.com/old-packages/ubuntu$(lsb_release -sc) $(lsb_release -sc) main" >> /etc/apt/sources.list.d/lizardfs.list
  ```

