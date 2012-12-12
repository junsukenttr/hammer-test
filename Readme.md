#Hammer for Mac TEST

MacのBuildアプリ「[Hammer for Mac](http://hammerformac.com/)」をお試ししてる一式。


- [Hammer Documentation](http://help.hammerformac.com/)
- [appstorm](http://mac.appstorm.net/reviews/web-dev-review/stop-hammertime-with-hammer-for-mac/)

---
##ざっくり特徴
**Hammer**はプロジェクトのフォルダを登録すると更新があるごとに自動でビルドしてくれるGUIアプリ。  
HamlやSassなどの様にアプリ内で主にhtmlに独自のメタ言語で変数やインクルードなどが使える。    
プロジェクトフォルダを指定すると監視をはじめフォルダ内に「Build」フォルダを作成し書き出す。


---

##Clever Paths
その名の通り賢いパス。「@path」で指定  
画像名やCSSファイル名を指定するだけで自動で検索、パスを補完してくれる。

	<img src="<!-- @path logo.png -->" />
これでlogo.pngまでのパスを補完してくれる。

CSSでもClever Pathsは効く模様。

	background-image: url(bg.png)
これでもimagesフォルダのbg.pngを補完してくれた。


同じファイル名は階層の浅い順かアルファベット順で拾ってくるっぽい(？)  
ただし「include」や「stylesheet」というフォルダはルートより優先された。  
関数名と同じフォルダ名は優先されるみたい。まだあるのかもしれない。  
同じファイル名使えないってのは悩ましい。


---
## HTML Includes


	<!-- @include header -->
で「header.html」をインクルード。  
ビルドしたhtmlに書き出される。  
Clever Pathsにて自動でパスを補完してくれる。

サンプルは

`<!-- @include _header.html -->`

ってなってるけど、アンダーバーも拡張子も無くても読み込む。  
Sassのパーシャルみたいにアンダーバーを付けているファイルだからWatchしないとかそうゆうことでも無いみたい。

---

##Stylesheets & JavaScript
	<!-- @stylesheet style -->
	<!-- @javascript app -->
と書くと

@styleseetでlinkタグ,CSSファイルパス挿入。  
@javascriptでscriptタグ,JSファイルパス挿入。

	<link rel="stylesheet" href="stylesheets/style.css" />
	<script src="assets/js/app.js" type="text/javascript"></script>
	
こう書き出される。  
これもパスはもちろん補完してくれる。	

scssファイルも拡張子無しで読み込んでcssで指定してくれる。  
coffeeファイルもjsで書き出される。

	<!-- @stylesheet assets/css/* -->
*でフォルダ内のファイルを全部書き出し	

	<link rel="stylesheet" href="assets/css/reset.css"/>
	<link href="base.css" rel="assets/css/style.css"  />
	<link href="text.css" rel="assets/css/mobile.css"  />
こんな感じになるらしい。

---

##Navigation Helpers

	<ul class="menu">
		<li><a href="<!-- @path index.html -->">Home</a></li>
		<li><a href="<!-- @path about.html -->">About</a></li>
		<li><a href="<!-- @path contact.html -->">Contact</a></li>
	</ul>
リンクをClever Pathsで書くと
	
	<ul class="menu">
		<li><a href="index.html">Home</a></li>
		<li class="current"><a href="about.html" class="current">About</a></li>
		<li><a href="contact.html">Contact</a></li>
	</ul>

現在地のファイルにはclass「current」を付けてくれる機能らしい。

---
##Variables

html内で変数が使える。

	<!-- $title My New Title -->

で変数セット。  

「$title」が変数  
「 My New Title」が値

	<title><!-- $title --></title>
で変数呼び出し

	<title>My New Title</title>
ビルドするとこうなる。	

---

##Automatic Reload
	<!-- @reload -->
このタグ書くと

	<script>
        setTimeout(function(){
          ws = new WebSocket('ws://localhost:00000')
          ws.onopen = function(){ws.onclose = function(){document.location.reload()}}
        }, 500)
      </script>
こんなタグを吐き出す。
  
buildフォルダ内のhtmlをブラウザで開いておけば自動で更新されるAutoReloadができる。

Documentだとhead要素の中に書けって書いてあるけど別にbody閉じタグの前でも動きます。

---

