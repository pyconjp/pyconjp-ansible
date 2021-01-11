# pyconjp-ansible

## このリポジトリについて

* pycon.jp のサーバー構成を管理するためのansibleのリポジトリです
* pycon.jp のサーバーはAWS EC2上で動作しており、主に以下のサービスが動いています
  * 過去のPyCon JPイベントのWebサイトとnginx
  * pyconjpbot(PyCon JP Slack上のbot)
  * jira-issue-report(JIRAの期限切れチケットを毎週通知するプログラム)
  * pyconjp-cron(PyCon JP用のさまざまな定期実行するプログラム群)
* 参考: [20210107 pycon.jpサイト移行レクチャー会](https://docs.google.com/document/d/1kVpYvLArSLhr4L037J6Jvhh1F1lHgwoHNsPY90cDCWU/edit)

## ログインやユーザー設定
* `origin.pycon.jp` にログインするユーザーは[`roles/users/vars/main.yml`](https://github.com/pyconjp/pyconjp-ansible/blob/master/roles/users/vars/main.yml)のファイルで管理されています。
* GitHubのユーザー名およびGitHubに登録してある公開鍵を用いてユーザーを作成します。
* （GitHubに登録した公開鍵は誰でもみることができます。 参考: https://github.com/rmanzoku.keys ）

### ログインユーザーを追加したい場合
* GitHubで公開鍵を設定し、JIRAで起票し Slack #common-infra にて依頼してください。

```
pyconjpサーバへのログインユーザー作成以来

GitHubユーザー名: ○○
目的: 〇〇
```


### ログインする場合
* sshコマンドでログインしてください。
* ログイン名は申請したGitHubユーザー、GitHubに登録している公開鍵に対応した秘密鍵でログインしてください。

```bash
$ ssh origin.pycon.jp -l GITHUB_USERNAME
```

パスワードを変更をするには、ログイン後に以下のコマンドを実行してください。
```bash
$ sudo passwd GITHUB_USERNAME
```

## Ansibleの実行

* 事前に上記ユーザーを作成を依頼し、担当者からansible-vault用のパスワードを教えてもらってください。
* ansible-vault用のパスワードは「PyCon JP Associationアカウント等秘密情報」シートに記述してあります。(PyCon JP Association理事のみがアクセス可能です)

### 再構築手順

* 各自の任意の方法でPythonの実行環境を作成してください。
* 推奨するPythonのバージョンは`.python-version`に書いてありますが、ansibleが実行できれば問題ありません。
* ansibleを含むライブラリをインストールします。

```bash
$ cd pyconjp-ansible
$ python3.9 -m venv env
$ . env/bin/activate
(env) $ pip install -r requirements.txt
```

* vaultのパスワードを隠しファイルに入力します。

```bash
(env) $ echo $VAULT_PASS > .vault_password_file
```

* Ansible-playbookを実行してサーバへ設定を反映させることができるようになります。

```bash
(env) $ ansible-playbook pyconjp.yml -u GITHUB_USERNAME
```

## Webサイトの更新

* 現在配信しているページは全て静的サイトになっているため、サーバ上の所定のパスへ`git pull`するだけで更新可能です。
* 詳しくは`roles/web-2020/tasks`等を参照しください。
* ansible経由でmaster branchの更新が可能です。

```
# `web2020` の部分は開催年を指定してください
(env) $ ansible-playbook pyconjp.yml -t web2020
```

## pyconjpbotの更新

* [pyconjpbot](https://github.com/pyconjp/pyconjpbot)を更新するコマンドです。
* 設定値等は`roles/pyconjpbot/tasks | vars`で管理されています。
* ansible経由でmaster branchの更新が可能です。
* pip及びソースコードの更新が実行されます。

```bash
(env) $ ansible-playbook pyconjp.yml -t bot
```

## pyconjp-cronの更新

* [pyconjp-cron](https://github.com/pyconjp/pyconjp-cron)を更新するコマンドです。
* 設定値及びCronの設定は`roles/pyconjp-cron/tasks | vars`で管理されています。
* ansible経由でmaster branchの更新が可能です。
* pip及びソースコードの更新が実行されます。

```bash
(env) $ ansible-playbook pyconjp.yml -t cron
```

## jira-issue-reportの更新

* [jira-issue-report](https://github.com/pyconjp/jira-issue-report)を更新するコマンドです。
* 設定値及びCronの設定は`roles/jira-issue-report/tasks | vars`で管理されています。
* ansible経由でmaster branchの更新が可能です。
* pip及びソースコードの更新が実行されます。

```bash
(env) $ ansible-playbook pyconjp.yml -t jira-issue-report
```
