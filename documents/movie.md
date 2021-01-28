# Movie のコントローラーとモデルを作成

作業に入る前に，ブランチを切っておきましょう。ターミナルで以下のコマンドを実行して下さい。

```zsh
git switch -c feature/movie-settings
```

​
なお，`git switch -c` は `git checkout -b` と同じです。
​
`checkout` に 2 種類の役割があったため，Git のバージョン 2.23 から `git switch` が導入されました。どちらを使用しても問題はありません。

### モデルの作成

まず，`Movie モデル` と `movies テーブル` を作成していきましょう。

今回はメッセージの投稿アプリということで，「タイトル（`title`）」と「内容（`content`）」の 2 つのカラムを用意することにします。「内容」の方は長文になる可能性を踏まえ `text` 型にしておきます。

ターミナルから以下を実行して下さい。（モデルを作成するコマンドは `Movie` のように `単数形` です。小文字 `movie` でも問題はありません）

```zsh
rails g model Movie title:string content:text
```

作成された 2 つのファイルを確認しておきましょう。

- `app/models/movie.rb`（モデル・確認のみ）

```rb
class Movie < ApplicationRecord
end
```

- `db/migrate/年月日時_create_movies.rb`(マイグレーションファイル・確認のみ)

```rb
class CreateMovies < ActiveRecord::Migration[6.0]
  def change
    create_table :movies do |t|
      t.string :title
      t.text :content

      t.timestamps
    end
  end
end
```

ここで 1 点注意があります。

`rails g model` コマンドでは，`テーブル` は作成されていません。

ターミナルから次のコマンドで `マイグレーション` を実行することで，`マイグレーションファイル` が実行され，データベースにテーブルが作成されます。

（本来，マイグレーションの前にカラムに制約を入れる必要がありますが，今回はそのままとします）

```zsh
rails db:migrate
```

マイグレーション後は，必ず `db/schema.rb` を確認しましょう。現在のテーブル一覧を確認できます。

- `db/schema.rb`(確認のみ)

```rb
# 略

  create_table "movies", force: :cascade do |t|
    t.string "title"
    t.text "content"
    t.datetime "created_at", precision: 6, null: false
    t.datetime "updated_at", precision: 6, null: false
  end
```

確認ができたら，ターミナルから以下を実行してコミットをしましょう。

```zsh
git add .
git commit -m "Movie モデルと movies テーブルを作成"
```

### コントローラの作成

それでは，次のコマンドで，`コントローラ` と `ビュー` のファイルを作成しましょう。

ターミナルから以下を実行して下さい。コントローラを作成する際の `movies` は原則 `複数形` にしなければならないので要注意です。

```zsh
rails g controller movies index show new create edit update destroy
```

作成されたファイルの内，次の 3 つは使用しませんので削除しておきましょう。

- app/views/movies/create.html.erb
- app/views/movies/destroy.html.erb
- app/views/movies/update.html.erb

なお，次のコマンドで削除することも可能です。

```zsh
rm -f app/views/movies/create.html.erb app/views/movies/destroy.html.erb app/views/movies/update.html.erb
```

これで最低限度の設定は完了ですので，GitHub にプッシュし，プルリクを出し，マージして，master ブランチをプルしておきましょう。

```zsh
git add .
git commit -m "moviesコントローラとビューファイルを作成"

git push origin HEAD

# GitHubでプルリク
# GitHubでコードの差分を確認
# Slackのチームチャンネルでレビュー依頼を出す
# （メンバーとメンターから Approve コメントを受けてから）GitHubでマージ


git switch master
git pull origin HEAD
```

なお，GitHub にプルリクを出す際は，次のように `マークダウン` を使用すると見やすくなります。

```
## 概要

- Movieのおすすめ投稿機能に必要なファイルを作成

### 内容

- Movieの「モデル」「コントローラ」「ビュー」のファイルを作成
```
