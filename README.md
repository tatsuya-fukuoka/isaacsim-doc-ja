# Isaac Sim 日本語ガイド(非公式) / isaacsim-doc-ja

NVIDIA Isaac Sim を日本語で学ぶための**非公式ガイド**です。[公式ドキュメント(英語)](https://docs.isaacsim.omniverse.nvidia.com/latest/index.html) の構成に沿って、概念・使い方を日本語で独自に解説しています。

**公開サイト**: https://tatsuya-fukuoka.github.io/isaacsim-doc-ja/

> [!IMPORTANT]
> 本サイトは公式ドキュメントの翻訳版(逐語訳)ではありません。NVIDIA 公式ドキュメントは NVIDIA Corporation の著作物であるため、本ガイドは独自執筆の解説と公式ページへのリンクで構成しています。正確な仕様は必ず公式ドキュメントを参照してください。

## ローカルでのプレビュー

```bash
pip install -r requirements.txt
mkdocs serve
# http://127.0.0.1:8000 で閲覧
```

## デプロイ

`main` ブランチへの push をトリガーに、GitHub Actions(`.github/workflows/deploy.yml`)が MkDocs でビルドし GitHub Pages へデプロイします。

初回のみ、リポジトリの **Settings → Pages → Build and deployment → Source** を **GitHub Actions** に設定してください。

## ディレクトリ構成

```
.
├── mkdocs.yml            # サイト設定・ナビゲーション
├── requirements.txt      # ビルド依存(mkdocs-material)
├── docs/                 # ドキュメント本体(Markdown)
│   ├── index.md
│   ├── getting-started/  # インストール・クイックスタート
│   ├── concepts/         # USD・物理・アセット
│   ├── robots/           # ロボットのインポート・セットアップ
│   ├── sensors/          # カメラ・LiDAR・物理センサー
│   ├── development/      # Python・拡張機能・ROS 2・Replicator・Isaac Lab
│   └── reference/        # 用語集・トラブルシューティング・リンク集
└── .github/workflows/deploy.yml  # GitHub Pages デプロイ
```

## 貢献

誤りの指摘・加筆修正の Pull Request を歓迎します。

## 免責事項

- 本プロジェクトは NVIDIA Corporation とは無関係です
- Isaac Sim、Omniverse、PhysX などは NVIDIA Corporation の商標または登録商標です
- 内容の正確性・完全性は保証されません

## ライセンス

本リポジトリの解説文書は [CC BY 4.0](https://creativecommons.org/licenses/by/4.0/deed.ja) で提供します。
