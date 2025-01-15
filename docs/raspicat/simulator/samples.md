---
title: シミュレータでサンプルを動かす
robot: Raspberry Pi Cat
---

# シミュレータでサンプルを動かす

=== "ROS"

    このページでは、 
    [raspicat_sim](https://github.com/rt-net/raspicat_sim){target=_blank rel=noopener}
    パッケージと
    [raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation){target=_blank rel=noopener}
    パッケージを使ってシミュレータ上  
    でSLAM / Navigationを実施する方法について説明します。

=== "ROS 2"

    このページでは、 
    [raspicat_sim](https://github.com/rt-net/raspicat_sim/tree/ros2){target=_blank rel=noopener}
    パッケージと
    [raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation/tree/ros2){target=_blank rel=noopener}
    パッケージを使ってシミュレータ上  
    でSLAM / Navigationを実施する方法について説明します。

## 使用機材 {: #requirements}

=== "ROS"

    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 18.04**）、ROS、シミュレータがインストール済みであることを前提としています
            * インストール方法については[ROSのインストール](../ros/install.md)と[シミュレータのインストール](./install.md)を参照してください。

=== "ROS 2"

    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 22.04**）、ROS 2、シミュレータがインストール済みであることを前提としています
            * インストール方法については[ROSのインストール](../ros/install.md)と[シミュレータのインストール](./install.md)を参照してください。

## シミュレータ環境を立ち上げる {: #launch}

!!! Tip
    初回起動時において、Gazeboはモデルをインターネットからダウンロードします。  
    そのため、シミュレータ環境の立ち上げに時間がかかる場合があります。  
    事前にモデルをダウンロードしている場合は、すぐにシミュレータ環境が立ち上がります。

=== "ROS"

    #### Gazeboのみ立ち上げ
    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false
    ```
    #### GazeboとRVizの立ち上げ
    ```sh
    # シミュレータおよびRVizの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch
    ```

=== "ROS 2"

    #### Gazeboのみ立ち上げ
    ```sh
    # シミュレータの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py rviz:=false
    ```
    #### GazeboとRVizの立ち上げ
    ```sh
    # シミュレータおよびRVizの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py
    ```

## 操縦する {: #teleop}

=== "ROS"

    ![raspicat_sim](https://rt-net.github.io/images/raspberry-pi-cat/raspicat_gazebo_with_iscas_museum.gif)

### キーボードで操縦する {: #teleop-keyboard}

=== "ROS"

    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ roslaunch raspicat_bringup teleop.launch joy:=false
    ```

=== "ROS 2"

    ```sh
    # シミュレータの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup teleop.launch.py teleop:=key
    ```

### ジョイスティックコントローラで操縦する {: #teleop-joy}

=== "ROS"

    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ roslaunch raspicat_bringup teleop.launch joy:=true
    ```

=== "ROS 2"

    ```sh
    # シミュレータの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py

    # キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup teleop.launch.py teleop:=joy
    ```

## シミュレータ環境でSLAM {: #slam}

#### キーボードで操縦 {: #slam-teleop-keyboard}
=== "ROS"

    === "gmapping"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # gmappingの実行
        $ roslaunch raspicat_slam raspicat_gmapping.launch

        # キーボード操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=false

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```
    === "cartographer"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # cartographerの実行
        $ roslaunch raspicat_slam raspicat_cartographer.launch

        # キーボード操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=false

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```
    === "slam_toolbox"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # slam_toolboxの実行
        $ roslaunch raspicat_slam raspicat_slam_toolbox.launch

        # キーボード操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=false

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```

=== "ROS 2"

    === "slam_toolbox"
    ```sh
    # シミュレータの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py

    # slam_toolboxの実行
    $ ros2 launch raspicat_slam raspicat_slam_toolbox.launch.py

    # キーボード操縦用ノードの立ち上げ
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup teleop.launch.py teleop:=key

    # 地図の保存（地図の保存先は任意のパスを指定できます）
    $ ros2 run nav2_map_server map_saver_cli -f ~/map
    ```

#### ジョイスティックコントローラで操縦 {: #slam-teleop-joy}
=== "ROS"

    === "gmapping"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # gmappingの実行
        $ roslaunch raspicat_slam raspicat_gmapping.launch

        # ジョイスティック操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=true

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```
        <video width="100%" controls>
            <source src="https://user-images.githubusercontent.com/40545422/166213251-22115ecb-11a2-4caa-95c7-c3b8e85db3ad.mp4" type="video/mp4">
        </video>

    === "cartographer"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # cartographerの実行
        $ roslaunch raspicat_slam raspicat_cartographer.launch

        # ジョイスティック操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=true

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```
        <video width="100%" controls>
            <source src="https://user-images.githubusercontent.com/40545422/166214138-624d3fd0-2bf8-46d7-a722-1602007df086.mp4" type="video/mp4">
        </video>

    === "slam_toolbox"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # slam_toolboxの実行
        $ roslaunch raspicat_slam raspicat_slam_toolbox.launch

        # ジョイスティック操縦用ノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=true

        # 地図の保存（map_fileへのパスは任意のパスを指定できます）
        $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
        ```
        <video width="100%" controls>
            <source src="https://user-images.githubusercontent.com/40545422/166214198-38cf95ee-57ce-45bf-82d4-447f93419560.mp4" type="video/mp4">
        </video>

=== "ROS 2"

    === "slam_toolbox"
    ```sh
    # シミュレータの起動
    $ ros2 launch  raspicat_gazebo raspicat_with_iscas_museum.launch.py

    # slam_toolboxの実行
    $ ros2 launch raspicat_slam raspicat_slam_toolbox.launch.py

    # キーボード操縦用ノードの立ち上げ
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup teleop.launch.py teleop:=joy

    # 地図の保存（地図の保存先は任意のパスを指定できます）
    $ ros2 run nav2_map_server map_saver_cli -f ~/map
    ```

## シミュレータ環境でナビゲーション {: #navigation}
=== "ROS"

    === "move_base"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # move_baseの実行
        $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="move_base" mcl_map_file:=map-path navigation_map_file:=map-path
        ```

        <video width="100%" controls>
            <source src="https://user-images.githubusercontent.com/40545422/166214286-c78d74d1-cab1-489d-85fc-68416d48c655.mp4" type="video/mp4">
        </video>

    === "neonavigation"
        ```sh
        # シミュレータの起動
        $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

        # neonavigationの実行
        $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="neonav" mcl_map_file:=map-path navigation_map_file:=map-path
        ```

        <video width="100%" controls>
            <source src="https://user-images.githubusercontent.com/40545422/166214304-23606730-3d8e-4ed4-9f9d-7834a3788aba.mp4" type="video/mp4">
        </video>

=== "ROS 2"

    === "Navigation 2"
        ```sh
        # シミュレータの起動
        $ ros2 launch raspicat_gazebo raspicat_with_iscas_museum.launch.py
        
        # Navigation 2の実行
        $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
        $ ros2 launch raspicat_navigation raspicat_nav2.launch.py map:=$HOME/map.yaml
        ```

    !!! Tip
        RVizのパネルにある`2D Pose Estimate`を使用して、初期位置をセットすることで
        ナビゲーションが始まります。