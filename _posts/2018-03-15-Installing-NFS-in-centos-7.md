---
layout: post
title: Installing NFS in Centos 7
tags: devops nfs
---

To export /nfsshare as the mount directory.

1. Install NFS and dependent packages

```shell
yum install nfs-utils
systemctl enable nfs-server.service
systemctl start nfs-server.service
```
2. Prepare the shared directory

```shell
mkdir /nfsshare
chown nfsnobody:nfsnobody /nfsshare
chmod 755 /nfsshare
```

3. Add the shared directory entry in the exports file

```shell
cat "/nfsshare 192.168.1.0/24(rw,sync,no_subtree_check)" >> /etc/exports
exportfs -a
```
#### Notes	

- To check the mount from another machine

```shell
showmount -e NFS_IP
```
- To mount the directory in another machine
	- Install `nfs-utils` in centos or `nfs-common` in ubuntu
	- Prepare the mount directory
	```shell
	sudo mkdir -p /mount/mntlocation
	sudo chattr +i /mount/mntlocation
	sudo mount NFS_IP:/nfsshare /mnt/mntlocation
	```
	- To check and unmount
	```shell
	mount #To check
	umount /mnt/mntlocation
	```
