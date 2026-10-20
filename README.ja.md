# CodeName VAIS

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

チームで開発中のマルチプレイヤーFPS。C++サーバーと試合状態をUnityのプレイ体験につなぎます。

プロジェクト概要、担当した実装、開発デモをまとめた公開ポートフォリオです。チームのソースリポジトリは非公開のままです。

**2026.03 - 現在, 10名：ソフトウェア5名、グラフィック5名**

**サーバー開発・Unityマルチプレイヤー連携**

C++, Windows IOCP, C#, Unity

[プロジェクト詳細](https://ysl-ryudo.github.io/ja/work/vector/)

[![CodeName VAIS — Lobby UI](https://i.ytimg.com/vi/WqRBn-yWVdo/hqdefault.jpg)](https://www.youtube.com/watch?v=WqRBn-yWVdo)

## 担当した実装

IOCPサーバーのセッション・パケット処理から、Unityの試合・プロフィール・ショップ連携、再接続時の表示状態復元まで担当しました。ReconnectJournalは持続状態を保持し、ダメージ・報酬・プレイヤー命令の再実行を除外します。

| 領域 | 実装 |
|---|---|
| 接続・通信 | IOCP・セッション・パケットディスパッチ |
| ゲームサービス | ルーム・試合のライフサイクル、キャリア・プロフィール・ショップ・ソーシャルデータ |
| Unity連携 | ゲームパケット、ロビー・結果、Sentinel能力、音声権限 |
| 復元の境界 | 最新の持続表示状態を保持し、ゲーム操作の再実行を除外 |

## ReconnectJournal

1. **復元する状態を選ぶ** — Observeは明示された持続表示パケットを収集します。ダメージ・報酬・プレイヤー命令は復元記録に含めません。
2. **最新状態に置き換える** — パケット種別とエンティティIDをキーに保存します。更新は同じ項目を置き換え、操作履歴の順次再生とは分離します。
3. **ライフサイクルの変更を反映** — 新しい試合・ラウンドのパケットで記録を初期化します。削除パケットは対応状態を消し、相互排他的な状態は以前の項目を整理します。
4. **残り時間で復元** — Snapshotは保存期間からsteady clockの経過時間を引き、0以上に制限します。0秒の状態パケットも残し、クライアントの古い状態を解除します。

CodeName VAISが正式名称で、Project Vectorは旧仮称です。

## 開発デモ動画

- [Lobby UI](https://www.youtube.com/watch?v=WqRBn-yWVdo)
- [Store system](https://www.youtube.com/watch?v=OBFWkZE2rzo)
- [Profile system](https://www.youtube.com/watch?v=A3CkQvnQEGI)
- [Earlier Project Vector demo](https://www.youtube.com/watch?v=oga4ai_1Ugg)

## ソースへのアクセスとクレジット

- [ProjectVectorServer](https://github.com/YSL-RyuDo/ProjectVectorServer) — 非公開・アクセス権が必要
- [projectVector](https://github.com/YSL-RyuDo/projectVector) — 非公開・アクセス権が必要

上記は私が担当したプログラミングの範囲です。グラフィック、音声、外部素材、他のメンバーの作業は元のクレジットを保持します。動画は撮影時点の記録であり、現行版の負荷試験結果を示すものではありません。
