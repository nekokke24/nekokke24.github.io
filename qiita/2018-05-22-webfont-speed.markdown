## 前段として

本日、SP時にハンバーガーメニューのアイコンがド真ん中にあるという

センセーショナルな体験をしました。

<a href="http://sukasuka-anime.com/" target="_blank">http://sukasuka-anime.com/</a>

まだまだ日本も捨てたもんじゃない。

## webfont系のよもやまやります。

今日はwebfont、主にgoogle font系のよもやまをやります。

最近ではよくあるwebfontですが

よくある形が下記です。

```
<link href="https://fonts.googleapis.com/css?family=Open+Sans" rel="stylesheet">
```

でも、これって中身がどうなっているか見たことありますか？

このhrefの値をブラウザのアドレスバーにいれると

下記みたいなコードがでてきます。

```
/* cyrillic-ext */
@font-face {
  font-family: 'Open Sans';
  font-style: normal;
  font-weight: 400;
  src: local('Open Sans Regular'),
  local('OpenSans-Regular'),
  url(https://fonts.gstatic.com/s/opensans/v15/mem8YaGs126MiZpBA-UFWJ0bf8pkAp6a.woff2)
  format('woff2');
  unicode-range: U+0460-052F,
  	U+1C80-1C88, U+20B4,
  	U+2DE0-2DFF, U+A640-A69F,
  	U+FE2E-FE2F;
}
...
(長いので省略)

```

これって結局、URL先でfont-faceを定義しているだけなんですね

なので、このコードをそのままCSSに貼り付ければ

同じことができます。

というより、そうするべきです。

## webfontの通信回数

https://fonts.googleapis.com/css?family=Open+Sans

で1回通信した後に

https://fonts.gstatic.com/s/opensans/v15/mem8YaGs126MiZpBA-UFWJ0bf8pkAp6a.woff2

でさらに通信してます。

ウェブサイト高速化の原則として「通信回数を減らす」というものがあります。

ウェブサイトは基本、通信が多ければ多いほど遅くなります。

つまりこれって無駄に1回通信して遅くなっているんですね。

これをCSSに直接貼り付けることで無駄な通信を1回減らせます。

この1回の通信でどうこうってわけでもないですが

webfontは重いとよく聞くので、こういう小さいところから心がけて行きたいですね。

## webfontが重い？

そもそも

webfontが重いって単語だけ聞くことがありますが

あれはなんで重いんでしょうか？

兎にも角にも日本語のwebfontは明らかに重いです。

英語はA-Z,a-z,0-9,(記号)などで良いところを

日本語の場合、あり得ない数の漢字+平仮名、片仮名ですから

情報の量もハンパじゃないです。

なので、日本語は重い。

後は不必要な読み込みが発生していないか？ですかね。

欲張ってなんでもポチって良いものでもありません。

例えば、ウェイト一つ追加するということは

その分、通信が発生するので読み込みが遅くなります。

## それでも改善できなかったら？

### 1.まずはデザイナーと相談しましょう。

制作においてデザインに関わる権限は全てデザイナーが持っています。

フォントに関しても、その例外ではありません。

デザイナーと相談して削れる読み込みがないかどうか？確認しましょう。

### 2.読み込み終わるまで代替テキスト

font-displayというプロパティを使用することで

webfontを読み終えるまではシステムフォントを代替的にロードして表示させます。

これによってユーザーはいち早くテキストを目にすることができるようになります。

```
/* cyrillic-ext */
@font-face {
  font-family: 'Open Sans';
  font-style: normal;
  font-weight: 400;
  font-display: swap; ←追加
  src: local('Open Sans Regular'),
  local('OpenSans-Regular'),
  url(https://fonts.gstatic.com/s/opensans/v15/mem8YaGs126MiZpBA-UFWJ0bf8pkAp6a.woff2)
  format('woff2');
  unicode-range: U+0460-052F,
  	U+1C80-1C88, U+20B4,
  	U+2DE0-2DFF, U+A640-A69F,
  	U+FE2E-FE2F;
}
...
(長いので省略)

```

こうゆう風にコードに追記ができる状態にするためにも

webfontはHTMLにlinkさせるのではなく

その先の記述をCSSに貼り付ける方が良さそうですね。

以上です。
