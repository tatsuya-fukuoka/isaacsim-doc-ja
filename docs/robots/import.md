# ロボットのインポート (URDF / MJCF)

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Importing Robots](https://docs.isaacsim.omniverse.nvidia.com/latest/robot_setup/index.html)

手持ちのロボットモデルを Isaac Sim で使うには、URDF や MJCF などの形式から USD へ変換します。

## URDF インポーター

ROS で広く使われる **URDF** を USD の Articulation へ変換します。

### 手順(GUI)

1. メニューから URDF インポーターを開く(**File > Import** で `.urdf` を選択、またはインポーター拡張の UI)
2. 対象の `.urdf` ファイルを選択
3. インポートオプションを設定して **Import**

### 主なインポートオプション

| オプション | 説明 |
| --- | --- |
| **Fix Base(ベース固定)** | ベースリンクを空間に固定する。アームは ON、移動ロボットは OFF が基本 |
| **Joint Drive Type** | 関節ドライブを位置制御/速度制御のどちらで初期化するか |
| **Stiffness / Damping** | ドライブの初期ゲイン。後から Property パネルで調整可能 |
| **Collision の近似方法** | 凸包/凸分解など。把持をするなら凸分解を検討 |
| **Self Collision** | リンク同士の自己衝突判定を有効にするか |

!!! warning "URDF に無い情報は補完が必要"
    URDF には「関節ドライブのゲイン」「摩擦の詳細」など、シミュレーションに必要な情報の一部が含まれていません。インポート後に Isaac Sim 側でのチューニングが前提と考えてください。

### インポート後のチェックリスト

- [ ] スケールが正しいか(URDF はメートル基準。異常に大きい/小さい場合は単位を確認)
- [ ] Play したときにロボットが弾け飛ばないか(ゲイン・質量・自己衝突を確認)
- [ ] 各関節が意図した方向・範囲で動くか
- [ ] コライダー形状が見た目と大きくずれていないか(物理デバッグ表示で確認)

## MJCF インポーター

MuJoCo の **MJCF** 形式にもインポーターが用意されています。MuJoCo 用に整備されたロボット・ハンドのモデル資産を活用できます。URDF と同様に、変換後は Articulation・ジョイントドライブの確認を行います。

## Onshape / CAD からの取り込み

CAD 由来のモデルは、次のいずれかの経路で取り込むのが一般的です。

1. CAD → URDF 化(既存のエクスポートツール)→ URDF インポーター
2. CAD → USD 変換(CAD コンバーター)→ 手動でジョイント・物理をリギング

## スクリプトからのインポート

インポーターは Python API からも実行でき、複数ロボットの一括変換や CI での自動変換に利用できます。

```python
# URDF インポートをスクリプトから行う概念例
# (API 名はバージョンにより異なるため、公式 API リファレンスを確認してください)
from isaacsim.asset.importer.urdf import _urdf

urdf_interface = _urdf.acquire_urdf_interface()
import_config = _urdf.ImportConfig()
import_config.fix_base = False
import_config.convex_decomp = True

result, prim_path = urdf_interface.parse_and_import_urdf(
    "/path/to/robot.urdf", import_config
)
```

## 変換後のファイル運用

インポート結果の USD は、**「変換直後の生成物」と「調整済みのロボットアセット」を分けて管理**するのがおすすめです。再インポートで調整が消えないよう、調整はレイヤーを分けるか、生成物を参照した別 USD で行うと安全です。

## 次のステップ

- [ロボットのセットアップ](setup.md) — ゲイン調整・コントローラ・検証
