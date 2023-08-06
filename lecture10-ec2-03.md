## Parameters:
▼論理ID：OS:<br>
以下の参考記事に習ってAmazon Linux 2の最新のAMIを取得するよう定義しました。  
![AMI取得](image_10/300_AMI取得.png)  
[参考記事）AMI自動取得](#ami自動取得)
## Resources:
▼論理ID：EC2Instance:<br>
緑マーク：最初パブリックアドレスが表示されなかったので、Propaties内にNetworkInterfaceのAssociatePublicAdressをOnしたというマークです。  
![ec2コンソール画面](image_10/301_ec2.png)<br>
**適用したSG**  
![ec2SG](image_10/303_ec2SG.png)<br>
**定義したEBS**  
![ec2EBS](image_10/304_ec2EBS.png)<br>
**一応接続確認**  
![ec2コンソール画面接続確認](image_10/302_ec2接続確認.png)

## 完了に至るまでに間違えたこと
### 1｜キーペアのプライベートキーファイル名を誤る
単純にキーファイル名のみ記載すればよかったのですが、拡張子含めて記載しておりエラー返されました。  
![keynamemiss](image_10/305_ketpairmiss.png)  
### 2｜パブリックIPなどが出ない
上記のKeyName問題解消したらスタック自体は作成成功しましたが、パブリックIPなどが生成されていませんでした。上述と重複しますが、Propaties内にNetworkInterfaceのAssociatePublicAdressをOnしたら解消されました。右のコンソール画面は課題5で作成したインスタンスです（比較しているキャプチャ）  
![PublicAdressmiss](image_10/306_PublicAdressmiss.png)
## その他参考記事
#### AMI自動取得
参考記事）  
* [AWS公式）AWS CloudFormation で最新の Amazon Linux AMI を使用する](https://aws.amazon.com/jp/blogs/news/query-for-the-latest-amazon-linux-ami-ids-using-aws-systems-manager-parameter-store/)
* [【CloudFormation / Terraform】EC2の最新版のAMI IDを自動的に取得、構築する (Windows / Linux)](https://blog.serverworks.co.jp/automate-latest-ami-ec2)
#### 組み込み関数の使い方（戻り値）
参考記事）  
* [AWS CloudFormation テンプレートリファレンス – 組み込み関数(Intrinsic Function)](https://dev.classmethod.jp/articles/cloudformation-tempate-reference-intrinsic-function/#getatt)
* [【AWS初学者向け・図解】CloudFormationの組み込み関数を現役エンジニアがわかりやすく解説①](https://o2mamiblog.com/aws-cloudformation-intrinsic-function-beginner-1/)