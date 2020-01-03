## Get the iso

You can get an iso-hybrid of debian [here](https://www.debian.org/CD/live/)
Once you get the iso, there are 2 steps :
* formating the drive
* copying to the drive

## Formating the drive

You need :
* a first partition in FAT with boot flag,
* and an other ext4 partition with label persistence. The label "persistence" is mandatory.

### Copying to the drive
Lets copy to the FAT and the ext partition.

### FAT

You have to mount the FAT partition, lets admit to `/mnt`. Then unarchive the iso to /mnt.

```
# mount /dev/sdd1 /mnt
# cd /mnt
# 7z x ~/iso/debian-live.iso
```

We need some changes on the files.
```
# mv isolinux syslinux
# for f in $( grep -FRil "splash quiet" ); do echo $f; LANG=C sed 's/splash quiet/persistence /;s/quiet splash/persistence /' -i $f; done;
# cd syslinux
# rename "s/iso/sys/" iso*
# cd ~
# sync
# umount /dev/sdd1
```

### Ext4

```
# mount /dev/sdd2 /mnt
# cd /mnt
# echo "/ union" > persistence.conf 
# sync
# umount /dev/sdd2
```

## Now time to boot it !
