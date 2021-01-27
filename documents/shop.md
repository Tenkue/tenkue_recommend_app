# CRUD 処理の見本

共同開発で各自に実装していただく、オススメ投稿機能の CRUD 処理について解説します。

## CRUD 処理

### ルーティングの設定

最初にルーティングを設定しておきましょう。

- `config/routes.rb`

```
Rails.application.routes.draw do
  # ***** 次を追加 *****
  resources :shops
end
```

`resources :shops` で作成されるルーティングは、ターミナルから `rails routes` を実行すれば確認ができます。

ただ，これをそのまま実行すると他のルーティングがたくさん含まれてしまいます。例えば次を実行すれば `shops` が含まれる行のみを表示できます。

```zsh
rails routes | grep shops
```

| Prefix    | HTTP メソッド | URL             | Controller#Action |
| --------- | ------------- | --------------- | ----------------- |
| shops     | GET           | /shops          | shops#index       |
| 〃        | POST          | /shops          | shops#create      |
| new_shop  | GET           | /shops/new      | shops#new         |
| edit_shop | GET           | /shops/:id/edit | shops#edit        |
| shop      | GET           | /shops/:id      | shops#show        |
| 〃        | PATCH         | /shops/:id      | shops#update      |
| 〃        | PUT           | /shops/:id      | shops#update      |
| 〃        | DELETE        | /shops/:id      | shops#destroy     |

ルーティングを確認したら，コミットしておきましょう。

```zsh
git add .
git commit -m "ルーティングを作成"
```

### 新規投稿(new, create)

それでは，新規投稿機能を作成していきましょう。

ルーティングは全て設定済みですので，コントローラとビューを順番に作成していきましょう。

- `app/controllers/shops_controller.rb`

```rb
class ShopsController < ApplicationController
  # 略

  def new
  # ***** 以下を追加 *****
    @shop = Shop.new
  # ***** 以上を追加 *****
  end

  def create
  # ***** 以下を追加 *****
    shop = Shop.create!(shop_params)
    redirect_to shop
  # ***** 以上を追加 *****
  end

  # 略

  # ***** 以下を追加 *****
  private

  def shop_params
    params.require(:shop).permit(:name, :description)
  end
  # ***** 以上を追加 *****
end
```

`create` アクションで， `redirect_to shop` が出てきていますが，この書き方で `投稿詳細ページ` にリダイレクトさせることができます。

具体的には，以下は全て同じです。

- `redirect_to` の書き方（全て同じ）

```rb
redirect_to shop
redirect_to "/shops/#{shop.id}"
redirect_to action: :show, id: shop.id
redirect_to shop_path(shop.id)
redirect_to shop_path(shop)
```

（`shop_params` の記法の解説は省略します。「ストロングパラメータ rails」で検索して下さい）

次にビューファイルを作成しましょう。新規投稿後は「詳細ページ（`show.html.erb`）」にリダイレクトさせますので，`create.html.erb` は不要です。

- `app/views/shops/new.html.erb`

```erb
<h1>新規投稿</h1>
<%= form_with model: @shop, local: true do |form| %>
  <div>
    <%= form.label :name, "名前" %>
    <%= form.text_field :name %>
  </div>
  <div>
    <%= form.label :description, "説明" %>
    <%= form.text_area :description %>
  </div>
  <div>
    <%= form.submit "送信" %>
  </div>
<% end %>
<%= link_to "戻る", shops_path %>
```

`form_with model: @shop` で「create アクションに対応する URL」を「投稿先の URL」に設定できます。

`@shop` の中身は `Shop.new` ですので，ここから Rails が自動で決定しています。

なお，古い記事で `form_for` や `form_tag` を見かけることがあると思いますが，Rails 5.1 から`form_with`に統合されていますので原則こちらを使用していくようにしましょう。

また, ここで `link_to` ヘルパーメソッドが出てきましたが，以下は全て同じものです。

- `link_to` の書き方（全て同じ）

```erb
<%= link_to "投稿一覧", shops_path %>
<%= link_to "投稿一覧", "/shops" %>
<a href="/shops">投稿一覧</a>
```

