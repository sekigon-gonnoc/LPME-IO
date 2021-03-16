# LPME-IO
BLE Micro ProとI2C接続して分割型自作キーボードのスレーブ側のキーをスキャンするモジュールです。

いままで、分割型自作キーボードをBLE Micro Proで無線化する場合の実用的な構成はBLE Micro Proを2個使用した完全ワイヤレス構成だけでしたが、このモジュールを使うことで左右の接続は有線・PCとの接続は無線という構成が実現できます。

I2Cで接続するのでキーボードの基板がI2C接続に対応している必要があります。
ただし、**外付けのプルアップ抵抗は取り付けないでください**  。
（標準ではI2C対応していないキーボードでもジャンパを飛ばすなどの改造で対応できますが、自己責任でお願いします）

電源はBLE Micro Pro側に搭載するだけで動作します。

## 販売リンク
- [BOOTH](https://nogikes.booth.pm/items/1656264)

## バージョンの説明
- V1: 初期バージョン
- V2: 待機時の消費電流を半分から4分の1くらいに改善(両手分合わせて200uA→50uAくらい)
  - 導入時に対象のキーボードに合わせてハンダブリッジする必要がある
- V2a: 性能はV2と同等でI2Cのアドレスのみ異なる
  - [BLE Micro Pro用QMK v0.9.3](https://github.com/sekigon-gonnoc/qmk_firmware/releases/tag/bmp-0.9.3)以降で対応

## 導入手順
### (V2の場合)キーボードの列(Col)に対応するピンのジャンパをハンダでブリッジする
  - ハンダのフラックスを軽く蒸発させるとブリッジしやすくなります
  - マトリクスがCOL2ROWの場合は行(Row)に対応するほうをブリッジしてください
  - どのピンをジャンパすればいいかわからない場合は、BLE Micro ProのストレージにあるCONFIG.JSNを開いてcol_pinsの後半のピンと対応をとってください
    |config|LPMEのシルク|
    |--|--|
    |1|0|
    |2|1|
    |7|2|
    |8|3|
    |9|4|
    |10|5|
    |11|6|
    |12|7|
    |13|8|
    |14|9|
    |15|A|
    |16|B|
    |17|C|
    |18|D|
    |19|E|
    |20|F|
  - 例：Helix(5row)の場合
       ```
        "col_pins":[20,19,18,17,16,15,14,20,19,18,17,16,15,14],
       ```  
       →9からFをブリッジ  
  ![solder_jumper](https://user-images.githubusercontent.com/43873124/97097560-fa0ccf80-16b4-11eb-996f-f2cc81ac0c04.jpg)

  
 
### BLE Micro Proに最新版のファームウェアを導入する
 - BLE Micro ProのブートローダとQMKを[こちらのページ](https://github.com/sekigon-gonnoc/BLE-Micro-Pro/blob/master/AboutDefaultFirmware/doc/getting_start.md)の手順に沿って最新版にアップデートします
   - `LPME-IO`にチェックを入れて設定を書き込んでください
 - V1, V2はQMK for BMP v0.3.0以上、V2aはv0.9.3以上が対応しています

### キーボードに取り付ける
 - Pro Microを取り付ける場所に同じ向きで取り付けてください。コンスルーを使う場合ははんだ付けしなくても導通します
 - 左右のキーボードをTRRSケーブルで繋ぎ、BLE Micro Proの電源入れてキー入力ができることを確認してください
   - TRRSケーブルとの相性によって動作しない場合があります
   - バッテリー駆動する場合電源はBLE Micro Pro側だけに搭載すれば動作します

### 開発者向け情報
#### LPME用の設定を用意する
 - [BLE Micro Proのリポジトリ](https://github.com/sekigon-gonnoc/BLE-Micro-Pro/tree/master/AboutDefaultFirmware/keyboards)に使いたいキーボード用の設定ファイルがある場合はダウンロードします
 - リポジトリにない場合、変換スクリプトを使って生成してください
   ```
    python config_converter.py ~/qmk_firmware/keyboard/helix/rev2
   ```
    - レイアウトが複数ある場合は選択肢が表示されるので使用したいレイアウト番号を入力してください  
    - _master.json, _slave.json, _lpme.json の3つのファイルが生成されます。lpmeがついているファイルが目的のファイルです
    - このリポジトリに置いてあるファイルはBLE Micro Proを左手、LPMEを右手に付けることを想定しています。左右を入れ替えたい場合はis_left_handを0にしてください
 
#### LPME対応のキーボードを設計する
  - TRRSにPro MicroのPD1, PD0(5, 6番ピン)を接続してください
    - TRRSケーブル接続時に左右で同じピン同士が接続されるようにしてください
    - TRSケーブル接続時にも動作するように結線の順番に注意が必要です
  - Pro Micro用のファームウェアでは`#define SOFT_SERIAL_PIN D2`(結線によってはD3)を指定することで通常通りシリアル通信で動作します
    - TRSケーブル接続時の結線に応じてピンを選択してください

