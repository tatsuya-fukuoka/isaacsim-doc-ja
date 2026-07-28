# Python スクリプティング

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Python Scripting](https://docs.isaacsim.omniverse.nvidia.com/latest/python_scripting/index.html) / [API リファレンス](https://docs.isaacsim.omniverse.nvidia.com/latest/py/index.html)

Isaac Sim のほぼすべての機能は Python から操作できます。実行形態は主に 3 つあります。

## 実行形態の比較

| 形態 | 説明 | 向いている用途 |
| --- | --- | --- |
| **Script Editor** | GUI 内の対話的エディタ | ちょっとした確認・試行錯誤 |
| **Standalone(スタンドアロン)** | Python スクリプトとして Isaac Sim を起動・制御 | 自動化、データ生成、学習、CI |
| **Extension(拡張機能)** | Kit 拡張として常駐するモジュール | UI 付きツール、恒常的な機能追加 |

## スタンドアロンスクリプト

`SimulationApp` を最初に生成してから、他のモジュールを import するのが約束事です。

```python
from isaacsim import SimulationApp

# ヘッドレスにするかどうかを指定して起動(最初に実行すること)
simulation_app = SimulationApp({"headless": True})

# SimulationApp 生成後に他モジュールを import する
from isaacsim.core.api import World
from isaacsim.core.api.objects import DynamicCuboid
import numpy as np

world = World()
world.scene.add_default_ground_plane()
cube = world.scene.add(
    DynamicCuboid(
        prim_path="/World/cube",
        name="cube",
        position=np.array([0.0, 0.0, 1.0]),
        size=0.1,
    )
)

world.reset()
for i in range(500):
    world.step(render=False)  # 物理を1ステップ進める
    if i % 100 == 0:
        print(cube.get_world_pose())

simulation_app.close()
```

!!! warning "import の順序"
    `SimulationApp` を作る前に `omni.*` や `isaacsim.core` 系のモジュールを import すると失敗します。「アプリ起動 → import」の順序は最重要ルールです。

### 実行方法

```bash
# バイナリ版に同梱の Python ランチャーを使う場合
./python.sh my_script.py

# pip インストール版の場合
python my_script.py
```

## World / Scene の考え方

`World` はシミュレーションループ(物理ステップ・レンダリング・コールバック)を管理する高レベル API です。

- `world.reset()` — シーンを初期化し、Articulation などのハンドルを有効化
- `world.step()` — 物理(+必要ならレンダリング)を 1 ステップ進める
- `world.add_physics_callback()` — 毎物理ステップで呼ばれる関数を登録

## タスク指向の API

ピック&プレースなどの定型タスクを構成するための `Task` クラスや、逆運動学・軌道生成のためのモーション生成 API も用意されています。サンプル(Examples)のソースコードが最良の教材です。

## Jupyter / VS Code からの利用

- pip 版はそのまま Jupyter カーネルから利用できます(ヘッドレス推奨)
- VS Code 用のデバッグ接続用拡張もあり、GUI 実行中の Isaac Sim にアタッチしてデバッグできます

## 次のステップ

- [拡張機能 (Extensions)](extensions.md) — 常駐ツールとして実装する
- [Isaac Lab と強化学習](isaac-lab.md) — 学習ワークフローへ進む
