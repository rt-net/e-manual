---
title: ROS / ROS 2の実行
robot: Raspberry Pi Cat
---

# ROS / ROS 2を使用してRaspberry Pi Catの制御

=== "ROS"

    このページでは
    実機を使用した、SLAMからナビゲーションまでの実行方法を説明します。
    ## 使用機材 {: #requirements}

    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 18.04**）がインストール済みであること
            * Raspberry Pi OSでのROSの動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
        * ROSがインストール済みであること
            * [ROSのインストール手順](./install.md)を参照してください
        * ROSパッケージがインストール済みであること
            * [ROSパッケージのインストール手順](./package-install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 18.04**）がインストール済みであること

    ## 事前準備 {: #preparation}

    ### ROSパッケージの読み込み {: #preparation-source-workspace}

    サンプルを実行する場合は下記コマンドを実行し、
    ROSとパッケージを読み込んでください。

    ```sh
    $ source /opt/ros/melodic/setup.bash
    $ source ~/catkin_ws/devel/setup.bash
    ```

    ### Raspberry PiとPC間のROSネットワークを接続する {: #preparation-ros-master-uri}

    環境変数`ROS_IP`、`ROS_MASTER_URI`を設定することで、
    PCからネットワークを介してRaspberry Pi側のトピックやサービスにアクセスできます。

    Raspberry PiのIPアドレスが`192.168.11.89`、
    PCのIPアドレスが`192.168.11.100`のとき、
    下記コマンドで環境変数を設定します。__この設定値は環境によって変わりますのでご注意ください__。

    ```sh
    # Raspberry Pi側
    $ export ROS_IP=192.168.11.89
    $ export ROS_MASTER_URI=http://192.168.11.89:11311/

    # PC側
    # ROS_MASTER_URIにはRaspberry PiのIPアドレスを入力する
    $ export ROS_IP=192.168.11.100
    $ export ROS_MASTER_URI=http://192.168.11.89:11311/
    ```

    ネットワーク接続を切る場合は、
    `ROS_MASTER_URI`をデフォルトの`http://localhost:11311/`に戻します。

    !!! Warning
        PCから操縦する場合はネットワークと環境変数を設定してください。

    ## キーボードで操縦する {: #teleop-keyboard}

    操作方法はターミナルに出力されます。
    #### Raspberry Pi {: #teleop-keyboard-pi}
    ```sh
    # センサやモータ用のノードの立ち上げ
    $ roslaunch raspicat_bringup raspicat_bringup.launch
    ```

    #### PC {: #teleop-keyboard-pc}
    ```sh
    # キーボード用のノードの立ち上げ
    $ roslaunch raspicat_bringup teleop.launch joy:=false
    ```

    ## ジョイスティックコントローラで操縦する {: #teleop-joy}

    操作方法は[こちら](https://github.com/rt-net/raspicat_ros/blob/main/raspicat_gamepad_controller/README.ja.md)のパッケージを参照してください。

    === "ジョイスティックコントローラをPCに接続した場合"
        #### Raspberry Pi
        ```sh
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch
        ```
        #### PC
        ```sh
        # ジョイスティックコントローラ用のノードの立ち上げ
        $ roslaunch raspicat_bringup teleop.launch joy:=true
        ```
    === "ジョイスティックコントローラをRaspberry Pi 4に接続した場合"
        #### Raspberry Pi
        ```sh
        # センサやモータ用およびジョイスティックコントローラ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch joy:=true
        ```

    ## LiDARでSLAM {: #lidar-slam}

    ジョイスティックコントローラおよびキーボードにてRaspberry Pi Catを操作出来る状態でSLAMを実行します。
    ここでは、**PC**上にて**SLAM**を実行する方法を紹介します。  
    [raspicat_slam](https://github.com/rt-net/raspicat_slam_navigation/tree/main/raspicat_slam)
    パッケージで実行が可能なSLAMは、[gmapping](http://wiki.ros.org/gmapping)、[cartographer](http://wiki.ros.org/cartographer)、[slam_toolbox](http://wiki.ros.org/slam_toolbox)の3種類です。
    **rosbag**を使用する場合はオフラインでSLAMを行うことができます。

    !!! Warning
        **Raspberry Pi**と**PC**間で通信を行い、**PC**上で**SLAM**を実行する場合は、時刻の同期が必要です。  
        これは、**tf**関連のエラーを防ぐためです。

    ### gmapping {: #lidar-slam-gmapping}

    === "Online SLAM"
        #### Raspberry Pi
        ```sh
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch
        ```
        #### PC
        ```sh
        # gmappingの実行
        $ roslaunch raspicat_slam raspicat_gmapping.launch joy:=true
        
        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    === "Offline SLAM"
        #### PC
        ```sh
        # rosbag playおよびgmappingの実行
        $ roslaunch raspicat_slam raspicat_gmapping.launch rosbag:=true rosbag_rate:=1 rosbag_topics:="/odom /scan /tf /tf_static" rosbag_filename:=rosbag-path

        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    ### cartographer {: #lidar-slam-cartographer}
    === "Online SLAM"
        #### Raspberry Pi
        ```sh
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch
        ```
        #### PC
        ```sh
        # cartographerの実行
        $ roslaunch raspicat_slam raspicat_cartographer.launch joy:=true
        
        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    === "Offline SLAM"
        #### PC
        ```sh
        # rosbag playおよびcartographerの実行
        $ roslaunch raspicat_slam raspicat_cartographer.launch rosbag:=true rosbag_rate:=1 rosbag_topics:="/odom /scan /tf /tf_static" rosbag_filename:=rosbag-path

        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    ### slam_toolbox {: #lidar-slam-toolbox}
    === "Online SLAM"
        #### Raspberry Pi
        ```sh
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch
        ```
        #### PC
        ```sh
        # slam_toolboxの実行
        $ roslaunch raspicat_slam raspicat_slam_toolbox.launch joy:=true
        
        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    === "Offline SLAM"
        #### PC
        ```sh
        # rosbag playおよびslam_toolboxの実行
        $ roslaunch raspicat_slam raspicat_slam_toolbox.launch rosbag:=true rosbag_rate:=1 rosbag_topics:="/odom /scan /tf /tf_static" rosbag_filename:=rosbag-path

        # 地図の保存
        $ roslaunch raspicat_slam map_save.launch map_file:=map-path
        ```

    ## LiDARでナビゲーション {: #lidar-navigation}

    SLAMによって作成した地図を使用し、Navigationを行います。  
    [raspicat_navigation](https://github.com/rt-net/raspicat_slam_navigation/tree/main/raspicat_navigation)
    パッケージで実行が可能なNavigationは、[move_base](http://wiki.ros.org/move_base)、[neonavigation](https://github.com/at-wat/neonavigation)の2種類です。

    !!! Warning
        **Raspberry Pi**と**PC**間で通信を行い、**PC**上で**Navigation**を実行する場合は、時刻の同期が必要です。  
        これは、**tf**関連のエラーを防ぐためです。

    === "move_base"
        #### Raspberry Pi
        ```sh
        $ roscore
        
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch lidar_ether:=false lidar_usb:=true joy:=false
        ```
        #### PC
        ```sh
        
        # move_baseの実行
        $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="move_base" mcl_map_file:=map-path navigation_map_file:=map-path
        ```

    === "neonavigation"
        #### Raspberry Pi
        ```sh
        $ roscore
        
        # センサやモータ用のノードの立ち上げ
        $ roslaunch raspicat_bringup raspicat_bringup.launch lidar_ether:=false lidar_usb:=true joy:=false
        ```
        #### PC
        ```sh
        # neonavigationの実行
        $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="neonav" mcl_map_file:=map-path navigation_map_file:=map-path
        ```

=== "ROS 2"
    このページでは
    実機を使用した、ジョイスティックコントローラで操縦する方法を説明します。
    ## 使用機材 {: #requirements}

    * セットアップ済みRaspberry Pi Cat本体
        * [製品マニュアル](https://rt-net.jp/products/raspberry-pi-cat/#downloads)を読んで組み立て済みの状態を前提としています
        * Raspberry PiにOS（**Ubuntu Server 22.04**）がインストール済みであること
            * Raspberry Pi OSでのROS 2の動作は確認していません。
        * **デバイスドライバ**がインストール済みであること
            * [デバイスドライバのインストール手順](../driver/install.md)を参照してください
        * ROS 2がインストール済みであること
            * [ROS 2のインストール手順](./install.md)を参照してください
        * ROS 2パッケージがインストール済みであること
            * [ROS 2パッケージのインストール手順](./package-install.md)を参照してください
    * USB Type-Bケーブル（Raspberry Pi Catに同梱されています）
    * ノートパソコン等のPC
        * OS（**Ubuntu Desktop 22.04**）がインストール済みであること

    ## 事前準備 {: #preparation}

    ### ROS 2パッケージの読み込み {: #preparation-source-workspace}

    サンプルを実行する場合は下記コマンドを実行し、
    ROS 2とパッケージを読み込んでください。

    ```sh
    $ source /opt/ros/humble/setup.bash
    $ source ~/catkin_ws/install/setup.bash
    ```

    ## ジョイスティックコントローラで操縦する {: #teleop-joy}

    #### Raspberry Pi
    ```sh
    # センサやモータ用のノードの立ち上げ
    $ ros2 launch raspicat_bringup raspicat.launch.py
    ```
    #### PC
    ```sh
    # ジョイスティックコントローラ用のノードの立ち上げ
    $ ros2 service call /motor_power std_srvs/SetBool '{data: true}'
    $ ros2 launch raspicat_bringup joy.launch.py
    ```

    ### 操作について
    使用するコントローラは[F710](https://www.logitech.com/assets/34885/4/f710620-002923003403gswamr.pdf)であり  
    F710のゲームインターフェイスモードがDirectInputである前提で説明をします。

    === "デジタル操作（ステータスインジケーターがオフの場合）"
        Dパッド（十字キー）とBボタンを同時に操作することで、Raspberry Pi Catを動かすことが出来ます。  
        また、この場合の操作は、並進速度が[0, 0.3]、回転速度が[0, ±0.4]のいずれかです。

        Xボタンを使用することで、より速く走行させることができます。
        
        | ボタン | 並進速度 [m/s] | 回転速度 [rad/s] | 
        | ------ | -------- | -------- | 
        | B      | 0.3      | 0.4      | 
        | X      | 0.5      | 1.0      | 
    === "アナログ操作（ステータスインジケーターがオフの場合）"
        アナログミニスティック（左側）とBボタンを同時に操作することで、Raspberry Pi Catを動かすことが出来ます。  
        また、この場合の操作は、並進速度が[0 ~ 0.3]、回転速度が[0 ~ ±0.4]の範囲です。

        Xボタンを使用することで、より速く走行させることができます。

        | ボタン | 並進速度 [m/s] | 回転速度 [rad/s] | 
        | ------ | -------- | -------- | 
        | B      | 0.3      | 0.4      | 
        | X      | 0.5      | 1.0      | 
