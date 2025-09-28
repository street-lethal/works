# SSD に書き込みできない(読取専用になる)

## 起きた事
ディスクをマウントはできるが書き込みができない
(Windows とのデュアルブートの場合、 Windows 使用後によく発生する)

## 解決方法

1. 以下で読取専用になっているディスクを調べる
   ```shell
   mount -l | grep -w ro
   ```
2. 対象のディスクに対して ntfsfix コマンドを実行 ( `xxx` の部分は `sdb2` 等)
   ```shell
   sudo ntfsfix /dev/xxx
   ```
3. 再起動
   ```shell
   reboot
   ```

## 実行例

```shell
$ mount -l | grep -w ro
/dev/sda1 on /boot/efi type vfat (rw,relatime,fmask=0022,dmask=0022,codepage=437,iocharset=iso8859-1,shortname=mixed,errors=remount-ro)
/dev/sdb2 on /mnt/Data type fuseblk (ro,nosuid,nodev,relatime,user_id=0,group_id=0,allow_other,blksize=4096,x-gvfs-show) [Data]
```

```shell
$ sudo ntfsfix /dev/sdb2
Mounting volume... The disk contains an unclean file system (0, 0).
Metadata kept in Windows cache, refused to mount.
FAILED
Attempting to correct errors... 
Processing $MFT and $MFTMirr...
Reading $MFT... OK
Reading $MFTMirr... OK
Comparing $MFTMirr to $MFT... OK
Processing of $MFT and $MFTMirr completed successfully.
Setting required flags on partition... OK
Going to empty the journal ($LogFile)... OK
Checking the alternate boot sector... OK
NTFS volume version is 3.1.
NTFS partition /dev/sdb2 was processed successfully.
```
