# ポリモーフィズム（Polymorphism）

## 単語の意味

ポリモーフィズムとは、日本語で「多態性」のこと。

**親クラスやインターフェースの型でオブジェクトを扱いながら、実際のオブジェクトに応じて処理を切り替える仕組み。**

一言でいうと、

> 同じメソッドを呼び出しても、実体によって動作が変わる。

---

## 本質理解

例えば、犬と猫はどちらも動物です。

```text
Animal
  ├─ Dog
  └─ Cat
```

どちらも`Animal`型として扱えますが、実際のオブジェクトによって` speak()`の動作が変わります。

```java
class Animal {
    void speak() {
        System.out.println("動物が鳴く");
    }
}

class Dog extends Animal {
    @Override
    void speak() {
        System.out.println("ワン");
    }
}

class Cat extends Animal {
    @Override
    void speak() {
        System.out.println("ニャー");
    }
}
```

```java
Animal animal1 = new Dog();
Animal animal2 = new Cat();

animal1.speak(); // ワン
animal2.speak(); // ニャー
```

```text
animal1
  変数の型：Animal
  実際のオブジェクト：Dog
  結果：ワン

animal2
  変数の型：Animal
  実際のオブジェクト：Cat
  結果：ニャー
```

重要なのは、変数の型は`Animal`でも、実際に生成されたオブジェクトは`Dog`や`Cat`である点。

---

## なぜ必要なのか

ポリモーフィズムを使わない場合、呼び出し側で種類ごとに分岐する必要があります。

```java
if (animal instanceof Dog) {
    // 犬の処理
} else if (animal instanceof Cat) {
    // 猫の処理
}
```

動物の種類が増えるたびに、`if`文も増えてしまいます。

ポリモーフィズムを使えば、呼び出し側は共通のメソッドを呼ぶだけで済みます。

```java
animal.speak();
```

```text
呼び出し側
    │
    └─ animal.speak()
           │
           ├─ Dogなら「ワン」
           ├─ Catなら「ニャー」
           └─ Birdなら「ピヨピヨ」
```

つまり、呼び出し側は具体的な種類を意識しなくてよくなります。

---

## インターフェースとポリモーフィズム

ポリモーフィズムは、インターフェースでも利用できます。

```java
interface Payment {
    void pay();
}
```

```java
class CreditCardPayment implements Payment {
    @Override
    public void pay() {
        System.out.println("クレジットカードで支払う");
    }
}
```

```java
class CashPayment implements Payment {
    @Override
    public void pay() {
        System.out.println("現金で支払う");
    }
}
```

呼び出し側では、`Payment`型として扱います。

```java
Payment payment = new CreditCardPayment();
payment.pay();
```

結果：

```text
クレジットカードで支払う
```

```java
Payment payment = new CashPayment();
payment.pay();
```

結果：

```text
現金で支払う
```

```text
Payment
   ↑
   ├─ CreditCardPayment
   └─ CashPayment
```

---

## メソッドの共通化

支払い処理を共通化すると、次のように書けます。

```java
void executePayment(Payment payment) {
    payment.pay();
}
```

```java
executePayment(new CreditCardPayment());
executePayment(new CashPayment());
```

同じ`executePayment()`に渡しているのに、実際の支払い方法に応じて処理が変わります。

```text
executePayment()
       │
       └─ Payment.pay()
              │
              ├─ CreditCardPayment → カード決済
              └─ CashPayment       → 現金決済
```

---

## オーバーライドとの関係

ポリモーフィズムは、主にオーバーライドと組み合わせて実現します。

```text
親クラス・インターフェース
        │
        └─ 共通メソッドを定義
                 │
                 ↓
子クラスがオーバーライド
                 │
                 ↓
親の型でメソッドを呼び出す
                 │
                 ↓
実際のオブジェクトの処理が実行される
```

### 違い

| 用語 | 意味 |
|---|---|
| オーバーライド | 子クラスが親クラスのメソッドを再定義すること |
| ポリモーフィズム | 親の型で呼び出しても、実体に応じた処理が実行される仕組み |

オーバーライドは、ポリモーフィズムを実現するための重要な要素です。

---

## Laravel・PHPの経験との比較

例えば、支払い方法ごとにクラスを分けます。

```text
PaymentInterface
    │
    ├─ CreditCardPayment
    ├─ BankPayment
    └─ CashPayment
```

サービス側は、具体的な支払い方法ではなく、`PaymentInterface`に依存します。

```text
Service
   │
   └─ PaymentInterface.pay()
          │
          ├─ クレジットカード決済
          ├─ 銀行振込
          └─ 現金決済
```

これにより、サービス側で次のような分岐を大量に書かずに済みます。

```text
if 支払い方法がカード
if 支払い方法が銀行振込
if 支払い方法が現金
```

新しい支払い方法を追加する場合も、`Payment`を実装したクラスを追加しやすくなります。

---

## ポリモーフィズムのメリット

- 呼び出し側の処理を共通化できる
- `if`や`switch`による種類ごとの分岐を減らせる
- 新しいクラスを追加しやすい
- 具体的なクラスへの依存を減らせる
- 保守性・拡張性が高くなる

---

## 覚え方

```text
extends / implements
        ↓
共通の型を作る
        ↓
子クラスが処理を実装する
        ↓
共通の型で呼び出す
        ↓
実体に応じて動作が変わる
```

ポリモーフィズムの本質は、

> 「何者か」ではなく、「何ができるか」に対して処理を書くこと。

です。


