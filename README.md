# MultiSongRequestManager

[![Build and Release](https://github.com/denpadokei/MultiSongRequestManager/actions/workflows/build-and-release.yml/badge.svg)](https://github.com/denpadokei/MultiSongRequestManager/actions/workflows/build-and-release.yml)

BeatSaberMultiplayerLite のマルチプレイルームで、Twitch チャットからの楽曲リクエストを受け付ける MOD です。`!bsr` `!sr` `!add` に対応しています。

## 機能

- ChatCore のチャットメッセージを監視し、`!bsr` `!sr` `!add` で始まるメッセージをリクエストとして処理
- BeatSaberMultiplayerLite のクライアントがルームに参加しているときだけ動作
- メッセージの最後の単語を BeatSaver のキーとして検索し、曲情報を取得
- 取得した曲を `Client.RequestSong` でマルチプレイルームへ送信 (曲名・ハッシュ・levelId・曲の長さ)
- BeatSaver API へのリクエストはセマフォで直列化し、同時アクセスを防止
- ホスト・クライアントのどちらでも、導入すれば動作する

## 動作環境

| 項目 | 内容 |
| --- | --- |
| Beat Saber | 1.11.1 |
| 依存 MOD | BSIPA 4.0.5 / BeatSaberMultiplayerLite 7.2.7 / ChatCore 1.0.0 / BeatSaverSharp 1.5.2 |

## インストール

1. Beat Saber を 1.11.1 に合わせる (BSManager などのバージョン管理ツールを推奨)
2. BSIPA と上記の依存 MOD をインストールする
3. [Releases](https://github.com/denpadokei/MultiSongRequestManager/releases) から `MultiSongRequestManager-0.1.2-bs1.11.1.zip` をダウンロードし、`Plugins` フォルダをゲームフォルダへ上書きコピーする

## 使い方

1. ChatCore で Twitch アカウントを設定する
2. BeatSaberMultiplayerLite でルームに接続する
3. チャットで次のいずれかを送る (`<key>` は BeatSaver のキー)

```text
!bsr <key>
!sr <key>
!add <key>
```

キーはメッセージの最後の単語が使われるため、コマンドの後ろにキーを置いてください。ルームに参加していないときのメッセージは無視されます。

## ビルド

このプロジェクトは `BuildTargets.targets` で `CodeTaskFactory` を使う従来形式の targets を持つため、`dotnet build` ではなく Visual Studio または Developer PowerShell の msbuild でビルドします。

```powershell
git clone https://github.com/denpadokei/MultiSongRequestManager.git
cd MultiSongRequestManager
msbuild .\MultiSongRequestManager\MultiSongRequestManager.csproj /t:Build /p:Configuration=Release
```

`BeatSaberDir` は csproj で `$(ProjectDir)Refs` を既定値にしているため、`/p:BeatSaberDir="C:\Program Files\Beat Saber"` のように指定するとゲームの参照アセンブリを解決できます。

GitHub Actions の `build-and-release.yml` が push ごとにビルドし、`X.X.X` 形式のタグを push するとリリースを作成します。

## ライセンス

MIT
