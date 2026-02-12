# KERNEL PANIC unable to mount root fs

## 起きた事

GRUB 画面で `Ubuntu` を選択すると `KERNEL PANIC!` というメッセージが出現

```
KERNEL PANIC!
Please reboot your computer.
VFS: Unable to mount root fs on unknown-block(0,0)
```

## 解決した手順

GRUB にて、 `Ubuntu` ではなく `Advanced options for Ubuntu` を選択

```
 Ubuntu
*Advanced options for Ubuntu
 Memory test(memtest86+x64.efi)
 Memory test(memtest86+x64.efi, serial console)
 UEFI Firmware Settings
```

最新のバージョン(6.17)に問題がある可能性が疑われるので、最新より1つ古いバージョン(6.14)を選択

```
 Ubuntu, with Linux 6.17.0-14-generic
 Ubuntu, with Linux 6.17.0-14-generic (recovery mode)
*Ubuntu, with Linux 6.14.0-37-generic
 Ubuntu, with Linux 6.14.0-37-generic (recovery mode)
```

OS の起動自体はできた。

ログインしてバージョンを確認

```sh
$ uname -r
6.14.0-37-generic
```

問題のある(と疑われる)バージョンを確認

```sh
$ dpkg --list 'linux-image*' 'linux-headers*' | grep 6.17 -i
iF  linux-headers-6.17.0-14-generic        6.17.0-14.14~24.04.1 amd64        Linux kernel headers for version 6.17.0
iU  linux-headers-generic-hwe-24.04        6.17.0-14.14~24.04.1 amd64        Generic Linux kernel headers
iF  linux-image-6.17.0-14-generic          6.17.0-14.14~24.04.1 amd64        Signed kernel image generic
ii  linux-image-generic-hwe-24.04          6.17.0-14.14~24.04.1 amd64        Generic Linux kernel image
un  linux-image-unsigned-6.17.0-14-generic <なし>               <なし>       (説明 (description) がありません)
```

```sh
$ sudo apt update
```

問題のある(と疑われる)バージョンを削除

```sh
$ sudo apt purge -y linux-image-6.17.0-14-generic linux-headers-6.17.0-14-generic linux-image-unsigned-6.17.0-14-generic
```

```sh
$ sudo apt --fix-broken install
$ sudo apt autoremove --purge -y
$ sudo update-grub
$ reboot
```
