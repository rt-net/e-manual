---
title: シミュレータのインストール
robot: Raspberry Pi Mouse
---

# シミュレータのインストール

Raspberry Pi Mouse Simulator（[rt-net/raspimouse_sim](https://github.com/rt-net/raspimouse_sim){target=_blank rel=noopener}）はメインボードにRaspberry Piを使った左右独立二輪方式の小型移動プラットフォームロボット、Raspberry Pi Mouseのシミュレータです。


このページでは、
[rt-net/raspimouse_sim](https://github.com/rt-net/raspimouse_sim){target=_blank rel=noopener}
パッケージを用いたシミュレーションのセットアップ方法と動作確認方法を説明します。

## 使用機材 {: #requirements}

* ノートパソコン等のPC
    * OS（**Ubuntu Desktop**）がインストール済みであることを前提としています
        * `ROS 2 Jazzy`を使用する場合: **Ubuntu Desktop 24.04**
        * `ROS 2 Humble`を使用する場合: **Ubuntu Desktop 22.04**
        * `ROS Noetic`を使用する場合: **Ubuntu Desktop 20.04**
    * ROSまたはROS 2がインストール済みであることを前提としています
        * インストール方法については[ROS/ROS 2のインストール](../ros/install.md)を参照してください。

## パッケージのダウンロードとインストール {: #install}

=== "ROS"
    次のコマンドを実行します。

    シミュレータパッケージのダウンロード

    ```sh
    cd ~/catkin_ws/src
    git clone https://github.com/rt-net/raspimouse_sim.git
    ```

    依存パッケージをインストール

    ```sh
    git clone https://github.com/rt-net/raspimouse.git
    git clone https://github.com/rt-net/raspimouse_description.git
    rosdep install -r -y -i --from-paths raspimouse*
    ```

    パッケージをビルド

    ```sh
    cd ~/catkin_ws && catkin_make
    source ~/catkin_ws/devel/setup.bash
    ```

    Gazeboで使用するハードウェアモデルデータをダウンロード

    ```sh
    rosrun raspimouse_gazebo download_gazebo_models.sh
    ```

=== "ROS 2 Humble"
    次のコマンドを実行します。

    シミュレータパッケージのインストール

    ```sh
    sudo apt install ros-$ROS_DISTRO-raspimouse-sim
    ```

    サンプルパッケージのインストール

    ```sh
    sudo apt install ros-$ROS_DISTRO-raspimouse-slam-navigation
    ```

    キーボードで操作するためのパッケージをインストール

    ```sh
    sudo apt install ros-$ROS_DISTRO-teleop-twist-keyboard
    ```

=== "ROS 2 Jazzy"
    次のコマンドを実行します。

    Raspberry Piとノートパソコン等のPCそれぞれで、次のコマンドを実行します。

    1. シミュレータパッケージとサンプルパッケージのダウンロードとインストール
    
    ```sh
    $ source /opt/ros/jazzy/setup.bash
    $ mkdir -p ~/ros2_ws/src
    $ cd ~/ros2_ws/src

    # Download packages
    $ git clone -b $ROS_DISTRO https://github.com/rt-net/raspimouse_sim.git
    $ git clone -b $ROS_DISTRO https://github.com/rt-net/raspimouse_slam_navigation_ros2.git

    # Install dependencies
    $ rosdep install -r -y --from-paths . --ignore-src

    # make & install
    $ cd ~/ros2_ws
    $ colcon build --symlink-install
    $ source ~/ros2_ws/install/setup.bash
    ```

    キーボードで操作するためのパッケージをインストール

    ```sh
    sudo apt install ros-$ROS_DISTRO-teleop-twist-keyboard
    ```

## 動作確認（キーボードで操縦） {: #teleop}

=== "ROS"
    次のコマンドを実行します。

    シミュレータの起動

    ```sh
    roslaunch raspimouse_gazebo raspimouse_with_samplemaze.launch
    ```

    別のターミナルでコマンドを実行

    ```sh
    rosrun teleop_twist_keyboard teleop_twist_keyboard.py _speed:=0.1 _turn:=1.57
    ```

    ![](https://rt-net.github.io/images/raspberry-pi-mouse/raspimouse_sim_samplemaze_animation.gif)

=== "ROS 2 Humble"
    次のコマンドを実行します。

    シミュレータの起動

    ```sh
    ros2 launch raspimouse_gazebo raspimouse_with_emptyworld.launch.py
    ```

    別のターミナルでコマンドを実行

    ```sh
    ros2 run teleop_twist_keyboard teleop_twist_keyboard
    ```

    ![](https://rt-net.github.io/images/raspberry-pi-mouse/raspimouse_sim_joystick.gif)

=== "ROS 2 Jazzy"
    次のコマンドを実行します。

    シミュレータの起動

    ```sh
    ros2 launch raspimouse_gazebo raspimouse_with_emptyworld.launch.py
    ```

    別のターミナルでコマンドを実行

    ```sh
    ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -p stamped:=true
    ```

    ![](https://rt-net.github.io/images/raspberry-pi-mouse/raspimouse_sim_joystick.gif)
