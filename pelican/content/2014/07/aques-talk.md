Title: AquesTalk pico LSIの動作確認
Date: 2014-07-21 17:49
Category: Embedded
Tags: Arduino, AquesTalk pico LSI
Slug: aquestalk

以前購入していた[ATP3011F1-PU](http://www.a-quest.com/products/aquestalkpicolsi.html)([秋月](http://akizukidenshi.com/catalog/g/gI-06220/)で850円)の動作確認をしてみました  
紹介動画にもありますがArduino UNOに挿してアンプ付きのスピーカと接続、  
PCからシリアルモニタで文字を送信するだけで音声を出すことができます  
(技術資料 `13.4. Arduinoボードを利用した簡易動作` を参照)  
通常Arduinoから使う場合はI2C接続で使うことになると思いますが、  
I2C接続は後日試すとして今回はこの方法で動作確認をしてみました

まずうちにはアンプ付きのスピーカなんて無いのでそれを用意するところから  
せっかくなのでこれも秋月で買ってきて組み立ててみました  
といっても個別にパーツを揃えたわけでなく

* [ＰＡＭ８０１２使用２ワットＤ級アンプモジュール](http://akizukidenshi.com/catalog/g/gK-08217/)
* [３．５ｍｍステレオミニジャックＤＩＰ化キット](http://akizukidenshi.com/catalog/g/gK-05363/)
* [電池ボックス　単３×２本（リード線・耳付）](http://akizukidenshi.com/catalog/g/gP-02679/)
* 100均で売っているスピーカ

これらを接続しただけです  
今回試した手順を以下にまとめておきます  
後半は技術資料に書いてあるものそのままで、どちらかと言うと前半が僕には必要でした

1. ArduinoのLSIを取り外し、AquesTalkを取り付け
1. Arduinoの `デジタル6番` をアンプの `IN+` に接続
1. Arduinoの `GND` をアンプの `IN-` に接続
1. アンプの `+V` と `GND` をそれぞれ電池に接続
1. アンプの `SPK+` と `SPK-` をジャックの `R` と `G` に接続
1. ジャックにスピーカを接続
1. ArduinoをPCに接続、Arduino IDEを起動
1. メニューの `Tools -> Serial Monitor` を開く
1. 右下のコンボボックスで `Carriage return` と `9600bps` を選択
1. 上のインプットボックスに `?` を入力してEnter
1. `konnichiwa` 等を入力してEnterを押すと発声する

発声させるためのテキストは技術資料を参照するか、  
[音声記号列生成 Webサービス](http://www.a-quest.com/demo/pico_kanji2roman.html)を使用して作成します

[![AquesTalk pico LSI](/static/images/2014/07/IMAG1027_s.jpg)](/static/images/2014/07/IMAG1027.jpg)
