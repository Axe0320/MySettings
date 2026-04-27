Title: Claude CodeとAntigravityを複数Macで同期させてみた｜じゃが

Description: こんにちは。ガジェットブロガーのじゃが（@jaga_farm）です。  「Mac miniで作業した続きを、外出先のMacBook Airでやりたい」って思ったこと、ありませんか？  最近、M4Pro Mac mini（自宅メイン機）とM4 MacBook Air（持ち出し用）の2台体制になったんですが、AIツールの設定や履歴がバラバラなのがずっと気になっていました。  Claude Code（Anthropicが出しているターミナルで使えるAIツール）やAntigravity（Geminiをターミナルから使えるツール）の会話履歴やスキル設定が、それぞれのMacで別々になってしまうんで

Source: https://note.com/jaga_farm/n/nefcc77a3de56

---

[投稿](https://note.com/signup)
[ログイン](https://note.com/login?redirectPath=)
[会員登録](https://note.com/signup)

[じゃが](https://note.com/jaga_farm)
こんにちは。ガジェットブロガーのじゃが（[@jaga_farm](https://x.com/jaga_farm)）です。
「Mac miniで作業した続きを、外出先のMacBook Airでやりたい」って思ったこと、ありませんか？
最近、M4Pro Mac mini（自宅メイン機）とM4 MacBook Air（持ち出し用）の2台体制になったんですが、AIツールの設定や履歴がバラバラなのがずっと気になっていました。
Claude Code（Anthropicが出しているターミナルで使えるAIツール）やAntigravity（Geminiをターミナルから使えるツール）の会話履歴やスキル設定が、それぞれのMacで別々になってしまうんですよね。解決策として母艦である自宅Mac miniにSSHやVNCで乗り込んでいましたが、それすら面倒にw
ということで今回、iCloudとシンボリックリンク（ショートカットのようなもの）を使って、この問題を解決したので共有します。
【注意事項】複数MACに同期することで予期できないトラブルが起こる可能性があります。実施は自己責任でお願いいたします。

仕組みはシンプルです。
1. Claude CodeやAntigravityの設定フォルダ（~/.claudeや~/.agent）をiCloud Drive内に移動
2. 各Macからシンボリックリンクで参照
Claude CodeやAntigravityの設定フォルダ（~/.claudeや~/.agent）をiCloud Drive内に移動
各Macからシンボリックリンクで参照
具体的にはこんな構成になっています：

```
~/.claude → iCloud Drive/XXX/.claude ~/.agent → iCloud Drive/XXX/.agent
```


```
~/.claude → iCloud Drive/XXX/.claude ~/.agent → iCloud Drive/XXX/.agent
```

シンボリックリンクというのは、Windowsでいうショートカットみたいなもの。本体ファイルは1箇所（iCloud Drive内）にあって、複数のMacから「ここにあるよ」と参照できるイメージですね。

実際の設定手順はこんな感じです。ターミナル（Macに標準で入っているコマンドを打つアプリ）で以下を実行します。
1. 既存フォルダをiCloud Driveに移動（最初の1台目のみ）

```
# Claude Codeの設定フォルダを移動 mkdir -p ~/.gemini/antigravity mv ~/.claude ~/Library/Mobile\ Documents/com~apple~CloudDocs/.claude # Antigravityのスキル（ワークフロー）フォルダを移動 mv ~/.agent ~/Library/Mobile\ Documents/com~apple~CloudDocs/.agent # Antigravityの会話履歴・記憶データを移動 mkdir -p ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity mv ~/.gemini/antigravity/conversations ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/ mv ~/.gemini/antigravity/brain ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/ mv ~/.gemini/antigravity/mcp_config.json ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/
```


```
# Claude Codeの設定フォルダを移動 mkdir -p ~/.gemini/antigravity mv ~/.claude ~/Library/Mobile\ Documents/com~apple~CloudDocs/.claude # Antigravityのスキル（ワークフロー）フォルダを移動 mv ~/.agent ~/Library/Mobile\ Documents/com~apple~CloudDocs/.agent # Antigravityの会話履歴・記憶データを移動 mkdir -p ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity mv ~/.gemini/antigravity/conversations ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/ mv ~/.gemini/antigravity/brain ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/ mv ~/.gemini/antigravity/mcp_config.json ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/
```

※この例ではiCloud Drive直下に配置しています。別のフォルダにまとめたい場合、「com~apple~CloudDocs/MySettings/.claude」 のようにパスを変更してください。
2. シンボリックリンクを作成（全てのMacで実行）

```
# Claude Code用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.claude ~/.claude # Antigravity スキル用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.agent ~/.agent # Antigravity 会話履歴・記憶データ用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/conversations ~/.gemini/antigravity/conversations ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/brain ~/.gemini/antigravity/brain ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/mcp_config.json ~/.gemini/antigravity/mcp_config.json
```


```
# Claude Code用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.claude ~/.claude # Antigravity スキル用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.agent ~/.agent # Antigravity 会話履歴・記憶データ用 ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/conversations ~/.gemini/antigravity/conversations ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/brain ~/.gemini/antigravity/brain ln -s ~/Library/Mobile\ Documents/com~apple~CloudDocs/.gemini/antigravity/mcp_config.json ~/.gemini/antigravity/mcp_config.json
```

ln -sがシンボリックリンクを作るコマンドです。これで~/.claudeにアクセスすると、実際にはiCloud Drive内のフォルダを見に行くようになります。
Antigravityの注意点: ~/.gemini/antigravity/installation_idは端末固有のIDなので同期しないでください。ライセンスや認証で問題が起きる可能性があります。上記のコマンドではconversations、brain、mcp_config.jsonのみを同期対象にしています。
2台目以降のMacでは、手順2だけ実行すればOKです。※既存フォルダーがある場合はmvに変わるので注意。iCloud経由で設定フォルダが同期されているので、リンクを張るだけで同じ環境が使えるようになります。

### 実際に同期されたもの
実際に同期されていたものをまとめていきます。これらは執筆時（2025/12）の情報です。
~/.claudeフォルダには以下のファイルが含まれていて、これらが全て同期されます。環境によっては ~/.claude.json 側にも設定があるので、必要ならこちらも同様に管理してください。
Mac miniで作ったスキルや会話履歴が、MacBook Airでもそのまま使えるのは本当に便利です。「あのスキルどこだっけ？」「前にどうやって解決したっけ？」がなくなりました。
iCloudに機密情報がアップロードされるので当然共用PCなどで実施してはいけません。セキュリティを上げたい場合 .env やトークン類はKeychain/1Password等を活用するか、同期対象から外してください。

### Antigravityの同期について
Antigravityは少し構成が複雑で、~/.agentだけでなく~/.gemini/antigravityも同期する必要がありましたね。
同期が必要なフォルダ
同期してはいけないファイル
フォルダごと（antigravity全体）ではなく、中身の特定フォルダだけをリンクする方法が安全です。

### 補足: chezmoiでもっと便利に（上級者向け）
シンボリックリンクを手動で作るのもいいですが、chezmoi（シェズモア）というdotfiles管理ツールを使うと、シンボリックリンク設定自体をGitで管理できます。
実際ぼくはこちらで設定しました。
新しいMacを買っても、コマンド1つで同じ環境が再現できるのがポイント。Mac入れ替えや3台目追加も「chezmoi init --apply」だけで設定が完了します。
Macだけでなく、LinuxやWSLでも（OSごとの条件分岐を使えば）同じdotfilesを使い回せるので、複数環境を行き来する人にも相性がいいと感じました。

正直なところ、最初は「iCloudで設定フォルダを同期する」という発想がありませんでした。普通はDropboxとかでファイルを同期するイメージですよね。
でもやってみたら、Macなら標準で使えるiCloud Driveが一番手軽でした。特別なアプリも要らないし、Appleのエコシステムに乗っかるだけ。
個人的に一番大きかったのは、「今後Mac入れ替えても環境構築が一瞬で終わる」という安心感です。M4 MacBook Airへの設定は、chezmoiのコマンド1つで完了しました。実際にやってみると5分もかからなかったです。
複数Mac使いで同じ悩みを持っている方の参考になれば嬉しいです。質問などあればコメントでお気軽にどうぞ。
最後までご覧いただきありがとうございました。ではまた〜！

- [#AI](https://note.com/hashtag/AI)
- [#生成AI](https://note.com/hashtag/%E7%94%9F%E6%88%90AI)
- [#AIとやってみた](https://note.com/hashtag/AI%E3%81%A8%E3%82%84%E3%81%A3%E3%81%A6%E3%81%BF%E3%81%9F)
- [#AIエージェント](https://note.com/hashtag/AI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88)
- [#Mac](https://note.com/hashtag/Mac)
- [#MacBookAir](https://note.com/hashtag/MacBookAir)
- [#Macmini](https://note.com/hashtag/Macmini)
- [#AIエージェント活用](https://note.com/hashtag/AI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88%E6%B4%BB%E7%94%A8)
- [#シンボリックリンク](https://note.com/hashtag/%E3%82%B7%E3%83%B3%E3%83%9C%E3%83%AA%E3%83%83%E3%82%AF%E3%83%AA%E3%83%B3%E3%82%AF)
[#AI](https://note.com/hashtag/AI)
[#生成AI](https://note.com/hashtag/%E7%94%9F%E6%88%90AI)
[#AIとやってみた](https://note.com/hashtag/AI%E3%81%A8%E3%82%84%E3%81%A3%E3%81%A6%E3%81%BF%E3%81%9F)
[#AIエージェント](https://note.com/hashtag/AI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88)
[#Mac](https://note.com/hashtag/Mac)
[#MacBookAir](https://note.com/hashtag/MacBookAir)
[#Macmini](https://note.com/hashtag/Macmini)
[#AIエージェント活用](https://note.com/hashtag/AI%E3%82%A8%E3%83%BC%E3%82%B8%E3%82%A7%E3%83%B3%E3%83%88%E6%B4%BB%E7%94%A8)
[#シンボリックリンク](https://note.com/hashtag/%E3%82%B7%E3%83%B3%E3%83%9C%E3%83%AA%E3%83%83%E3%82%AF%E3%83%AA%E3%83%B3%E3%82%AF)
[じゃが](https://note.com/jaga_farm)
[http://lit.link/jagafarm](http://lit.link/jagafarm)
1. [トップ](https://note.com)
2. [コンピュータ・IT](https://note.com/topic/computer_it)
3. [アプリケーション](https://note.com/topic/computer_it/applications)
[トップ](https://note.com)
[コンピュータ・IT](https://note.com/topic/computer_it)
[アプリケーション](https://note.com/topic/computer_it/applications)
- [noteプレミアム](https://premium.lp-note.com)
- [note pro](https://pro.lp-note.com/?utm_source=notecom&utm_medium=footer)
- [ヘルプ](https://help-note.com/hc/ja)
- [プライバシー](https://terms.help-note.com/hc/ja/articles/44948981050649)
- [クリエイターへのお問い合わせ](https://note.com/jaga_farm/message)
- [フィードバック](https://help-note.com/hc/ja/requests/new?ticket_form_id=360000081181)
- [ご利用規約](https://terms.help-note.com/hc/ja/articles/44943817565465)
- [通常ポイント利用特約](https://note.com/terms/paid_point)
- [加盟店規約](https://note.com/terms/seller_creators)
- [資金決済法に基づく表示](https://note.com/terms/payment_service_act)
- [特商法表記](https://note.com/jaga_farm/terms/specified)
- [投資情報の免責事項](https://note.com/terms/investment_disclaimer)
[noteプレミアム](https://premium.lp-note.com)
[note pro](https://pro.lp-note.com/?utm_source=notecom&utm_medium=footer)
[ヘルプ](https://help-note.com/hc/ja)
[プライバシー](https://terms.help-note.com/hc/ja/articles/44948981050649)
[クリエイターへのお問い合わせ](https://note.com/jaga_farm/message)
[フィードバック](https://help-note.com/hc/ja/requests/new?ticket_form_id=360000081181)
[ご利用規約](https://terms.help-note.com/hc/ja/articles/44943817565465)
[通常ポイント利用特約](https://note.com/terms/paid_point)
[加盟店規約](https://note.com/terms/seller_creators)
[資金決済法に基づく表示](https://note.com/terms/payment_service_act)
[特商法表記](https://note.com/jaga_farm/terms/specified)
[投資情報の免責事項](https://note.com/terms/investment_disclaimer)

