---
layout: posts
title:  "Typescriptにおけるクラス"
---


## クラス宣言とnew構文
### クラス宣言

```typescript
class User {
    name:string = "";
    age: number =0;
}
```
typescriptではクラス宣言にプロパティの初期化は必須である。
### インスタンス生成
```typescript
const takeda = new User("takeda", 20)
```

### メソッドの宣言
```typescript
class User {
    name: string = "";
    age: number = 0;

    isAdult(name:string, age:number):boolean {
        return this.age >= 20; // true
    }
}
```

### コンストラクタ
```typescript
class User {
    name: string;
    age: number;

    constructor(name: string, age: numbe) {
        this.name = name;
        this.age = age;
    }
}
```

```typescript
const takeda = new User("takeda", 20);
```
クラスの宣言ではプロパティの初期化は必須であったが、コンストラクタを定義するのであれば初期化は必要ない、なぜなら、コンストラクタは、インスタンスの生成中に実行され、インスタンスの生成終了時にはぷrパティの値として格納されているからであり、実質的には初期化をおこなったのと変わらない。
### 静的プロパティ、動的プロパティ