Title: Antigravityのチャットを複数端末で同期させる方法 #Antigravity - Qiita

Description: はじめに 私はWindows PCを2つ使用していて、その両方でAntigravityを使っていて、 チャットの内容がそれぞれで同期されないことが今まで不満でした。 しかし、それを解決できる方法を見つけたので紹介します。 Antigravityでは現在公式でチャットを同期...

Source: https://qiita.com/spellsan/items/4469df272e3ce90221d8

---

[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fspellsan%2Fitems%2F4469df272e3ce90221d8&realm=qiita)
[Signup](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fspellsan%2Fitems%2F4469df272e3ce90221d8&realm=qiita)
1. [Trend](https://qiita.com/)
2. [Stock List](https://qiita.com/stock-feed)
3. [Question](https://qiita.com/question-feed)
4. [Qiita Conference](https://qiita.com/official-campaigns/conference/2026?utm_source=qiita&utm_medium=referral&utm_campaign=global_navigation)
5. [Official Event](https://qiita.com/official-events)
6. [Official Columnopen_in_new](https://qiita.com/official-columns)
7. [Organization](https://qiita.com/organizations)
8. [Qiita Careersopen_in_new](https://careers.qiita.com)
9. [AI x Dev x Teamopen_in_new](https://qiita.com/official-campaigns/ai-dev-team)
[Trend](https://qiita.com/)
[Stock List](https://qiita.com/stock-feed)
[Question](https://qiita.com/question-feed)
[Qiita Conference](https://qiita.com/official-campaigns/conference/2026?utm_source=qiita&utm_medium=referral&utm_campaign=global_navigation)
[Official Event](https://qiita.com/official-events)
[Official Columnopen_in_new](https://qiita.com/official-columns)
[Organization](https://qiita.com/organizations)
[Qiita Careersopen_in_new](https://careers.qiita.com)
[AI x Dev x Teamopen_in_new](https://qiita.com/official-campaigns/ai-dev-team)
[4](https://qiita.com/spellsan/items/4469df272e3ce90221d8/likers)
Go to list of users who liked
Share on X(Twitter)
Share on Facebook
Add to Hatena Bookmark
Delete article
Deleted articles cannot be recovered.
Draft of this article would be also deleted.
Are you sure you want to delete this article?
[@spellsan](https://qiita.com/spellsan)

# Antigravityのチャットを複数端末で同期させる方法
- [Antigravity](https://qiita.com/tags/antigravity)
[Antigravity](https://qiita.com/tags/antigravity)

# https://qiita.com/spellsan/items/4469df272e3ce90221d8#%E3%81%AF%E3%81%98%E3%82%81%E3%81%AB はじめに
私はWindows PCを2つ使用していて、その両方でAntigravityを使っていて、 チャットの内容がそれぞれで同期されないことが今まで不満でした。 しかし、それを解決できる方法を見つけたので紹介します。
Antigravityでは現在公式でチャットを同期させる方法は（おそらく）提供されていません。 そのためこの方法はAntigravityの変更によって動作しなくなる可能性があります。
ちなみに今回の方法では、Githubを使って同期させます。 GoogleDriveなどを使いたい方は適宜読み替えてください。

# https://qiita.com/spellsan/items/4469df272e3ce90221d8#%E3%82%A2%E3%83%97%E3%83%AD%E3%83%BC%E3%83%81 アプローチ
Antigravityはチャット履歴をローカルに保存しています。 そのため、理論上はそのローカルファイルを同期させてあげれば良いということになります。 ちなみに、今回の方法では、conversationsと、brainというフォルダーを同期させます。

```
conversations
```


```
brain
```

それぞれのフォルダの中身は以下の通りです。
- conversations

チャットの内容本体が含まれる（おそらく）


- チャットの内容本体が含まれる（おそらく）
- brain

task.mdやImplementation Plan、アップロードした画像などが含まれる


- task.mdやImplementation Plan、アップロードした画像などが含まれる
- チャットの内容本体が含まれる（おそらく）
- task.mdやImplementation Plan、アップロードした画像などが含まれる
というわけで、これらをgitの管理下に置いて、もとの場所にシンボリックリンクを貼るというのが今回のアプローチです。 やっていること自体は単純なので、追加で同期させたいフォルダがある場合も簡単にできると思います。

# https://qiita.com/spellsan/items/4469df272e3ce90221d8#%E5%AE%9F%E9%9A%9B%E3%81%AB%E3%82%84%E3%81%A3%E3%81%A6%E3%81%BF%E3%82%8B 実際にやってみる
- まず、必要なフォルダーを任意のgit管理下フォルダに移動させます
antigravityのフォルダはwindowsの場合は以下の通りです。 C:\Users\ユーザー名\.gemini\antigravity\

```
C:\Users\ユーザー名\.gemini\antigravity\
```

この中に入っている、brainフォルダーとconversationsフォルダーを移動させてください。

```
brain
```


```
conversations
```

今回私は、brainとconversationsを以下のディレクトリへと移動させました。 C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations

```
brain
```


```
conversations
```


```
C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain
```


```
C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations
```

- このままではantigravity側からbrainフォルダー達を認識することが出来ませんので、シンボリックリンクを貼って、正しく認識されるようにします。

```
brain
```


```
mklink /D "C:\Users\ユーザー名\.gemini\antigravity\brain" 移動先のディレクトリ # 実行結果 symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\brain <<===>> 移動先のディレクトリ mklink /D "C:\Users\ユーザー名\.gemini\antigravity\conversations" 移動先のディレクトリ # 実行結果 symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\conversations <<===>> 移動先のディレクトリ
```


```
mklink /D "C:\Users\ユーザー名\.gemini\antigravity\brain" 移動先のディレクトリ # 実行結果 symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\brain <<===>> 移動先のディレクトリ mklink /D "C:\Users\ユーザー名\.gemini\antigravity\conversations" 移動先のディレクトリ # 実行結果 symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\conversations <<===>> 移動先のディレクトリ
```


```
mklink /D "C:\Users\ユーザー名\.gemini\antigravity\brain" "C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain" symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\brain <<===>> C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain mklink /D "C:\Users\ユーザー名\.gemini\antigravity\conversations" "C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations" symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\conversations <<===>> C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations
```


```
mklink /D "C:\Users\ユーザー名\.gemini\antigravity\brain" "C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain" symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\brain <<===>> C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\brain mklink /D "C:\Users\ユーザー名\.gemini\antigravity\conversations" "C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations" symbolic link created for C:\Users\ユーザー名\.gemini\antigravity\conversations <<===>> C:\Users\ユーザー名\Documents\GitHub\~~~\Utility\Antigravity\conversations
```

最後にGitでpushして、同期先の別端末でも同じことを繰り返せば、antigravityのチャットフォルダーが同期されるようになります。

# https://qiita.com/spellsan/items/4469df272e3ce90221d8#%E6%B3%A8%E6%84%8F%E7%82%B9 注意点
Windowsのユーザー名が端末ごとに違う場合、同期した先のマシンでチャットを開く際に、おそらく以下のような確認画面が出ると思います。この際に、Open in workspaceを選んでしまうと、別マシンの方のパスで開こうとして、パスが存在しないとしてエラーになってしまいます。

```
Open in workspace
```

そのため、Open in current windowを選択することをおすすめします。

```
Open in current window
```

このアプローチとして、以下のGithubリポジトリを参考にしました。ありがとうございました。 [antigravity-storage-manager](https://github.com/unchase/antigravity-storage-manager)
[4](https://qiita.com/spellsan/items/4469df272e3ce90221d8/likers)
Go to list of users who liked
[comment0](https://qiita.com/spellsan/items/4469df272e3ce90221d8#comments)
Go to list of comments
Register as a new user and use Qiita more conveniently
1. You get articles that match your needs
2. You can efficiently read back useful information
3. You can use dark theme
[What you can do with signing up](https://help.qiita.com/ja/articles/qiita-login-user)
[Sign up](https://qiita.com/signup?callback_action=login_or_signup&redirect_to=%2Fspellsan%2Fitems%2F4469df272e3ce90221d8&realm=qiita)
[Login](https://qiita.com/login?callback_action=login_or_signup&redirect_to=%2Fspellsan%2Fitems%2F4469df272e3ce90221d8&realm=qiita)
[4](https://qiita.com/spellsan/items/4469df272e3ce90221d8/likers)
Go to list of users who liked
Delete article
Deleted articles cannot be recovered.
Draft of this article would be also deleted.
Are you sure you want to delete this article?
How developers code is here.
Guide & Help
- [About](https://qiita.com/about)
- [Terms](https://qiita.com/terms)
- [Privacy](https://qiita.com/privacy)
- [Guideline](https://help.qiita.com/ja/articles/qiita-community-guideline)
- [Media Kit](https://help.qiita.com/ja/articles/others-brand-guideline)
- [Feedback/Requests](https://github.com/increments/qiita-discussions/discussions/116)
- [Help](https://help.qiita.com)
- [Advertisement](https://business.qiita.com/?utm_source=qiita&utm_medium=referral&utm_content=footer)
[About](https://qiita.com/about)
[Terms](https://qiita.com/terms)
[Privacy](https://qiita.com/privacy)
[Guideline](https://help.qiita.com/ja/articles/qiita-community-guideline)
[Media Kit](https://help.qiita.com/ja/articles/others-brand-guideline)
[Feedback/Requests](https://github.com/increments/qiita-discussions/discussions/116)
[Help](https://help.qiita.com)
[Advertisement](https://business.qiita.com/?utm_source=qiita&utm_medium=referral&utm_content=footer)
Contents
- [Release Note](https://qiita.com/release-notes)
- [Official Event](https://qiita.com/official-events)
- [Official Column](https://qiita.com/official-columns)
- [Advent Calendar](https://qiita.com/advent-calendar/2025)
- [Qiita Tech Festa](https://qiita.com/tech-festa/2025)
- [Qiita Award](https://qiita.com/qiita-award)
- [Engineer White Paper](https://qiita.com/white_papers/2026)
- [API](https://qiita.com/api/v2/docs)
[Release Note](https://qiita.com/release-notes)
[Official Event](https://qiita.com/official-events)
[Official Column](https://qiita.com/official-columns)
[Advent Calendar](https://qiita.com/advent-calendar/2025)
[Qiita Tech Festa](https://qiita.com/tech-festa/2025)
[Qiita Award](https://qiita.com/qiita-award)
[Engineer White Paper](https://qiita.com/white_papers/2026)
[API](https://qiita.com/api/v2/docs)
Official Accounts
- [@Qiita](https://x.com/qiita)
- [@qiita_milestone](https://x.com/qiita_milestone)
- [@qiitapoi](https://x.com/qiitapoi)
- [Facebook](https://www.facebook.com/qiita/)
- [YouTube](https://www.youtube.com/@qiita5366)
- [Podcast](https://open.spotify.com/show/4E7yCLeCLeQUsNqM4HXFXA)
[@Qiita](https://x.com/qiita)
[@qiita_milestone](https://x.com/qiita_milestone)
[@qiitapoi](https://x.com/qiitapoi)
[Facebook](https://www.facebook.com/qiita/)
[YouTube](https://www.youtube.com/@qiita5366)
[Podcast](https://open.spotify.com/show/4E7yCLeCLeQUsNqM4HXFXA)
Our service
- [Qiita Team](https://teams.qiita.com/)
- [Qiita Zine](https://zine.qiita.com?utm_source=qiita&utm_medium=referral&utm_content=footer)
- [Official Shop](https://suzuri.jp/qiita)
[Qiita Team](https://teams.qiita.com/)
[Qiita Zine](https://zine.qiita.com?utm_source=qiita&utm_medium=referral&utm_content=footer)
[Official Shop](https://suzuri.jp/qiita)
Company
- [About Us](https://corp.qiita.com/company)
- [Careers](https://corp.qiita.com/jobs/)
- [Qiita Blog](https://blog.qiita.com)
- [News Release](https://corp.qiita.com/releases/)
[About Us](https://corp.qiita.com/company)
[Careers](https://corp.qiita.com/jobs/)
[Qiita Blog](https://blog.qiita.com)
[News Release](https://corp.qiita.com/releases/)

