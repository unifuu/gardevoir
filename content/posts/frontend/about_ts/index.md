---
title: TypeScriptについて
date: 2025-12-22
summary: 📝 TypeScriptのメモ...
tags:
  - frontend
  - typescript
---
## TS vs JS
- TypeScriptはJavaScriptに静的型付けを追加することで、コンパイル時にエラーを検知でき、保守性と可読性が向上する。
- 特にバックエンドではAPIの入出力やDBモデルの安全性を高めるために有効。
## Interface vs Type
- 基本的なオブジェクトの構造定義には`interface`を使い、union 型が必要な場合は`type`を使う。
- 基本は`interface`を使う
	- バックエンドのAPIレスポンス、DBのモデル、クラスの定義など。
	- 「オブジェクトの構造」を定義する場合は`interface`の方がパフォーマンスが良いとされている。
- 以下の場合は`type`を使う
	- `type Status = 'active' | 'inactive'`のようなUnion型を作りたい時。
	- プリミティブ型(stringやnumber)に別名をつけたい時。
	- ReactのPropsなどで、複雑な型の合成が必要な時。
### Interface
- 「オブジェクトがどのようなプロパティを持つか」という構造を定義することに特化する。
```ts
interface User {
  id: number;
  name: string;
}
```
- 宣言の結合 (Declaration Merging)
  - 同じ名前で複数回定義すると、自動的に中身が合体する。
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
- 「型に名前を付ける」という仕組み。
```ts
// Union 型
type Status = "active" | "inactive";
type UserID = string | number;
type Point = [number, number];
```
## Generics
- ジェネリクスは、型をパラメータとして扱う仕組みで、関数やクラスを複数の型に対応できるようにするために使う。
- anyを使うと型安全が失われるが、ジェネリクスを使えば柔軟性を保ちながら型チェックも可能。
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
- 特にAPIレスポンスの形式は共通だが、 中身のデータ型が異なる場合にジェネリクスを使う。
### Extends
- extendsを使ってジェネリクスに制約をつけることができる。
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
- Promiseを同期的な書き方で扱うための構文。非同期処理の可読性を向上させ、エラーハンドリングを簡単にする。
- try / catch を使って非同期処理のエラーを一箇所で扱える点もメリット。
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
- Promiseは、非同期処理の結果を表すオブジェクトで、処理が成功(resolve)するか失敗(reject)するかを将来表現する。
- Promiseはチェーンでき、エラーハンドリングを一箇所でまとめられる点が callbackより優れている。
- async関数は、常にPromiseを返す。
  - `return 1 → Promise.resolve(1)`
### Promise vs Callback
- callbackは処理完了後に関数を呼び出す方式だが、ネストが深くなりやすく、エラーハンドリングが複雑になる。
- Promiseはthen / catchによって処理をチェーンでき、エラーを一箇所で扱える点が特徴。
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
