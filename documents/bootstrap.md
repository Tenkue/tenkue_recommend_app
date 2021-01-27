## Bootstrap でスタイルを適用

最後にスタイルを適用し，見た目もよりよい状態にしていきましょう。

ここでは，簡単に実装するため `Bootstrap` を利用します。

作業に入る前に，ブランチを切っておきましょう。

ターミナルで以下のコマンドを実行して下さい。
（担当タスクに応じたものを 1 つだけ選んで実行して下さい）

```zsh
git switch -c feature/apply-bootstrap-book-style
git switch -c feature/apply-bootstrap-music-style
git switch -c feature/apply-bootstrap-movie-style
```

おおよそ次の手順になりますが，まずは自分で実装してみることをお勧めします。

- 各ページを中央寄せにする
- フォームは Bootstrap の [Forms](https://getbootstrap.jp/docs/4.5/components/forms/) を利用
- 詳細ページは Bootstrap の [Cards](https://getbootstrap.jp/docs/4.5/components/card/) を利用
- 一覧ページは Bootstrap の [Tables](https://getbootstrap.jp/docs/4.5/content/tables/) を利用
- `link_to` の部分は，Bootstrap の [Buttons](https://getbootstrap.jp/docs/4.5/components/buttons/) を利用

### 新規投稿機能の Bootstrap の フォーム を利用

【参考】[Bootstrap Forms](https://getbootstrap.jp/docs/4.5/components/forms/)

`form-group`, `form-control` を適切に入れるだけでフォームのスタイルが整います。

- `app/views/shops/new.html.erb`

```erb
<h1 class="text-center">新規投稿</h1>
<%= form_with model: @shop, local: true do |form| %>
  <div class="form-group">
    <%= form.label "名前" %>
    <%= form.text_field :name, class: "form-control" %>
  </div>
  <div class="form-group">
    <%= form.label "説明" %>
    <%= form.text_area :description, class: "form-control", rows: 10 %>
  </div>
  <div>
    <%= form.submit "送信", class: "btn btn-primary btn-block" %>
  </div>
<% end %>
<%= link_to "戻る", shops_path, class: "btn btn-secondary mt-3" %>
```

ボタンに `btn-block` を入れることで横長になりますが，これは好み次第でしょう。

- `app/views/posts/edit.html.erb`

新規投稿ページと同様に, 編集ページでもフォームのスタイルを修正して下さい。

```zsh
git add .
git commit -m "フォームに Bootstrap でスタイルを追加"
```

### 詳細表示で Bootstrap の カード を利用

【参考】[Bootstrap Cards](https://getbootstrap.jp/docs/4.5/components/card/)

詳細表示ページは，今回は Bootstrap の `カード` を使用することにします。

- `app/views/shops/show.html.erb`

```erb
<h1 class="text-center">投稿詳細</h1>
<section class="card border-dark mt-5">
  <div class="card-header">
    <h4><%= @shop.name %></h4>
  </div>
  <div class="card-body">
    <p class="card-text"><%= simple_format(h @shop.description) %></p>
  </div>
</section>
<%= link_to "戻る", shops_path, class: "btn btn-secondary mt-3" %>
```

```zsh
# add, commitを実行
# コミットメッセージは各自考えましょう。
```

### 一覧表示で Bootstrap の テーブル を利用

【参考】[Bootstrap Tables](https://getbootstrap.jp/docs/4.5/content/tables/)

`<table>` タグのクラスに `table` を付けるだけで最低限度のスタイルが付きます。以下では，枠を付けるために `table-bordered`，背景色をストライプにするため `table-striped` を付けています。

- `app/views/posts/index.html.erb`

```erb
<h1 class="text-center">投稿一覧</h1>
<table class="table table-bordered table-striped mt-4">
  <thead class="table-primary">
    <tr>
      <th scope="col">No.</th>
      <th scope="col" class="w-100">タイトル</th>
      <th scope="col"></th>
    </tr>
  </thead>
  <tbody>
    <% @shops.each.with_index(1) do |shop, i| %>
      <tr>
        <th scope="row" class="text-right"><%= i %></th>
        <td class="break-word"><%= shop.name %></td>
        <td class="text-nowrap">
          <%= link_to "詳細", shop, class: "btn btn-primary" %>
          <%= link_to "編集",  edit_shop_path(shop), class: "btn btn-success" %>
          <%= link_to "削除", shop, method: :delete, data: { confirm: "削除しますか?" }, class: "btn btn-danger" %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>
<div class="new-post-btn">
  <%= link_to "新規投稿", new_shop_path, class: "btn btn-info btn-block" %>
</div>
```

次のような設定を入れています。

- ヘッダーの背景を水色に設定
- タイトル部分をなるべく広く取るために `w-100` を入れる
- ボタンの文字が改行されないよう `text-nowrap` を追加
- 番号を右寄せにするため `text-right` を追加
- 上下中央にするため `vertical-align` を `middle` に設定

```zsh
# add, commitを実行
# コミットメッセージは各自考えましょう。
```

Bootstrap でスタイルを適用したら, オススメ投稿アプリが完成です！

GitHub にプッシュし，プルリクを出し，マージして，master ブランチをプルしておきましょう。
お疲れ様でした！

```zsh
git push origin HEAD

# GitHubでプルリク
# GitHubでコードの差分を確認
# 共同開発の場合はここでレビュー依頼を出す
# （共同開発の場合はLGTMを受けてから）GitHubでマージ

git switch master
git pull origin HEAD
```

---
