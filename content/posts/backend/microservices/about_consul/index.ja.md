---
title: "Consul について"
date: "2025-12-15"
summary: "📝 Consul のメモ..."
tags: ["backend", "microservices", "consul"]
---

Consul は、HashiCorp によって作成されたサービスディスカバリおよびサービスメッシュツールです。

- 自分自身を登録する (Register themselves)
- 他のサービスを発見する (Discover other services)
- ヘルスチェックを実行する (Perform health checks)
- Key/Value 設定を行う (Do key/value configuration)
- サービス間通信を保護する (Secure service communication / service mesh)

マイクロサービスにおける Consul の役割：

1. サービスレジストリ (Service Registry)
   - 各サービスが自分自身（IP、ポート、ヘルスチェック）を登録します。
2. サービスディスカバリ (Service Discovery)
   - 他のサービスが Consul に問い合わせて、通信したいサービスのその時点のアドレスを見つけます。
3. ヘルスチェックに基づいたロードバランシング (Health-based Load Balancing)
   - Consul は健全な（ヘルシーな）サービスインスタンスのみを返し、負荷を分散させることができます。

```go
// 直接指定の場合 (Direct):
http.Get("http://player-service:8001/players/1")

// Consulによるディスカバリ (Consul Discovery):
url, _ := serviceDiscovery.DiscoverService("player-service")
http.Get(url + "/players/1")
// url は次のようになります: [http://172.20.0.5:8001](http://172.20.0.5:8001) (Consulから発見されたアドレス)

// メリット (Benefits):
// - player-serviceが3つのインスタンスにスケールした場合でも、Consulはそれら全てを把握しています
// - インスタンス間でのランダムなロードバランシング（負荷分散）が可能
// - 健全な（ヘルシーな）インスタンスのみを呼び出します
// - サービスが移動（IP変更など）しても、コードの変更は不要です
```
