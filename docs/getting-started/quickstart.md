# クイックスタート

!!! info "対応する公式ドキュメント"
    [Isaac Sim Quickstart](https://docs.isaacsim.omniverse.nvidia.com/latest/introduction/quickstart_index.html)

このページでは、Isaac Sim を起動してから「物体を置いて物理シミュレーションを動かす」までの基本操作を説明します。

## 画面構成

起動直後の主なパネルは次の通りです。

| パネル | 役割 |
| --- | --- |
| **Viewport** | 3D シーンの表示・操作を行うメイン画面 |
| **Stage** | シーン内のプリム(オブジェクト)をツリー表示 |
| **Property** | 選択中プリムの属性(位置、物理設定など)を編集 |
| **Content Browser** | ローカル/リモートのアセットを参照・ドラッグ&ドロップ |
| **Console** | ログとエラーの確認 |

## ビューポートの基本操作

| 操作 | マウス/キー |
| --- | --- |
| 回転(オービット) | `Alt` + 左ドラッグ |
| パン | マウス中ボタンドラッグ |
| ズーム | ホイール / `Alt` + 右ドラッグ |
| 選択 | 左クリック |
| 選択対象へフォーカス | `F` キー |

## 最初のシミュレーション

### 1. 地面を追加する

メニューから **Create > Physics > Ground Plane** を選ぶと、物理コライダー付きの地面が追加されます。

### 2. 剛体オブジェクトを追加する

**Create > Mesh > Cube** で立方体を追加し、位置を地面より上(例: Z = 1.0 m)に移動します。

このままではただの見た目だけのメッシュなので、物理属性を付与します:

1. Stage パネルで Cube を選択
2. Property パネルで **Add > Physics > Rigid Body with Colliders Preset** を適用

これで「質量を持ち、衝突判定のある剛体」になります。

### 3. 再生する

ツールバーの **Play(▶)** を押すとシミュレーションが始まり、立方体が重力で落下して地面の上に静止します。**Stop(■)** で初期状態に戻ります。

!!! tip "Play と Stop の関係"
    Play 中に加えた変更(物体の移動など)は、Stop すると基本的に初期状態へ巻き戻ります。シーンの編集は Stop 状態で行うのが基本です。

## サンプルロボットを動かしてみる

Isaac Sim には多数のロボットアセット(マニピュレータ、AMR、ヒューマノイドなど)が付属しています。

1. メニューの **Window > Examples** 系メニューからロボットのサンプルを開く、または Content Browser のアセットライブラリからロボット USD をステージへドラッグ
2. Play を押すと、サンプルによってはコントローラ付きでロボットが動作します

## スクリプトエディタを使う

**Window > Script Editor** を開くと、実行中の Isaac Sim 内で Python コードを直接実行できます。

```python
from pxr import UsdGeom
import omni.usd

stage = omni.usd.get_context().get_stage()
cube = UsdGeom.Cube.Define(stage, "/World/MyCube")
cube.AddTranslateOp().Set((0.0, 0.0, 2.0))
```

このように、GUI 操作と Python スクリプトを組み合わせて作業できるのが Isaac Sim の大きな特徴です。

## 次のステップ

- [USD とステージ](../concepts/usd-stage.md) — シーンの内部構造を理解する
- [ロボットのインポート](../robots/import.md) — 自分のロボットモデルを取り込む
