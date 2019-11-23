# LPME-IO

BLE Micro ProとI2C接続して分割型自作キーボードのスレーブ側のキーをスキャンするモジュールです。

I2Cで接続するのでキーボードの基板がI2C接続に対応している必要があります。  
ただし、**外付けのプルアップ抵抗は取り付けないでください**  
（標準ではI2C対応していないキーボードでも改造して対応させることもできますが、自己責任でお願いします）


## 導入手順
 
### BLE Micro Proに最新版のファームウェアを導入する
 - BLE Micro ProのブートローダとQMKを[こちらのページ](https://github.com/sekigon-gonnoc/BLE-Micro-Pro/blob/master/AboutDefaultFirmware/doc/getting_start.md)の手順に沿って最新版にアップデートします
 - QMK for BMP v0.3.0以上がLPMEに対応しています

### LPME用の設定を用意する
 - [BLE Micro Proのリポジトリ](https://github.com/sekigon-gonnoc/BLE-Micro-Pro/tree/master/AboutDefaultFirmware/keyboards)に使いたいキーボード用の設定ファイルがある場合はダウンロードします
 - リポジトリにない場合、変換スクリプトを使って生成してください
   ```
    python config_converter.py ~/qmk_firmware/keyboard/helix/rev2
   ```
    - レイアウトが複数ある場合は選択肢が表示されるので使用したいレイアウト番号を入力してください  
    - _master.json, _slave.json, _lpme.json の3つのファイルが生成されます。lpmeがついているファイルが目的のファイルです
    - このリポジトリに置いてあるファイルはBLE Micro Proを左手、LPMEを右手に付けることを想定しています。左右を入れ替えたい場合はis_left_handを0にしてください
 
### 設定ファイルをBLE Micro Proに書き込む
 - 用意したjsonファイルをBLE Micro Proに書き込みます

### キーボードに取り付ける
 - Pro Microを取り付ける場所に同じ向きで取り付けてください。コンスルーを使う場合ははんだ付けしなくても導通します
 - 左右のキーボードをTRRSケーブルで繋ぎ、BLE Micro Proの電源入れてキー入力ができることを確認してください
   - TRRSケーブルとの相性によって動作しない場合があります
   - バッテリー駆動する場合電源はBLE Micro Pro側だけに搭載すれば動作します
