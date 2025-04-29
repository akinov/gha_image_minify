# 画像自動圧縮チェッカー GitHub Action

PRで追加または変更された画像ファイル（JPEG、PNG）を自動的にチェックし、圧縮可能な場合は通知するGitHub Actionです。

## 機能

- PRの差分に含まれる画像ファイル（JPEG、PNG）を検出
- 圧縮可能かどうかを自動判定
- 圧縮後のサイズが元のサイズの70%以下になる場合にCIをFailに
- 圧縮方法を含む通知をPRにコメント

## 使用方法

リポジトリの`.github/workflows`ディレクトリに`image-optimization-checker.yml`ファイルを配置するだけで利用できます。

```yml
name: 画像圧縮チェッカー

on:
  pull_request:
    types: [opened, synchronize]

jobs:
  check-image-optimization:
    name: 画像の最適化チェック
    runs-on: ubuntu-latest
    
    steps:
      # ワークフローの詳細は.github/workflowsディレクトリのファイルを参照
```

## 判定基準

- JPEG形式 (*.jpg, *.jpeg): jpegoptimで圧縮
- PNG形式 (*.png): pngquantで圧縮
- 圧縮後のサイズが元のサイズの70%以下になる場合に圧縮を推奨

## ライセンス

MIT License 
