# nautilus で PDF 等のサムネイルが生成されない

## 解決方法

```
sudo sysctl -w kernel.apparmor_restrict_unprivileged_userns=0 
```

https://www.reddit.com/r/Ubuntu/comments/1tbck88/thumbnails_not_working_on_ubuntu_24044_lts
