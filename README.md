# daily-learning-log
2026/03/04
つまずいたことと解決方法
1. SourceTreeでPullが固まって進まない
状況
WindowsにインストールしたSourceTreeから \\wsl.localhost\Ubuntu\... のパスを直接指定してリポジトリを追加した。
Pull操作をするとずっとPulling状態のまま固まってしまった。
原因
SourceTreeはWindowsアプリのため、WSL内のファイルシステムにネットワークドライブ経由でアクセスする構造になる。
このWindows→WSL間の経路が不安定でGit操作が詰まった。
解決方法
WSLターミナルから code . でVSCodeを起動し、VSCodeのSource Control機能でGit管理を行うようにした。
VSCodeはWSL拡張機能によってLinux内にVSCode Serverを立てて通信するため、ファイル操作がWSL内で完結して安定する。
bashcd ~/attendance-management-app
code .
教訓
WSL + Docker環境でのGit操作は、SourceTreeではなくWSLターミナルまたはVSCodeで行うのがベストプラクティス。

2. git pull で "no tracking information" エラー
状況
VSCodeのSource ControlからPullを試みたところ、以下のエラーが発生した。
There is no tracking information for the current branch
原因
ローカルの main ブランチとリモートの origin/main が紐付いていなかった。
解決方法
以下のコマンドでブランチの上流を設定し、強制的にリモートの状態に合わせた。
bashgit branch --set-upstream-to=origin/main main
git reset --hard origin/main
教訓
git branch -a でローカル・リモートのブランチ一覧を確認する習慣をつける。
紐付けが済んでいれば以降は git pull が普通に使えるようになる。
