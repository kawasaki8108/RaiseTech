### VPC（新しく作ったもの）
![VPC](image_04/作成したVPC.png)

### 構築したEC2インスタンス
![EC2_1.png](image_04/作成したEC2インスタンス1.png)
![EC2_2.png](image_04/作成したEC2インスタンス2.png)

### 構築したRDS
![RDS.png](image_04/作成したRDS1.png)

### EC2 から RDS へ接続確認
![EC2→RDS接続.png](image_04/EC2→RDS接続確認.png)

### 理解したこと
* AWS上で各種サーバを立てること自体は難しくはないが、それぞれのつながり方がイメージすることが大事
* EC2インスタンスの構築過程でOSをAmazon Linux 2ではなくAmazon Linux 2023を選択していたことに後で気づいた。気づいたのは、MySQLのインストールが一向に完了しなかったことから。
  * 当該OSに対応していないパッケージをDLしており、インストールがOSに許容されなかったからだと思う。
  * 結局ＲｅｄＨａｔ系として用意されていたパッケージ（MySQLのリポジトリ）をDLして、インストールできた。
* RDS内のMySQLへの接続のコマンドは`mysql -u admin -p -h "RDSエンドポイント"`とわかった。adminをいれておかないとダメっぽい。
* "RDSエンドポイント"の例：`databaseXX.xxxxxxxxxxxxxx.ap-northeast-1.rds.amazonaws.com`

