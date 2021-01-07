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

### 再構築手順
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
上記サーバログイン時に変更したパスワードを入力してください。
```
$ ansible-playbook pyconjp.yml
```

## Webサイトの更新
現在配信しているページは全て静的サイトになっているため、サーバ上の所定のパスへ`git pull`するだけで更新可能です。
詳しくは`roles/web-%Y`をみてください。

ansible経由でmaster branchの更新が可能です。

```
# 2020は開催年を指定してください
$ ansible-playbook pyconjp.yml -t web2020
BECOME password
```

## pyconjpbotの更新
[pyconjpbot](https://github.com/pyconjp/pyconjpbot)
設定値等は`roles/pyconjpbot/tasks | vars`で管理されています。

ansible経由でmaster branchの更新が可能です。
pip及びソースコードの更新が実行されます。

```
$ ansible-playbook pyconjp.yml -t bot
BECOME password
```

## pyconjp-cronの更新
[pyconjp-cron](https://github.com/pyconjp/pyconjp-cron)
設定値及びCronの設定は`roles/pyconjp-cron/tasks | vars`で管理されています。

ansible経由でmaster branchの更新が可能です。
pip及びソースコードの更新が実行されます。

```
$ ansible-playbook pyconjp.yml -t cron
BECOME password
```

## jira-issue-reportの更新
[jira-issue-report](https://github.com/pyconjp/jira-issue-report)
設定値及びCronの設定は`roles/jira-issue-report/tasks | vars`で管理されています。

ansible経由でmaster branchの更新が可能です。
pip及びソースコードの更新が実行されます。

```
$ ansible-playbook pyconjp.yml -t jira-issue-report
BECOME password
```