## Resources:
▼論理ID：ALB、ALBListener<br>
![albcomp1](image_10/501_albcomp1.png)  
**設定しないことによるデフォ確認**（緑マーク）
![設定しないことによるデフォ確認](image_10/503_albcomp.png)<br>
▼論理ID：TargetGroup<br>
![TG](image_10/502_albcomp.png)  

## 完了に至るまでに間違えたこと
### 1｜EC2停止状態で作成
* はじめ、EC2スタックが作成完了したあと、翌日ALB設定しようと思って停止しました。そしてALBスタック作成試みると以下のエラーがでました。EC2起動したらスタック作成されました。→**ロードバランサーはEC2起動中でないと適用できない。**
![albstack作成されず-EC2見つからない](image_10/504_albstack作成されず-EC2見つからない.png) 

