---
title: ロックについて
date: 2025-12-18
summary: 📝 ロックのメモ...
tags:
  - backend
  - database
  - locks
---
## 悲観ロック
- 競合は必ず発生すると考え、先にロックを取得する。
	- `FOR UPDATE`により行をロックし、他のトランザクションはロックが解放されるまでブロックされる。
``` sql
BEGIN;
SELECT coins FROM players WHERE id = 39 FOR UPDATE;
UPDATE players SET coins = coins + 10 WHERE id = 39;
COMMIT;
```
- 使用するケース
    - 競合が発生する可能性が高い場合
    - 強い一貫性が要求される場合
- デメリット
    - デッドロックが発生する可能性がある。
## 楽観ロック
- 競合はおそらく発生しないと考え、発生した場合はリトライする。
	- `version`カラムや`updated_at`カラムを利用する。
``` sql
-- 1. Read
SELECT coins, version FROM players WHERE id = 39; -- if version = 5

-- 2. Update
UPDATE players
SET coins = coins + 10, version = version + 1
WHERE id = 39 AND version = 5;

-- 3. If affected rows = 0, it means conflict, just retry.
SELECT coins, version FROM players WHERE id = 39; -- if version = 6
UPDATE players
SET coins = coins + 10, version = version + 1
WHERE id = 39 AND version = 6;
```
## 分散ロック
- 複数のサーバーが存在する環境で、この処理を必ず1台のサーバーだけが実行する必要がある場合。
    - サーバー間での重複報酬受け取りを防止
    - アイテムの二重消費を防止
    - スケジュールジョブが1回だけ実行されることを保証
- 分散ロックの種類
    - Redisロック