# sg-02.ymlの設定によるもの
* ある程度論理IDのまとまりでキャプチャ貼っています。
* 相互の繋がりはID等を見て確認しています。

## Resources:
いろいろエラーはありましたが最終的に以下の通り作成できました。<br>
![sgAll](image_10/201_sgAll.png)<br>
▼論理ID：SecurityGroupEC2:<br>
![sgEC2](image_10/202_sgEC2.png)<br>
▼論理ID：SecurityGroupALB::<br>
![sgALB](image_10/203_sgALB.png)<br>
▼論理ID：SecurityGroupRDS:<br>
![sgRDS](image_10/204_sgRDS.png)<br>


## 完了に至るまでに間違えたこと
### 1｜要素の文字列を誤る
▼「VPCIdはサポートされていない」旨のエラーがでました。  
![rollbackerror](image_10/205_rollbackerror.png)  
以下の公式ドキュメントに立ち返りますと、赤線のところが間違えていました。公式の指定する通りに直し、UPDATE_COMPLETEしました。  
参考記事）[（公式）AWS::EC2::セキュリティグループ](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group.html)  
![sgVPCIdmodify](image_10/206_sgVPCIdmodify.png)


### 2｜SourceSecurityGroupId又はName記述漏れ
▼Stack作成は完了したもののインバウンドルールがない状態になっていました。  
![sgRDSingressMiss](image_10/207_sgRDSingressMiss.png)<br>
▼その時のyamlファイルの状況です
![sgRDSingressMiss-yaml](image_10/208_sgRDSingressMiss-yaml.png)<br>
調べた結果、RDS(MySQL)用のセキュリティグループのインバウンドルールとしてsourceを記述できていないことが問題だとわかりました。  
参考記事）<br> [（公式）RDS｜セキュリティグループによるアクセス制御](https://docs.aws.amazon.com/ja_jp/AmazonRDS/latest/UserGuide/Overview.RDSSecurityGroups.html)<br>[（公式）AWS::EC2::SecurityGroupIngress](https://docs.aws.amazon.com/ja_jp/AWSCloudFormation/latest/UserGuide/aws-properties-ec2-security-group-ingress.html)<br>[【AWS】 CloudFormationで基本的な構成のEC2とRDSを作る](https://qiita.com/kobayashi_0226/items/d0f49dbe84937de73a4d)
▼AWS公式の抜粋  
![sgAccessCont-AWS](image_10/209_sgAccessCont-AWS.png)  
▼以下のとおり追記し、上述のとおりインバウンドルールが適用できました。
![sgRDSIngressModify-yaml](image_10/210_sgRDSIngressModify-yaml.png) 