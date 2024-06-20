# CI/CDを用いたRailsアプリケーションのインフラストラクチャ自動構築

## 概要
- CRUD 処理が出来る簡単な[Railsアプリケーション](https://github.com/yuta-ushijima/raisetech-live8-sample-app)を稼働できるインフラストラクチャを構成しました
- CI/CDツールを使い、AWSリソース構築からアプリケーションの実行環境構築およびデプロイまで自動化しました
- ソースコードは別リポジトリ（[CircleCI-Ansible-test](https://github.com/kawasaki8108/CircleCI-Ansible-test)）に格納しております
#### インフラ構成図
![circleci-cfn-ansible-serverspecのインフラ構成図](image_14/circleci-cfn-ansible-serverspecのインフラ構成図(白背景).png)


## 動作環境
### ruby
```bash
3.2.3
```
### Bundler
```bash
2.3.14
```
### Rails
```bash
7.1.3.2
```
### Node
```bash
v17.9.1
```
### yarn
```bash
1.22.19
```

## 環境構成説明
### 概要
CircleCIのワークフローで以下のjobを実行し、プロビジョニングしています。
- CloudFormationのテンプレートファイルに対してリンターテスト実行
- CloudFormationでAWS Cloudに各種リソースを作成
- AnsibleでEC2に対してRailsアプリケーションを実行するための環境を構築およびサービス起動
- ServerSpecでRailsアプリケーションのレスポンスを確認

### 事前設定
詳細は[lecture13.md](lecture13.md)を参照ください。
- CircleCI上で各種環境変数を設定しています
  - SSH Key(事前に設定しているキーペアの秘密鍵)を用意し、そのFingerprintを環境変数として設定
  - IAM Roleのarn（OIDC用）
  - AWSのRegion
  - RDS(MySQL)のパスワード
- AWSでCircleCIをIDプロバイダとして登録し、OIDC用のIAM Roleを作成(フェデレーションの設定)
- RDSのパスワードをParameterStoreに登録

### 各AWSリソースの説明
#### EC2
- 今回は簡単なRailsアプリケーションを実行するため＋コストと複雑性を抑えるため、1台のEC2インスタンスを使用しています。

#### RDS
- アプリケーションのデータベースとしてAWS RDS MySQLを採用しています。
- 管理が容易で、スケーラビリティと高可用性が求められるデータベース要件に適しています。ただし今回はコストを抑えるため、Multi-AZとはしていません。

#### ALB
- EC2インスタンスの前に配置して、受信するHTTP/HTTPSトラフィックをEC2にルーティングします。
- アプリケーションの拡張性と可用性を考慮し、ALBを使用しています。

#### S3
- データの耐久性と可用性を確保するため＋スケーラビリティがあるため、画像ファイルの保存先としてS3を採用しています。

---

## 学習記録の説明
2023.6.14から入会した以降の学習記録です。講座ごとで出される各課題を.mdファイルにまとめて、mainブランチにmergeしています。
下表に講座・課題・レポートの対応を示します。<br>
|講座|概要|レポート|備考|
|:---|:---|:---|:---|
|第1回|AWSアカウント作成<br>IAMユーザー作成<br>MFA設定<br>Cloud9でHellow World(Ruby)|本リポジトリへの反映はなし|Discord上でキャプチャ提出|
|第2回|バージョン管理システム<br>GitHubでの開発の流れ<br>markdown|[lecture02.md](lecture02.md)|-|
|第3回|webアプリケーション(Rails)のEC2へのデプロイ|[lecture03.md](lecture03.md)|IDEはCloud9|
|第4回|手動でクラウドインフラ環境構築<br>VPC,EC2,RDS<br>EC2からRDSへの接続確認|[lecture04.md](lecture04.md)|IDEはこれ以降VScode|
|第5回|手動でクラウドインフラ環境構築<br>ALBとS3(IAMロール適用)を追加<br>EC2でNginxとUnicornで構成|[lecture05.md](lecture05.md)|S3は画像保存として適用|
|第6回|システムの安定稼働<br>CloudTrail、CloudWatch<br>Amazon SNS<br>メトリクス<br>Cost Explorer<br>Billing|[lecture06.md](lecture06.md)|-|
|第7回|システムにおけるセキュリティ|該当なし|-|
|第8回|第5回課題のライブコーディング（組み込みサーバでの稼働まで）|該当なし|-|
|第9回|第5回課題のライブコーディング（Webサーバ・Railsアプリサーバ分離～最後まで）|該当なし|-|
|第10回|CloudFormation<br>└第5回課題で作成した環境構築|[lecture10.md](lecture10.md)<br>└[～-vpc-01.md](lecture10-vpc-01.md)<br>└[～-sg-02.md](lecture10-sg-02.md)<br>└[～-ec2-03.md](lecture10-s3-06.md)<br>└[～-rds-04.md](lecture10-rds-04.md)<br>└[～-alb-05.md](lecture10-alb-05.md)<br>└[～-s3-06.md](lecture10-s3-06.md)|lecture10.mdをサマリーとして、詳細はスタック名を付けた名前の.mdファイルに分けました。<br>ymlファイルはlecture10_CFnTemlateフォルダに入れています|
|第11回|インフラのコード化を支援するツール<br>ServerSpec|[lecture11.md](lecture11.md)|2023.8.6からSAAの学習に入りました|
|第12回|CI/CDツール<br>CircleCI導入<br>config.ymlをリポジトリへ組込んで稼働|[lecture12.md](lecture12.md)|-|
|第13回|プロビジョニングツールとCI/CDツールの併用|[lecture13.md](lecture13.md)|CircleCIでCFn→ansible→ServerSpecのワークフローを回す|
|第14回|第13回課題のライブコーディング-1-|該当なし|-|
|第15回|第13回課題のライブコーディング-2-|該当なし|-|
|第16回|現場へ出ていくにあたって|該当なし|最終回|

<br>
以上
