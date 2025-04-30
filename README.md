# 画像最適化ワークフロー GitHub Action

PRで追加または変更された画像ファイル（JPEG、PNG）を自動的にチェックし、圧縮可能な場合は最適化された画像を含む新しいPRを自動で作成するGitHub Actionです。

## 機能

- PRの差分に含まれる画像ファイル（JPEG、PNG）を検出
- 圧縮可能かどうかを自動判定
- 圧縮後のサイズが元のサイズの70%以下になる場合：
  - 最適化された画像を含む新しいPRを自動作成
  - 元のPRにコメントを追加（最適化PRへのリンクを含む）
  - CIをFailに設定

## 使用方法

リポジトリの`.github/workflows`ディレクトリに`image-optimization.yml`ファイルを配置するだけで利用できます。

```yml
name: 画像最適化ワークフロー

on:
  pull_request:
    types: [opened, synchronize]

permissions:
  contents: write
  pull-requests: write

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

## 自動PR機能

削減率が30%以上（圧縮後のサイズが元のサイズの70%以下）の場合：
1. 最適化された画像を含む新しいブランチを作成
2. 自動的にPRを作成
3. 元のPRにコメントを追加し、最適化PRへのリンクを提供

これにより、画像の最適化作業を簡単に取り込むことができます。

## ライセンス

MIT License
