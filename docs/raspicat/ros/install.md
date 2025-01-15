---
title: ROS / ROS 2のインストール
robot: Raspberry Pi Cat
---

# ROS / ROS 2のインストール

このページでは、以下の2つのデバイスへのROS / ROS 2のインストール方法を説明します。

* Ubuntuインストール済みのノートパソコン等のPC
* Ubuntuインストール済みのRaspberry Pi

なお、ROS / ROS 2をすでにインストール済みの場合はこのページの手順は不要です。

## 使用機材 {: #requirements}
=== "ROS"
    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 18.04**）がインストール済みであること
            * Raspberry Pi OSでのROSの動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 18.04**）がインストール済みであること

=== "ROS 2"
    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 22.04**）がインストール済みであること
            * Raspberry Pi OSでのROSの動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 22.04**）がインストール済みであること

## 事前準備 {: #preparation}

[デバイスドライバのインストール](../driver/install.md)の[Raspberry Piへのログイン](../driver/install.md#raspberry-pi-login)に沿ってRaspberry Piへログインします。

## ROS / ROS 2のインストール（公式のドキュメントに沿って進める場合） {: #official-document}

=== "ROS"
    [https://wiki.ros.org/melodic/Installation/Ubuntu](https://wiki.ros.org/melodic/Installation/Ubuntu){target=_blank rel=noopener}
    を参考に、**ROS Melodic**をインストールします。

=== "ROS 2"
    [https://docs.ros.org/humble/Installation/Ubuntu](https://docs.ros.org/en/humble/Installation/Ubuntu-Install-Debians.html){target=_blank rel=noopener}
    を参考に、**ROS 2 Humble**をインストールします。

GUIを使用する場合は**Desktop-Full**を、 使用しない場合は**ROS-Base**パッケージをインストールします。

## ROS / ROS 2のインストール（インストールスクリプトを使う場合） {: #install-script}

非公式のインストールスクリプトを使うことで簡単にROS / ROS 2をインストールできます。

### ノートパソコン等のPC

=== "ROS"

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

=== "ROS 2"

    このROS 2インストールスクリプトは`curl`と`git`を使用します。以下のコマンドでインストールします。
    
    ```sh
    $ sudo apt install -y curl git
    ```
    
    以下のコマンドでROS 2インストールスクリプトを実行します。  
    環境にもよりますが完了までは10分以上かかる場合があります。
    
    ```sh
    $ git clone https://github.com/Tiryoh/ros2_setup_scripts_ubuntu.git
    $ cd ros2_setup_scripts_ubuntu
    $ ./ros2-humble-desktop-main.sh
    ```
    
    スクリプトが問題なく実行できれば以下のメッセージが表示されます。
    
    ```txt
    Success installing ROS 2 humble
    Run 'source /opt/ros/humble/setup.bash'
    ```
    
    以下のコマンドでインストールした設定を読み込みます。
    
    ```sh
    $ source ~/.bashrc
    ```

### Raspberry Pi

!!! Warning 
    **Ubuntu PC**とssh接続された状態において、**Raspberry Pi**内で  
    `sudo apt install`、`git clone`コマンドを正常に実行するためには、それぞれ正しい時刻に設定しておく必要があります。


=== "ROS"

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


=== "ROS 2"

    このROS 2インストールスクリプトは`curl`と`git`を使用します。以下のコマンドでインストールします。
    
    ```sh
    $ sudo apt install -y curl git
    ```
    
    以下のコマンドでROS 2インストールスクリプトを実行します。  
    環境にもよりますが完了までは10分以上かかる場合があります。
    
    ```sh
    $ git clone https://github.com/Tiryoh/ros2_setup_scripts_ubuntu.git
    $ cd ros2_setup_scripts_ubuntu
    $ ./ros2-humble-ros-base-main.sh
    ```
    
    スクリプトが問題なく実行できれば以下のメッセージが表示されます。
    
    ```txt
    Success installing ROS 2 humble
    Run 'source /opt/ros/humble/setup.bash'
    ```
    
    以下のコマンドでインストールした設定を読み込みます。
    
    ```sh
    $ source ~/.bashrc
    ```
