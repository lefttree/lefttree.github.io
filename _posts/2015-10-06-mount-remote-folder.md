---
layout: post
title: NFS Mount Remote Folder
cover: cover.jpg
category: Linux
tags: [Linux]
---

To mount a remote folder, popular options are

- NFS
- Samba/CIFS
- SSHFS

`SSHFS` mount remote filesystems via `ssh`.

`Samba` will alow Windows and Unix machines to access the remote folder.

`NFS` authenticate just via IP(no usernames....faster). **It should only be used inside your non-hostile LAN**

##NFS 

server 

`apt-get install nfs-server portmap nfs-common`

client

`apt-get install nfs-client nfs-common`

server

put the folders in `/etc/exports`

`/path_to_tmp_folder/tmp 192.168.0.2(rw,sync,no_subtree_check,no_root_squash)`

>192.168.0.2 should be the IP of the machine you wanna share with

Then:

```
exportfs -ra
/etc/init.d/nfs-kernel-server restart
/etc/init.d/portmap restart
```

client

`sudo mount -o soft,intr,rsize=8192,wsize=8192 server_ip:/path_to_tmp_folder/tmp /local_path_to_empty_tmp_folder/tmp`

auto mount, put in `/etc/fstab`

`server_ip:/path_to_tmp/tmp /local_empty_folder/tmp nfs rsize=16384,wsize=16384,rw,auto,nolock`

##Reference

- [nfs, samba/cifs, sshfs](http://unix.stackexchange.com/questions/62677/best-way-to-mount-remote-folder)
- [how to mount using nfs](http://superuser.com/questions/300662/how-to-mount-a-folder-from-a-linux-machine-on-another-linux-machine)

