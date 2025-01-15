---
title: シミュレータのインストール
robot: Raspberry Pi Cat
---

# シミュレータのインストール

=== "ROS"

    このページでは、
    [raspicat_sim](https://github.com/rt-net/raspicat_sim){target=_blank rel=noopener}
    パッケージを用いたシミュレーションのセットアップ方法と実行方法について説明します。

=== "ROS 2"

    このページでは、
    [raspicat_sim](https://github.com/rt-net/raspicat_sim/tree/ros2){target=_blank rel=noopener}
    パッケージを用いたシミュレーションのセットアップ方法と実行方法について説明します。

## 使用機材 {: #requirements}

=== "ROS"

    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 18.04**）およびROSがインストール済みであることを前提としています
            * インストール方法については[ROS / ROS 2のインストール](../ros/install.md)を参照してください

=== "ROS 2"

    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 22.04**）およびROS 2がインストール済みであることを前提としています
            * インストール方法については[ROS / ROS 2のインストール](../ros/install.md)を参照してください

## パッケージのダウンロードとインストール {: #install}

=== "ROS"

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

=== "ROS 2"

    制御用のパッケージである
    [rt-net/raspimouse2](https://github.com/rt-net/raspimouse2){target=_blank rel=noopener}、[rt-net/raspicat_ros](https://github.com/rt-net/raspicat_ros/tree/ros2){target=_blank rel=noopener}  
    SLAM/ナビゲーション用のパッケージである
    [rt-net/raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation/tree/ros2){target=_blank rel=noopener}  
    RaspberryPiCat用のURDF記述マクロを含んだパッケージである
    [rt-net/raspicat_description](https://github.com/rt-net/raspicat_description/tree/ros2){target=_blank rel=noopener}  
    をダウンロードしてインストールします。

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

    # Gazeboで使用するハードウェアモデルデータをダウンロード
    $ ros2 run raspicat_gazebo download_gazebo_models.sh 
    ```

## 動作確認（キーボードで操縦） {: #teleop}

=== "ROS"

    !!! Tip
        初回起動時において、Gazeboはモデルをインターネットからダウンロードします。  
        そのため、シミュレータ環境の立ち上げに時間がかかる場合があります。

    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ roslaunch raspicat_bringup teleop.launch joy:=false
    ```

=== "ROS 2"

    !!! Tip
        初回起動時において、Gazeboはモデルをインターネットからダウンロードします。  
        そのため、シミュレータ環境の立ち上げに時間がかかる場合があります。

    ```sh
    # シミュレータの起動
    $ ros2 launch raspicat_gazebo raspicat_with_iscas_museum.launch.py

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup teleop.launch.py teleop:=key
    ```