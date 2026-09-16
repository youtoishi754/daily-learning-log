````md id="38417"
# インターフェース

## 単語の意味

クラスが「何ができるか」というルールを定義するもの。

インターフェース自身は基本的に具体的な処理を持たず、実装するクラスに処理を任せる。

```java
interface Animal {
    void speak();
}
```

```java
class Dog implements Animal {
    @Override
    public void speak() {
        System.out.println("ワン");
    }
}
```

---

## 本質理解

インターフェースは、

> 「このインターフェースを実装するなら、このメソッドを持ってください」

という**契約・ルール**。

```text
Animal interface
      │
      │ implements
      ↓
    Dog
      │
      └─ speak()を実装
```

### extendsとの違い

```text
extends
→ クラスを継承する

implements
→ インターフェースのルールを実装する
```

インターフェースは「何者か」よりも、**「何ができるか」**を表すのに向いている。

例えば、

```text
Payment
  ↓
「支払いができる」

CreditCardPayment
  ↓
カードで支払える

CashPayment
  ↓
現金で支払える
```

---

# コレクション

## 単語の意味

複数のデータをまとめて管理するための仕組み。

Javaでは、用途に応じて`List`、`Set`、`Map`などを使う。

---

## 本質理解

配列も複数のデータを扱えるが、コレクションは**用途に応じたデータ構造を選べる**。

```text
コレクション
│
├─ List
│   └─ 順番を持つ・重複OK
│
├─ Set
│   └─ 重複を許さない
│
└─ Map
    └─ キーと値で管理
```

### List

順番を持ったデータの集まり。

```java
List<String> names = new ArrayList<>();

names.add("田中");
names.add("佐藤");
names.add("鈴木");
```

```text
[田中, 佐藤, 鈴木]
  0    1    2
```

同じ値を複数入れることもできる。

```text
[田中, 佐藤, 田中]
```

---

### Set

重複を許さないデータの集まり。

```java
Set<String> names = new HashSet<>();

names.add("田中");
names.add("佐藤");
names.add("田中");
```

```text
[田中, 佐藤]
```

`田中`は2回追加しても1つだけ。

---

### Map

キーと値をセットで管理する。

```java
Map<Integer, String> users = new HashMap<>();

users.put(1, "田中");
users.put(2, "佐藤");
```

```text
1 → 田中
2 → 佐藤
```

PHPの連想配列にかなり近い。

```php
$users = [
    1 => "田中",
    2 => "佐藤"
];
```

Javaでは、キーと値の型を明確に指定できる。

```java
Map<Integer, String>
```

```text
キー → Integer
値   → String
```

---

# インターフェースとコレクションの関係

Javaでは、

```java
List<String> names = new ArrayList<>();
```

のように書くことが多い。

ここが重要。

```text
List<String>
    ↑
インターフェース

ArrayList<>
    ↑
実際のクラス
```

つまり、

```text
List
 ↑
「リストとして扱えるもの」というルール

ArrayList
 ↑
そのルールを実装した具体的なクラス
```

そのため、

```java
List<String> names = new ArrayList<>();
```

と書ける。

これはポリモーフィズムともつながっている。

```text
インターフェース
      ↓
List
      ↓
実装クラス
      ↓
ArrayList
```

**「具体的な実装ではなく、インターフェースを通して扱う」**

というJavaの設計思想が、コレクションでも登場する。

---

# ジェネリクスとの関係

```java
List<String>
```

の`<String>`がジェネリクス。

```text
List<String>
    │   │
    │   └─ 扱うデータ型
    │
    └─ コレクションの種類
```

例えば、

```java
List<String>
List<Integer>
List<User>
```

のように、**何を入れるListなのかを型として指定できる**。

---

# 3つをまとめる

```text
インターフェース
→ 「こういうことができます」というルール

コレクション
→ 複数のデータを管理する仕組み

ジェネリクス
→ 「このコレクションにはこの型を入れる」と指定する仕組み
```

実際のコードでは、

```java
List<User> users = new ArrayList<>();
```

となり、

```text
List
 ↓
コレクションのルール

<User>
 ↓
User型を扱う

ArrayList
 ↓
Listを実装した具体的なクラス
```

という3つの概念が同時に登場する。




