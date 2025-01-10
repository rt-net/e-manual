---
title: ROS パッケージのインストール
robot: Jetson Nano Mouse
---

# ROSパッケージのインストール

このページでは、Jetson Nano MouseのROSサンプル集のインストール方法を説明します。

## 前提条件 {: #requirements}

- [ROSのインストール方法](./install.md)に沿ってROSをインストール済みであること

## ROSパッケージのインストール {: #ros-pkg-installation}

Jetson Nano Mouseを動かすためのROSパッケージ（制御用のパッケージ）はRaspberry Pi Mouseと同じものを使用します。

Jetson Nano Mouseへのインストールは必須です。ノートパソコン等のPCとJetson Nano Mouseで処理を分散する場合（[ROSサンプル](./samples.md)で扱います）はPCへのインストールも必要です。

=== "ROS"
    * 制御用のパッケージ（[ryuichiueda/raspimouse_ros_2](https://github.com/ryuichiueda/raspimouse_ros_2){target=_blank rel=noopener}）
    * サンプル集パッケージ
        * Raspberry Pi Mouseと共通のもの（[rt-net/raspimouse_ros_examples](https://github.com/rt-net/raspimouse_ros_examples){target=_blank rel=noopener}）
        * Jetson Nano Mouse用のもの（[rt-net/jnmouse_ros_examples](https://github.com/rt-net/jnmouse_ros_examples){target=_blank rel=noopener}）
    * ナビゲーション用のパッケージ（[rt-net/raspimouse_slam_navigation_ros](https://github.com/rt-net/raspimouse_slam_navigation_ros){target=_blank rel=noopener}）

    上記をダウンロードしてインストールします。

    ```sh
    $ source /opt/ros/melodic/setup.bash
    $ mkdir -p ~/catkin_ws/src
    $ cd ~/catkin_ws/src
    # Clone ROS packages
    $ git clone https://github.com/ryuichiueda/raspimouse_ros_2
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse_ros_examples 
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/jnmouse_ros_examples
    $ git clone https://github.com/rt-net/jetson_nano_csi_cam_ros.git
    $ git clone https://github.com/rt-net/gscam.git
    $ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspimouse_slam_navigation_ros 

    # Install dependencies
    $ rosdep update --include-eol-distros
    $ rosdep install -r -y --from-paths . --ignore-src      

    # make & install
    $ cd ~/catkin_ws && catkin_make
    $ source ~/catkin_ws/devel/setup.bash
    ```
