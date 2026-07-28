# 拡張機能 (Extensions)

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Extensions / Kit 開発](https://docs.isaacsim.omniverse.nvidia.com/latest/utilities/index.html) / [Omniverse Kit ドキュメント](https://docs.omniverse.nvidia.com/kit/docs/)

Isaac Sim の機能は **Extension(拡張機能)** の集合として実装されています。ユーザー自身も拡張機能を作成して、独自ツールや UI を追加できます。

## Extension とは

- Python(または C++)モジュール + 設定ファイル(`extension.toml`)で構成されるプラグイン
- **Window > Extensions** の Extension Manager から検索・有効化・無効化が可能
- Isaac Sim 本体の機能(インポーター、センサー、ROS ブリッジなど)もすべて拡張機能

## 最小構成

```
my_extension/
├── config/
│   └── extension.toml       # メタデータと依存関係
└── my_company/
    └── my_extension/
        └── extension.py     # エントリポイント
```

`extension.toml` の例:

```toml
[package]
title = "My Extension"
version = "0.1.0"

[dependencies]
"omni.kit.uiapp" = {}

[[python.module]]
name = "my_company.my_extension"
```

`extension.py` の例:

```python
import omni.ext
import omni.ui as ui


class MyExtension(omni.ext.IExt):
    def on_startup(self, ext_id):
        self._window = ui.Window("My Tool", width=300, height=200)
        with self._window.frame:
            with ui.VStack():
                ui.Label("Hello Isaac Sim!")
                ui.Button("Run", clicked_fn=self._on_click)

    def _on_click(self):
        print("clicked")

    def on_shutdown(self):
        self._window = None
```

## 開発の流れ

1. 拡張機能用フォルダを作成し、Extension Manager の設定で**検索パスに追加**
2. Extension Manager で自作拡張を有効化
3. コードを編集すると**ホットリロード**され、再起動なしで反映される

ホットリロードにより「編集 → 即確認」の高速な開発サイクルが回せるのが Kit 拡張開発の魅力です。

## UI フレームワーク (omni.ui)

拡張機能の UI は `omni.ui` で構築します。宣言的にウィジェット(Button、Slider、TreeView など)を組み合わせるスタイルで、Isaac Sim 本体のパネルと同じ見た目のツールを作れます。

## テンプレート

公式の Kit 拡張テンプレートリポジトリ(kit-app-template など)を出発点にすると、ビルド設定・パッケージングが整った状態から始められます。

## 配布

- フォルダごと共有して検索パスに追加してもらう
- レジストリ経由での配布(組織内レジストリの構築も可能)

## 次のステップ

- [Python スクリプティング](python-scripting.md) — スクリプトベースの自動化
- [ROS 2 連携](ros2.md) — ROS 2 ブリッジ拡張の利用
