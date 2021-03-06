Title: Arduinoをリモコンにしてみる
Date: 2014-11-25 23:02
Category: Embedded
Tags: Arduino, IR
Slug: ir

秋月電子で赤外線の受信モジュールとLEDを購入して学習リモコン的なものを作ってみた  
今回使用したのは以下の部品

* Arduino Uno
* 赤外線リモコン受信モジュール [GP1UXC41QS](http://akizukidenshi.com/catalog/g/gI-06487/) x1 1個50円
* 5mm赤外線LED [OSI5LA5113A](http://akizukidenshi.com/catalog/g/gI-04311/) x3 10個100円
* タクトスイッチ [DTS-6](http://akizukidenshi.com/catalog/g/gP-03647/) x1 1個10円
* プルダウン用の抵抗1KΩ x1

それぞれを以下のように接続した

Arduino | 部品
--------|-----------------
2       | GP1UXC41QSのVout
GND     | GP1UXC41QSのGND
5V      | GP1UXC41QSのVcc
5V      | タクトスイッチの2番
3       | タクトスイッチの4番 (プルダウン抵抗をつける)
4       | OSI5LA5113A x3 の+
GND     | OSI5LA5113A x3 の-

GP1UXC41QSのデータシートの `6. 製品の実装について` の項に `CRフィルターを実装してください` とあるけれど  
お試しなので今回は省略して回路を組んだ  
手元の環境では特に問題は起きていないけれど誤動作する可能性があるので注意

実際に接続するとこのようになる

[![image](/static/images/2014/11/IMAG1268_s.jpg)](/static/images/2014/11/IMAG1268.jpg)

ソースコードは [Github](https://github.com/lostman-github/arduino/blob/master/Uno/IR/sample/sample.ino) に置いてある

書き込んでシリアルモニタを表示すると `.` が1秒毎に表示される、これは受信状態であることを表している  
この状態で本物のリモコンをセンサに向けてボタンを押すと読み込んだ結果を出力する

ここで出力される情報は他の色々なサイトで見ることができるサンプルと同様に  
センサの出力の値が切り替わるまでの時間を10µs単位で計測したもの  
`4384,1032,510,67,....` と出力されたら、先頭から43840µsのOFF、10320µsのON、5100µsのOFF、670µsのONを表している  
先頭のOFFは無通信時間で不要な情報なので、これを除外してソースに埋め込めばこのデータを使ってLEDを発光させることができる

タクトスイッチを押すと `Send` が表示されソースコードに埋め込まれている情報を元にLEDを発光させる

実行例

    :::
    .
    .
    .4384,1032,510,67,62,67,62,66,187,67,61,70,62,66,61,67,62,67,187,69,188,66,62,66,61,67,62,69,187,67,61,66,187,67,61,70,61,66,187,67,62,66,188,70,187,66,62,66,62,66,187,69,188,66,61,66,188,66,61,70,62,66,188,66,187,66,62,66,
    .
    .
    .
    .
    .
    Send
    .
    .
    .

ソースに関しては他のサイトを参考にして簡潔になるように書きなおした程度で  
細かい説明は説明は不要だと思われるけれど、1点ハマったポイントがあったので経緯をメモしておく

作成にあたってソースコードは初めからほぼ最終形に近い形になっていて、受信に関しては期待通り動作していた  
ただ送信が全く動かなくて原因もよくわからない状態だった

赤外線は携帯のカメラを使うと見ることができるので、それでLEDが発光していることは確認した  
とりあえず光っていることは確認できたので受信側が認識するために必要な38KHzが  
うまく作り出せていないのかと思ってオシロで確認する以外に何か方法がないか考えてみた  
よくよく考えたら単純に自分で送信したものを今回作成した受信部で受信して、  
本物のリモコンと同じ結果になっていることを確認すればいいことに気がついた

試してみたら本物のリモコンはきっちり毎回同じ結果が出力されるのに対し、自作の方は安定しない様子  
色々本物のリモコンが再現できるように調節しようとしてみたけれど  
Arduinoの性能に依存しているのか完璧に再現することはできなかった

で、色々やっている時にふと気がついて本物のリモコンを携帯のカメラで覗いてみたら  
今回使用しているLEDよりずいぶん明るく見えたのと、LEDが1個でなく3個付いていることに気がついた  
そこで自作の方も元々は1個だったんだけれど、3個に増やしてみたら期待通りに動作するようになった

期待通りの信号が送信できているかについては、自分で受信してみるか、オシロで確認  
光量については本物のリモコンと自作のものをカメラを使って目でで確認するしかない  
受信側が認識できるくらいの光量を確保しなければいけないので、同じ部品、同じソースを使っても  
受信側の機器が異なっていればうまく動作しないことがあることがわかった

参考にしたサイト

* [赤外線リモコンの通信フォーマット](http://elm-chan.org/docs/ir_format.html)
* [リモコンフォーマット](http://akizukidenshi.com/download/k4174_format.pdf)
* [ArduinoとRubyで赤外線リモコン作ってWebから操作できるようにした](http://shokai.org/blog/archives/8012)
* [Arduinoでリモコン作成](http://eikatou.net/blog/2012/07/1796/)
* [Arduinoで学習リモコン](http://d.hatena.ne.jp/NeoCat/20090419/1240158722)
