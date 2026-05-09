はい、その理解は正確です。

## なぜ毎回インストールが必要なのか

GitHub Actions は **毎回まっさらな仮想マシン（コンテナ）を起動**します。前回の実行結果は一切残りません。

```
1回目の push
  └─ 新しい Ubuntu 起動 → composer install → テスト → マシン破棄

2回目の push
  └─ また新しい Ubuntu 起動 → また composer install → テスト → マシン破棄
```

なので毎回 `composer install` や `.env` の作成が必要になります。

---

## ローカル（Docker）との違い

| | ローカル（Laravel Sail） | GitHub Actions |
|---|---|---|
| 環境の寿命 | `sail up` している間は維持される | 1回の実行ごとに破棄 |
| `composer install` | 最初の1回だけ | **毎回必要** |
| `.env` | 手元に残り続ける | **毎回作り直す** |
| 目的 | 開発作業 | テストの自動実行 |

---

## 毎回やるのが遅くない？

`composer install` は時間がかかるので、実務では **キャッシュ機能**を使って2回目以降を高速化します。

```yaml
# こういう設定を追加するとキャッシュが効く（今は不要）
- uses: actions/cache@v4
  with:
    path: vendor/
    key: ${{ runner.os }}-composer-${{ hashFiles('composer.lock') }}
```

今の段階では不要ですが、CI に慣れてきたら導入するとよいです。
