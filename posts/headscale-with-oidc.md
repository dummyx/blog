---

title: 必要以上も必要である
published: 2022-12-06T16:09:53+00:00

---

#  OIDC付きでHeadscale立ててみた

## Head
研究室のVPNはルータの付いてるL2TPなので、その辺を改善したく、(勝手に)Headscaleを立てることにした。

## Neck
最終的にラズパイやらなんやらでサーバルームに置きたいが、まずは適当にどこからVPSを借りてやってみる。検証の気持で。

## Body

### Breast
ディストロはArchで、自分でコンパイルすると多分OOMになっちゃうので、Headscaleの入てるchaoric-aurリポジトリを追加する。あとは*pacman*に食わせてやるだけ。

~~Goで書かれてるので普通にバイナリを引っぱてくれば実行できそうだが~~

### Belly
OIDC機能は付いてるものの、まだ実験的扱いで、Headscaleのドキュメントもあまり言及していなく、恐らく拙作が電子の海に出たこれについて初めての記事かもしれない。

幸いconfigのexampleに設定の仕方は書かれてある。

研究室はGoogle Workspace使ってるので、まずGoogleにもろもろを登録する。

Google Cloud ConsoleでOAuth同意画面とスコープなどを設定して、OAuth 2.0クライアントを作成する。そうするとcredentialのjsonがダウンロードできるので、それをダウンロードしてなかの情報をHeadscaleの設定ファイルにぶちこむ。

Issuerについては

>The Issuer Identifier for the Issuer of the response. Always https://accounts.google.com or accounts.google.com for Google ID tokens.

だそう。

Redirect URIについてもなんも書かれてないが、[コード](https://github.com/juanfont/headscale/blob/34107f9a0f1a16b3326e3bdfd4b2b258ca4220f5/app.go#L449)を覗いてみれば、 *server_url* + */oidc/callback* だと分かる。これをGoogle Cloud Consoleの*承認済みのリダイレクト URI*に登録する。

config.yamlのほかの部分も適当に埋めれば設定完了。

httpsまわりも自動的にやってくれる。いまどきのサーバものは便利だなぁって感動した。

### NO TRESPASSING
動かしてみる。

↓

動いた。

と通信量親切に言いたいだが、実は「↯」的な感じだった。まぁ、そういうつまらない話だれも聞きたくないので、通信量節約のため省かせていただく。

思ったよりは、すんなりいけた。すんあり。

これで快適にぶいぴーえぬできる、いえい。

## Tail
scale? このスケールでOIDCやる必要ある？

人間にとって必要のないしっぽもまた、萌え要素ではないか。