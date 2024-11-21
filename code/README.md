#   git hubの備忘録

##  初期設定系
バージョン管理
git  -–version

•  リポジトリ情報を更新：
sudo apt update

•  Gitをアップデート：
sudo apt upgrade git

Gitの初期設定（必要なら）：
git config --global user.name "Your Name"
メールアドレス：
git config --global user.email "youremail@example.com"

(1) プロジェクトごとのローカル設定
1.	プロジェクトディレクトリに移動：
cd /path/to/your/project
2.	ローカルでユーザー名とメールを設定：
git config --local user.name "Your Name"

2. SSHキーの生成
すでにSSHキーを持っている場合はスキップできます。次のコマンドでSSHキーを作成します。
(1) SSHキーの生成コマンド
ssh-keygen -t ed25519 -C "your_email@example.com"
•  -t ed25519：現在推奨される鍵の種類（セキュリティが高い）。
•  -C "your_email@example.com"：識別のためのコメント（通常はGitHubに登録したメールアドレス）。
3. 公開鍵をGitHubに登録
1.	公開鍵を表示 公開鍵をクリップボードにコピーします：
cat ~/.ssh/id_ed25519.pub
出力例
ssh-ed25519 AAAAC3NzaC1lZDI1NTE5AAAAIB3eg... your_email@example.com
(1) SSH接続をテスト
以下のコマンドを実行してGitHubとの接続を確認します：
ssh -T git@github.com


##  リポジトリとか

### リポジトリの作成
1. GitHubリポジトリを作成
(1) GitHubで新しいリポジトリを作成
1.	GitHubにログインします。
2.	右上の「+」ボタンをクリックし、「New repository」を選択。
3.	必要事項を入力：
o	Repository name: プロジェクト名（例: my-project）。
o	Visibility: PublicまたはPrivate（自由に選択）。
4.	「Create repository」をクリック。
________________________________________
2. GitHubリポジトリとローカルプロジェクトを連携
ローカルプロジェクトをGitHubのリポジトリと連携させます。
(1) GitHubのリポジトリURLを取得
作成したリポジトリの画面に表示されるSSH形式のURLをコピーします：
git@github.com:your-username/my-project.git
(2) リモートリポジトリを登録
ローカルのプロジェクトディレクトリに移動して、リモートリポジトリを登録します：
cd /path/to/your/project
git remote add origin git@github.com:your-username/my-project.git

git remote -v でつながっているかの確認


### 操作関係
1. 必要なファイルをステージングする
追跡したいファイルやフォルダをgit addでステージングします。
•	個別にファイルを追加： 例：kojima/README.mdとds/だけを追跡したい場合：

git add kojima/README.md
git add ds/
•	一括でフォルダやファイルを追加： 必要なすべてのファイルを一括でステージングするには、次のコマンドを使います：
git add .
※ただし、追跡したくないファイルが混ざらないよう注意してください。
2. ステージング内容を確認
ステージング後に現在の状態を確認します：

git status
•	緑色で表示されるファイルが次のコミットに含まれます。
________________________________________
3. 無視したいファイルを除外
追跡したくないファイルやフォルダを.gitignoreに追加して無視します。
1.	.gitignoreの作成または編集：

nano .gitignore
以下のように、追跡したくないファイルやフォルダを記載します：
plaintext
コードをコピーする
.config/
.docker/
.dotnet/
.vscode-remote-containers/
.vscode-server/
.wget-hsts
kojima/example.zip
2.	.gitignoreを反映： .gitignoreをステージングしてコミット：
bash
コードをコピーする
git add .gitignore
git commit -m "Add .gitignore to exclude unnecessary files"

#### git branch -M main
2. git branch -M mainの役割
このコマンドは、現在のローカルブランチ名をmainに変更します。

git branch -M main を行う理由は、ローカルのデフォルトブランチ名をGitHubのデフォルトブランチ名（通常はmain）と一致させるためです。これにより、ローカルリポジトリとリモートリポジトリの間で名前の不一致による問題を防ぐことができます。

リモートリポジトリにプッシュ
git push -u origin main
•  origin：リモートリポジトリの名前。最初に登録する必要がある。
•  -u：ローカルブランチ（例：main）とリモートブランチ（例：origin/main）を対応付ける。
•  初回のみ-uを使い、以降の操作を簡略化できる。

####  git status
git statusは、現在のリポジトリの状態を確認するためのコマンド
•  表示例（変更が未ステージングの場合）：
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  modified:   example.txt
•	意味：
o	ファイルexample.txtが変更されているが、まだステージングされていない。
•  表示例（ステージング済みの変更がある場合）：
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
  new file:   example.txt
•	意味：
o	ファイルexample.txtがステージングされ、次のコミットに含まれる準備ができている。
•  追跡されていないファイルがある場合：
Untracked files:
  (use "git add <file>..." to include in what will be committed)
  new_file.txt
•	意味：
o	ファイルnew_file.txtはまだGitに登録されていない（追跡されていない）。

### ブランチ作成⇒コミットなどの流れ
####  ブランチの作成・コミット
1.	新しい作業（ブランチ）を作る：
git checkout -b feature/add-login

•  作業する → コミットする：
git add .
git commit -m "ログイン機能追加"
•  完成したら、mainに戻る：
git checkout main

•  Changes not staged for commit：
•	変更はあるけど、まだ「ステージング領域」に追加されていません。
•  提案されている操作：
•	git add <file>：
o	ファイルをステージングして、コミットに含める。
•	git restore <file>：
o	作業ディレクトリの変更を元に戻して、変更を破棄する。

変更が終わったらgit addからのgit commit -mでコミット

####  ステージングとコミット
(2) ブランチ切り替え時に変更をステージングまたはコミットする
•	保存していない変更を反映させるには：
1.	編集内容を保存。
2.	変更をステージング（git add）またはコミット（git commit）する。
git add .
git commit -m "一時保存"

git statusの役割
git statusは、以下の情報を提供します：
1.	現在のブランチ名（例: feature/2）。
2.	ステージングされているファイル（コミットされる準備が整っている）。
3.	ステージングされていない変更（作業ツリーで変更済みだがgit addされていないファイル）。
4.	追跡されていないファイル（Gitが管理していない新しいファイル）。

####  stashしてから退避
(3) 編集内容を一時退避させる（git stashを使う）
•	切り替え前に変更を一時的に退避させる：
git stash
•	他のブランチに切り替え後、退避した内容を戻す：
git stash apply

##### ブランチを移動するときの注意事項
git checkoutをcommitかstashしてから行わないと、
移動後のブランチで保存されるのでマジで注意！！！

Mergeとgit rebase


#### リモートが更新されているとき

git fetch originの動作
1. リモートリポジトリ全体を取得
originにあるすべてのリモートブランチを対象とします。
取得内容：
リモートブランチの最新のコミット情報。
ただし、ローカルの作業ブランチには影響を与えず、リモート追跡ブランチ（例: origin/main）に更新内容を保存します。

2. 特定のブランチだけ取得

git fetch origin feature/1

注意点
  1. ローカル作業ブランチには影響しない

git fetch originはリモート情報を取得するだけで、
現在の作業ブランチ（例: main）には変更を適用しません。
変更をローカルに適用するには、git mergeやgit pullが必要です。

2.  リモート追跡ブランチが更新される

取得した情報は、リモート追跡ブランチ（例: origin/main）に反映されます。

