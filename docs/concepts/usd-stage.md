# USD とステージ

!!! info "対応する公式ドキュメント"
    [Isaac Sim — OpenUSD 関連ページ](https://docs.isaacsim.omniverse.nvidia.com/latest/omniverse_usd/index.html) / [OpenUSD 公式](https://openusd.org/)

Isaac Sim のシーンはすべて **OpenUSD (Universal Scene Description)** で記述されます。USD の基本概念を押さえておくと、Isaac Sim の挙動の多くが理解しやすくなります。

## 基本用語

| 用語 | 意味 |
| --- | --- |
| **Stage(ステージ)** | 開いているシーン全体。複数の USD レイヤーを合成した結果 |
| **Prim(プリム)** | ステージ上のノード。Xform、Mesh、Camera、ロボットなどすべてがプリム |
| **Attribute(属性)** | プリムが持つ値(位置、色、質量など) |
| **Path(パス)** | プリムの位置を示す一意な文字列(例: `/World/Robot/base_link`) |
| **Layer(レイヤー)** | USD ファイル 1 つに相当する編集単位。複数レイヤーを重ねて 1 つのステージを構成 |

## コンポジション(合成)の仕組み

USD の強力さは、複数のソースを**非破壊的に合成**できる点にあります。代表的な仕組み:

- **Reference(参照)**: 別の USD ファイルをステージ内に取り込む。ロボットや棚などの部品アセットを再利用する基本手段
- **Payload**: 参照の遅延読み込み版。大規模シーンでメモリと読み込み時間を節約
- **Variant(バリアント)**: 1 つのアセット内に複数の構成(色違い、グレード違いなど)を切り替え可能な形で持たせる
- **Sublayer**: レイヤーを重ね、上位レイヤーの編集(オーバーライド)で下位レイヤーの値を上書き

!!! tip "編集がどのレイヤーに入るか"
    Isaac Sim(Omniverse)には「**Edit Target**(編集対象レイヤー)」の概念があります。GUI での変更が意図しないレイヤーに書き込まれてシーンが壊れたように見えることがあるため、Layer パネルで編集対象を意識すると安全です。

## 単位と座標系

- 距離の標準単位は**メートル**(`metersPerUnit = 1.0` が推奨)
- Isaac Sim の標準では **Z 軸が上方向**(`upAxis = Z`)
- 外部からインポートしたアセットは単位・アップ軸が異なることがあり、スケールや姿勢の不整合の主因になります

## Isaac Sim における典型的なステージ構成

```
/World                  ← ルートの Xform
 ├── /World/ground      ← 地面(コライダー付き)
 ├── /World/Robot       ← ロボット(参照で取り込み、Articulation ルート)
 │    ├── base_link
 │    ├── link_1 ...
 │    └── joints/...
 ├── /World/Camera      ← カメラプリム
 └── /physicsScene      ← 物理シーン設定(重力など)
```

## Python から USD を操作する

Isaac Sim 内の Python(Script Editor や標準スクリプト)では、`pxr` モジュール経由で USD API をそのまま使えます。

```python
import omni.usd
from pxr import Usd, UsdGeom, Gf

stage = omni.usd.get_context().get_stage()

# プリムの取得
prim = stage.GetPrimAtPath("/World/Robot")

# 属性の読み書き
xform = UsdGeom.Xformable(prim)
for op in xform.GetOrderedXformOps():
    print(op.GetOpName(), op.Get())

# 新しいプリムの作成
UsdGeom.Xform.Define(stage, "/World/NewGroup")
```

## USD ファイルの形式

| 拡張子 | 内容 |
| --- | --- |
| `.usd` | バイナリまたはテキスト(自動判別) |
| `.usda` | テキスト形式。差分管理・目視確認に向く |
| `.usdc` | バイナリ形式。読み込みが高速 |
| `.usdz` | 配布用のアーカイブ形式(zip 包装) |

## 次のステップ

- [物理シミュレーション](physics.md) — 剛体・関節・接触の扱い
- [アセットとコンテンツ](assets.md) — 付属アセットの場所と使い方
