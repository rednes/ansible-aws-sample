# AWS-01

AnsibleでAWS環境とAmazon EC2にWordPress環境を構築するサンプルPlaybookです。

以下の内容をAnsibleでAWSに自動構築します。

* VPCの作成
* Internet Gatewayの作成
* サブネットの作成
* ルートテーブルの作成
* セキュリティグループの作成
* EC2インスタンスの作成
* EC2インスタンスにWordPress環境の構築

## AWS Configuration Diagram

![AWS構成図](https://raw.githubusercontent.com/rednes/ansible-aws-sample/img/img/aws01.png)


## Requirements
- ansible 2.4 or later
- boto

## Usage

### Download
適当なディレクトリにPlaybookをダウンロードしてください。

```
$ git clone https://github.com/rednes/ansible-aws-sample.git
$ cd ansible-aws-sample/aws01
```

### Edit ansible.cfg

ansible.cfg.sampleファイルがあるので、ansible.cfgにコピーしてください。
```
$ cp ansible.cfg.sample ansible.cfg
```

ansible.cfgのprivate_key_fileに、AWSに登録している秘密鍵のパスを記載してください。

```
[defaults]
inventory = hosts/ec2.py
retry_files_enabled = False
private_key_file=XXXXXX.pem # set your environment

[privilege_escalation]
become = False

[ssh_connection]
ssh_args = -o ControlMaster=auto -o ControlPersist=60s -o StrictHostKeyChecking=no -o UserKnownHostsFile=/dev/null
```

### Edit localhost.yml

host_varsフォルダにlocalhost.yml.sampleファイルがあるので、localhost.ymlにコピーしてください。
```
$ cp host_vars/localhost.yml.sample host_vars/localhost.yml
```

key_nameに、AWSに登録しているキーペア名を記載してください。

```
---
my_vars:
  ec2:
     key_name: XXXX # AWSに登録したキーペア名を入力
```

### Execute ansible

ansibleを実行すると、AWSのEC2上にWordPress環境が構築できます。

```
$ ansible-playbook site.yml -v
```

### Setup WordPress

構築したWordPressにアクセスしてWordPressのセットアップを行ってください。
MySQLのDB名、ユーザー名、パスワードは以下をデフォルト値にしています。

* データベース名：wordpress
* ユーザー名：wordpress-user
* パスワード：wordpress

```
http://(EC2インスタンスのパブリックDNS)/wordpress/wp-admin/setup-config.php
```
