# pyconjp-ansible


## ログインやユーザー設定
ユーザーは[こちら](https://github.com/pyconjp/pyconjp-ansible/blob/master/roles/users/vars/main.yml)のファイルで管理されています。
GitHubのユーザー名およびGitHubに登録してある公開鍵を用いてユーザーを作成します。
（GitHubに登録した公開鍵は誰でもみることができます。 参考 https://github.com/rmanzoku.keys ）

### 新規でログインしたい場合
GitHubで公開鍵を設定し、JIRAで起票し Slack #common-infra にて依頼してください。

```
pyconjpサーバへのログインユーザー作成以来

GitHubユーザー名: ○○
目的: 〇〇
```


### ログインする場合
sshコマンドでログインしてください。ログイン名は申請したGitHubユーザー、GitHubに登録している公開鍵に対応した秘密鍵でログインしてください。

```
$ ssh origin.pycon.jp -l GITHUB_USERNAME
```

PWの変更をする場合は以下のコマンドで変更できます。
```
$ sudo passwd GITHUB_USERNAME
```

## Ansibleの実行
事前に上記ユーザーを作成を依頼し、担当者からansible-vault用のパスワードを教えてもらってください。

### 環境の構築
各自の任意の方法でPythonの実行環境を作成してください。
推奨するPythonのバージョンは`.python-version`に書いてありますが、ansibleが実行できれば問題ありません。

ansibleを含むライブラリをインストールします。

```
$ cd pyconjp-ansible
$ pip install -r requirements.txt
```

vaultのパスワードを隠しファイルに入力します。
```
$ echo $VAULT_PASS > .vault_password_file
```

ansible-playbookを実行してサーバへ設定を反映させることができるようになります。
```
$ ansible-playbook pyconjp.yml
```
