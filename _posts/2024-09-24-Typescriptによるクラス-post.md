---
layout: posts
title:  "Typescriptにおけるクラスメモ"
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

### コンストラクタによる簡便なインスタンス生成
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
これを以下のように簡便にできる。
```typescript
class User{
    constructor(public name: string, private age: number) {}
}
```
アクセス修飾子を必ずつけないといけないことに注意。ずいぶん楽にかける。
まあでも好みの問題っぽい。かならずこちらが良いということもでもなさそう。



### 静的プロパティ、動的プロパティ
静的プロパティ・メソッドはクラスそのものに属する。
プロパティやメソッド名にstaticとつけることで実現できる。

```typescript
class User {
    static adminName: string = "admin";
    static getAdminUser() {
        return new User(User.adminName, 26);
    }

    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    isAdult():boolean {
        return this.age >= 20;
    }
}

console.log(User.adminName); //OK
const admin = User.getAdminUser();
console.log(admin.age); // 26
console.log(admin.isAdult()); // true

const uhyo = new User("uhyo", 20);
console.log(uhyo.adminName); // エラー！　
```
User.adminName等はUserクラスが持つプロパティに直接アクセス捨ているのでOK
一方、最後の2行当たりのuhyo.adminNameではuhyoインスタンスからのアクセスはできない。

### クラス宣言のアクセス修飾子
なにもかかない、今までのアクセスはpublicである。
アクセス修飾子はpublic, private, protectedからなる。
アクセス修飾子では、そのプロパティ・メソッドがどこからアクセス可能であるかを定義することができる。
```typescript
class User {
    name: string;
    private age: number;

    isAdult():boolean {
        return this.age >= 20;
    }
}
```
private修飾子がついたプロパティには直接アクセスできない。つまり、以下はエラーである。
```typescript
const takeda = new User("uhyo", 20); // OK
console.log(takeda.age) // エラー
```
これは、ageにprivate修飾子がついているので直性参照ができなくなっている。
しかし、privateなプロパティでも、メソッドからは参照できるので以下はＯＫ
```typescript
class User {
    name: string;
    private age: number;

    isAdult():boolean {
        return this.age >= 20;
    }
}

```
```typescript
const takeda = new User("uhyo", 20); // OK
console.log(takeda.iaAdult()) // OK
```
これは、メソッドがクラス定義の中で定義されているからである。
privateは定義されたクラス内のメソッド；コンストラクタからのアクセスのみ許可する。
こうすることで、その人の年齢自体はわからないが、とにかくメソッドを実行すれば、成人かどうかわかるという、内部の詳しい実装は外から隠したうえで機能を提供できる。

### クラス式によるインスタンス生成
```typescript

const takeda = class {
    name: string;
    age: number;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
    }

    public isAdult(): boolean {
        return this.age >= 20;
    }
}

```
あまり利用頻度は高くないらしい。普通のクラス宣言のほうが良いらしいが、動的にクラス生成したいときもあるだろう

### クラスに型引数を持たせる
```typescript
class User<T> {
    name: string;
    age: number;
    readonly data: T;

    constructor(name: string, age: number) {
        this.name = name;
        this.age = age;
        this.data  data;
    }
}

```
```typescript
const uhyo = new User<string?>("uhyo", 20, "追加データ");
const data = uhyo.data;
```
```typescript
const john = new User("John smith", 30, { num: 123});
const data23 = john.data;
```
インスタンスごとに違う型のデータを持たせたいときに役立つ。
2つ目の例のように、インスタンス生成の時には<string>のようなクラスの型引数を省略することがｄけ