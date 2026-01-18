---
title: Kong について
date: 2025-12-16
summary: 📝 Kong のメモ...
tags:
  - backend
  - api
  - kong
---

## 定義

- API ゲートウェイ
- リバースプロキシ (Nginx + OpenResty ベース)
- マイクロサービス向けのトラフィック制御レイヤー

```
Client (Browser / Mobile / Game)
        |
        v
┌─────────────────────────┐
│        API Gateway      │
│  (Routing / Auth / CORS)│
└─────────────────────────┘
        |
        v
┌───────────────┬───────────────┐
│ player-service│ battle-service│
└───────────────┴───────────────┘
        |
        v
┌─────────────────────────┐
│   Database / Cache      │
└─────────────────────────┘
```

## コンセプト

### Service

上流のバックエンドサービスを表す。

```
Service:    player-service
URL:        http://player-service:8080
```

1 つのバックエンドサービスは、通常 1 つの Kong Service に対応する。

### Route

- リクエストをどのようにマッチングし、どの Service に転送するかを定義する。
- マッチングの条件:
  - パス (/api/players)
  - ホスト名 (api.example.com)
  - HTTP メソッド (GET, POST)
  - ヘッダー (Route → Service)

### Plugin

- バックエンドコードに手を加えずに機能を追加する仕組み
- 代表的なプラグイン:
  - 認証 (JWT, Key Auth)
  - CORS
  - レートリミット
  - リクエスト／レスポンス変換
  - ログ & メトリクス
- プラグインは以下の単位に紐付け:
  - グローバル
  - Service
  - Route
  - Consumer

### Consumer

- API を利用するクライアント／ユーザーを表す。
- 主に認証系プラグインと組み合わせて使用される:
  - API キー
  - JWT クレデンシャル
  - OAuth トークン