HTML ではリンクは `<a>` タグを使用しますが，Rails ではよく `link_to` を使用されます。「ページのソース」を確認すると明らかなように，中身は通常の `a` タグです。

URL を用いた書き方もできますが， `link_to` の場合は `rails routes` で表示される `Prefix` を利用したパスで表すことができます。通常，こちらの記法が使用されます。

| Prefix   | HTTP メソッド | URL        | Controller#Action |
| -------- | ------------- | ---------- | ----------------- |
| shops    | GET           | /shops     | shops#index       |
| new_shop | GET           | /shops/new | shops#new         |

例えば，新規投稿（`shops#new`）に対応する Prefix は `new_shop` ですので，path は `new_shop_path` です。 （ `Prefix` に `_path` を付けた形式です）

ここまで実装したらターミナルで以下のコマンドを実行してください。

```zsh
git add .
git commit -m "新規投稿機能を実装"
```

### 詳細(show)

次に，詳細表示ページを作成していきましょう。

- `app/controllers/shops_controller.rb`

```rb
class ShopsController < ApplicationController
  # 略

  def show
    # ***** 以下を追加 *****
    @shop = Shop.find(params[:id])
    # ***** 以上を追加 *****
  end

  # 略

end
```

`params[:id]` は，URL の `shops/:id` の `:id` に対応する数です。ここから，対応する投稿をデータベースから取り出し `@shop` に代入しています。

- `app/views/shops/show.html.erb`

```erb
<h1>投稿詳細</h1>
<p>名前： <%= @shop.name %></p>
<p>説明： <%= simple_format(h @shop.description) %></p>
<%= link_to "戻る", shops_path %>
```

ここで `simple_format` ヘルパーメソッドが出てきていますが, これは改行文字を含むテキストをブラウザ上で表示させる際に使われるメソッドです。
単に `@shop.description` と書いてしまうと改行が反映されないので注意してください。

また, `h` メソッド（`html_escape` メソッド）を加えることで,html タグが含まれていてもそのまま表示できます。

それでは，ターミナルで `rails s` を実行し, 動作確認をしていきましょう。

```zsh
rails s
```

サーバーを起動したら新規投稿ページにアクセスし，問題なく投稿ができるか確認して下さい。

