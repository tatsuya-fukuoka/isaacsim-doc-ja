# 合成データ生成 (Replicator)

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Synthetic Data Generation](https://docs.isaacsim.omniverse.nvidia.com/latest/replicator_tutorials/index.html)

**Omniverse Replicator** は、アノテーション付きの学習データを大量生成するためのフレームワークです。物体検出・セグメンテーション・姿勢推定などのモデル学習に必要なデータを、実データ収集より低コストで作成できます。

## 基本コンセプト

Replicator のワークフローは次の 3 要素で構成されます。

1. **ランダマイザー (Randomizer)** — 物体の位置・回転・色・照明・テクスチャなどをフレームごとにランダム化(ドメインランダマイゼーション)
2. **アノテーター (Annotator)** — RGB、深度、セグメンテーション、バウンディングボックスなどの正解データを取得
3. **ライター (Writer)** — 取得したデータをディスクへ書き出し(標準の BasicWriter、KITTI / COCO 形式など)

## 最小のスクリプト例

```python
import omni.replicator.core as rep

# カメラとレンダープロダクト
camera = rep.create.camera(position=(3, 0, 1), look_at=(0, 0, 0))
render_product = rep.create.render_product(camera, (1024, 768))

# 対象オブジェクト(セマンティクスラベル付き)
cube = rep.create.cube(semantics=[("class", "cube")], position=(0, 0, 0.5))

# ランダマイズ定義:毎フレーム、位置と色を変える
with rep.trigger.on_frame(num_frames=100):
    with cube:
        rep.modify.pose(
            position=rep.distribution.uniform((-1, -1, 0.2), (1, 1, 1.0)),
            rotation=rep.distribution.uniform((0, 0, 0), (0, 0, 360)),
        )

# ライター:RGB と 2D バウンディングボックスを出力
writer = rep.WriterRegistry.get("BasicWriter")
writer.initialize(
    output_dir="_output",
    rgb=True,
    bounding_box_2d_tight=True,
    semantic_segmentation=True,
)
writer.attach([render_product])

rep.orchestrator.run()
```

## セマンティクスラベル

アノテーションの対象にするには、プリムに**セマンティクスラベル**(例: `class: pallet`)を付与します。GUI のセマンティクススキーマエディタ、または API で設定できます。ラベルの無いオブジェクトはセグメンテーションや BBox の対象になりません。

## ランダム化の代表例

| 対象 | 効果 |
| --- | --- |
| 物体の位置・姿勢・スケール | 配置バリエーションの網羅 |
| 照明(強度・色・位置) | 照明変化へのロバスト性 |
| テクスチャ・マテリアル | 背景・外観の過学習防止 |
| カメラ位置・パラメータ | 視点変化への対応 |
| ディストラクター(無関係物体)の散布 | 偽陽性への耐性向上 |

## 実行形態

- **GUI 上で実行**: 挙動を目視確認しながら開発
- **ヘッドレス実行**: スタンドアロンスクリプトとしてサーバーで大量生成
- 高品質な出力にはパストレーシングモード + サブフレーム蓄積を使用

## Sim-to-Real のヒント

!!! tip
    - 「リアルさ」より「**多様性**」が効く場面が多い(ドメインランダマイゼーションの基本思想)
    - 実環境の照明条件・カメラ特性(ノイズ、露出)を範囲に含める
    - 少量の実データと混ぜて学習すると精度が上がることが多い

## 次のステップ

- [Isaac Lab と強化学習](isaac-lab.md)
- [カメラと RTX センサー](../sensors/camera-rtx.md)
