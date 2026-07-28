# 物理ベースセンサー

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Sensors](https://docs.isaacsim.omniverse.nvidia.com/latest/sensors/index.html)

レンダリングを使わず、物理エンジン(PhysX)の状態から値を生成するセンサー群です。レンダリング負荷に依存しないため高レートで動作させやすいのが特徴です。

## IMU センサー

リンクに取り付けて、加速度(線形加速度)と角速度、姿勢を出力します。

- 取り付け先のリンク(Rigid Body)の運動から値を算出
- ノイズや出力レートは、必要に応じて後段で付加・調整
- ROS 2 ブリッジで `sensor_msgs/Imu` としてパブリッシュ可能

## 接触センサー (Contact Sensor)

リンクに働く接触力を検出します。

- 対象リンクへの接触の有無・力の大きさ・接触位置を取得
- 足裏の接地判定(脚ロボット)、把持成功判定(グリッパー)などに利用
- 検出半径・最小しきい値の設定で感度を調整

```python
# 接触センサーを扱う概念例(API 名はバージョン確認を)
from isaacsim.sensors.physics import ContactSensor

sensor = ContactSensor(
    prim_path="/World/Robot/gripper/contact_sensor",
    min_threshold=0.0,
    max_threshold=1e6,
    radius=0.05,
)
value = sensor.get_current_frame()  # 接触力などを含む辞書
```

## 関節状態(エンコーダ相当)

専用のセンサープリムを置かなくても、Articulation API から各関節の位置・速度・トルク(努力値)を毎ステップ取得できます。実機のエンコーダ・トルクセンサー相当の情報源です。

```python
positions = robot.get_joint_positions()
velocities = robot.get_joint_velocities()
efforts = robot.get_measured_joint_efforts()
```

## 力/トルクセンサー

ジョイントに作用する力・トルクを計測できます。手首の 6 軸力覚センサーの模擬などに利用します。

## PhysX ベースのレンジセンサー

レイキャストによる簡易的な距離センサー(旧来の LiDAR / 近接センサー)も利用できます。RTX LiDAR より物理表現は簡素ですが、軽量で高速です。

- 単純な障害物検知や、レンダリング無しのヘッドレス学習環境に向く
- 材質反射・強度などのリアリズムが必要なら [RTX LiDAR](camera-rtx.md) を使用

## センサー選択の指針

| 目的 | 推奨 |
| --- | --- |
| 見た目込みのカメラ画像・学習データ | RTX カメラ + Replicator |
| 実センサー相当の点群(材質・パターン込み) | RTX LiDAR |
| 高速・軽量な距離計測(学習ループ内など) | PhysX レンジセンサー / レイキャスト |
| 姿勢・加速度 | IMU センサー |
| 接触・把持判定 | 接触センサー / 力トルクセンサー |

## 次のステップ

- [ROS 2 連携](../development/ros2.md) — センサーデータを ROS 2 へ流す
