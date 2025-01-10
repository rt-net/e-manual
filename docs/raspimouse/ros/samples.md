---
title: ROS / ROS 2サンプル集の実行
robot: Raspberry Pi Mouse
---

# ROS / ROS 2サンプル集の実行


このページでは
[ROSサンプル集(rt-net/raspimouse_ros_examples)](https://github.com/rt-net/raspimouse_ros_examples){target=_blank rel=noopener} /
[ROS 2サンプル集(rt-net/raspimouse_ros2_examples)](https://github.com/rt-net/raspimouse_ros2_examples){target=_blank rel=noopener}
のサンプル実行方法を説明します。

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
    * ROSまたはROS 2用のROSパッケージがインストール済みであることを前提としています
        * [ROS/ROS 2パッケージのインストール手順](./package-install.md)を参照してください
* Raspberry Pi Mouse用電源
    * バッテリでも電源変換ケーブルつきACアダプタでも可
* ノートパソコン等のPC
    * OS（**Ubuntu Desktop**）がインストール済みであることを前提としています
        * `ROS 2 Jazzy`を使用する場合: **Ubuntu Desktop 24.04**
        * `ROS 2 Humble`を使用する場合: **Ubuntu Desktop 22.04**
        * `ROS Noetic`を使用する場合: **Ubuntu Desktop 20.04**

## ROSパッケージ情報の読み込み {: #load-workspace}

=== "ROS"

    [ROSサンプル集(rt-net/raspimouse_ros_examples)](https://github.com/rt-net/raspimouse_ros_examples){target=_blank rel=noopener}
    のサンプルを実行する場合は下記コマンドを実行し、
    ROSとパッケージを読み込んでください。

    ```sh
    $ source /opt/ros/noetic/setup.bash
    $ source ~/catkin_ws/devel/setup.bash
    ```


=== "ROS 2 Humble"
    [ROS 2サンプル集(rt-net/raspimouse_ros2_examples)](https://github.com/rt-net/raspimouse_ros2_examples){target=_blank rel=noopener}
    のサンプルを実行する場合は下記コマンドを実行し、ROS 2の設定を読み込んでください。

    ```sh
    $ source /opt/ros/humble/setup.bash
    ```

=== "ROS 2 Jazzy"
    [ROS 2サンプル集(rt-net/raspimouse_ros2_examples)](https://github.com/rt-net/raspimouse_ros2_examples){target=_blank rel=noopener}
    のサンプルを実行する場合は下記コマンドを実行し、ROS 2の設定を読み込んでください。

    ```sh
    $ source /opt/ros/jazzy/setup.bash
    ```
    
## Raspberry PiとPC間のROSネットワークを接続する {: #network}

=== "ROS"
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

=== "ROS 2"
    PCとRaspberry Piに同じ環境変数`ROS_DOMAIN_ID`を設定することで、
    PCからネットワークを介してRaspberry Pi側のトピックやサービスにアクセスできます。

    下記コマンドでは、PCとRaspberry Piにそれぞれ`ROS_DOMAIN_ID=22`を設定します。

    ```sh
    # Raspberry Pi側
    $ export ROS_DOMAIN_ID=22

    # PC側
    $ export ROS_DOMAIN_ID=22
    ```

    ネットワーク接続を切る場合は、
    `ROS_DOMAIN_ID`を削除します。

    ```sh
    $ unset ROS_DOMAIN_ID
    ```

## キーボードで操縦する {: #teleop}

=== "ROS"
    実行手順は[ROS サンプル集のkeyboard_control](https://github.com/rt-net/raspimouse_ros_examples#keyboard_control){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

## ジョイスティックコントローラで操縦する {: #joystick}

=== "ROS"
    実行手順は[ROS サンプル集のjoystick_control](https://github.com/rt-net/raspimouse_ros_examples#joystick_control){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

=== "ROS 2"
    実行手順は[ROS 2サンプル集のjoystick_control](https://github.com/rt-net/raspimouse_ros2_examples#joystick_control){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/GswxdB8Ia0Y" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## カメラで物体追跡する {: #object-tracking}

=== "ROS"
    実行手順は[ROS サンプル集のobject_tracking](https://github.com/rt-net/raspimouse_ros_examples#object_tracking){target=_blank rel=noopener}
    を参照してください。
    物体検出画像をPCで表示する場合はネットワークと環境変数を設定してください。

=== "ROS 2"
    実行手順は[ROS 2サンプル集のobject_tracking](https://github.com/rt-net/raspimouse_ros2_examples#object_tracking){target=_blank rel=noopener}
    を参照してください。
    物体検出画像をPCで表示する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/U6_BuvrjyFc" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## ライントレース {: #line-follower}

=== "ROS"
    実行手順は[ROS サンプル集のline_follower](https://github.com/rt-net/raspimouse_ros_examples#line_follower){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

=== "ROS 2"
    実行手順は[ROS 2サンプル集のline_follower](https://github.com/rt-net/raspimouse_ros2_examples#line_follower){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/oPm0sW2V_tY" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## カメラによるライントレース {: #camera-line-follower}

=== "ROS 2"
    実行手順は[ROS 2サンプル集のcamera_line_follower](https://github.com/rt-net/raspimouse_ros2_examples#camera_line_follower){target=_blank rel=noopener}
    を参照してください。
    PCから操縦する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/Rxa1UBMUgq4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## LiDARでSLAM {: #slam}

=== "ROS"
    実行手順は[SLAMパッケージ](https://github.com/rt-net/raspimouse_slam_navigation_ros#raspimouse_slam){target=_blank rel=noopener}
    を参照してください。
    SLAMパッケージをPCで起動する場合はネットワークと環境変数を設定してください。

=== "ROS 2"
    実行手順は[ROS 2 SLAMパッケージ](https://github.com/rt-net/raspimouse_slam_navigation_ros2#raspimouse_slam){target=_blank rel=noopener}
    を参照してください。
    SLAMパッケージをPCで起動する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/gWozU47UqVE" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

<iframe width="560" height="315" src="https://www.youtube.com/embed/hV68UqAntfo" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## LiDARでナビゲーション {: #navigation}

=== "ROS"
    実行手順は[ナビゲーションパッケージ](https://github.com/rt-net/raspimouse_slam_navigation_ros#raspimouse_navigation){target=_blank rel=noopener}
    を参照してください。
    ナビゲーションパッケージをPCで起動する場合はネットワークと環境変数を設定してください。

=== "ROS 2"
    実行手順は[ROS 2 ナビゲーションパッケージ](https://github.com/rt-net/raspimouse_slam_navigation_ros2#raspimouse_navigation){target=_blank rel=noopener}
    を参照してください。
    ナビゲーションパッケージをPCで起動する場合はネットワークと環境変数を設定してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/xSMtBZYmhFw" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>

## IMUで角度制御 {: #direction-control}

=== "ROS"
    実行手順は[ROS サンプル集のdirection_control](https://github.com/rt-net/raspimouse_ros_examples#direction_control){target=_blank rel=noopener}
    を参照してください。

=== "ROS 2"
    実行手順は[ROS 2サンプル集のdirection_controller](https://github.com/rt-net/raspimouse_ros2_examples#direction_controller){target=_blank rel=noopener}
    を参照してください。

<iframe width="560" height="315" src="https://www.youtube.com/embed/LDpC2wqIoU4" title="YouTube video player" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
