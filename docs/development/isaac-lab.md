# Isaac Lab と強化学習

!!! info "対応する公式ドキュメント"
    [Isaac Lab 公式ドキュメント](https://isaac-sim.github.io/IsaacLab/) / [GitHub (isaac-sim/IsaacLab)](https://github.com/isaac-sim/IsaacLab)

**Isaac Lab** は、Isaac Sim を基盤とするロボット学習(強化学習・模倣学習)用のオープンソースフレームワークです。旧 Isaac Gym / Orbit の系譜を統合した後継にあたります。

## Isaac Sim との関係

| | Isaac Sim | Isaac Lab |
| --- | --- | --- |
| 役割 | 汎用ロボティクスシミュレータ | 学習ワークフロー特化のフレームワーク |
| 実行形態 | GUI アプリ / Python | Isaac Sim 上で動く Python ライブラリ |
| 主な用途 | 検証・合成データ・ROS 連携 | RL / IL の環境定義と学習実行 |

Isaac Lab は Isaac Sim の物理・レンダリングを利用しつつ、**数千環境の GPU 並列シミュレーション**、タスク(環境)定義の枠組み、主要 RL ライブラリ(RSL-RL、skrl、RL Games、Stable-Baselines3 など)との接続を提供します。

## 特徴

- **GPU 並列環境**: 1 GPU 上で数百〜数千の環境インスタンスを同時に走らせ、サンプル収集を高速化
- **タスク定義の枠組み**: Manager ベース / Direct 方式による環境(観測・報酬・リセット条件)の構造化された定義
- **豊富な同梱タスク**: 四足歩行、ヒューマノイド、マニピュレーション、ハンドの器用な操作など
- **模倣学習・テレオペ**: デモ収集(テレオペレーション)から模倣学習までのパイプライン

## セットアップの概要

Isaac Lab は Isaac Sim とは別リポジトリとして配布されます。

```bash
# 例: pip ベースのセットアップの流れ(詳細は公式ドキュメント参照)
git clone https://github.com/isaac-sim/IsaacLab.git
cd IsaacLab
./isaaclab.sh --install  # 依存関係と RL ライブラリのインストール
```

## 学習の実行例

```bash
# 同梱タスクの学習を実行する例(スクリプト名・タスク名は公式で確認)
./isaaclab.sh -p scripts/reinforcement_learning/rsl_rl/train.py \
  --task Isaac-Velocity-Rough-Anymal-C-v0 --headless
```

- `--headless` でレンダリング無しの高速学習
- 学習済みポリシーは play 用スクリプトで可視化・評価

## どちらを使うべきか

| やりたいこと | 推奨 |
| --- | --- |
| RL でロコモーション・マニピュレーションを学習 | Isaac Lab |
| ROS 2 と繋いだシステム検証 | Isaac Sim(本体) |
| 合成データ生成 | Isaac Sim + Replicator |
| 学習済みポリシーをシステムに組み込んで検証 | 両方(Lab で学習 → Sim で統合検証) |

## 次のステップ

- [Python スクリプティング](python-scripting.md) — Isaac Sim 側の自動化基盤
- [用語集](../reference/glossary.md)
