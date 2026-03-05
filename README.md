
#2026/03/04
## 環境構成

```
Windows
├── Windows Terminal
│   └── WSL（Ubuntu）
│       ├── Docker Engine
│       │   ├── laravel.test コンテナ
│       │   ├── mysql コンテナ
│       │   ├── redis コンテナ
│       │   └── mailpit コンテナ
│       └── attendance-management-app（ソースコード）
│
└── VSCode（Windowsアプリ）
    └── WSL拡張経由でUbuntu内のリポジトリを編集・Git管理
```

---

## つまずいたことと解決方法

### 1. SourceTreeでPullが固まって進まない

**状況**
WindowsにインストールしたSourceTreeから `\\wsl.localhost\Ubuntu\...` のパスを直接指定してリポジトリを追加した。
Pull操作をするとずっとPulling状態のまま固まってしまった。

**原因**
SourceTreeはWindowsアプリのため、WSL内のファイルシステムにネットワークドライブ経由でアクセスする構造になる。
このWindows→WSL間の経路が不安定でGit操作が詰まった。

**解決方法**
WSLターミナルから `code .` でVSCodeを起動し、VSCodeのSource Control機能でGit管理を行うようにした。
VSCodeはWSL拡張機能によってLinux内にVSCode Serverを立てて通信するため、ファイル操作がWSL内で完結して安定する。

```bash
cd ~/attendance-management-app
code .
```

**教訓**
WSL + Docker環境でのGit操作は、SourceTreeではなくWSLターミナルまたはVSCodeで行うのがベストプラクティス。

---

### 2. git pull で "no tracking information" エラー

**状況**
VSCodeのSource ControlからPullを試みたところ、以下のエラーが発生した。

```
There is no tracking information for the current branch
```

**原因**
ローカルの `main` ブランチとリモートの `origin/main` が紐付いていなかった。

**解決方法**
以下のコマンドでブランチの上流を設定し、強制的にリモートの状態に合わせた。

```bash
git branch --set-upstream-to=origin/main main
git reset --hard origin/main
```

**教訓**
`git branch -a` でローカル・リモートのブランチ一覧を確認する習慣をつける。
紐付けが済んでいれば以降は `git pull` が普通に使えるようになる。

#2026/03/04

###学習ログ：未経験エンジニアのポートフォリオ作り方まとめ

ポートフォリオが必要な理由

Webエンジニアを目指す人が増加し、書類選考の倍率が上昇
履歴書だけでは差別化できないため、技術力・企画力を可視化するポートフォリオが必須


差がつく3つのポイント
① サービス志向（マーケットイン）

「習った技術を使いたい」ではなく、ユーザーの課題解決を起点に考える
自分の好きなこと・身近な悩みをサービスのアイデア源にする

② 自走力

新しい技術を自分で調べながら実装する姿勢を示す
ただし技術ありきで機能を追加しない（あくまでユーザー中心）

③ 運用実績

作って終わりにしない。公開して実ユーザーに使ってもらう
フィードバックをもとに改善・アップデートを繰り返す
