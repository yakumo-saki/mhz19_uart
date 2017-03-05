# MHZ19_Serial  
Arduino IDE 用の MH-Z19 二酸化炭素センサ用のライブラリです。  
version 0.2

# ライセンス  
License MIT

# 使い方

* このライブラリを Arduino IDE にインクルードしてください。
* MH-Z19 を Arduino や ESP-WROOM-02 にシリアル接続してください。

    MH-Z19 Vout → Arduino Vin(5V)  
    MH-Z19 GND  → Arduino GND  
    MH-Z19 Tx   → Arduino Digital Pin (Software Serial の Rx ピン用)  ※ Arduino の Rx ピンには接続しないでください。  
    MH-Z19 Rx   → Arduino Digital Pin (Software Serial の Tx ピン用)  ※ Arduino の Tx ピンには接続しないでください。  
    MH-Z19 の他のピンは使用しません。  

* Arduino IDE からスケッチ例を参照してください。

# 注意事項

* MH-Z19 は PWM での接続もサポートしていますが、このライブラリはシリアル接続のみ対応しています。
* ESP-WROOM-02 でのみテストをしています。他の Arduino(もしくは互換機)で動作しない場合はご連絡ください。

# MHZ19_Serial ライブラリ説明

## コンストラクタ

* MHZ19_Serial  
  素のコンストラクタ。こちらを使って変数定義した場合は、事後に begin関数を呼び出して Rx/Tx ピンを設定してください。

* MHZ19_Serial(int rx, int tx)  
  使用するRx/Txピン番号を設定できるコンストラクタ。

## public関数

* void begin(int rx, int tx)  
  Rx/Txピンを設定する
  
* void setAutoCalibration(bool autocalib)  
  MH-Z19 は、デフォルトで「オートキャリブレーション」機能が有効になっています。
  オートキャリブレーションが有効な場合、電源投入後24時間ごとに「ゼロキャリブレーション」（その時点の二酸化炭素濃度を「大気=400ppm」と判断すること）して、以後の数値を出力します。
  MH-Z19を屋外で使う場合は非常に有効な機能ですが、屋内で使う場合は迷惑な機能でしかありません。  
  この関数は、オートキャリブレーション機能を有効にするか無効にするかを設定します。
 
* void calibrateZero()  
  ゼロキャリブレーションを実施します。ゼロキャリブレーションは大気下での算出精度を高めるためのものです。
  ゼロキャリブレーションを実施する場合は、MH-Z19の電源を投入後、最低20分以上大気（屋外）で動作させ、その後この関数を実行してください。

* void calibrateSpan(int ppm)  
  スパンキャリブレーションを実施します。スパンキャリブレーションは1000～2000ppm程度のもとでの算出精度を高めるためのものです。
  ゼロキャリブレーションを実施する場合は、MH-Z19の電源を投入後、最低20分以上2000ppm程度（最低でも1000ppm以上）の環境で動作させ、その後この関数を実行してください。
  なお、1000ppm未満でこの関数を実行した場合、スパンキャリブレーションは実行されません。  
  また、スパンキャリブレーションを実行する前にゼロキャリブレーションを実施してください。
  
* int getPPM()  
  現在の二酸化炭素濃度(ppm)を取得します。
  
* int getTemperature()  
  現在の温度（℃）を取得します。（MH-Z19の隠し機能？のため動作保証はしません。また、取得できる値は整数値であるため精度はイマイチです。）

* int getStatus()  
  現在のステータス値を取得します。なお、ステータス値が何を意味するのかはデータシートにも記載がないため、詳細は不明です。（0=開始中、1=暖機中、64=通常動作中、らしい）

* bool isWarming()  
  MH-Z19センサが暖機中かを確認します。

# リンク
* MH-Z19 Data sheet  
  http://www.winsen-sensor.com/d/files/PDF/Infrared%20Gas%20Sensor/NDIR%20CO2%20SENSOR/MH-Z19%20CO2%20Ver1.0.pdf

* MH-Z19B Data sheet  
  http://www.winsen-sensor.com/d/files/infrared-gas-sensor/mh-z19b-co2-ver1_0.pdf
