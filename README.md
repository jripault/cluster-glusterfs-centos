Simple Vagrantfile with 3 VMs:

* 2 glusterfs servers
* 1 client

IPs are fixed for simplicity.

All setup is done by provisioning. Then juste run these commands in root to initialize cluster:

On server1:

```
gluster peer probe 192.168.10.22
gluster peer status
mkdir /DATA
gluster volume create DATA replica 2 192.168.10.21:/DATA 192.168.10.22:/DATA force
gluster volume start DATA
gluster volume info
cd /DATA/
```

On server2:

```
gluster peer probe 192.168.10.21
gluster peer status
mkdir /DATA
gluster volume info
cd /DATA/
```

On client:

```
mkdir /DATA
mount -t glusterfs 192.168.10.21:/DATA /DATA
df -hT | grep DATA
touch /DATA/test
```