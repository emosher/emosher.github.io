---
title: "Linux Volumes Commands"
date: 2023-02-10T20:13:45-05:00
draft: false
---

I've never claimed to be a SysAdmin, [I only cosplay as one](https://www.jeffgeerling.com/blog/2022/cosplaying-sysadmin).  As such, it has been a while since I had to manage Linux volumes.  Every time I have to do it, I end up wading through Google results for a while before I really find what I want.  I'm 100% making this post for myself to refer to later, but maybe you will find it useful too!

## The Typical Situation
Generally I'm dealing with one, maybe two volumes.  And more often than not, all I'm trying to do is increase the size of the volume to 100% of the space on disk.  I recently hit this after installing Ubuntu server on a machine in my house.  Here's the commands I found helpful to get this done.

## Data Gathering
The following commands are how we gather data about the volumes.  In this case, we know it is a 500GB drive.

### List the Volume Group(s)
```bash
root@server:/# vgs
  VG        #PV #LV #SN Attr   VSize    VFree
  ubuntu-vg   1   1   0 wz--n- <462.71g <362.71g
root@server:/# vgdisplay
  --- Volume group ---
  VG Name               ubuntu-vg
  System ID
  Format                lvm2
  Metadata Areas        1
  Metadata Sequence No  2
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                1
  Act PV                1
  VG Size               <462.71 GiB
  PE Size               4.00 MiB
  Total PE              118453
  Alloc PE / Size       25600 / 100.00 GiB
  Free  PE / Size       92853 / <362.71 GiB
  VG UUID               DvqiV6-FyCN-l6UA-B5kX-9M4E-FsrU-FkwM76
```

Of importance here, is we can see that the volume group is of a size of `<462.71 GiB` - that means we don't need to add any storage to the volume group.

### List the Volume(s)
```bash
root@server:/# lvs
  LV        VG        Attr       LSize   Pool Origin Data%  Meta%  Move Log Cpy%Sync Convert
  ubuntu-lv ubuntu-vg -wi-ao---- 100.00g

root@server:/# lvdisplay
  --- Logical volume ---
  LV Path                /dev/ubuntu-vg/ubuntu-lv
  LV Name                ubuntu-lv
  VG Name                ubuntu-vg
  LV UUID                nTNiMs-YMl2-euj3-bUEp-RVWd-rZhy-VOGSLd
  LV Write Access        read/write
  LV Creation host, time ubuntu-server, 2023-02-10 17:22:20 +0000
  LV Status              available
  # open                 1
  LV Size                100.00 GiB
  Current LE             25600
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:0
```

Here we can see we only have one volume at `/dev/ubuntu-vg/ubuntu-lv`, and it is only 100 GiB.  So how do we use the rest of the disk?

### Resize the Volume

There's 2 steps here.  First we need to extend the volume, to tell Linux to use the rest of the disk.  Next, we have to resize the filesystem so we can use the space we've just allocated.  

```bash
root@server:/# lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv
  Size of logical volume ubuntu-vg/ubuntu-lv changed from 100.00 GiB (25600 extents) to <462.71 GiB (118453 extents).
  Logical volume ubuntu-vg/ubuntu-lv successfully resized.

root@server:/# resize2fs /dev/ubuntu-vg/ubuntu-lv
resize2fs 1.46.5 (30-Dec-2021)
Filesystem at /dev/ubuntu-vg/ubuntu-lv is mounted on /; on-line resizing required
old_desc_blocks = 13, new_desc_blocks = 58
The filesystem on /dev/ubuntu-vg/ubuntu-lv is now 121295872 (4k) blocks long.
```

We can confirm everything worked with `df -h`, and we can see that we now have 455G of space on the drive.

```bash
root@server:/# df -h
Filesystem                         Size  Used Avail Use% Mounted on
tmpfs                              774M  1.7M  772M   1% /run
/dev/mapper/ubuntu--vg-ubuntu--lv  455G   12G  424G   3% /
tmpfs                              3.8G     0  3.8G   0% /dev/shm
tmpfs                              5.0M     0  5.0M   0% /run/lock
/dev/sda2                          2.0G  246M  1.6G  14% /boot
/dev/sda1                          1.1G  6.1M  1.1G   1% /boot/efi
tmpfs                              774M  4.0K  774M   1% /run/user/1000
```