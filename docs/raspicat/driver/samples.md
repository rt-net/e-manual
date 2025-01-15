---
title: デバイスドライバのサンプルを動かす
robot: Raspberry Pi Cat
---

# サンプルプログラムの実行

デバイスドライバをインストールしたら、
サンプルプログラムを実行しましょう。

このサンプルプログラムは[Apache License, Version 2.0](https://www.apache.org/licenses/LICENSE-2.0)でオープンソースソフトウェアとして公開しています。  
[https://github.com/rt-net/RaspberryPiMouse/tree/master/SampleProgram](https://github.com/rt-net/RaspberryPiMouse/tree/master/SampleProgram){target=_blank rel=noopener}


## 使用機材 {: #requirements}

* セットアップ済みRaspberry Pi Cat本体
    * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
    * Raspberry PiにOS（**Ubuntu Server 18.04** or **Ubuntu Server 22.04**）がインストール済みであること
        * Raspberry Pi OSでのROS / ROS 2の動作は確認していません。
    * **デバイスドライバ**がインストール済みであること
        * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
* USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
* ノートパソコン等のPC

## 事前準備 {: #preparation}

[デバイスドライバのインストール](./install.md)の[Raspberry Piへのログイン](./install.md#raspberry-pi-login)に沿ってRaspberry Piへログインします。

サンプルプログラムのディレクトリに移動します。

```sh
$ cd ~/RaspberryPiMouse/SampleProgram
```

ディレクトリにあるプログラムを実行すると、LEDやブザーを動かせます。

## LED {: #step1}

LED0〜LED3が点滅します。

=== "Shell Script"
    ```sh
    $ bash step1.sh
    ```

=== "C"
    ```sh
    $ gcc step1.c -o step1
    $ ./step1
    ```

=== "Python"
    ```sh
    $ python3 step1.py
    ```

## ブザー {: #step2}

キーボード入力でブザーを鳴らします。 入力キーと音階のペアは[SCALE](https://github.com/rt-net/RaspberryPiMouse/blob/master/SampleProgram/SCALE)を参照してください。

++0++を入力するとブザーが止まります。 ++c++を入力するとプログラムが終了します。（Shell Scriptでは++ctrl+c++で終了します。）

=== "Shell Script"
    ```sh
    $ bash step2.sh
    ```

=== "C"
    ```sh
    $ gcc step2.c -o step2
    $ ./step2
    ```

=== "Python"
    ```sh
    $ python3 step2.py
    ```

## スイッチ {: #step3}

スイッチを押してLEDを点消灯します。 組み合わせは次のとおりです。

* SW0 : LED3
* SW1 : LED1 & LED2
* SW2 : LED0

=== "Shell Script"
    ```sh
    $ bash step3.sh
    ```

=== "C"
    ```sh
    $ gcc step3.c -o step3
    $ ./step3
    ```

=== "Python"
    ```sh
    $ python3 step3.py
    ```

## モータ {: #step4}

モータを回して右旋回、左旋回します。

=== "Shell Script"
    ```sh
    $ bash step4.sh
    ```

=== "C"
    ```sh
    $ gcc step4.c -o step4
    $ ./step4
    ```

=== "Python"
    ```sh
    $ python3 step4.py
    ```

## ライトセンサ {: #step5}

ライトセンサの値を読み込みます。

=== "Shell Script"
    ```sh
    $ bash step5.sh
    ```

=== "C"
    ```sh
    $ gcc step5.c -o step5
    $ ./step5
    ```

=== "Python"
    ```sh
    $ python3 step5.py
    ```

## パルスカウンタ {: #step6}

モータを回して、パルスカウンタの値を読み込みます。

=== "Shell Script"
    ```sh
    $ bash step6.sh
    ```

    次のようにstep6.shを編集することで、符号付きのカウント値を読み取ることができます。

    ```sh
    # step6.sh
    MOTOR_R=/dev/rtmotor_raw_r0
    MOTOR_L=/dev/rtmotor_raw_l0
    COUNTER_R=/dev/rtcounter_r1  # r0からr1に書き換え
    COUNTER_L=/dev/rtcounter_l1  # l0からl1に書き換え
    ```

=== "C"
    ```sh
    $ gcc step6.c -o step6
    $ ./step6
    ```

    次のようにstep6.cを編集することで、符号付きのカウント値を読み取ることができます。  
    C言語の場合は、書き換えたあとに`gcc`コマンドでコンパイルを行う必要があります。

    ```sh
    // step6.c
    #define FILE_MOTOR_L "/dev/rtmotor_raw_l0"
    #define FILE_MOTOR_R "/dev/rtmotor_raw_r0"
    #define FILE_COUNT_L "/dev/rtcounter_l1"  # r0からr1に書き換え
    #define FILE_COUNT_R "/dev/rtcounter_r1"  # l0からl1に書き換え
    ```

=== "Python"
    ```sh
    $ python3 step6.py
    ```

    次のようにstep6.pyを編集することで、符号付きのカウント値を読み取ることができます。

    ```sh
    # step6.py
    filename_motor_r = "/dev/rtmotor_raw_r0"
    filename_motor_l = "/dev/rtmotor_raw_l0"
    filename_count_r = "/dev/rtcounter_r1"  # r0からr1に書き換え
    filename_count_l = "/dev/rtcounter_l1"  # l0からl1に書き換え
    ```