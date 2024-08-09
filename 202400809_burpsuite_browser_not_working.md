# マザーボード故障・復旧記録

## 起きた事
Burp Suite のブラウザーが起動しない

## 起きた事(詳細)
- Burp Suite 自体は起動する
- "Open browser" ボタン押下しても何も起動しない(エラーが出るわけでもない)
- "Help" メニュー内の "Health check for Burp's browser" を実行すると "Checking headless browser" の項目で Failed となる
  - 再現できなくなってしまったので、エラーメッセージは不明

## 試みた事

```shell
sudo touch /etc/apparmor.d/burpbrowser
sudo vi /etc/apparmor.d/burpbrowser # 記載内容は下記
sudo apparmor_parser -r /etc/apparmor.d/burpbrowser
```

```
# This profile allows everything and only exists to give the
# application a name instead of having the label "unconfined"

abi <abi/4.0>,
include <tunables/global>

profile burp-browser @{HOME}/BurpSuiteCommunity/burpbrowser/**/chrome flags=(unconfined) {
  userns,

  # Site-specific additions and overrides. See local/README for details.
  include if exists <local/chrome>
}
```

## 結果

- "Help" メニュー内の "Health check for Burp's browser" を実行するとすべての項目が Success となった
- "Open browser" ボタン押下でブラウザーも開くようになった

## 参考

https://forum.portswigger.net/thread/burp-s-browser-renderer-is-not-working-how-do-i-fix-this-a1194e46
