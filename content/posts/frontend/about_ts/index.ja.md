---
title: TypeScriptについて
date: 2025-12-22
summary: 📝 TypeScriptのまとめ...
tags:
  - frontend
  - typescript
---

## TS vs JS

- TypeScript は JavaScript に静的型付けを追加することで、コンパイル時にエラーを検知でき、保守性と可読性が向上します。特にバックエンドでは API の入出力や DB モデルの安全性を高めるために有効です。

## Interface vs Type

- 基本的なオブジェクトの構造定義には`interface`を使い、union 型が必要な場合は`type`を使います。
- 使わわけ
  - 基本は`interface`を使う
    - バックエンドの API レスポンス、DB のモデル、クラスの定義など。
    - 「オブジェクトの構造」を定義する場合は`interface`の方がパフォーマンスが良いとされています。
  - 以下の場合は`type`を使う
    - `type Status = 'active' | 'inactive'`のような Union 型を作りたい時。
    - プリミティブ型(string や number)に別名をつけたい時。
    - React の Props などで、複雑な型の合成が必要な時。

### Interface

- 「オブジェクトがどのようなプロパティを持つか」という構造を定義することに特化しています。

```ts
interface User {
  id: number;
  name: string;
}
```

- 宣言の結合 (Declaration Merging)
  - 同じ名前で複数回定義すると、自動的に中身が合体します。

```ts
interface User {
  name: string;
}
interface User {
  age: number;
}

// 自動的に結合される
const person: User = { name: "Alice", age: 25 };
```

- API のレスポンス、クラスの仕様定義、拡張性を残したいオブジェクト定義。

### Type

- 「型に名前を付ける」という仕組みです。

```ts
// Union 型
type Status = "active" | "inactive";
type UserID = string | number;
type Point = [number, number];
```

## Generics

- ジェネリクスは、型をパラメータとして扱う仕組みで、関数やクラスを 複数の型に対応できるようにするために使います。
- any を使うと型安全が失われてしまいますが、ジェネリクスを使えば 柔軟性を保ちながら型チェックも可能です。

```ts
function success<T>(data: T) {
	return { success: true, data }
}

// Response format (success + data) are same:
{
	"success": true,
	"data": {
		"id": 1,
		"name": "Alice"
	}
}

{
	"success": true,
	"data": {
		"id": 10,
		"title": "TypeScript Guide"
	}
}
```

- 特に API レスポンスの形式は共通ですが、 中身のデータ型が異なる場合にジェネリクスを使います。

### Extends

- extends を使ってジェネリクスに制約をつけることができます。

```ts
function printId<T extends { id: number }>(obj: T) {
  console.log(obj.id);
}
```

## async / await

- Keywords:
  - Promise
  - 可読性
  - エラーハンドリング
- Promise を 同期的な書き方で扱うための構文です。非同期処理の可読性を向上させ、エラーハンドリングを簡単にします。
- try / catch を使って非同期処理のエラーを一箇所で扱える点もメリットです。

```ts
// Old
getUser(id, (user) => {
  getOrders(user.id, (orders) => {
    res.send(orders);
  });
});

// New
const user = await getUser(id);
const orders = await getOrders(user.id);
res.send(orders);

// Old
getUser()
  .then((user) => {
    console.log(user.name);
  })
  .catch((err) => {
    console.error(err);
  });

// New
try {
  const user = await getUser();
  console.log(user.name);
} catch (err) {
  console.error(err);
}
```

## Promise

- Promise
  - Pending
  - Fulfilled
  - Rejected
- Promise は、非同期処理の結果を表すオブジェクトで、処理が 成功(resolve)するか失敗(reject)するかを将来表現します。
- Promise はチェーンでき、エラーハンドリングを一箇所でまとめられる点が callback より優れています。
- async 関数は、常に Promise を返します。
  - `return 1 → Promise.resolve(1)`

### Promise vs Callback

- callback は処理完了後に関数を呼び出す方式ですが、ネストが深くなりやすく、エラーハンドリングが複雑になります。
- Promise は then / catch によって処理をチェーンでき、エラーを一箇所で扱える点が特徴です。

```ts
// Callback
getUser(id, (err, user) => {
  if (err) {
    return handleError(err);
  }

  getOrders(user.id, (err, orders) => {
    if (err) {
      return handleError(err);
    }
    res.send(orders);
  });
});

// Promise
getUser(id)
  .then((user) => getOrders(user.id))
  .then((orders) => res.send(orders))
  .catch((err) => handleError(err));

// JS
getUser().then((user) => {
  console.log(user.name);
});

// TS
getUser().then((user: User) => {
  console.log(user.name);
});
```
