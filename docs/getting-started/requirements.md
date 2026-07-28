# 動作要件

!!! info "対応する公式ドキュメント"
    [Isaac Sim Requirements](https://docs.isaacsim.omniverse.nvidia.com/latest/installation/requirements.html)

!!! warning
    要件はバージョンごとに更新されます。**最終的な確認は必ず公式の Requirements ページで行ってください。** 以下は執筆時点(Isaac Sim 5.x 世代)の一般的な目安です。

## ハードウェア要件の目安

| 項目 | 最小構成の目安 | 推奨構成の目安 |
| --- | --- | --- |
| CPU | 近年の x86-64 CPU(例: Intel Core i7 / AMD Ryzen 7 クラス) | より多コアの CPU |
| メモリ | 32 GB | 64 GB 以上 |
| GPU | RTX 対応 NVIDIA GPU(VRAM 8〜10 GB クラス) | RTX 4080 / RTX 6000 Ada クラス以上、VRAM 16 GB 以上 |
| ストレージ | SSD 50 GB 以上の空き | NVMe SSD 500 GB 以上 |

ポイント:

- **RTX 対応の NVIDIA GPU が必須**です。レンダラが RTX(レイトレーシング)前提のため、GTX 世代や他社製 GPU では動作しません
- 合成データ生成や大規模シーンでは **VRAM 容量が実用性を大きく左右**します
- GeForce 系でも動作しますが、長時間の運用では RTX A / Ada 世代のプロフェッショナル GPU が安定します

## ソフトウェア要件の目安

| 項目 | 内容 |
| --- | --- |
| OS | Ubuntu LTS(22.04 / 24.04 など)、Windows 10/11 |
| NVIDIA ドライバ | 公式ページで指定される推奨バージョン(新しめの Production Branch) |
| Python | pip インストール時は対応バージョンの Python(5.x 世代では 3.10〜3.11 が目安) |

!!! tip "ドライババージョンに注意"
    Isaac Sim はドライババージョンとの相性問題が起きやすいアプリケーションです。起動しない・レンダリングが乱れる場合は、まず公式 Requirements ページに記載された推奨ドライバとの一致を確認してください。

## 互換性チェッカー

Isaac Sim には **Compatibility Checker** という軽量ツールが用意されており、本体をインストールする前に、お使いのマシンが要件を満たすかを確認できます。GPU・ドライバ・メモリなどがチェック項目ごとに合否表示されるため、導入前の確認に便利です。

## クラウド / リモートでの利用

ローカルに RTX GPU がない場合は、以下の選択肢があります。

- **クラウド GPU インスタンス**(AWS / GCP / Azure の NVIDIA GPU 搭載インスタンス)上でコンテナ版を実行
- **リモート表示**: ヘッドレスで起動し、WebRTC ベースのライブストリーミングクライアントで手元の PC から操作

## 次のステップ

- [インストール](installation.md) — Workstation / コンテナ / pip 方式から選択
