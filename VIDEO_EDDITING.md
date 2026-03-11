# 動画編集手順（回転の修正／複数動画の連結）

この文書は、デジタルサイネージ用の動画 `signage.mp4` を作成するための手順です。
サイネージ端末（DS-ASTB2等）では、動画の「回転メタデータ」の解釈が環境によって異なり、90°回転して表示されることがあります。
そのため、**必要であれば回転を映像に焼き込み**、複数素材がある場合は **事前に1本へ連結**してからアップロードします。

---

## ゴール（最終成果物）
- 出力ファイル名：`signage.mp4`
- 期待動作：PCでもDS-ASTB2でも **向きが正しい**／**ループ再生できる**
- GitHub上では `signage.mp4` を差し替える（同名上書き）

---

## 1) コマンド不要：Clipchampで「回転」＋「連結」＋「書き出し」

Windows標準で使えることが多く、初心者でも手順が分かりやすい方法です。
回転メタデータに依存しにくく、端末側の表示トラブルが減りやすいのが利点です。
MacOSの場合、iMovieやFinalCutProでもほとんど同じ操作で実行できます。

### 1-1. 回転（90°ズレの修正）
1. Clipchamp を起動
2. 「新しいビデオを作成」
3. 「メディアのインポート」で動画を取り込む
4. 取り込んだ動画を **タイムライン**へドラッグ
5. タイムライン上の動画をクリックして選択
6. プレビュー画面で「回転（90度）」を必要回数クリック  
   - 右回りに90°：1回  
   - 180°：2回  
   - 左回りに90°：3回  
7. プレビューで向きが正しいことを確認

### 1-2. 複数動画の連結（1本化）
1. 連結したい動画を複数インポート
2. タイムラインに **再生したい順**に並べる（端と端をぴったり繋ぐ）
3. 必要なら不要部分をトリミング
4. 右上の「エクスポート（書き出し）」を選択  
   - 迷ったら 1080p 推奨（学内回線・端末負荷のバランスが良いことが多い）
5. 出力ファイル名を `signage.mp4` にする（後でリポジトリへアップロードするため）

> 重要：素材ごとに向きが違う場合は、**各クリップをクリックして個別に回転**してから書き出してください。
> 動画ファイルが重くなりがちなので、別途圧縮などをする必要がある場合があります。

---

## 2) 速い（GUI）：LosslessCutで無劣化連結※条件付き

「画質を変えずに高速で繋ぎたい」場合の選択肢です。
ただし、**入力動画の条件（解像度・fps・コーデック等）が揃っている必要**があります。
また回転する必要がある素材は、連結前にClipchamp等で向きを焼き込む方が安全です。

### 手順（概要）
1. LosslessCut を起動
2. 連結したい動画をまとめてドラッグ＆ドロップ
3. 「Merge/Concatenate（結合）」を選択
4. 出力ファイルを `signage.mp4` にして実行
5. PCで再生して向き・順番を確認

---

## 3) コマンドにコピペだけでOK：FFmpeg

コマンドに慣れていない人でもコピペだけで使える最小セットを置いておきます。

### 3-1. 回転を焼き込む（90°）
- 右に90°（時計回り）
```powershell
ffmpeg -noautorotate -i input.mp4 -vf "transpose=1" -c:a copy fixed.mp4
```


- 左に90°（反時計回り）
```powershell
ffmpeg -noautorotate -i input.mp4 -vf "transpose=2" -c:a copy fixed.mp4
```

※input.mp4 を手元のファイル名に、fixed.mp4 を出力名に置き換えてください。

### 3-2. 複数動画を無劣化で連結（条件：全ファイルの仕様が同じ）

- 同じフォルダに list.txt を作成し、以下のように書く
file '01.mp4'
file '02.mp4'
file '03.mp4'

- 連結して signage.mp4 を作る
```powershell
ffmpeg -f concat -safe 0 -i list.txt -c copy signage.mp4
```

## 4) アップロード前のチェックリスト
- PCで signage.mp4 を再生し、向きが正しい
- 連結した場合、順番が正しい
- 再生開始～終了まで問題なく再生できる
- ファイル名が signage.mp4 になっている

## 5) GitHubへの反映（サイネージ更新）

1. リポジトリの signage.mp4 を差し替え（同名で上書き）
2.  commit → push
3.  GitHub Pages の反映を待つ（10分程度）
4.  DS-ASTB2 側でページを開き直す（または定時リロードまで待つ）

## 付録：よくある失敗

- 90°回転してしまう
  - 回転メタデータの影響の可能性があります。Clipchampで回転→書き出し（推奨）またはFFmpegで回転焼き込みを行ってください。

- 連結すると途中で黒くなる／止まる
  - 入力動画の解像度・fps・コーデックがバラバラな場合に起きやすいです。
  - 迷ったら Clipchampで1本化（再エンコード）に寄せるのが安全です。
---

# 付録：必要なツールの入れ方（LosslessCut / FFmpeg）
## A) LosslessCut のインストール（コマンド不要）
1. LosslessCut 公式のGithubを開く
3. README.mdのDownloadの項目の中腹にある、'If you prefer to download the executables manually, this will of course always be free'から、対応するOSのものをダウンロードする。
4. 7zipファイルがダウンロードされるので、右クリックで展開して、LosslessCut.exeを起動

公式サイト：([mifi.no](https://mifi.no/losslesscut/))

公式Github：（[mifi.no](https://github.com/mifi/lossless-cut)）

---

## B) FFmpeg のインストール（Windows）

FFmpegはコマンドラインツールです。  
「コマンドは使い慣れていないが、コピペならできる」方向けに、できるだけ簡単な方法を示します。

### 方法1（最も簡単）：winget（Windowsの標準パッケージ管理）
※ winget が使える環境であれば、これが最短です。

1. PowerShell を開く
2. 以下をコピペして実行：

```powershell
winget install Gyan.FFmpeg
```
インストール後、確認：
```powershell
ffmpeg -version
```
### 方法2：ZIPをダウンロードして手動で入れる（winget不可の場合）

1. FFmpeg配布（Windows builds）を入手する（例：gyan.dev などの配布）
2. ZIPを解凍して、bin フォルダ（ffmpeg.exe が入っている場所）を確認
3. ffmpeg.exe を使う方法は2通りあります：
- そのフォルダで実行（PATH設定なし）
  - ffmpeg.exe があるフォルダで PowerShell を開き、.\ffmpeg -version を実行
- PATHを通す（どこでも ffmpeg が使えるようにする）
  - 環境変数 Path に .../ffmpeg/bin を追加
  - 追加後に PowerShell を開き直して ffmpeg -version を実行
  - PATHの通し方は別途調べてください。
