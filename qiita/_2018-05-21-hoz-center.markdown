現場レベルでよく使っているtipsを載せます。

## 1.ブロック系の中央寄せ

<div class="box1">block</div>
　

```
<div class="box1">block</div>

.box1 {
	width: 200px;
	margin-left: auto;
	margin-right: auto;
}
```

ここでのポイントはブロック要素の性質に起因します。

ブロック要素は特に幅を指定していない状態、つまりwidth: auto;時は

横にいっぱい広がります。

つまり、幅を100%以下に設定しない限り、横位置の調整はありえません。

なので、ブロック要素の位置調整の場合、幅がauto以外且つ100%以下のときにしか効果がでません。

その上で左右マージンをauto指定することで中央に寄ります。


## 2.インライン系の中央寄せ

<div class="box2"><span>inline</span></div>
　

```
<div class="box2"><span>inline</span></div>

.box2 {
	text-align: center;
}
```

これのポイントはtext-alignというプロパティはブロック要素及びインラインブロック要素にしか効かないということ。

そして、対象のブロック要素にかけた場合、その内部にある要素にカスケーディングしていき

インライン要素を対象として横位置が値通りの指定になる。

例の場合、inlineという文字を中央寄せにしたい場合は、inlineにかけるのではなく

外郭のブロック要素にかけないと意味がない。

例えば、これはかからない例

<div class="box3"><span >inline</span></div>
　

```
<div class="box3"><span>inline</span></div>

.box3 > span {
	text-align: center;
}
```

spanは通常インライン要素なのでこれではかからない。

ただし、このやり方の良いところがあります。

それは、それぞれ外郭のブロック要素も内部のインライン要素もインラインブロック要素で代用できるということです。

ブロック > インライン

は

インラインブロック（ブロックの代用） > インライン

でも

ブロック > インラインブロック（インラインの代用）

でも

インラインブロック（ブロックの代用） > インラインブロック（インラインの代用）

でも可能ということです。

中央にする要素がインラインの場合

スタイル的な制限がでてしまうため、できればブロック要素でありたいです。

ですがブロック要素だと幅を決めなくてはいけないということだと

汎用性が失われます。

例えば、ページネーションを実装する時に

```
< prev        1 2 3 4 5        next >
```

こんな形のだった場合、中央の数字は幅が何pxになるかわかりますか？

2個だった時、3個だった時とパターンに対しての対処はできるものの

全てにおいて対応できるコードというとどうでしょう？

その場合は幅を指定しなくても中央寄せができる実装でなければ耐えられません。

<div class="box"><span>1 2 3 4 5</span></div>
　

```
<div class="box"><span>1 2 3 4 5</span></div>
.box {
	text-align: center;
}
.box > span {
	display: inline-block;
}
```

これでどうでしょうか？

これなら幅を決めていないので、中の数字が何個になったとしても

このコードだけで耐えうる事ができます。

## 3.table系の中央寄せ

<div class="box4"><span>table</span></div>
　

```
<div class="box4"><span>table</span></div>

.box4 {
	display: table;
	width: 100%;
}
.box4 > span {
	display: table-cell;
	text-align: center;
}
```

これは現場ではあまり使わないですね。

table系のレイアウト操作は横位置より縦位置の恩恵の方が強いので

横位置を操作したいからtableにするというよりは縦位置のためにtableにし、横位置はその副産物みたいな扱いです。


## 4.flex系の中央寄せ

<div class="box5"><span>flex</span></div>
　

```
<div class="box5"><span>flex</span></div>

.box5 {
	display: flex;
	 justify-content: center;
}
```

これは現場ではあまり使わないです。

技術的には可能ですが
flexはfloatの代替でもあるように単体レイアウトというよりは複数レイアウト向けの横並びのプロパティなので
ただ中央に寄せたいためだけに使うのもコストに合わないです。

## 5.absolute系の中央寄せ

<div class="box6"><span>absolute1</span></div>
　

```
<div class="box6"><span>absolute1</span></div>

.box6 {
	position: relative;
}
.box6 > span {
	position: absolute;
	width: 200px;
	left: 0;
	right: 0;
	margin-left: auto;
	margin-right: auto;
}
```
　
<div class="box7"><span>absolute2</span></div>
　

```
<div class="box7"><span>absolute2</span></div>

.box7 {
	position: relative;
}
.box7 > span {
	position: absolute;
	width: 200px;
	left: 50%;
	margin-left: -100px;
}
```

これらは結構、使ったりします。

ライトボックスやカルーセル、1画面にキービジュアルをフィットさせるなど

JSを使ってなんやかんやする時は

結構absoluteを使用するので、その上での中央寄せの仕方は覚えておきましょう。

注意しなくてはいけないのが、状態をabsoluteにしているため外郭の要素が中身の要素の高さを認識できていません。

ただ、absoluteはfloatみたいにclearがなく、完全に浮遊しているためこれはもう仕方がありません。

外郭に高さを指定してあげる他ないです。

そしてこれだと、先のブロック要素の中央寄せと同じですが

幅を決めないと中央に寄りません。

特に最近ではレスポンシブレイアウトが増えている中なので

不用意に幅を決めてしまうと後で修正コストが増えて大変になる事が多々あります。

なので、現場では上記のコードを派生させた下記のようなコードを使います。

<div class="box8"><span>absolute3</span></div>
　

```
<div class="box8"><span>absolute3</span></div>

.box8 {
	position: relative;
}
.box8 > span {
	position: absolute;
	left: 50%;
	transform: translateX(-50%);
}
```

これだと幅を指定する必要がありません。

上記コードの（absolute2）をベースとしています。

そもそもabsolute2の仕組みはabsoluteで横位置を外郭の半分の50%にした後、

absoluteの相対値は外郭（要素から遡って一番近いposition static以外の要素）の幅を100%とし

要素の左上を起点として値を変更します。

ですが、それだと要素の左上を起点とするため

要素の幅の半分の値分中央の位置がずれてしまいます。

それをネガティブマージンを使用し、半分の値分マニュアルで調整してあげることで実現しています。

translateの相対値は自身の幅を100%として計算するため

-50%という入力で要素自身の半分の値分左にずらすということができます。

なので、コードを書く人はその要素が何pxかどうかを知る必要がないのです。

上記にやり方は結構使用されます。

変更にも強いです。

## まとめ

tipsだけ載せると一番優秀なものを使用すればと考えますが

実際のサイトだとそうはいきません。

というのは、実際のサイトはスタイルが何重にも重なってかかっている可能性が非常に高いからです。

その時に、このやり方だと他のスタイルに影響がでるなどの壁にぶつかるでしょう。

なので状況に応じての使い分けができると良いでしょう。

結論として

現場では出来る限り、widthに絶対値を入力するような中央寄せは避けましょう。

後々の修正にコストがかかるからです。

ですが、それはその指定の仕方が可能であればの話です。

使用できない場合は次に使うものを用意するべきし、臨機応変に対応できると良いですね。

以上です。
