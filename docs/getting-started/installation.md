# インストール

!!! info "対応する公式ドキュメント"
    [Isaac Sim Installation](https://docs.isaacsim.omniverse.nvidia.com/latest/installation/index.html)

Isaac Sim には複数のインストール方式があります。用途に応じて選択してください。

| 方式 | 向いている用途 |
| --- | --- |
| **Workstation(バイナリ)** | GUI で対話的に使う。最も標準的な方式 |
| **コンテナ (Docker)** | サーバー・クラウドでのヘッドレス実行、CI、大規模データ生成 |
| **pip パッケージ** | Python プロジェクトへの組み込み、軽量なスクリプト実行 |
| **ソースからビルド** | 本体の改造・コントリビューション |

## 方式1: Workstation(バイナリ)インストール

1. [NVIDIA 公式ダウンロードページ](https://developer.nvidia.com/isaac/sim) から、OS に対応した Isaac Sim 本体の zip をダウンロードします
2. 任意のディレクトリ(例: `~/isaacsim`)に展開します
3. 展開先のランチャースクリプトを実行します

```bash
# Linux の例
cd ~/isaacsim
./isaac-sim.sh
```

```powershell
# Windows の例
.\isaac-sim.bat
```

初回起動は、シェーダーコンパイルやキャッシュ生成のため**数分〜十数分かかる**ことがあります。2回目以降は高速化されます。

!!! tip "セレクターについて"
    同梱の `isaac-sim.selector.sh`(App Selector)を使うと、通常版・ヘッドレス版などの起動モードを GUI から選択できます。

## 方式2: コンテナ (Docker)

NGC(NVIDIA GPU Cloud)で公式コンテナイメージが配布されています。ヘッドレスなサーバー環境での実行に適しています。

前提: NVIDIA ドライバ、Docker、NVIDIA Container Toolkit がインストール済みであること。

```bash
# イメージの取得(タグは利用したいバージョンに置き換え)
docker pull nvcr.io/nvidia/isaac-sim:5.0.0

# ヘッドレスで起動する例
docker run --name isaac-sim --entrypoint bash -it --gpus all \
  -e "ACCEPT_EULA=Y" -e "PRIVACY_CONSENT=Y" \
  --network=host \
  -v ~/docker/isaac-sim/cache/kit:/isaac-sim/kit/cache:rw \
  -v ~/docker/isaac-sim/cache/ov:/root/.cache/ov:rw \
  nvcr.io/nvidia/isaac-sim:5.0.0
```

コンテナ内では以下のように起動します。

```bash
# ヘッドレス(WebRTC ストリーミング)で起動
./runheadless.sh
```

手元の PC から **Isaac Sim WebRTC Streaming Client** で接続すると、リモートの画面を操作できます。

!!! note "キャッシュのマウント"
    シェーダーキャッシュ等をホスト側にマウントしておくと、コンテナ再作成後も初回起動の待ち時間を短縮できます。

## 方式3: pip インストール

Python 環境に直接インストールする方式です。スクリプト主体のワークフローに向いています。

```bash
# 仮想環境の作成(対応する Python バージョンを使用)
python3 -m venv env_isaacsim
source env_isaacsim/bin/activate

# インストール(パッケージ構成はバージョンにより異なる)
pip install "isaacsim[all,extscache]" --extra-index-url https://pypi.nvidia.com
```

インストール後、`isaacsim` コマンドや Python スクリプトから起動できます。

```bash
# GUI 付きで起動する例
isaacsim
```

!!! warning
    pip 方式は GLIBC バージョンや Python バージョンの制約が比較的厳しいため、エラーが出る場合は公式ページの対応表を確認してください。

## 環境変数と EULA

コンテナやヘッドレス実行では、初回に EULA(使用許諾)への同意が必要です。CI などの非対話環境では `ACCEPT_EULA=Y` を環境変数で渡します。

## インストール後の確認

1. Isaac Sim を起動し、空のステージが表示されることを確認
2. メニューから サンプルシーン(例: 倉庫環境やロボットのサンプル)を開く
3. **Play ボタン(▶)** を押して物理シミュレーションが動くことを確認

## 次のステップ

- [クイックスタート](quickstart.md) — UI の基本操作と最初のシミュレーション
