---
title: シミュレータのインストール
robot: Raspberry Pi Cat
---

# シミュレータのインストール


このページでは、
[raspicat_sim](https://github.com/rt-net/raspicat_sim){target=_blank rel=noopener}
パッケージを用いたシミュレーションのセットアップ方法と実行方法について説明します。


## 使用機材 {: #requirements}

* ノートパソコン等のPC
    * OS（**Ubuntu Desktop 18.04**）およびROSがインストール済みであることを前提としています
        * インストール方法については[ROSのインストール](../ros/install.md)を参照してください。

## パッケージのダウンロードとインストール {: #install}

制御用のパッケージである
[rt-net/raspimouse](https://github.com/rt-net/raspimouse){target=_blank rel=noopener}、[rt-net/raspicat_ros](https://github.com/rt-net/raspicat_ros){target=_blank rel=noopener}   
SLAM/ナビゲーション用のパッケージである
[rt-net/raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation){target=_blank rel=noopener}  
シミュレータ用のパッケージである
[rt-net/raspicat_sim](https://github.com/rt-net/raspicat_sim.git){target=_blank rel=noopener}  
RaspberryPiCat用のURDF記述マクロを含んだパッケージである
[rt-net/raspicat_description](https://github.com/rt-net/raspicat_description){target=_blank rel=noopener}  
をダウンロードしてインストールします。

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
$ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspicat_sim.git
$ git clone -b $ROS_DISTRO-devel https://github.com/rt-net/raspicat_description.git

# Install dependencies
$ rosdep update
$ rosdep install -r -y -i --from-paths raspicat* raspimouse*

# make & install
$ cd ~/catkin_ws && catkin_make
$ source ~/catkin_ws/devel/setup.bash

# Gazeboで使用するハードウェアモデルデータをダウンロード
$ rosrun raspicat_gazebo download_gazebo_models.sh
```

## 動作確認（キーボードで操縦） {: #teleop}

!!! Tip
    初回起動時において、Gazeboはモデルをインターネットからダウンロードします。  
    そのため、シミュレータ環境の立ち上げに時間がかかる場合があります。

```sh
# シミュレータの起動
$ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

# キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
$ roslaunch raspicat_bringup teleop.launch joy:=false
```
