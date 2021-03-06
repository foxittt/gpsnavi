# 完成写真  
６条田植え機（作業幅180cm）の先端マーカーの上にアンテナとグランドプレーン、横にLED表示器、  
ハンドルとの中間に二輪車用防水スマホケースを取り付けました。  
Raspberry Piは写っていませんがフロントカバーを開けた中に収納しています  
![kitai.jpg](/image/14e59b5a-01e3-900c-f17f-9d60f82067de.jpeg)  
![antena.jpg](/image/8ad093f6-4b25-260a-3d93-07ea434b5afd.jpeg)  
スマホへの出力画面のスクリーンショット（紹介したpythonプログラムと一部異なります）  
▲マークが中心で｜の本数が左右のズレです（1本が2cm）この｜と前に付いている緑黄赤のLEDが対応します  
![Screenshot.png](/image/799eb7d0-ba4f-db3e-cb64-304de11c4533.png)  
  
入力用のキーパッド　泥のついた手袋でも操作できます  
![keypad.jpg](/image/011a9d44-a8e0-fefe-ccb5-ad533a5f9c69.jpeg)  
posファイルをRTKPOSTとRTKPLOTを使って表示。こちらが１マス５m  
![pos1.jpg](/image/09b92ed6-a3f3-7507-f21a-5bbd2779c29e.jpeg)  
拡大して１マス２０cmにしたのがこちら。田植え機の精度としては十分実用的です  
![pos2.jpg](/image/98d151c7-ac55-7dcd-17bb-f2a4b470c576.jpeg)  
苗補給のため停車しているときのプロットです。1マスが1cmでFixしているとこの精度まででます![pixpoint.jpg](/image/641cf76b-ba29-9502-a1fd-39fd8fea3693.jpeg)  
最後に実際に植えた田んぼです（移植後３０日）  
![taue.jpg](/image/3a5d57ae-3305-6f04-95a8-728a92486287.jpeg)  
  
# 課題  
・機体の傾きの補正を行っていないので片側沈むと誤差が大きくなる。  
（実は数百円の加速度センサーを付けてみたのですがフレが10cm単位と大きく使い物にならなかったのでフィルタやセンサーを再検討）  
**（7/7追記)⇒**その持っていた6軸センサー「MPU6050」はデジタルモーションプロセッサー(DMP)というが内蔵されて補正された信号を出してくれることを知る。Aruduino用のC++ライブラリは見つかったのですが、どうしてもPythonで使いたかった（それしか使えない）ので探したらありました。  
https://github.com/thisisG/MPU6050-I2C-Python-Class  
テストしてみたところ静止状態では高さ2m換算で１mm精度の値を安定して出していて使えそうだ  
**（1/2追記）**　傾斜補正を実装GitHubにて公開  
**(2019追記）⇒**使ってみたら１～２cmぐらいしか補正されてないので必要なかったかも  
  
・RTK2GO.comは無料で使えるのですが安定して使いたいとなると自前でNTRIPCasterを設置したい  
オープンソースのNTRIPCaster これが使えそうか？  
https://github.com/nunojpg/ntripcaster  
**（7/7追記)⇒**さくらVPSのCentOS7で動作確認OK  
**(2019追記)⇒**AWS Lightsail で運用中  
  
・ステアリングモーターを付けて手放し走行。井関が8条用自動直進アシスト田植え機を今年から市販しているのでその部品を使ってできないか計画中  
**（7/7追記)⇒**新製品発表で来シーズンから6条の自動直進機が発売  
**(2019追記）⇒**モーターだけでなくミッションまで変更が必要と聞き断念  
  
・今月（2018年6月）に入ってからQZSSがトラブルのため4機中3機が運用停止してFIX率が落ちた気がする。基準局のアンテナも性能の高いものに替えたほうがよいか？  
**（7/7追記)⇒**posファイルを眺めていたら5/21（日本時間）のFix率が悪くてQZSSの運用情報を見ていたら  
http://sys.qzss.go.jp/dod/naqu.html　　  
5/20から一機停止してたらしい。7/7現在もSVN001号機のみしか受信できず。  
QZSSの回復か、NEO-M8PがGalileoに対応してくれるを期待するかどちらにしろどうすることもできないので祈るのみ  
**(7/30追記）**QZSSが再開したもよう  
**(1/2追記）**基準局アンテナもTW2710に交換　Fix率が90％以上で安定  
**(2019追記）⇒**日によってはFix率60%まで落ちる日もあったが使用上は大きな問題なし  
  
・手動操作なら5Hzでも十分だが、M8Pは10Hz出力まで対応しているそうなのでモーター制御に備え最適な出力レートをさぐる  
・**(7/30追記）**BASE局のSDカードが壊れ交換。Raspberry PiのSDカードは消耗品と割り切っていたほうがよいそうです。あらかじめバックアップをとっておきましょう。ログファイルを溜めておかなければ８GBの容量で十分です。  
# 苦労した点  
・できるだけ圃場でスマホの操作をしたくないので電源を入れるだけでrtkrcvの起動、スマホのテザリングに接続、pythonプログラムの起動まで行えるようにした所  
・購入したM8P-0のうち一つが不具合（？）で測位が安定しなかったのでファームウェアを古いF/W3.01HPG1.30にダウングレード  
・角度計算でatan関数で求めると第１～第４象限までの場合分けで苦労したのでatan2関数を使った  
・直射日光下ではスマホ画面が見づらいこともあり、また運転しながらできるだけ単純に視認できるようLEDを付けた  
・Raspbian(stretch)に入っているRealVNCは操作しないと60分で自動的に切断されます。  
https://www.realvnc.com/docs/faq/session.html  
IdleTimeout　=　0　として切断されないようにしておきます  
  
  
  
# 気づいた点  
・天気によってFIX率が落ちるかと予想したが、曇天雨天でもあまり影響なく逆に快晴の日より薄曇りのほうがFIXしやすい印象があった。ただし高圧送電線の真下では測位が不安定になったがこれは仕方がない。  
・スマホのテザリングを使用するのでデータ通信量が心配されたが、5月はほぼ毎日使って1.5GBほど最低クラスのデータ通信プランでもOK。  
・（1/26追記)データ通信の速度制限がかかり200kbpsの低速モードでも支障なく使用できた  
  
  
[続く](https://github.com/mnltake/gpsnavi/blob/master/Raspberry Piでcm精度のRTK-GPSガイダンスの制作（その４）.md)  
