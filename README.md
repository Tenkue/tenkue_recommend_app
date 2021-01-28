# 転クエ 共同開発

## 目的

共同開発での開発フロー、コミュニケーションの取り方、ツールの使い方を体験すること
​

## 成果物

- オススメ投稿アプリ
  - [完成後のサンプルアプリ](https://tenkue-recommend-app.herokuapp.com/)

## 参加要件

- Git 課題をクリアしたこと
  - https://github.com/Tenkue/git_teaching_material
- 環境構築が完了し、指定バージョンに揃えられること
  - [参加要件の環境構築確認方法](#参加要件の環境構築確認方法)
- Progate で Ruby、Rails を 1 周していること（もしくはそれと同程度のレベル感）
- お互いを尊重して丁寧なコミュニケーションを取れること
- Slack や GitHub でのレスポンスをできる限り早く行えること（どんなに遅くても 26 時間以内）

## 人数

- 1 チーム 3 名

## 環境/技術

- Ruby 2.7.2
- Rails 6.0.3.6
- bundler 2.1.6
- PostgreSQL
- 記法：erb, Sass
- ソースコード管理：GitHub

## 参加要件の環境構築確認方法

以下を実行し、トップページが表示できることを条件とさせていただきます。

- ターミナルから以下を実行

```zsh
cd ~/Desktop
git clone https://github.com/Tenkue/tenkue_recommend_app.git
cd tenkue_recommend_app
bundle install
yarn install --check-files
rails db:create
rails s
```

- ブラウザで [トップページ](http://localhost:3000/) にアクセスできることを確認

- 確認後、サーバーを停止してターミナルから以下を実行

```zsh
# control + c でサーバーを停止後
rails db:drop
cd ~/Desktop
rm -rf tenkue_recommend_app
```

## 開発全体の流れ

1. メンターが Slack のチームチャンネルを作成して招待
2. 参加メンバーで相談し、リーダー１名を決め、`Book`, `Music`, `Movie` のタスクを誰が行うかを決定
3. リーダーがこのリポジトリを [フォーク](https://docs.github.com/ja/github/getting-started-with-github/fork-a-repo)
4. フォークしたリポジトリに必要な設定を行う
   - 【参考】[Git 課題: 2.3 リポジトリの初期設定](https://github.com/Tenkue/git_teaching_material#23-%E3%83%AA%E3%83%9D%E3%82%B8%E3%83%88%E3%83%AA%E3%81%AE%E5%88%9D%E6%9C%9F%E8%A8%AD%E5%AE%9A)
   - `Required approving reviews` は `2` に設定して下さい（メンバー１名とメンターからの Approve コメントがないとマージできないようにして下さい）
5. リーダーがフォークしたリポジトリの URL をメンバーに通知
6. [開発の流れ](/documents/dev_flow.md) に従って各メンバーの実装開始
7. 実装が完了し GitHub にプッシュしたら、Slack のチームチャンネルにて、まずメンバーにレビューを依頼
8. 少なくとも１名以上のメンバーから Approve コメントを受ける（メッセージは「LGTM です！」で OK）
   プルリクチーム内でコードレビューを行い、少なくとも１名から Approve コメントを受ける
   - 【参考】[Git 課題: 2.4.5 レビュー依頼](https://github.com/Tenkue/git_teaching_material#245-%E3%83%AC%E3%83%93%E3%83%A5%E3%83%BC%E4%BE%9D%E9%A0%BC)
   - CRUD 実装とスタイル調整では、「Files changed」の確認だけでなく、ローカルで「動作確認」も行いましょう
   - 【参考】[GitHub: プルリクエストをローカルでチェック アウトする](https://docs.github.com/ja/github/collaborating-with-issues-and-pull-requests/checking-out-pull-requests-locally)
9. Slack のチームチャンネルにて、メンターにレビューを依頼
10. メンターから Approve コメントを受け次第、GitHub でマージを行う
11. 全員の実装がマージできれば、各自 master ブランチをプルして共同開発終了！

## 注意点・特記事項

- メンターのレビュー対応期間は開発開始から 2 週間以内です。それを超えたらレビューは行いません（開発自体は継続していただいて問題ありません）
- この共同開発の目的は、技術的なキャッチアップではなく「共同開発での開発フロー、コミュニケーションの取り方、ツールの使い方を体験すること」です
- 開発中は至る所で「ここってどうやるの？」ということが出てくるかと思いますが、① まず自分で調べる ② チーム内で相談する、ということを徹底してください

## よくある質問

**Q：プレミアムプラン以上でないと参加できませんか？**
A：はい。参加期間（参加希望の連絡をするタイミングから共同開発が終了するまで）はプレミアムプラン以上である必要があります。

**Q：Git の使い方など教えてくれますか？**
A：いいえ。この共同開発ではメンターからの技術的なレクチャーは行いません。  
基本的に、ご自身もしくはチーム内で学習したり調べていただき、解決していただく形になります。

**Q：どれくらい学習していれば参加できますか？**
A：[参加要件](#参加要件) をご参照ください。

## 変更履歴

| ver | date      | summary  |
| --- | --------- | -------- |
| 1.0 | 2021/1/28 | 初版作成 |
