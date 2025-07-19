# VirtualBox 起動不可

## 起きた事
VirtualBox 仮想マシン起動を試みると、以下のエラーが出て起動しない

```
VirtualBox can't operate in VMX root mode. Please disable the KVM kernel extension, recompile your kernel and reboot (VERR_VMX_IN_VMX_ROOT_MODE).
```


## 試みた事

```shell
cd /etc/modprobe.d/
sudo touch black-list-kvm.conf
sudo vi black-list-kvm.conf # 内容は以下
reboot
```

```
blacklist kvm
blacklist kvm_intel
```

## 結果

起動できた

## 参考

https://discuss.cachyos.org/t/virtualbox-stopped-working/4287/2
