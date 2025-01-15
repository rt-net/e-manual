---
title: ROSのインストール
robot: Raspberry Pi Cat
---

# ROSのインストール

このページでは、以下の2つのデバイスへのROSのインストール方法を説明します。

* Ubuntuインストール済みのノートパソコン等のPC
* Ubuntuインストール済みのRaspberry Pi

なお、ROSをすでにインストール済みの場合はこのページの手順は不要です。

## 使用機材 {: #requirements}

* セットアップ済みRaspberry Pi Cat本体
    * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
    * Raspberry PiにOS（**Ubuntu Server 18.04**）がインストール済みであること
        * Raspberry Pi OSでのROSの動作は確認していません。
    * **デバイスドライバ**がインストール済みであること
        * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
* USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
* ノートパソコン等のPC
    * OS（**Ubuntu Desktop 18.04**）がインストール済みであること

## 事前準備 {: #preparation}

[デバイスドライバのインストール](../driver/install.md)の[Raspberry Piへのログイン](../driver/install.md#raspberry-pi-login)に沿ってRaspberry Piへログインします。

## ROSのインストール（公式のドキュメントに沿って進める場合） {: #official-document}


[https://wiki.ros.org/melodic/Installation/Ubuntu](https://wiki.ros.org/melodic/Installation/Ubuntu){target=_blank rel=noopener}
を参考に、**ROS Melodic**をインストールします。

GUIを使用する場合は**Desktop-Full**を、 使用しない場合は**ROS-Base**パッケージをインストールします。

## ROSのインストール（インストールスクリプトを使う場合） {: #install-script}

### ノートパソコン等のPC

非公式のインストールスクリプトを使うことで簡単にROSをインストールできます。

このROSインストールスクリプトは`curl`を使用します。以下のコマンドでインストールします。

```sh
$ sudo apt install -y curl
```

以下のコマンドでROSインストールスクリプトを実行します。  
環境にもよりますが完了までは10分以上かかる場合があります。

```sh
$ bash -c "$(curl -SsfL u.ty0.jp/ros-melodic-desktop)"
```

スクリプトが問題なく実行できれば以下のメッセージが表示されます。

```txt
Success installing ROS melodic
Run 'source ~/.bashrc'

If any error occurs, please refer to the following URL.
https://github.com/Tiryoh/ros_setup_scripts_ubuntu/
```

以下のコマンドでインストールした設定を読み込みます。

```sh
$ source ~/.bashrc
```

### Raspberry Pi

非公式のインストールスクリプトを使うことで簡単にROSをインストールできます。

このROSインストールスクリプトは`curl`を使用します。以下のコマンドでインストールします。

```sh
$ sudo apt install -y curl
```

以下のコマンドでROSインストールスクリプトを実行します。  
環境にもよりますが完了までは10分以上かかる場合があります。

```sh
$ bash -c "$(curl -SsfL u.ty0.jp/ros-melodic-ros-base)"
```

スクリプトが問題なく実行できれば以下のメッセージが表示されます。

```txt
Success installing ROS melodic
Run 'source ~/.bashrc'

If any error occurs, please refer to the following URL.
https://github.com/Tiryoh/ros_setup_scripts_ubuntu/
```

以下のコマンドでインストールした設定を読み込みます。

```sh
$ source ~/.bashrc
```
