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
