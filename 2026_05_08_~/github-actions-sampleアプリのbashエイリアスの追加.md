# 学習ログ：bashエイリアスの追加

**日付：** 2026-05-07

---

## やったこと

よく使うコマンドを短縮するエイリアスを `~/.bashrc` に追加した。

---

## エイリアスとは

長いコマンドに短い別名をつける仕組み。  
`.bashrc` に書いておくと、ターミナル起動時に自動で読み込まれる。

```bash
alias 短縮名='実行したいコマンド'


1 Hidden Terminal
追加した設定（~/.bashrc）
alias attend='cd ~/attendance-management-app && ./vendor/bin/sail up -d'
alias gitact='cd ~/github-actions-sample && ./vendor/bin/sail up -d'
