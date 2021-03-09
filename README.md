# Requirement
- VirtualBox
- VirtualBox Extention Pack
- Vagrant
- Ansible

# Usage
## 1. 初期設定
- Vagrantfileの `config.vm.box` の名前を確認（box追加の段階で任意の名前をつけた場合は同一の名前に変更する）
- ホストマシン、ゲストマシンそれぞれの `mkcert -CAROOT` コマンドで表示されるルート証明書ディレクトリパスを変更（ [group_vars/variables.yml](https://github.com/taka-harada/vb-win10/blob/master/group_vars/variables.yml#L12-L14) ）
- ゲストマシンにコピーするhostsファイルテンプレートに必要なドメインを追加する（ [playbook/templates/custom_hosts.j2](https://github.com/taka-harada/vb-win10/blob/master/playbook/templates/custom_hosts.j2) ）

## 2. VMを起動する
`vagrant up` でVMを起動する

## 3. ゲストマシンの初期設定
`ansible-playbook playbook/setup.yml -i inventries/hosts` でWindows VMに必要なツール類のインストールを行う

- chocolateyのインストール
- vimのインストール
- Chromeのインストール
- mkcertのインストール
- 用意したhostsファイルテンプレートをWindows VMにコピー
- コピーしてきた証明書を認証機関（CA）が正しいものかどうか検証し、Windows VMにインストールする

## 4. ホストマシンのカメラをビルドしたWindows VMに認識させる
`vagrant reload`

## 5. 作業が終わったらVMを停止する
`vagrant halt`
