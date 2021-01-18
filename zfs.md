# ZFS Notes

## Sources

https://blog.quindorian.org/2019/08/how-to-install-proxmox-and-setup-a-zfs-pool.html/#more-2962


## Erasing Disk Partition Table

## FreeBSD CLI

```
# geom disk list
# geom disk list ada5
```

More: https://www.cyberciti.biz/faq/freebsd-hard-disk-information/

## zpool CLI

To setup a RAIDz2 pool:

```
zpool create NAME -o ashift=12 raidz2 /dev/disk/by-id/DISK1 /dev/disk/by-id/DISK2 /dev/disk/by-id/DISK3 /dev/disk/by-id/DISK4 /dev/disk/by-id/DISK5
```

To look at the pool status:
```
zpool status
```

To turn compression on for the pool:

```
zfs set compression=lz4 POOLNAME
```

Create a dataset for ISO storage:

```
zfs create POOL/ISO
```

## ZFS Command History

On my (old) nass:

```
zpool create -f tank raidz2 /dev/ada0 /dev/ada1 /dev/ada2 /dev/ada3 /dev/ada4
zfs create -o aclinherit=restricted -o aclmode=discard -o atime=off -o casesensitivity=sensitive -o compression=lz4 -o dedup=off -o sync=standard tank/home
zfs create -o aclinherit=restricted -o aclmode=discard -o atime=off -o casesensitivity=sensitive -o compression=lz4 -o dedup=off -o sync=standard tank/downloads
zfs create -o aclinherit=restricted -o aclmode=discard -o atime=off -o casesensitivity=sensitive -o compression=off -o dedup=off -o sync=standard tank/moviesMore
zfs create -o aclinherit=restricted -o aclmode=discard -o atime=off -o casesensitivity=sensitive -o compression=off -o dedup=off -o sync=standard tank/movies
zfs create -o aclinherit=restricted -o aclmode=discard -o atime=off -o casesensitivity=sensitive -o compression=lz4 -o dedup=off -o sync=standard tank/music

zpool offline tank 10462354999995531015
zpool import -d /dev -f -a
zpool replace tank 10462354999995531015 ada1
zpool import -d /dev -f -a
zpool import -d /dev -f -a

```

And:

```
[alex@nass /dev]$ zdb -C | grep ashift
            ashift: 12
```
