# 画像圧縮チェッカーワークフロー

このディレクトリには、PR時に画像ファイルの圧縮可能性をチェックするGitHub Actionsワークフローが含まれています。

## ワークフローの概要

`image-optimization-checker.yml`は以下の処理を行います：

1. PRの差分から画像ファイル（JPEG、PNG）を抽出
2. ファイル形式に応じた圧縮ツールを使用して圧縮テスト
   - JPEGの場合: `jpegoptim`
   - PNGの場合: `pngquant`
3. 圧縮前後のファイルサイズを比較
4. 圧縮後のサイズが元のサイズの70%以下になる場合:
   - CIをFailにする
   - PRにコメントで通知
   - 通知内容には、手動でCLIから圧縮する方法を含める

## 技術的な詳細

- トリガー条件: PRが作成または更新された時
- 使用環境: ubuntu-latest
- 使用ツール: 
  - jpegoptim: JPEG圧縮用
  - pngquant: PNG圧縮用

## カスタマイズ方法

圧縮の閾値やパラメータを変更する場合は、`image-optimization-checker.yml`ファイル内の以下の箇所を編集してください：

- JPEG圧縮率の変更: `jpegoptim --max=80` の数値を調整
- PNG圧縮品質の変更: `pngquant --quality=65-80` の数値を調整
- サイズ削減率の閾値変更: `if (( $(echo "$REDUCTION > 30" | bc -l) ));` の `30` を変更 
