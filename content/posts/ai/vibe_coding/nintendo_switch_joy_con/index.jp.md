---
title: Joy-ConでVibe Codingしてみた
date: 2026-01-24
summary: 🤖 x 🎮 Switch Joy-Con と Claude Code の初コラボレーション。ゲーム機も生産ツールに進化するか。
tags:
  - nintendo-switch
  - ai
  - vide-coding
  - claude-code
---

## 🤔 きっかけ

Nintendo Switch 2を購入したので、旧Switchの使い道を考えていました。本体はもう使わないけど、Joy-Conはまだまだ現役で使えそう。そこで思いついたのが「Joy-Conをコーディングツールとして活用する」というアイデアです。

**仕組みはシンプル:**

```
1. Joy-ConのCaptureを押す → 音声入力開始
2. 喋る
3. もう一度Captureを押す→ 音声入力終了
4. Joy-ConのZLを押す → Enterキーで送信
```

これだけで、Vibe Codingができる。

## 🛠️ デバイス

- Nintendo SwitchのJoy-Con
- MacBook
- ボタンマッピングソフト（JoyKeyMapperなど）

## ⚙️ セットアップ方法

### 1. Joy-ConをMacに接続

```
1. システム設定 → Bluetooth
2. Joy-Conのシンクロボタン長押し
3. Mac側で検出されたらペアリング
```

### 2. 音声入力を有効化

```
1. システム設定 → キーボード
2. 音声入力をオン
3. ショートカットキーを確認
```

### 3. ボタンマッピング設定

JoyKeyMapperで以下のように設定しました:

- **ZLボタン** → Returnキー（送信・確定）
- **Captureボタン** → ⌘⇧V（音声入力開始）
- **方向キー** → 上下左右の矢印キー（カーソル移動）
- **L/SLボタン** → Delete/Tab（編集補助）
- その他、Left Stick Pushでマウス左クリックなど便利な設定も追加

<img src="../001.png" width="55%" />

## 💭 使ってみた感想

- キーボードから手を離してリラックスしながらコーディングできる
- 音声入力で長文も楽々
- コントローラーを握ってる感じがなんかゲーム感覚で楽しい
- 使わなくなったJoy-Conに新しい命を吹き込めた

## 📝 まとめ

Switch 2への移行で使わないはずだった古いJoy-Conが、まさかの「開発デバイス」に転生。フルコーディングは厳しくても、リラックスしてClaude Codeに簡単な指示を出す分には最適でした。
