DCI: 単一責任原則の王(The King of the Single Responsibility Principle)

This is a translation of the article of Make Pack's Blog.

The original URL is below.

[http://www.mikepackdev.com/blog_posts/38-dci-the-king-of-the-single-responsibility-principle](http://www.mikepackdev.com/blog_posts/38-dci-the-king-of-the-single-responsibility-principle)

> The single responsibility principle (SRP) is one of the most widely followed ideals in object oriented programming. For decades, developers have been striving to ensure their classes take on just enough, but not too much, responsibility. A valiant effort and by far one of the best ways to produce maintainable code.

単一責任原則(SRP)は、オブジェクト指向プログラミングの中でももっとも広く支持された理想の一つです。数十年間、開発者はクラスが十分でありながら多すぎない責務を負わせとうと試行錯誤してきました。勇ましい努力、保守性の高いコードを生み出すのに最良の方法です。(TODO:)

> SRP is hard, though. Of all the SOLID design principles, it is the most difficult to embrace. Due to the abstract nature if its definition, based purely on example instead of directive process, it's hard to concretize. More specifically, it's difficult to define "responsibility", in general or in context. There are some rules-of-thumb to help, like reasons for change, but even these are enigmatic and hard to apply.

しかし、SRPは難しいのです。SOLID設計原則の中で、受け入れることがもっとも難しいのです。

さらに特別なことに、一般的にであろうと特定のコンテキストによろうと、「責務」を定義するのが難しいのです。変更するときの理由といった、助けとなる経験則はありますが、むしろ、不可解で、適用が難しい場合が多いのです。

> Simply put, SRP says a class should be comprised of just one responsibility, and only a single reason should force modification.

簡単に言うと、SRPは、一つのクラスがただ一つの責務で構成されていて、ただ一つの理由によって変更されるべきだということです。

> Let's take a look at the following class which clearly has three responsibilities and therefore breaks SRP:

次のクラスを見てみましょう。明らかに三つの責務を持ち、SRPに反します。

````
class User < ActiveRecord::Base
  def check_in(location); ... end
  def solr_search(term); ... end
  def request_friendship(other_user); ... end
end
````

> This class would require churn for a variety of reasons, some of which include:

このクラスはさまざまな理由で不安をかき立てます。次のようなものです。

> 1. The algorithm for checking in changes.
> 2. The fields used to search SOLR are renamed.
> 3. Additional information needs to be stored when requesting a friendship.

1. 課金をチェックするアルゴリズム
2. Solrの検索に使われるフィールドの名称変更(TODO:)
3. 友人関係のリクエストすると保存されるのに追加情報(TODO:)

> So, based on the rules of SRP, this class needs to be broken out into three different classes, each with their own responsibility. Awesome, we're done, right? This is often the extent of discussions around SRP, because it's extremely difficult to provide solutions beyond contrived, minute examples. Theoretically, SRP is very easy to follow. In practice, it's much more opaque. It's too pie in the sky for my taste; like most OOP principles, I think SRP should be more of a guideline than a hard-and-fast rule.

SRPの規則に従うと、このクラスは 3 つの責務をそれぞれが持った、異なるクラスに分割される必要があります。すごい、もうできたじゃない？こういう話が、SRPについて議論される程度(TODO:)なのは、説得力のない、些細な例を超えた解決策を提示するのは極めて難しいからです。理論的には、SRPは従うのがとても容易です。実際には、ずっと不透明です。



ほとんどのOOP原則のように、厳格な規則というよりは、ガイドラインとしてあるべきだろ考えます。

> The real difficulty of SRP surfaces when your project grows beyond 100 lines of code. SRP is easy if you're satisfied with single method classes or decide to think about responsibility exclusively in terms of methods. In my opinion, neither of these are suitable options.

SRPの本当の難しさは、プロジェクトが100行のコードを超えて成長してきたときに表面化します。単独のメソッドを持つクラスに満足しているか、メソッドの責務を(TODO:)

> DCI provides more robust guidelines for following SRP, but we need to redefine responsibility. That's the focus of this article.

DCIはSRPに従うのに堅牢なガイドラインとなりますが、責務というものを再定義してやる必要があります。これがこの記事の焦点です。

> Identity and Responsibility

アイデンティティと責務

> OK, let's try to elucidate responsibility, but first, let's talk about object orientation.

ここで、責務というものを明らかにしてみましょう。ですが、まずはオブジェクト指向について話します。

> The word "object" can be defined as a resource that contains state, behavior and identity. Its state is the value of the class's attributes. Its behavior is the actions it can perform. And its identity is...well...it's object id? It feels strange to narrow the definition of identity into a mere number. I certainly don't identify myself by my social security number. My identity is derived from my name, the things I enjoy doing, and potentially my environment. More importantly, my identity is always changing.

「オブジェクト」という言葉は、状態と振る舞いとアイデンティティをもつリソースだと定義できるでしょう。状態は、クラスの属性がもつ値です。振る舞いは、可能なアクションです。アイデンティティは、うん、オブジェクトのIDでしょうか？アイデンティティの定義を単なる数字に狭めてしまうのは、奇妙に感じます。間違いなく、私は自分のアイデンティティを社会保障番号によって規定したりしません。私のアイデンティティは、名前や、好きなことや、潜在的には私の環境によって規定されます。さらに重要なこととして、私のアイデンティティは、常に変化しています。

> While building a class, when was the last time you thought about the forthcoming object ids of the instances of that class? I hope never. We don't program classes with identity in mind, yet if we're trying to model the world, it's an intrinsic component. Identity means nothing to us while building classes, yet everything to us in the real world.

> Therefore, it's appropriate to say that the mental model of the programmer is to map identity to state and behavior, rather than to object id. Object id is a quality of uniqueness.

> Identity is closely related to responsibility. As expressed above, I don't identify by my social security number, but by my state and behavior. When we attempt to find the appropriate location for a method definition, we look at the responsibility of the prospective classes. "Does this method fit into this class's single responsibility?" If we consider that identity should truly be a representation of an object's state and behavior, we can deduce that identity is a derivative of responsibility.

> An example of this observation is polymorphism; probably the most predominant and powerful object-oriented technique. When we consider the possible duck-typed objects in a scenario, we don't think, "this object will work if its object id is within the following set..." We think "this object will work if it responds to the right methods." We rarely care about the object id. What's important is the true identity of an object: the methods it responds to.









> The King of SRP

> It's hard to define responsibility. It's even harder to program for it. As an artifact, the responsibility of a class often ends up being either too narrowly or too broadly defined. Define responsibility too narrowly, and it's daunting to wrap your head around 1000 classes. Define responsibility too broadly, and it's arduous to maintain and refactor.

責務を定義することは難しい。ましてやそれをプログラムすることはさらに難しい。


> By defining responsibility as a role, we have a clear notion of behavioral locality. We can ask questions like, "as a customer, can I add an item to my cart?" If the answer is yes and we've appropriately named our roles, the method belongs in that role. This gives us a means for defining responsibility, and we can refactor accordingly.

> Roles won't alleviate potential clutter, but they can give us a structure for defining responsibility. With DCI, we can talk about responsibility in terms of directive process instead of contrived examples.

> First, understand the business objectives of the system and subsequently understand the roles. Understand the roles and subsequently understand the responsibilities.


