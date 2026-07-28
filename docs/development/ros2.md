# ROS 2 連携

!!! info "対応する公式ドキュメント"
    [Isaac Sim — ROS 2](https://docs.isaacsim.omniverse.nvidia.com/latest/ros2_tutorials/index.html)

Isaac Sim は **ROS 2 Bridge** 拡張機能を通じて ROS 2 と双方向に通信できます。シミュレーション内のセンサーを ROS 2 トピックとして配信し、ROS 2 側からロボットを制御する SIL(Software-in-the-Loop)構成が組めます。

## 対応と前提

- 対応 ROS 2 ディストリビューションはバージョンごとに異なります(Humble / Jazzy 世代が目安)。公式ページで確認してください
- ブリッジには内部用の ROS 2 ライブラリが同梱されており、**ホストに ROS 2 が無くても**トピック配信自体は可能です
- ホスト側 ROS 2 と通信する場合は、同一の `ROS_DOMAIN_ID` と RMW 実装の整合に注意

## 有効化

1. **Window > Extensions** で ROS 2 Bridge 拡張(`isaacsim.ros2.bridge`)を有効化
2. 必要に応じて起動設定に組み込み、常時有効化

## OmniGraph(Action Graph)による構成

ROS 2 の入出力は **OmniGraph** のノードとして構成するのが基本です。代表的なノード:

| ノード | 役割 |
| --- | --- |
| ROS2 Context | ドメイン ID などの通信設定 |
| ROS2 Publish/Subscribe Twist | 速度指令の送受信(`geometry_msgs/Twist`) |
| ROS2 Publish Joint State / Subscribe Joint State | 関節状態の配信・受信 |
| ROS2 Camera Helper | カメラ画像(`sensor_msgs/Image`、`camera_info`)の配信 |
| ROS2 RTX Lidar Helper | 点群(`PointCloud2`)・`LaserScan` の配信 |
| ROS2 Publish TF | TF ツリーの配信 |
| ROS2 Publish Clock | シミュレーション時刻(`/clock`)の配信 |

### シミュレーション時刻の扱い

ROS 2 ノード側で `use_sim_time` を有効にし、Isaac Sim から `/clock` を配信することで、シミュレーション時刻に同期した動作になります。TF のタイムスタンプずれによる Nav2 / RViz の不具合はここが原因のことが多いです。

## 典型構成の例

### 差動二輪ロボットのテレオペ

1. ロボットに Articulation(左右ホイールの速度ドライブ)を用意
2. Action Graph で `ROS2 Subscribe Twist` → 差動駆動ノード → Articulation Controller を接続
3. ROS 2 側から `teleop_twist_keyboard` で `/cmd_vel` を送信

### Nav2 との接続

- Isaac Sim から: LiDAR 点群 / オドメトリ / TF / `/clock` を配信
- Nav2 から: `/cmd_vel` を受信してロボットを駆動
- 倉庫環境アセット + AMR で、実機なしのナビゲーション検証が可能

### MoveIt 2 との接続

- `joint_states` の配信と `joint_trajectory` 系の受信を構成し、アームのモーションプランニングを検証できます

## ヘッドレス / コンテナでの利用

コンテナ実行時は `--network=host` を使うと ROS 2(DDS)の通信がシンプルになります。異なるホスト間では DDS のディスカバリ設定(ピア指定)が必要になる場合があります。

## 次のステップ

- [合成データ生成 (Replicator)](replicator.md)
- [Isaac Lab と強化学習](isaac-lab.md)
