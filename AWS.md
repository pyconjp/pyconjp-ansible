# AWS利用について

新サーバ origin.pycon.jp(3.112.157.188)は、AWS上で構築してあります。
特筆がない限り、リソースは全てTokyo(ap-northeast-1)リージョンにあります。

## リソース
### EC2
`pyconjp`というサーバが一台あり、中の構成は**全て**、このAnsible Playbookで管理されています。変更したい場合は、**Ansible経由で変更をしてください**。[README.md](/README.md)

### AWS Certificate Manager (ACM)
pycon.jp系WebページをHTTPSで配信するための証明書を発行、管理しています。
Route53によるDNS認証で発行しているため、AWSにより自動更新されます。

後述のCloudFrontで利用するため、リソースは、N.Virginiaにあります。Tokyoリージョンにはないので注意してください。

`pycon.jp`、`*.pycon.jp`の証明書にしてあるので幅広く対応できます。

### Route53
前述のACMを便利に使うために、`pycon.jp`ドメインのNSサーバとしてRoute53を利用しています。ドメイン登録自体は、さくらインターネットで管理していますので、もし変更する場合は、一社担当者に相談してください。

### CloudFront
ACMでのTLS証明書を利用するため、CoundFrontを利用しています。
基本的にはデフォルトの設定ですが、Hostヘッダをオリジンに転送するようにしています。

ここまでのリソースを整理すると、ブラウザから`pycon.jp`へアクセスする場合、
1. `pycon.jp`は、Route53で名前解決され、CloudFrontへアクセス
1. CloudFrontは、オリジンである`origin.pycon.jp`へ`pycon.jp`と言うHostヘッダをつけてHTTPリクエストをする
1. `origin.pycon.jp`でHTTPリクエストをNginxが受け付け、Hostヘッダに従い`pycon.jp`のVirtualHostの処理を行う

と言うアクセスフローになっています。