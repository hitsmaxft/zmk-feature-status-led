# ZMK Status LED Module

This module provides LED status indicators for ZMK keyboards using gpio-leds. It displays different patterns to show the keyboard's current state:

- **Battery Level**: Blinks 1-3 times at startup depending on battery level
- **Advertising**: Blinks periodically while the keyboard is advertising
- **Connected**: Lights up briefly when a connection is established

## Installation

1. Add the module to your `zmk-config/config/west.yml` file in the `projects` section:

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: sekigon-gonnoc
      url-base: https://github.com/sekigon-gonnoc
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: zmk-feature-status-led
      remote: sekigon-gonnoc
      revision: main
  self:
    path: config
```

## Configuration

### 1. Add to your keyboard's device tree overlay

Update your keyboard's `.overlay` file to include the LED configuration and status LED node:

```dts
/ {
    // Existing keyboard config...

    // Add a LED node here if not already defined by the board
    leds {
        compatible = "gpio-leds";
        status_led: status_led {
            gpios = <&gpioX Y GPIO_ACTIVE_HIGH>; // Replace X and Y with your GPIO pin
            label = "Status LED";
        };
    };
    
    status_led_device: status_led_device {
        compatible = "zmk,status-led";
        led = <&status_led>;
    };
};
```

### 2. Enable the module in your keyboard's Kconfig file

Add the following to your keyboard's `Kconfig.defconfig`:

```kconfig
if ZMK_KEYBOARD_YOUR_KEYBOARD

config ZMK_STATUS_LED
    default y

endif
```

## Configuration Options

The module provides several configuration options that can be adjusted in your `prj.conf` file:

```
# Enable/disable specific features
# Show battery level at startup
CONFIG_ZMK_STATUS_LED_BATTERY_LEVEL=y
# Blink during advertising
CONFIG_ZMK_STATUS_LED_ADVERTISING=y
# Light on pairing
CONFIG_ZMK_STATUS_LED_CONNECTED=y
```

## Battery Level Indication

The module shows battery level at startup with the following patterns:

- **Low Battery** (0-30%): Blinks 1 time
- **Medium Battery** (31-70%): Blinks 2 times
- **High Battery** (71-100%): Blinks 3 times

## Troubleshooting

If your LED isn't working:

1. Check that your GPIO pin is correctly specified in the device tree
2. Verify that the module is enabled in your config
3. Make sure the LED is properly connected (consider polarity and current limiting resistors)
4. Check the ZMK logs for any errors related to the status LED initialization

## License

This module is released under the MIT License.

---

# ZMK ステータスLEDモジュール

このモジュールは、ZMKキーボード用のLEDステータスインジケーターをgpio-ledsを使用して提供します。キーボードの現在の状態を示すために異なるパターンを表示します：

- **バッテリーレベル**: 起動時にバッテリーレベルに応じて1〜3回点滅します
- **アドバタイジング**: キーボードがアドバタイジング中は定期的に点滅します
- **接続完了**: 接続が確立されると短時間点灯します

## インストール方法

1. モジュールを`zmk-config/config/west.yml`ファイルの`projects`セクションに追加します：

```yaml
manifest:
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: sekigon-gonnoc
      url-base: https://github.com/sekigon-gonnoc
  projects:
    - name: zmk
      remote: zmkfirmware
      revision: main
      import: app/west.yml
    - name: zmk-feature-status-led
      remote: sekigon-gonnoc
      revision: main
  self:
    path: config
```

## 設定方法

### 1. キーボードのデバイスツリーオーバーレイに追加

キーボードの`.overlay`ファイルを更新して、LED設定とステータスLEDノードを含めます：

```dts
/ {
    // Existing keyboard config...

    // Add a LED node here if not already defined by the board
    leds {
        compatible = "gpio-leds";
        status_led: status_led {
            gpios = <&gpioX Y GPIO_ACTIVE_HIGH>; // Replace X and Y with your GPIO pin
            label = "Status LED";
        };
    };
    
    status_led_device: status_led_device {
        compatible = "zmk,status-led";
        led = <&status_led>;
    };
};
```

### 2. キーボードのKconfigファイルでモジュールを有効化

キーボードの`Kconfig.defconfig`に以下を追加します：

```kconfig
if ZMK_KEYBOARD_YOUR_KEYBOARD

config ZMK_STATUS_LED
    default y

endif
```

## 設定オプション

このモジュールは、`prj.conf`ファイルで調整できるいくつかの設定オプションを提供します：

```
# 特定の機能の有効化/無効化
# 起動時にバッテリーレベルを表示
CONFIG_ZMK_STATUS_LED_BATTERY_LEVEL=y
# アドバタイジング中に点滅
CONFIG_ZMK_STATUS_LED_ADVERTISING=y
# ペアリング時に点灯
CONFIG_ZMK_STATUS_LED_CONNECTED=y
```

## バッテリーレベル表示

モジュールは起動時に以下のパターンでバッテリーレベルを表示します：

- **バッテリー残量：低** (0-30%): 1回点滅
- **バッテリー残量：中** (31-70%): 2回点滅
- **バッテリー残量：高** (71-100%): 3回点滅

## トラブルシューティング

LEDが動作しない場合：

1. デバイスツリーでGPIOピンが正しく指定されていることを確認
2. 設定でモジュールが有効になっていることを確認
3. LEDが適切に接続されていることを確認（極性と電流制限抵抗を考慮）
4. ZMKログでステータスLEDの初期化に関連するエラーを確認

## ライセンス

このモジュールはMITライセンスの下でリリースされています。
