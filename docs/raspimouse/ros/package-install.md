---
title: ROS / ROS 2パッケージのインストール
robot: Raspberry Pi Mouse
---

# ROS / ROS 2パッケージのインストール

このページでは、Raspberry Pi MouseへのROSサンプル集のインストール方法を説明します。

## 使用機材 {: #requirements}

* 組み立て済みRaspberry Pi Mouse本体
    * [製品マニュアル](https://rt-net.jp/products/raspberrypimousev3/#downloads)を読んで組み立て済みの状態を前提としています
    * OS（**Ubuntu Server**）がインストール済みであることを前提としています
        * `ROS 2 Jazzy`を使用する場合: **Ubuntu Server 24.04**
        * `ROS 2 Humble`を使用する場合: **Ubuntu Server 22.04**
        * `ROS Noetic`を使用する場合: **Ubuntu Server 20.04**
        * Raspberry Pi OSでのROSの動作は確認していません
    * **デバイスドライバ**がインストール済みであることを前提としています
        * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
    * ROSまたはROS 2がインストール済みであることを前提としています
        * [ROS/ROS 2のインストール手順](./install.md)を参照してください
* Raspberry Pi Mouse用電源
    * バッテリでも電源変換ケーブルつきACアダプタでも可
* ノートパソコン等のPC
    * OS（**Ubuntu Desktop**）がインストール済みであることを前提としています
        * `ROS 2 Jazzy`を使用する場合: **Ubuntu Desktop 24.04**
        * `ROS 2 Humble`を使用する場合: **Ubuntu Desktop 22.04**
        * `ROS Noetic`を使用する場合: **Ubuntu Desktop 20.04**

## ROS / ROS 2パッケージのインストール {: #installation}

=== "ROS"
    Raspberry Piとノートパソコン等のPCそれぞれで、
    制御用のパッケージである
    [ryuichiueda/raspimouse_ros_2](https://github.com/ryuichiueda/raspimouse_ros_2){target=_blank rel=noopener}
    と、サンプル集パッケージ
    [rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples){target=_blank rel=noopener}
    および、ナビゲーション用のパッケージ
    [rt-net/raspimouse_slam_navigation_ros](https://github.com/rt-net/raspimouse_slam_navigation_ros){target=_blank rel=noopener}
    をダウンロードしてインストールします。

    ```sh
    $ source /opt/ros/noetic/setup.bash
    $ mkdir -p ~/catkin_ws/src
    $ cd ~/catkin_ws/src
    # Clone ROS packages
    $ git clone https://github.com/ryuichiueda/raspimouse_ros_2
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse_ros_examples 
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse_slam_navigation_ros 
    # For direction control example
    $ git clone https://github.com/rt-net/rt_usb_9axisimu_driver

    # Install dependencies
    $ rosdep install -r -y --from-paths . --ignore-src      

    # make & install
    $ cd ~/catkin_ws && catkin_make
    $ source ~/catkin_ws/devel/setup.bash
    ```

=== "ROS 2 Humble"
    Raspberry Piとノートパソコン等のPCそれぞれで次のコマンドを実行します。

    ```sh
    $ sudo apt install ros-humble-raspimouse-slam-navigation
    ```

    これにより、以下のパッケージがインストールされます。

    * [raspimouse2](https://github.com/rt-net/raspimouse2) : 制御パッケージ
    * [raspimouse_ros2_examples](https://github.com/rt-net/raspimouse_ros2_examples) : サンプル集パッケージ
    * [raspimouse_slam_navigation_ros2](https://github.com/rt-net/raspimouse_slam_navigation_ros2) : SLAMとナビゲーションのパッケージ 
    * [raspimouse_description](https://github.com/rt-net/raspimouse_description) : ロボットモデルパッケージ
    * [rt_usb_9axisimu_driver](https://github.com/rt-net/rt_usb_9axisimu_driver/tree/ros2-devel) : 9軸IMU制御パッケージ

=== "ROS 2 Jazzy"
    Raspberry Piとノートパソコン等のPCそれぞれで次のコマンドを実行します。

    ```sh
    $ sudo apt install ros-jazzy-raspimouse-slam-navigation
    ```

    これにより、以下のパッケージがインストールされます。

    * [raspimouse2](https://github.com/rt-net/raspimouse2) : 制御パッケージ
    * [raspimouse_ros2_examples](https://github.com/rt-net/raspimouse_ros2_examples) : サンプル集パッケージ
    * [raspimouse_slam_navigation_ros2](https://github.com/rt-net/raspimouse_slam_navigation_ros2) : SLAMとナビゲーションのパッケージ 
    * [raspimouse_description](https://github.com/rt-net/raspimouse_description) : ロボットモデルパッケージ
    * [rt_usb_9axisimu_driver](https://github.com/rt-net/rt_usb_9axisimu_driver/tree/ros2-devel) : 9軸IMU制御パッケージ

!!! info
    ROS/ROS 2を同時にインストールした環境で実行する場合、`/opt/ros/*/setup.bash`を実行したいROSのバージョンにあわせて都度読み込む必要があります。  
    `catkin_make`や`colcon build`でエラーが出た場合はご確認ください。