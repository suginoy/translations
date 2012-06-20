# よりよい JavaScript を書くための 10 の Tips (10 tips to write better JavaScript)

この文章は、以下のブログの翻訳です。

10 tips to write better JavaScript

[http://kinesis.io/blog/10-tips-to-write-better-javascript/](http://kinesis.io/blog/10-tips-to-write-better-javascript/)

## 1 セミコロンを忘れない(Don’t forget the semi-colons)

> JavaScript allows developers the freedom to skip semi-colons in the source code. Most of us, being lazy as we are, tend to follow this.
> 
> However, the JavaScript compiler still needs them, so it is the parsers job to insert them whenever there is a parse error due to a missing semi-colon. It tries to execute the statement, and if it fails, it tries again with the semicolons
> 
> When you’re writing JavaScript, you should always include semicolons after statements.

JavaScript は、開発者にソースコード中のセミコロンを飛ばす自由がある。ほとんどの人は、私たちがそうであるように、怠け者なので、そうする傾向がある。
しかし、JavaScript のコンパイラは依然としてセミコロンが必要で、セミコロンが無い箇所でパースエラーがあると必ず、パーサがセミコロンを挿入する。ステートメントを実行しようとして失敗すると、セミコロンから実行を再開しようとする。

JavaScript を書くときは、ステートメントの後ろにかならずセミコロンを含めるべきだ。

## 2 言語をよく知る(Know the language)

> When we talk about Arrays and Objects in JavaScript, there are two ways to initializing them listed below.

JavaScript で Array とObject の話題といえば、以下のように、2 種類の初期化の方法がある。

````javascript
var arr = [], obj = {};
````

と

````javascript
var arr = new Array, obj = new Object;
````

だ。

> Note that in modern browsers, the first way is faster and should be used.

モダンなブラウザでは、最初の方が高速で、こちらを使うべきだ。

## 3 For ループ 対 For in ループ (For vs For in Loop)

> The for in loop looks like

ループで使用する for はこんな感じだ。

````javascript
for(item in obj) {
 //do something with item
}
````

> This has a drawback. It will not just access the properties of “obj”, it will also include any properties inherited by the object through the prototype chain.
> 
> Therefore, most of the times, we have to use a if statement inside the loop along with the method “hasOwnProperty()” on the “obj”.

これには欠点がある。アクセスを許すのが obj のプロパティのみではないということだ。プロトタイプチェーンによるオブジェクトを継承したプロパティをすべて含んでいるということでもある。

そのため、大抵は、ループ内で obj に hasOwnProperty() メソッドを付けた if ステートメントを使わないといけない。


## 4 == の代わりに === を使う(Use === instead of ==)

> The identity (===) operator behaves identically to the equality (==) operator except no type conversion takes place. The types must be the same for === to return true.
>
> The == operator will compare for equality after doing any necessary type conversions. The === operator will not do the conversion, so if two values are not the same type === will simply return false. It’s this case where === will be faster, and may return a different result than ==. In all other cases performance will be the same.

'===' 演算子は、等値か判断するのに必要な変換を行ってから比較を行う。'==' 演算子は変換をしない。2 つの値が同じ型でない場合、 '===' は単純に false を返す。'===' が速いのは事実だし、'==' と異なる結果を返すこともあるが、それ以外の多くの場合で、パフォーマンスは同じだ。

````javascript
"abc" == new String("abc")    // true
"abc" === new String("abc")   // false
````

## 5 with の使用を避ける(Avoid using “with”)

> JavaScript’s with statement was intended to provide a shorthand for writing recurring accesses to objects. So instead of writing

JavaScript の with ステートメントは、オブジェクトに繰り返しアクセスするための略記法を提供するためのものだ。次のように書く代わりに、

````javascript
a.s.d.f.g.h.j.k.hello = true;
a.s.d.f.g.h.j.k.world = true;
````

このように書ける。

> You can write

````javascript
th (a.s.d.f.g.h.j.k) {
    hello = true;
    world = true;
}
````

> How can you be sure if the variable a.s.d.f.g.h.j.k.hello is being set or a global variable named hello is being set.
>
> Fortunately, we have a better way of doing this.

a.s.d.f.g.h.j.k.hello という変数がセットされるか、hello というグローバル変数がセットされるか自信を持って言えるだろうか？
幸運にも、これをするのによい方法がある。

````javascript
var obj = a.s.d.f.g.h.j.k;
obj.hello = true;
obj.world = true;
````

## 6 メソッド呼び出しを避ける（できるかぎり）(Avoid method calls (as much as you can))

> Inline code is faster in all modern browsers that calling functions. I know that writing code in this manner can lead to clutter, but this is just one those JavaScript annoyances that we deal with.

インラインコードはすべてのモダンプラウザにおいて、関数呼び出しを行うよりも速い。このやり方は、散らかりの元となることを知ってはいるが、JavaScript のいらいらを扱う方法の一つだ。

````javascript
function explicitCall() {
  function cube(x) { return x*x*x };
  var i = 1000;

  while(i--) { console.info(cube(i)); }

}

function inlineCall() {
  var i = 1000;

  while(i--) { console.info(i*i*i); }

}
````

## 7 JSLinst を使う(Use JSLint)

> [http://www.jslint.com/](http://www.jslint.com/) is The JavaScript Code Quality Tool.
> Use this and check your JavaScript for errors as much as you can. Running your code through this is a great way to improve your code and follow the best practices.

[http://www.jslint.com/](http://www.jslint.com/) は JavaScript の品質測定ツールだ。
これを使って、 JavaScript のエラーをできるだけチェックしよう。この中に自分のコードを走らせれば、コードを改善してベストプラクティスに従う素晴らしい方法となる。


## 8 ドット記法と数値型(Dot Notation and Numbers)	

> There is a problem in the JavaScript parsers dot notation when dealing with numbers.

JavaScript のパーサは、数値を扱う際のドット記法に問題がある。


````javascript
10.toString() //Error
````

> The JavaScript parser expects a number to follow right after the first period. This causes a syntax error.

JavaScript のパーサは、最初のピリオドの右側に数字を期待している。これがシンタックスエラーを引き起こす。

> To fix this, there are two ways of going forward

これを直すには、次のように 2 つのやり方がある。

````javascript
(10).toString();
10..toString();
````

## 9 ブロックスコープが存在しない(No block scope)
> There is no block scope in JavaScript.
> Statements like if and for do not create a scope, and therefore variables can be overwritten.
>
>In the following code, we will see how the value changes if the variable x

JavaScript にはブロックスコープが存在しない。
if や for といったステートメントはスコープを作らない。それため、変数は上書きされる可能性がある。

次のコードで、変数 x の値が変更される様子がわかるだろう。

````javascript
var x = 1; // x = 1

if(true) {
 x = 2; // x = 2

 for(var i=0; i < 4; i++) {
  x = i; //i = 0,1,2,3
 }

}
````

> So x is changing as the code executes.

変数 x がコードの実行とともに変化していく。

## 10 グローバル変数をキャッシュする(Cache Globals)

> It is always better when caching globals in terms of execution time.

実行時間の点で、グローバル変数はキャッシュした方がよい場合が多い。

````javascript
function notcached() {
 var i = 1000;
 while (i--) window.test = 'test';
}

function cached() {
 var i = 1000;
 var w = window;
 while (i--) w.test = 'test';
}
````
