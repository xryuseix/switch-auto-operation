# joycontrol-pluginloader

Bluetooth 経由で Nintendo Switch コントローラのエミュレートが行える joycontrol 用のプラグインローダと Nintendo Switch 自動化用プラグインです。  
[joycontrol](https://github.com/Poohl/joycontrol/blob/amiibo_edits) [joycontrol-pluginloader(fork元)](https://github.com/Almtr/joycontrol-pluginloader)

## 検証環境

Raspberry pi 4B

## インストール

- joycontrol のインストール  
  詳細: [GitHub - Poohl/joycontrol - README.md](https://github.com/Poohl/joycontrol/blob/amiibo_edits/README.md)

    ```sh
    sudo apt update
    sudo apt install git python3-pip python3-dbus libhidapi-hidraw0 libbluetooth-dev bluez -y
    git clone https://github.com/Poohl/joycontrol.git -b amiibo_edits
    sudo pip3 install aioconsole hid crc8
    sudo pip3 install joycontrol/
    ```

- joycontrol-pluginloader のインストール
  詳細: [GitHub - Almtr/joycontrol-pluginloader - README.md](https://github.com/Almtr/joycontrol-pluginloader/blob/master/README_ja.md)

    ```sh
    git clone https://github.com/xryuseix/switch-auto-operation/
    sudo pip3 install switch-auto-operation/
    ```

## joycontrol の Proコントローラをペアリングする

1. Nintendo Switch の「持ちかた/順番を変える」メニューを開く

    HOME > コントローラー > 持ちかた/順番を変える

2. スクリプトを実行する

    ```sh
    cd joycontrol-pluginloader/
    # 結構うまくいかないことが多い
    # これがうまくいかない時はVNCでラズパイ開いてBluetooth設定を消す
    sudo joycontrol-pluginloader plugins/tests/PairingController.py
    ```

    実行結果:  

    ```sh
    $ sudo joycontrol-pluginloader plugins/tests/PairingController.py
    [17:03:13] joycontrol.server create_hid_server::58 WARNING - [Errno 98] Address already in use
    [17:03:13] joycontrol.server create_hid_server::60 WARNING - Fallback: Restarting bluetooth due to incompatibilities with the bluez "input" plugin. Disable the plugin to avoid issues. See https://github.com/mart1nro/joycontrol/issues/8.
    [17:03:13] joycontrol.server create_hid_server::65 INFO - Restarting bluetooth service...
    [17:03:14] joycontrol.device set_name::69 INFO - setting device name to Pro Controller...
    [17:03:14] joycontrol.device set_class::61 INFO - setting device class to 0x002508...
    [17:03:14] joycontrol.server create_hid_server::84 INFO - Advertising the Bluetooth SDP record...
    [17:03:14] joycontrol.server create_hid_server::94 INFO - Waiting for Switch to connect... Please open the "Change Grip/Order" menu.
    [17:03:16] joycontrol.server create_hid_server::98 INFO - Accepted connection at psm 17 from ('01:23:45:67:89:AB', 17)
    [17:03:16] joycontrol.server create_hid_server::100 INFO - Accepted connection at psm 19 from ('01:23:45:67:89:AB', 19)
    [17:03:18] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.REQUEST_DEVICE_INFO
    [17:03:18] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SET_SHIPMENT_STATE
    [17:03:18] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SPI_FLASH_READ
    [17:03:18] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SPI_FLASH_READ
    [17:03:18] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SET_INPUT_REPORT_MODE

    <snip>

    [17:04:04] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SET_NFC_IR_MCU_CONFIG
    [17:04:04] root _reply_to_sub_command::295 INFO - received output report - Sub command SubCommand.SET_PLAYER_LIGHTS
    [17:04:04] JoycontrolPlugin.loader load_plugin::9 INFO - Loading: plugins/tests/PairingController.py
    [17:04:05] plugins/tests/PairingController.py run::11 INFO - Pairing completed.
    ```

    > 備考:  
    > 「01:23:45:67:89:AB」 は、Nintendo Switch Bluetooth Mac address です。
    > この Mac address は環境によって異なります。プラグイン実行時に使用するので、覚えておいてください。

    我が家のSwitchの場合，MACアドレスは**1C:45:86:D0:25:BF**です．

3. ボタンを押してみる
    Nintendo Switch の「設定」メニューを開く
    HOME > 設定 >コントローラーとセンサー > 入力デバイスの動作チェック > ボタンの動作チェック

   ```sh
   sudo joycontrol-pluginloader -r 1C:45:86:D0:25:BF plugins/utils/RepeatA.py
   ```

## その他リンク

[mart1nro/joycontrol (original)](https://github.com/mart1nro/joycontrol)
[Poohl/joycontrol (fork)](https://github.com/Poohl/joycontrol)
[Almtr/joycontrol-pluginloader](https://github.com/Almtr/joycontrol-pluginloader)
[プラグインのサンプル](https://github.com/Almtr/joycontrol-plugins/tree/master)
[プラグインの作り方](https://github.com/Almtr/joycontrol-pluginloader#how-to-create-a-plugin)
[joycontrolのボタン一覧](https://github.com/mart1nro/joycontrol/blob/18a09da1a04306534ff9e1df8a1a69c0192a3244/joycontrol/controller_state.py#L113-L122)
