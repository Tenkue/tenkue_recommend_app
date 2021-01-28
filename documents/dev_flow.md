## 開発の流れ

## 1. 開発環境構築

共同開発で作成するアプリは, 「Book, Music, Movie」の 3 つの CRUD で実装出来ます。

メンバー 3 名で 1 つずつ担当していただきます。

各メンバーは，合計３回プルリクを出し，メンバーとメンターからレビューを受け，マージしていただきます。

1. モデルとコントローラーを作成
2. CRUD 処理を実装
3. Bootstrap でスタイルを追加

まずは, 以下の手順で準備を行って下さい。

- ターミナルを開き, チーム開発用のアプリを入れたいディレクトリまで `cd` コマンドで移動
  - 特にこだわりがなければ次を実行すると良いでしょう。

```zsh
mkdir ~/rails
cd ~/rails
```

- この GitHub リポジトリをクローン
  - 必ずリーダーがフォークしたリポジトリをクローンすること！

```zsh
git clone リポジトリURL
```

- 作成された Rails アプリのディレクトリに `cd` コマンドで移動

- 次の 3 コマンドを順番に実行

```zsh
bundle install
yarn install --check-files
rails db:create
```

これで準備が完了です！

## 2. モデルとコントローラーを作成

CRUD 処理を実装する前にモデルとコントローラーを作成します。

各自担当のタスクに応じた教材を参考に, モデルとコントローラーを作成して下さい。

- [Book の開発手順](/documents/book.md)
- [Music の開発手順](/documents/music.md)
- [Movie の開発手順](/documents/movie.md)

実装が完了したら Github にプッシュしてプルリクを作成して下さい。（`5. プッシュ・プルリクの流れ` を参照）

## 3. CRUD 処理を実装

実際にオススメ投稿機能を実装していきます。

下記の教材を参考に, CRUD 処理を実装して下さい。
教材では Shop のオススメ投稿機能を実装していますが, 各自担当のタスクに置き換えて CRUD 処理を全て実装して下さい。

作業に入る前に，ブランチを切っておきましょう。

ターミナルで以下のコマンドを実行して下さい。
（担当タスクに応じたものを 1 つだけ選んで実行して下さい）

```zsh
git switch -c feature/crud-for-books
git switch -c feature/crud-for-musics
git switch -c feature/crud-for-movies
```

- [Shop の開発手順](/documents/shop.md)

- Github にプッシュする前に Rails サーバーを起動して, 動作確認を行いましょう。
- CRUD 処理の実装が完了したら Github にプッシュしてプルリクを作成して下さい。

## 4. Bootstrap でスタイルを追加

Bootstrap を利用してスタイルを追加していきます。（すでにアプリには Bootstrap が導入済みです）

下記の教材を参考に各ページにスタイルを実装して下さい。

- [Bootstrap](/documents/bootstrap.md)

- Bootstrap でスタイルの追加が完了したら, Github にプッシュしてプルリクを作成して下さい。

## 5. プッシュ・プルリクの流れ

- GitHub にプッシュ

```zsh
git push origin HEAD
```

- プルリクを出す

  - プルリクを出す際，「タイトル」と「内容」を書くようにして下さい。日本語で OK です。
  - 「内容」ではサンプル定型文に沿って記載して下さい。Markdown 形式で記述できます。不要な文言は削除して下さい。
  - コードの差分（変更内容）はプルリクの `Files changed` タブで確認できます。何を実装したかが伝わりやすいようにまとめましょう。
  - 「レビューをお願いします」などの文言を GitHub に書く必要はありません

- Slack にレビュー依頼のメッセージを投稿
  - 必ず，プルリクの URL を貼るようにして下さい
  - URL は `Conversation` の方を貼り付けて下さい。差分 `Files changed` の方を貼らないようにして下さい。

【注意】「コンフリクト」が出ていないか必ず確認をして下さい。コンフリクトが出ている場合は，解消をしてからレビュー依頼して下さい。

プルリクのページの一番下に次のような表示が出ていれば，コンフリクトが起きています。

```
This branch has conflicts that must be resolved

Conflicting files

config/routes.rb
db/schema.rb
```

## 6. コンフリクトの解消手順

コンフリクトについて復習されたい場合は [こちら](https://github.com/Tenkue/git_teaching_material#31-%E3%82%B3%E3%83%B3%E3%83%95%E3%83%AA%E3%82%AF%E3%83%88%E3%81%A8%E3%81%AF) をご覧下さい。

### 6.1 コンフリクトが起きているかどうかの確認

チーム開発（基礎）でコンフリクトが起こる可能性があるのは，次の 2 つのファイルです。

- `config/routes.rb`
- `db/schema.rb`

【注意】コンフリクトの解消は，**GitHub から行わないで下さい**。以下の手順で解消して下さい。

### 6.2 ローカルで master ブランチをマージ

（コミットがまだであれば，先に行ってください）

```zsh
# 現在のブランチが作業ブランチになっているか確認

git branch

# フェッチ（リモートブランチを取り込む）

git fetch

# リモートの master ブランチをマージ

git merge origin/master
```

### 6.3 コンフリクトの確認

`git status` を実行した際に， `both added: ファイル名`と書かれているファイルがコンフリクトしています

コンフリクトが起きているファイルをエディタで開きましょう。

`<<<<<<<`, `=======`, `>>>>>>>`が追加されている箇所でコンフリクトしています。

master ブランチをマージした場合は，`======`の下側が元の内容です。それに自分の変更を加えるのが原則です。

（単純に「両方残せばよい」「片方を削ればよい」という話ではありません）

### 6.6 コンフリクトの解消

- `config/routes.rb`

両方残すように修正しましょう。`<<<<<<<`, `=======` などが含まれる行は削除しましょう。

- `db/schema.rb`

このファイルは，マイグレーションを行った際に自動的に編集されるファイルです。エディタで直接編集するのは望ましくありません。コマンドで対応しましょう。

```
# データベースを作成し直し，マイグレーションを実行
# データベースのデータが全て消えますので，一般にはマイグレーションを理解した対応が必要です
rails db:migrate:reset
```

### 6.6 マージを完了させる

- `git diff` で差分を確認

  - `<<<<<<<`, `=======`などが残っていないか確認

- サーバーを起動して動作確認を行う

- コンフリクトの解消後，コミットを行う必要があります。その後，プッシュしましょう。

```
git add .
git commit

# vimが起動します。コミットメッセージはデフォルトでよいので，shiftキーを押しながら「z」を2回

git push origin HEAD
```

## 7. レビュー後の修正手順

- 該当箇所をエディタで修正

- コミット・プッシュ

  - 何をどう修正したのかが分かるようにコミットメッセージを書きましょう。「修正」のようないい加減なメッセージを書かないこと。

- Slack にレビュー依頼のメッセージを投稿
  - 必ず，プルリクの URL を貼るようにして下さい
  - 前回のプルリクをそのまま使用できます。プルリクを新しく出さないで下さい。

## 8. アプリ完成後

全員のタスクが完了し，アプリが完成したらマージして動作確認をしましょう。

```zsh
git checkout master
git pull origin HEAD
rails db:migrate
rails s
```

これで共同開発は完了です。お疲れさまでした！