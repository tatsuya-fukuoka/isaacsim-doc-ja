# カメラと RTX センサー

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Sensors](https://docs.isaacsim.omniverse.nvidia.com/latest/sensors/index.html)

Isaac Sim のセンサーは大きく **RTX(レンダリング)ベース**と**物理ベース**に分かれます。このページでは前者を扱います。

## カメラ

### カメラの作成

- メニュー **Create > Camera** でカメラプリムを作成
- ロボットのリンク配下に置けば、ロボットに追従するオンボードカメラになる
- Viewport のカメラ切り替えで、そのカメラ視点を確認可能

### 主なパラメータ

| パラメータ | 説明 |
| --- | --- |
| Focal Length | 焦点距離。視野角(FOV)を決める |
| Horizontal/Vertical Aperture | センサーサイズ相当。FOV とアスペクト比に影響 |
| Clipping Range | 描画する距離範囲(近接/遠方クリップ) |
| 解像度 | レンダープロダクト(出力)側で指定 |

実カメラを模擬する場合は、実機の内部パラメータ(焦点距離・センサーサイズ)から換算して設定します。歪みモデルに対応したカメラ設定も用意されています。

### 出力できるデータ

カメラは RGB だけでなく、Synthetic Data / Replicator の仕組みを通じて多様な出力(AOV)を取得できます。

- RGB 画像
- 深度(distance to camera / distance to image plane)
- セマンティック/インスタンスセグメンテーション
- 2D/3D バウンディングボックス
- 法線、モーションベクトル、ポイントクラウド

```python
# Replicator API でカメラ出力を取得する概念例
import omni.replicator.core as rep

camera = rep.create.camera(position=(2, 0, 1), look_at=(0, 0, 0))
render_product = rep.create.render_product(camera, (1280, 720))

rgb = rep.AnnotatorRegistry.get_annotator("rgb")
rgb.attach(render_product)
```

## RTX LiDAR

**RTX LiDAR** は、レイトレーシングを使って LiDAR の光線を物理的にシミュレートするセンサーです。

- 実在センサーの**コンフィグファイル(JSON)**でスキャンパターン・チャンネル数・回転数などを定義
- 主要メーカーの LiDAR に対応するプリセット設定が付属
- 出力はポイントクラウド、デプス、強度など
- ROS 2 ブリッジ経由で `PointCloud2` / `LaserScan` としてパブリッシュ可能

!!! tip "RTX LiDAR と PhysX LiDAR"
    旧来の PhysX ベースのレンジセンサーも存在しますが、材質反射・詳細なスキャンパターンを再現できる RTX LiDAR の使用が現在の主流です。

## その他の RTX センサー

- **RTX Radar**: レーダーのシミュレーション。自動運転・AMR 用途
- 超音波センサー等も、レイベースの仕組みで模擬されます

## パフォーマンスの考慮

- カメラの本数と解像度はフレームレートに直結します。必要最小限に
- 高品質レンダリング(パストレーシング)は合成データ生成向け、リアルタイム検証にはリアルタイムレンダリングモードを使い分けます
- ヘッドレス+複数 GPU 構成で大量データ生成をスケールできます

## 次のステップ

- [物理ベースセンサー](physics-sensors.md) — IMU・接触センサーなど
- [合成データ生成 (Replicator)](../development/replicator.md) — アノテーション付きデータの生成
