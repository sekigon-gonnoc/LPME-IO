# LPME-IO

BLE Micro ProとI2C接続して分割型自作キーボードのスレーブ側のキーをスキャンするモジュールです。

I2Cで接続するのでキーボードの基板がI2C接続に対応している必要があります。（標準では対応していないキーボードでも改造して対応させることもできますが、自己責任でお願いします）

## 導入手順
 
### BLE Micro Proに最新版のファームウェアを導入する
### LPME用の設定を用意する
 - [BLE Micro Proのリポジトリ](https://github.com/sekigon-gonnoc/BLE-Micro-Pro/tree/master/AboutDefaultFirmware/keyboards)に使いたいキーボード用の設定ファイルがある場合はダウンロードする
 - リポジトリにない場合、変換スクリプトを使って生成する
   ```
    python config_converter.py ~/qmk_firmware/keyboard/helix/rev2
   ```
    - レイアウトが複数ある場合は選択肢が表示されるので使用したいレイアウト番号を入力してください  
    - _master.json, _slave.json, _lpme.json の3つのファイルが生成されます。lpmeがついているファイルが目的のファイルです
 
### 設定ファイルをBLE Micro Proに書き込む
 用意したjsonファイルをBLE Micro Proに書き込みます

### キーボードに取り付ける
Pro Microを取り付ける場所に同じ向きで取り付けてください。コンスルーを使う場合ははんだ付けしなくても導通します
