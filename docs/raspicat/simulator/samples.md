---
title: シミュレータでサンプルを動かす
robot: Raspberry Pi Cat
---

# シミュレータでサンプルを動かす

このページでは、 
[raspicat_sim](https://github.com/rt-net/raspicat_sim){target=_blank rel=noopener}
パッケージと
[raspicat_slam_navigation](https://github.com/rt-net/raspicat_slam_navigation){target=_blank rel=noopener}
パッケージを使ってシミュレータ上でSLAM/Navigationを実施する方法について説明します。

## 使用機材 {: #requirements}

* ノートパソコン等のPC
    * OS（**Ubuntu Desktop 18.04**）、ROS、シミュレータがインストール済みであることを前提としています
        * インストール方法については[ROSのインストール](../ros/install.md)と[シミュレータのインストール](./install.md)を参照してください。

## シミュレータ環境を立ち上げる {: #launch}

!!! Tip
    初回起動時において、Gazeboはモデルをインターネットからダウンロードします。  
    そのため、シミュレータ環境の立ち上げに時間がかかる場合があります。

#### Gazeboのみ立ち上げ
```sh
#シミュレータの起動
$ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false
```
#### GazeboとRVizの立ち上げ
```sh
#シミュレータおよびRVizの起動
$ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch
```

## 操縦する {: #teleop}

![raspicat_sim](https://rt-net.github.io/images/raspberry-pi-cat/raspicat_gazebo_with_iscas_museum.gif)

### キーボードで操縦する {: #teleop-keyboard}

```sh
# シミュレータの起動
$ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

# キーボード操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
$ roslaunch raspicat_bringup teleop.launch joy:=false
```

### ジョイスティックコントローラで操縦する {: #teleop-joy}

```sh
# シミュレータの起動
$ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch

# ジョイスティックコントローラ操縦用ノードの立ち上げ（別のターミナルでコマンドを実行）
$ roslaunch raspicat_bringup teleop.launch joy:=true
```

## シミュレータ環境でSLAM {: #slam}

#### キーボードで操縦 {: #slam-teleop-keyboard}

=== "gmapping"
    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

    # gmappingの実行
    $ roslaunch raspicat_slam raspicat_gmapping.launch

    # キーボード操縦用ノードの立ち上げ
    $ roslaunch raspicat_bringup teleop.launch joy:=false

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
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

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
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

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
    $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
    ```

#### ジョイスティックコントローラで操縦 {: #slam-teleop-joy}
=== "gmapping"
    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

    # gmappingの実行
    $ roslaunch raspicat_slam raspicat_gmapping.launch

    # ジョイスティック操縦用ノードの立ち上げ
    $ roslaunch raspicat_bringup teleop.launch joy:=true

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
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

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
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

    # 地図の保存（map_fileのパスは任意のディレクトリへのパスを指定できます）
    $ roslaunch raspicat_slam map_save.launch map_file:=$(rospack find raspicat_slam)/config/maps
    ```
    <video width="100%" controls>
        <source src="https://user-images.githubusercontent.com/40545422/166214198-38cf95ee-57ce-45bf-82d4-447f93419560.mp4" type="video/mp4">
    </video>

## シミュレータ環境でナビゲーション {: #navigation}

=== "move_base"
    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

    # move_baseの実行
    # 任意の地図データへのパスを指定できます
    $ MAP_FILE=$(rospack find raspicat_slam)/config/maps/iscas_museum_map.yaml
    $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="neonav" mcl_map_file:=$MAP_FILE navigation_map_file:=$MAP_FILE
    ```

    <video width="100%" controls>
        <source src="https://user-images.githubusercontent.com/40545422/166214286-c78d74d1-cab1-489d-85fc-68416d48c655.mp4" type="video/mp4">
    </video>

=== "neonavigation"
    ```sh
    # シミュレータの起動
    $ roslaunch raspicat_gazebo raspicat_with_iscas_museum.launch rviz:=false

    # neonavigationの実行
    # 任意の地図データへのパスを指定できます
    $ MAP_FILE=$(rospack find raspicat_slam)/config/maps/iscas_museum_map.yaml
    $ roslaunch raspicat_navigation raspicat_navigation.launch navigation:="neonav" mcl_map_file:=$MAP_FILE navigation_map_file:=$MAP_FILE
    ```

    <video width="100%" controls>
        <source src="https://user-images.githubusercontent.com/40545422/166214304-23606730-3d8e-4ed4-9f9d-7834a3788aba.mp4" type="video/mp4">
    </video>
