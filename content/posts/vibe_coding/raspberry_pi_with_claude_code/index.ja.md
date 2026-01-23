---
title: Raspberry Pi を「Vibe Coding専用サーバー」に構築してみた
date: 2026-01-20
summary: 🤖 いつでもどこでもVibe Codingできる環境を構築した...
tags:
  - raspberry-pi
  - ai
  - vide-coding
  - claude-code
  - home-server
---

**手元のRaspberry Pi 5を「Vibe Coding専用サーバー」に仕立ててみた。**

モニターもキーボードも繋がない。MacやiPhoneからSSHで入って、Claude Codeと対話しながらコードを書く。そんな環境を作ってみたら、思った以上に集中できる開発空間になった。

## 🤔 なぜ専用端末なのか

正直、Macのターミナルで全部完結する。わざわざPiを使う必要なんてない。

でも、物理的に分離された「AIが常駐しているサーバー」があると、体験が変わった。Macは普通の作業用。Piは「純粋にコードを書く場所」。

それに、電車の中でiPhoneからSSH経由で自宅のサーバーに繋いで、「さっきのコード動かしといて」とClaudeに投げておく。家に着いたらMacから確認する。この非同期な感じが妙に心地いい。

## 🛠️ セットアップ

OSはRaspberry Pi OS Lite（64-bit）。GUIは一切なし。Raspberry Pi Imagerでホスト名とSSH、Wi-Fiを設定して焼いた。SDカードを挿して電源を入れて終わり。

```bash
ssh user@raspberry-pi.local
```

中身は空なので、必要なものを入れていく。

```bash
# システム更新
sudo apt update && sudo apt install -y locales-all
sudo apt upgrade -y

# Node.js v20（Claude Codeに必要）
curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -
sudo apt install -y nodejs

# Claude Code
sudo npm install -g @anthropic-ai/claude-code

# OpenCode (AI coding エージェント)
sudo npm i -g opencode-ai

# 開発ツール
sudo apt install -y build-essential git tmux htop bat ripgrep

# Docker
curl -fsSL https://get.docker.com | sh
sudo usermod -aG docker $USER

# Go
wget https://go.dev/dl/go1.22.5.linux-arm64.tar.gz
sudo rm -rf /usr/local/go && sudo tar -C /usr/local -xzf go1.22.5.linux-arm64.tar.gz
echo 'export PATH=$PATH:/usr/local/go/bin' >> ~/.profile
```

セットアップが終わったら、`claude`と打つだけ。

## 📱 Tailscaleで外から繋ぐ

家の中だけだと勿体無いので、Tailscaleを入れた。

```bash
curl -fsSL https://tailscale.com/install.sh | sh
sudo tailscale up
```

iPhoneにもTailscaleアプリを入れてログイン。PiのIPアドレス（`100.x.y.z`）をコピーして、TermiusなどのSSHクライアントから接続。

これで、カフェにいようが電車の中だろうが、iPhoneから自宅のPiに繋がる。移動中に「あのAPIのレスポンス確認しといて」とClaudeに投げておいて、あとで結果を見る。こういう使い方ができるようになった。

## 💭 実際に使ってみて

意外と便利だったのが「常時起動」していること。Macはスリープするが、Piは24時間動いている。深夜にふと思いついたアイデアを試したくなった時、すぐに繋げる。この「いつでもそこにいる感じ」が思った以上に良かった。

Dockerも動くので、簡単なWebアプリを立ち上げてTailscale経由でアクセスすることもできる。外出先からiPhoneでデプロイして、その場で動作確認。

tmuxでセッションを残しておけば、接続が切れても作業が継続している。電車で降りる時に接続を切って、家に着いてMacから繋ぎ直すと、Claudeとの会話がそのまま残っている。デバイスを跨いでも文脈が途切れない。

## 📝 まとめ

結局、この環境の本質は「物理的な分離」と「常時接続」にあると思う。

Macの上で全部やることもできるが、専用のサーバーがあることで、開発モードへの切り替えがスムーズになった。そして、どこからでも繋がれることで、ふとした瞬間のアイデアを逃さなくなった。

Raspberry Pi一台と月額無料のTailscale。これだけで、自分専用のAIが常駐するサーバーが手に入る。
