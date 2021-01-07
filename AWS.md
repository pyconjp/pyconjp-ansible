# AWS利用について

新サーバ origin.pycon.jp(3.112.157.188)は、AWS上で構築してあります。
特筆がない限り、リソースは全てTokyo(ap-northeast-1)リージョンにあります。

## 構成図

## リソース
### EC2
`pyconjp`というサーバが一台あり、中の構成は**全て**、このAnsible Playbookで管理されています。変更したい場合は、**Ansible経由で変更をしてください**。

### AWS Certificate Manager (ACM)
pycon.jp系WebページをHTTPSで配信するための証明書を発行、管理しています。
Route53によるDNS認証で発行しているため、AWSにより自動更新されます。

後述のCloudFrontで利用するため、リソースは、N.Virginiaにあります。



### Route53



### CloudFront
ACMでのTLS証明書を利用するため、ALBを利用しています。
