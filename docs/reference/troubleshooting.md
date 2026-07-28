# トラブルシューティング

!!! info "対応する公式ドキュメント"
    [Isaac Sim — Troubleshooting](https://docs.isaacsim.omniverse.nvidia.com/latest/overview/known_issues.html) / [NVIDIA 開発者フォーラム](https://forums.developer.nvidia.com/c/omniverse/simulation/69)

よくある問題と確認ポイントをまとめます。

## 起動関連

### 起動しない / 起動が非常に遅い

- **初回起動はシェーダーコンパイルで数分〜十数分かかるのが正常**です。2 回目以降で改善するか確認
- NVIDIA ドライバが公式推奨バージョンか確認(古すぎ・新しすぎの両方で問題が出ることがある)
- キャッシュ破損が疑われる場合は、キャッシュディレクトリ(`~/.cache/ov` など)を退避して再起動
- ログを確認: `~/.nvidia-omniverse/logs/`(Linux の例)にログが出力されます

### GPU が認識されない / レンダリングが真っ黒

- `nvidia-smi` で GPU とドライバが正常か確認
- ハイブリッド GPU(ノート PC)では、NVIDIA GPU が使われる設定になっているか確認
- コンテナでは `--gpus all` と NVIDIA Container Toolkit の導入を確認

## 物理・ロボット関連

### ロボットが Play した瞬間に暴れる・弾け飛ぶ

- ジョイントドライブの Stiffness / Damping が過大でないか
- リンク同士がめり込んだ初期姿勢になっていないか(自己衝突を無効化するか、初期姿勢を調整)
- 質量・慣性が異常値(0 や極端な値)になっていないか
- 物理タイムステップを細かくする(60 Hz → 120/240 Hz)

### 物体が床をすり抜ける

- 床・物体の両方にコライダーが付いているか
- 高速で動く小物体は、タイムステップを細かくするか CCD(連続衝突判定)を検討
- スケールが極端(ミリメートル基準のモデルなど)になっていないか

### インポートしたロボットのサイズ・向きがおかしい

- 元データの単位(mm/m)とアップ軸(Y-up/Z-up)を確認
- ステージの `metersPerUnit` と `upAxis` を確認

## ROS 2 関連

### トピックが見えない

- ブリッジ拡張が有効か、シミュレーションが Play 中か(多くのノードは Play 中のみ動作)
- `ROS_DOMAIN_ID` がホスト側と一致しているか
- RMW 実装(FastDDS / CycloneDDS)の組み合わせを確認
- コンテナの場合はネットワークモード(`--network=host` 推奨)を確認

### TF / Nav2 の時刻ずれエラー

- `/clock` を配信し、ROS 2 側ノードの `use_sim_time` を有効化しているか確認

## パフォーマンス関連

### FPS が低い

- カメラ(レンダープロダクト)の数と解像度を減らす
- レンダリングモードをリアルタイムに(パストレーシングは重い)
- 物理のみが必要な用途ではヘッドレス+`render=False` でステップ実行
- VRAM 不足が起きていないか `nvidia-smi` で確認

## 情報の探し方

1. 公式ドキュメントの Known Issues / Troubleshooting ページ
2. [NVIDIA 開発者フォーラム](https://forums.developer.nvidia.com/c/omniverse/simulation/69) — 事例が最も豊富
3. [GitHub Issues (isaac-sim/IsaacSim)](https://github.com/isaac-sim/IsaacSim/issues)
4. ログファイル(起動ログ・クラッシュログ)の該当箇所を検索
