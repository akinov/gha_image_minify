# 画像最適化ワークフロー GitHub Action

PRで追加または変更された画像ファイル（JPEG、PNG）を自動的にチェックし、圧縮可能な場合は最適化された画像を含む新しいPRを自動で作成するGitHub Actionです。

## 機能

- PRの差分に含まれる画像ファイル（JPEG、PNG）を検出
- 圧縮可能かどうかを自動判定
- 削減率が30%以上になる場合：
  - 最適化された画像を含む新しいPRを自動作成
  - 元のPRにコメントを追加（最適化PRへのリンクを含む）
  - CIをFailに設定

## 使用方法

リポジトリの`.github/workflows`ディレクトリに`image-optimization.yml`ファイルを配置するだけで利用できます。

```yml
name: 画像最適化ワークフロー

env:
  IMAGE_OPTIMIZATION_TITLE: "画像の最適化"
  JPEG_QUALITY: 80
  PNG_QUALITY_RANGE: "65-80"
  COMPRESSION_THRESHOLD: 30

on:
  pull_request:
    types: [opened, synchronize]

permissions:
  contents: write
  pull-requests: write

jobs:
  image-optimization:
    name: 画像の最適化チェック
    runs-on: ubuntu-latest
    steps:
      # ワークフローの詳細は.github/workflowsディレクトリのファイルを参照
```

## 設定・カスタマイズ

ワークフローの動作は環境変数で調整できます：

```yml
env:
  IMAGE_OPTIMIZATION_TITLE: "画像の最適化"  # PRタイトルに使用
  JPEG_QUALITY: 80                          # JPEG圧縮品質（1-100）
  PNG_QUALITY_RANGE: "65-80"               # PNG圧縮品質範囲
  COMPRESSION_THRESHOLD: 30                # 最適化判定の閾値（削減率%）
```

## 判定基準

- **JPEG形式** (*.jpg, *.jpeg): jpegoptimで圧縮（品質: 80）
- **PNG形式** (*.png): pngquantで圧縮（品質: 65-80）
- **最適化判定**: 削減率が30%以上の場合に圧縮を推奨

## 自動PR機能

削減率が30%以上の場合：
1. 最適化された画像を含む新しいブランチを作成
2. 自動的にPRを作成
3. 元のPRにコメントを追加し、最適化PRへのリンクを提供

これにより、画像の最適化作業を簡単に取り込むことができます。

## パフォーマンス最適化

ワークフローは効率的に動作するよう設計されています：

- **画像ファイルがない場合**: jqのみインストール（高速実行）
- **画像ファイルがある場合**: 必要な画像最適化ツールを追加インストール
- **エラーハンドリング**: ツールの実行失敗時は適切にスキップ

## ライセンス

MIT License
