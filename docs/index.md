# Isaac Sim 日本語ガイド(非公式)

NVIDIA **Isaac Sim** は、Omniverse プラットフォーム上に構築されたロボティクス向けシミュレーションアプリケーションです。物理的に正確なシミュレーション、フォトリアルなレンダリング、合成データ生成、ROS 2 連携などの機能を備え、ロボットの開発・テスト・学習データ生成のワークフローを支援します。

本サイトは、[公式ドキュメント(英語)](https://docs.isaacsim.omniverse.nvidia.com/latest/index.html) の構成に沿って、Isaac Sim の概念と使い方を**日本語で独自に解説する非公式ガイド**です。

!!! warning "非公式ガイドについて"
    本サイトは NVIDIA 社とは無関係のコミュニティによる非公式ガイドであり、公式ドキュメントの翻訳版ではありません。正確な仕様・最新情報は必ず [公式ドキュメント](https://docs.isaacsim.omniverse.nvidia.com/latest/index.html) を参照してください。

## このガイドの読み方

| 目的 | 参照する章 |
| --- | --- |
| Isaac Sim を初めて触る | [概要と特徴](getting-started/overview.md) → [インストール](getting-started/installation.md) → [クイックスタート](getting-started/quickstart.md) |
| USD やステージの考え方を知りたい | [基本概念](concepts/usd-stage.md) |
| 手持ちのロボットモデルを動かしたい | [ロボットのインポート](robots/import.md) |
| センサーをシミュレートしたい | [センサー](sensors/camera-rtx.md) |
| Python で自動化・スクリプト化したい | [Python スクリプティング](development/python-scripting.md) |
| ROS 2 と接続したい | [ROS 2 連携](development/ros2.md) |
| 学習用の合成データを作りたい | [合成データ生成 (Replicator)](development/replicator.md) |
| 強化学習をしたい | [Isaac Lab と強化学習](development/isaac-lab.md) |

## 主なトピック

- **インストールと環境構築** — Workstation / コンテナ / pip の各インストール方式と GPU 要件
- **USD ベースのシーン構築** — OpenUSD によるステージ・プリム・レイヤーの概念
- **物理シミュレーション** — PhysX による剛体・関節・接触のシミュレーション
- **ロボットモデルの取り込み** — URDF / MJCF インポーターとロボットのリギング
- **センサーシミュレーション** — RTX ベースのカメラ・LiDAR、物理ベースの IMU・接触センサー
- **ROS 2 ブリッジ** — トピック・TF・センサーデータのパブリッシュ
- **Replicator による合成データ生成** — ドメインランダマイゼーションとアノテーション出力
- **Isaac Lab** — 強化学習・模倣学習のためのフレームワーク

## 公式リソース

- [Isaac Sim 公式ドキュメント](https://docs.isaacsim.omniverse.nvidia.com/latest/index.html)
- [Isaac Sim ダウンロード(NVIDIA 公式)](https://developer.nvidia.com/isaac/sim)
- [isaac-sim GitHub Organization](https://github.com/isaac-sim)
- [NVIDIA 開発者フォーラム (Isaac Sim)](https://forums.developer.nvidia.com/c/omniverse/simulation/69)
