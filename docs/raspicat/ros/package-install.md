---
title: ROS / ROS 2パッケージのインストール
robot: Raspberry Pi Cat
---

# ROSパッケージのインストール

このページでは、Raspberry Pi/Ubuntu PCへのROS / ROS 2パッケージのインストール方法を説明します。

## 使用機材 {: #requirements}

=== "ROS"

    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 18.04**）がインストール済みであること
            * Raspberry Pi OSでのROSの動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
        * ROSがインストール済みであること
            * [ROS / ROS 2のインストール手順](./install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 18.04**）がインストール済みであること

=== "ROS 2"

    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 22.04**）がインストール済みであること
            * Raspberry Pi OSでのROS 2の動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
        * ROS 2がインストール済みであること
            * [ROS / ROS 2のインストール手順](./install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 22.04**）がインストール済みであること

## 事前準備 {: #preparation}

[デバイスドライバのインストール](../driver/install.md)の[Raspberry Piへのログイン](../driver/install.md#raspberry-pi-login)に沿ってRaspberry Piへログインします。

## ROS / ROS 2パッケージのインストール {: #install-ros-packages}

=== "ROS"

    制御用のパッケージである
    [rt-net/raspimouse](https://github.com/rt-net/raspimouse){target=_blank rel=noopener}、[rt-net/raspicat_ros](https://github.com/rt-net/raspicat_ros){target=_blank rel=noopener}  
    SLAM/ナビゲーション用のパッケージである
    [rt-net/raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation){target=_blank rel=noopener}  
    RaspberryPiCat用のURDF記述マクロを含んだパッケージである
    [rt-net/raspicat_description](https://github.com/rt-net/raspicat_description){target=_blank rel=noopener}  
    をダウンロードしてインストールします。

    !!! Tip
        **Raspberry Pi**と**PC**の両方にダウンロードおよびインストールを行います。
    
    !!! Warning 
        **Ubuntu PC**とssh接続された状態において、**Raspberry Pi**内で  
        `git clone`、`rosdep install`、`catkin_make`コマンドを正常に実行するためには、それぞれ正しい時刻に設定しておく必要があります。

    ```sh
    # Create workspace
    $ source /opt/ros/melodic/setup.bash
    $ mkdir -p ~/catkin_ws/src
    $ cd ~/catkin_ws/src

    # Clone ROS packages
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse.git
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse_description.git
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspicat_ros.git
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspicat_slam_navigation.git
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspicat_description.git

    # Install dependencies
    $ rosdep update
    $ rosdep install -r -y -i --from-paths raspicat* raspimouse*

    # make & install
    $ cd ~/catkin_ws && catkin_make
    $ source ~/catkin_ws/devel/setup.bash
    ```

=== "ROS 2"

    制御用のパッケージである
    [rt-net/raspimouse2](https://github.com/rt-net/raspimouse2){target=_blank rel=noopener}、[rt-net/raspicat_ros](https://github.com/rt-net/raspicat_ros/tree/ros2){target=_blank rel=noopener}  
    SLAM/ナビゲーション用のパッケージである
    [rt-net/raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation/tree/ros2){target=_blank rel=noopener}  
    RaspberryPiCat用のURDF記述マクロを含んだパッケージである
    [rt-net/raspicat_description](https://github.com/rt-net/raspicat_description/tree/ros2){target=_blank rel=noopener}  
    をダウンロードしてインストールします。

    !!! Tip
        **Raspberry Pi**と**PC**の両方にダウンロードおよびインストールを行います。

    !!! Warning 
        **Ubuntu PC**とssh接続された状態において、**Raspberry Pi**内で  
        `git clone`、`rosdep install`、`colcon build`コマンドを正常に実行するためには、それぞれ正しい時刻に設定しておく必要があります。

    ```sh
    # Create workspace
    $ source /opt/ros/humble/setup.bash
    $ mkdir -p ~/catkin_ws/src
    $ cd ~/catkin_ws/src

    # Clone ROS 2 packages
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse2.git
    $ git clone -b ros2 https://github.com/rt-net/raspicat_ros.git
    $ git clone -b ros2 https://github.com/rt-net/raspicat_description.git
    $ git clone -b ros2 https://github.com/rt-net/raspicat_sim.git
    $ git clone -b ros2 https://github.com/rt-net/raspicat_slam_navigation.git

    # Install dependencies
    $ rosdep update
    $ rosdep install -r -y -i --from-paths raspicat* raspimouse*
    $ sudo apt install xterm

    # make & install
    $ cd ~/catkin_ws && colcon build --symlink-install
    $ source ~/catkin_ws/install/setup.bash
    ```