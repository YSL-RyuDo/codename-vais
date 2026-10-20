# CodeName VAIS

[English](README.md), [Korean](README.ko.md), [Japanese](README.ja.md)

チームで開発中のマルチプレイヤーFPS。C++サーバーと試合状態をUnityのプレイ体験につなぎます。

プロジェクト概要、担当した実装、開発デモをまとめた公開ポートフォリオです。チームのソースリポジトリは非公開のままです。

| プロジェクト情報 | 内容 |
|---|---|
| 期間とチーム構成 | 2026.03 ~ 現在, 10名：ソフトウェア5名、グラフィック5名 |
| 担当 | サーバー開発・Unityマルチプレイヤー連携 |
| 使用技術 | C++, Windows IOCP, C#, Unity |

[プロジェクト詳細](https://ysl-ryudo.github.io/ja/work/vector/)

[![CodeName VAIS, Lobby UI](https://i.ytimg.com/vi/WqRBn-yWVdo/hqdefault.jpg)](https://www.youtube.com/watch?v=WqRBn-yWVdo)

## 担当した実装

### ProjectVectorServer

ゲームクライアント向けのIOCP接続処理、セッション、パケットディスパッチを実装しました。

ルーム・試合のライフサイクル、プロフィール・キャリア情報、ソーシャル機能、ショップサービスとクライアント契約を担当しました。

再接続時の表示状態復元と、パーティー・チーム音声の権限制御を追加しました。ReconnectJournalはダメージ・報酬・プレイヤー命令の再実行ではなく、持続する状態を保持します。

### projectVector

センチネルを含むキャラクタースキルのパケット処理とゲームプレイ連携を実装しました。

マッチングと試合・結果演出、キャリア・プロフィール情報、フレンド、ショップ連携、再接続・音声のクライアント側フローを担当しました。

チーム提供のモデル・モーション統合、ADS調整、訓練場拡張、エディター検証ツールを担当しました。統合とモデル・アニメーション自体の制作は区別します。

## 実装と設計

マルチプレイヤーFPSでは通信、試合のライフサイクル、プレイヤーに見える状態をつなぐ必要があります。再接続では復元する状態と再実行しない操作を区別します。

IOCPサーバーのセッション・パケット処理から、Unityの試合・プロフィール・ショップ連携、再接続時の表示状態復元まで担当しました。ReconnectJournalは持続状態を保持し、ダメージ・報酬・プレイヤー命令の再実行を除外します。

| 領域 | 実装 |
|---|---|
| 接続・通信 | IOCP・セッション・パケットディスパッチ |
| ゲームサービス | ルーム・試合のライフサイクル、キャリア・プロフィール・ショップ・ソーシャルデータ |
| Unity連携 | ゲームパケット、ロビー・結果、Sentinel能力、音声権限 |
| 復元の境界 | 最新の持続表示状態を保持し、ゲーム操作の再実行を除外 |

## ReconnectJournal

**復元する状態を選ぶ**

Observeは明示された持続表示パケットを収集します。ダメージ・報酬・プレイヤー命令は復元記録に含めません。

**最新状態に置き換える**

パケット種別とエンティティIDをキーに保存します。更新は同じ項目を置き換え、操作履歴の順次再生とは分離します。

**ライフサイクルの変更を反映**

新しい試合・ラウンドのパケットで記録を初期化します。削除パケットは対応状態を消し、相互排他的な状態は以前の項目を整理します。

**残り時間で復元**

Snapshotは保存期間からsteady clockの経過時間を引き、0以上に制限します。0秒の状態パケットも残し、クライアントの古い状態を解除します。

CodeName VAISが正式名称で、Project Vectorは旧仮称です。

## 開発デモ動画

[Lobby UI](https://www.youtube.com/watch?v=WqRBn-yWVdo)

[Store system](https://www.youtube.com/watch?v=OBFWkZE2rzo)

[Profile system](https://www.youtube.com/watch?v=A3CkQvnQEGI)

[Earlier Project Vector demo](https://www.youtube.com/watch?v=oga4ai_1Ugg)

## ソースへのアクセスとクレジット

[ProjectVectorServer](https://github.com/YSL-RyuDo/ProjectVectorServer), 非公開・アクセス権が必要

[projectVector](https://github.com/YSL-RyuDo/projectVector), 非公開・アクセス権が必要

以下は説明の根拠となるコード版と変更記録です。元の非公開リポジトリの閲覧権限が必要です。

| リポジトリ | 実装記録 |
|---|---|
| ProjectVectorServer | [IOCP](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/IocpCore.cpp) |
| ProjectVectorServer | [ReconnectJournal](https://github.com/YSL-RyuDo/ProjectVectorServer/blob/3066fe6d90fb4a56a6c440af56c4e119188e73fc/ProjectVectorServer/ProjectVectorServer/ReconnectJournal.h) |
| ProjectVectorServer | [Validation sources](https://github.com/YSL-RyuDo/ProjectVectorServer/tree/3066fe6d90fb4a56a6c440af56c4e119188e73fc/Tests) |
| projectVector | [スキル](https://github.com/YSL-RyuDo/projectVector/commit/756c9a09f32ed4a073d5d3d60008b1aaa44f0f60) |
| projectVector | [訓練場・照準](https://github.com/YSL-RyuDo/projectVector/commit/f4457e2d77c251b9e0da5d976927303d04a64b43) |
| projectVector | [再接続・音声](https://github.com/YSL-RyuDo/projectVector/commit/b62cbeeb16e72738d2dd7c491c8d42ec31afcd18) |
| projectVector | [YSLの追加作業](https://github.com/YSL-RyuDo/projectVector/commit/5e5c3c53b010fb2807f6b53c0eb0f5ceec8d1350) |

上記は私が担当したプログラミングの範囲です。グラフィック、音声、外部素材、他のメンバーの作業は元のクレジットを保持します。動画は撮影時点の記録であり、現行版の負荷試験結果を示すものではありません。