新規投稿ページの URL: [http://localhost:3000/shops/new](http://localhost:3000/shops/new)

（URL は，`rails routes | grep shops` で確認できます）

確認ができたら，コミットをしておきましょう。

ターミナルで以下のコマンドを実行してください。

```zsh
git add .
git commit -m "詳細表示機能を実装"
```

---

#### おまけ

`shops_controller` において， `create` アクションでは変数名を `shop`, `show` アクションでは変数名を `@shop` にしています。

`@` を付けた変数はビューで使用できますが，`@`を付けていない変数はビューで使用できません。

変数に`@`を付ければ，ビューで使用する場合にも使用しない場合にも対応できますが，無駄に`@`を付けないように心がけましょう。

---

### 一覧(index)

一覧ページを作成していきましょう。一覧ページでは「名前」のみを表示する形式としておきます。

- `app/controllers/shops_controller.rb`

```rb
class ShopsController < ApplicationController
  def index
    # ***** 以下を追加 *****
    @shops = Shop.order(id: :asc)
    # ***** 以上を追加 *****
  end

  # 略

end
```

`Shop.all` を使用すると，「編集時に順番が入れ替わってしまう」問題が起きるため，id 昇順で並べるようにしておきます。

- `app/views/shops/index.html.erb`

```erb
<h1>投稿一覧</h1>
<table>
  <thead>
    <tr>
      <th scope="col">No.</th>
      <th scope="col">名前</th>
      <th scope="col"></th>
    </tr>
  </thead>
  <tbody>
    <% @shops.each.with_index(1) do |shop, i| %>
      <tr>
        <th scope="row"><%= i %></th>
        <td><%= shop.name %></td>
        <td>
          <%= link_to "詳細", shop %>
          <%= link_to "編集", edit_shop_path(shop) %>
          <%= link_to "削除", shop, method: :delete, data: { confirm: "削除しますか?" } %>
        </td>
      </tr>
    <% end %>
  </tbody>
</table>
<%= link_to "新規投稿", new_shop_path %>
```

「詳細」「編集」などのリンクが縦に並ぶように，`<table>` タグを利用しています。

1 行目のヘッダ部分は固定で，2 行目以降はデータベースから取り出した「名前（name）」などが並ぶように `each` メソッドで繰り返し処理を行っています。

さらに，`link_to` について 2 つ解説しておきます。

`<%= link_to "詳細", shop %>` が出てきていますが，これで詳細ページへのリンクを作成できます。次は全て同じです。

- `link_to` の書き方（全て同じ）

```erb
<%= link_to "詳細", shop %>
<%= link_to "詳細", shop_path(shop.id) %>
<%= link_to "詳細", shop_path(shop) %>
<%= link_to "詳細", "/shops/#{shop.id}" %>
<a href="/shops/<%= shop.id %>">詳細</a>
```

一方，削除のリンクに `method: :delete` が入っています。

| Prefix | HTTP メソッド | URL        | Controller#Action |
| ------ | ------------- | ---------- | ----------------- |
| shop   | DELETE        | /shops/:id | shops#destroy     |

`link_to` は特に指定しない限り HTTP メソッドが `GET` です。`DELETE` の場合は`method: :delete` のように追記する必要があります。

また，`data: { confirm: "削除しますか?" }` を入れることで，クリック時に確認ダイアログを表示することができます。

実際にサーバーを起動して投稿一覧を確認できたら, コミットしましょう。

```zsh
git add .
git commit -m "一覧表示機能を実装"
```

### 削除（destroy）

削除のリンクは作成済みですので，コントローラだけ追記しましょう。

- `app/controllers/shops_controller.rb`

```rb
class ShopsController < ApplicationController
  # 略

  def destroy
    # ***** 以下を追加 *****
    shop = Shop.find(params[:id])
    shop.destroy!
    redirect_to shops_path
    # ***** 以上を追加 *****
  end

  # 略

end
```

サーバーを起動し，削除ができるか確認した上でコミットしましょう。

```zsh
git add .
git commit -m "削除機能を実装"
```

### 編集（edit, update）

最後に編集機能を実装しましょう。

- `app/controllers/shops_controller.rb`

```rb
class ShopsController < ApplicationController
  # 略

  def edit
    # ***** 以下を追加 *****
    @shop = Shop.find(params[:id])
    # ***** 以上を追加 *****
  end

  def update
    # ***** 以下を追加 *****
    shop = Shop.find(params[:id])
    shop.update!(shop_params)
    redirect_to shop
    # ***** 以上を追加 *****
  end

  # 略

end
```

- `app/views/shops/edit.html.erb`

```erb
<h1>編集</h1>
<%= form_with model: @shop, local: true do |form| %>
  <div>
    <%= form.label :name, "名前" %>
    <%= form.text_field :name %>
  </div>
  <div>
    <%= form.label :description, "説明" %>
    <%= form.text_area :description %>
  </div>
  <div>
    <%= form.submit "更新" %>
  </div>
<% end %>
<%= link_to "戻る", shops_path %>
```

中身は `new.html.erb` とほぼ同じです。

今回は，`@shop` はデータベースに存在するデータですので，ここから Rails が update に対応する 「URL」「HTTP メソッド」を自動で決めているのです。

サーバーを起動し，編集ができるか確認した上でコミットしましょう。

```zsh
git add .
git commit -m "編集機能を実装"
```

これで，投稿機能を一通り実装することができました！

GitHub にプッシュし，プルリクを出し，マージして，master ブランチをプルしておきましょう。

```zsh
git push origin HEAD

# GitHubでプルリク
# GitHubでコードの差分を確認
# 共同開発の場合はここでレビュー依頼を出す
# （共同開発の場合はLGTMを受けてから）GitHubでマージ

git switch master
git pull origin HEAD
```
