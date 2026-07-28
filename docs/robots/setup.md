# ロボットのセットアップ

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Robot Setup](https://docs.isaacsim.omniverse.nvidia.com/latest/robot_setup/index.html)

インポートしたロボットを「実際に使える状態」にするための調整項目をまとめます。

## Articulation の構成確認

Stage パネルでロボットのプリム構造を確認します。

- **Articulation Root** がロボットのルート(またはベースリンク)に設定されているか
- 各リンクに Rigid Body とコライダーが付いているか
- ジョイントが正しいリンク間に張られているか

## ジョイントドライブのチューニング

関節ごとに Drive の **Stiffness / Damping / Max Force** を調整します。

| 制御方式 | Stiffness | Damping |
| --- | --- | --- |
| 位置制御(アーム関節など) | 大きい値 | 適度な値 |
| 速度制御(ホイールなど) | 0 | 大きめの値 |

チューニングの目安:

- 目標角度に対して振動する → Damping を増やす、または Stiffness を下げる
- 到達が遅い・垂れ下がる → Stiffness を上げる、Max Force を確認
- 関節が力に負けて動かされる → Max Force と質量バランスを確認

## Gain Tuner などの支援ツール

Isaac Sim にはゲイン調整やロボット構造の確認を支援するツール(Gain Tuner、Physics Inspector 系のツール)が拡張機能として用意されています。関節ごとにステップ応答を確認しながらゲインを詰める際に便利です。

## コントローラと動作生成

ロボットを動かす方法は複数あります。

| 方法 | 用途 |
| --- | --- |
| **Articulation API(Python)** | 関節目標値を直接指定する基本手段 |
| **OmniGraph / Action Graph** | ノードベースでキーボード操作・差動駆動などを構成 |
| **モーション生成ライブラリ** | アームの逆運動学(IK)・軌道生成(RMPflow、モーションポリシー系) |
| **ROS 2 経由** | ROS 2 側のコントローラ(Nav2、MoveIt 2 など)から制御 |

### Python からの制御例

```python
# Articulation を Python から制御する概念例
# (モジュール名はバージョンで異なることがあります)
from isaacsim.core.api.robots import Robot
import numpy as np

robot = Robot(prim_path="/World/Robot", name="my_robot")
robot.initialize()

# 関節位置目標を設定
robot.set_joint_positions(np.array([0.0, -1.0, 0.0, -2.2, 0.0, 2.0, 0.8]))
```

## 移動ロボット固有の設定

- ホイールのコライダーは、シンプルな円筒/球に置き換えると安定しやすい
- 差動二輪は「左右輪の速度制御+キャスターの摩擦低減」が基本構成
- 地面との摩擦係数(Physics Material)が走行挙動を大きく左右する

## 検証のすすめ

1. **静置テスト**: Play 直後にロボットが静止し続けるか(震える・沈む・跳ねるは要調整)
2. **単関節テスト**: 関節を 1 つずつ動かし、方向・範囲・速度を確認
3. **負荷テスト**: 把持対象や積載物を持たせた状態での挙動確認

## 次のステップ

- [カメラと RTX センサー](../sensors/camera-rtx.md) — ロボットにセンサーを載せる
- [ROS 2 連携](../development/ros2.md) — ROS 2 から制御する
