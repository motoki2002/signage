# Digital Signage (DS-ASTB2)

このリポジトリは、DS-ASTB2 のブラウザで GitHub Pages を開き、
動画1本（signage.mp4）を全画面でループ再生するための最小構成です。

## 構成
- index.html : 表示ロジック（全画面・ループ・自動復旧・定時リロード）
- signage.mp4 : 再生する動画（差し替え対象）
- manifest.webmanifest / sw.js / icons : ホーム画面アイコン（PWA）

## ふだんの更新（最短手順）
1. `signage.mp4` を **同名で** 差し替える（ファイル名は固定）
2. main ブランチへ反映（PR → merge 推奨）
3. GitHub Pages のビルド完了後、DS-ASTB2 側は
   - いったん終了して起動し直す、または
   - 定時リロード時刻まで待つ

## 注意（ファイルサイズ）
- ブラウザからのアップロードは 1ファイル 25MiB まで
- git push は 1ファイル 100MiB まで（それ以上は LFS 等が必要）
- Pages は “推奨 1GB” の上限があるため、動画サイズ管理に注意
