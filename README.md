
# ハンズオン仮想環境構成
```
[centos7]
    | .9
    |
    |   172.16.0.1/24
    |
    | .1 (ge-0/0/1)
[vsrx1]
```

# 構築手順
## 前提環境
- Windows 7 / 10
- PowerShell 5.x
- VirtualBox 5.x
- Vagrant 2.0.x
- BOIS上のVT(Virtualization Technology）有効
- インターネット接続環境あり

## 手順1. 環境構築用データのダウンロード
-  https://github.com/apc-nw/handson の画面右の緑色のボタン `Clone or download` をクリックし、さらにポップアップ表示された `Donwload ZIP` をクリックする
- `handson-master.zip` というファイル名でダウンロードされるので、解凍して任意の場所に `handson` フォルダとして配置する（ここでは `C:\handson` に配置とする）


## 手順2．環境の起動
- コマンドプロンプトで `c:\handson` フォルダに移動し、`vagrant up` コマンドを実行する（初回起動時は10分以上かかることがある）
    - 仮想マシン2台（centos7 (Ansibleホスト)、 vsrx1（junos））が起動する
- 初回起動時は以下のログが表示されると正常に起動完了
```
PLAY [vsrx] ********************************************************************

TASK [provision] ***************************************************************
changed: [172.16.0.1]

PLAY RECAP *********************************************************************
172.16.0.1                 : ok=1    changed=1    unreachable=0    failed=0

C:\handson>  (プロンプトが戻る)
```

- 2回目以降起動時は以下のログが表示されると正常に起動完了
```
centos7: flag to force provisioning. Provisioners marked to run always will still run.

C:\handson>  (プロンプトが戻る)
```
# ターミナル接続方法
## centos7 (Ansibleホスト)
=======
- 接続情報
    - ホスト: localhost
    - サービス: SSH
    - ポート: 2209/TCP
- 認証情報
    - ユーザー名: vagrant
    - パスワード:（ハンズオン当日と同じ）
    - 秘密鍵 `C:\handson\.vagrant\machines\centos7\virtualbox\private_key` でもログイン可
### vsrx1（junos）
- 接続情報
    - ホスト: localhost
    - サービス: SSH
    - ポート: 2201/TCP
    - 秘密鍵 `C:\handson\.vagrant\machines\vsrx1\virtualbox\private_key` でもログイン可
- 認証情報
    - ユーザー名: root
    - パスワード:（ハンズオン当日と同じ）

# 環境の停止
- コマンドプロンプトで `c:\handson` フォルダに移動し、`vagrant up` コマンドを実行する（初回起動時は10分以上かかることがある）
    - 仮想マシン2台（centos7 (Ansibleホスト)、 vsrx1（junos））が停止する
- 以下のログが表示されると正常に停止完了

```
C:\handson>vagrant halt
==> centos7: Attempting graceful shutdown of VM...
==> vsrx1: Attempting graceful shutdown of VM...

C:\handson>  (プロンプトが戻る)
```

# 環境の削除
- コマンドプロンプトで `c:\handson` フォルダに移動し、`vagrant destroy --force` コマンドを実行する
    - 仮想マシン2台（centos7 (Ansibleホスト)、 vsrx1（junos））が削除される





