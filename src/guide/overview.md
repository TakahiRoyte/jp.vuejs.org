---
title: 概要
type: guide
order: 2
---

Vue.js (発音は / v j u ː /、**view** と同様) はインタラクティブな Web インタフェースを構築するためのライブラリです。Vue.js のゴールは、できる限りシンプルな API で**リアクティブデータバインディング** と **構成可能な View コンポーネント**を提供することです。

Vue.js 自体は本格的なフレームワークではありません、Vue.js は View レイヤーだけに焦点を当てています。したがって、Vue.js のいいところだけをピックアップしたり、Vue.js を他のライブラリや既存のプロジェクトに統合することはとても簡単です。一方、Vue.js を適切なツールとサポートするライブラリによる組み合わせで使用する場合、Vue.js は完全に洗練されたシングルページアプリケーションを提供することができます。

あなたが経験豊富なフロントエンド開発者で、 Vue.js を他のライブラリ/フレームワークと比較したい場合、[他のフレームワークとの比較](comparison.html)をチェックしてください。Vue.js で大規模アプリケーションを扱う方法に興味がある場合は、[大規模アプリケーションの構築](application.html)をチェックしてください。

## リアクティブデータバインディング

Vue.js のコアは、非常にシンプルにデータと DOM を同期し続けるリアクティブデータバインディングシステムです。手動で DOM を操作するために jQuery を使用すると、命令的で、繰り返しが多く、間違いを起こしやすいコードを書くことがよくあります。Vue.js は**データ駆動 View **のコンセプトを採用しています。つまり、それは基本となるデータに DOM を "バインド" するために通常の HTML テンプレート内で特別な構文を使用するということです。一度バインディングが作成されると、DOM とデータは同期され続けます。データを変更するたびに、DOM はそれに応じて更新されます。その結果、ほとんどのアプリケーションロジックは、DOM の更新をいじくり回すよりも、直接的なデータ操作が可能になります。したがってコードはより書きやすく、より論理的に、より保守しやすくなります。

![MVVM](/images/mvvm.png)

最も単純な例:

``` html
<!-- これは View です -->
<div id="example-1">
  Hello {{ name }}!
</div>
```

``` js
// これは Model です
var exampleData = {
  name: 'Vue.js'
}

// Vue インスタンス、
// または View と Model にリンクする "ViewModel" を作成
var exampleVM = new Vue({
  el: '#example-1',
  data: exampleData
})
```

結果:
{% raw %}
<div id="example-1" class="demo">Hello {{ name }}!</div>
<script>
var exampleData = {
  name: 'Vue.js'
}
var exampleVM = new Vue({
  el: '#example-1',
  data: exampleData
})
</script>
{% endraw %}

一見するとただテンプレートをレンダリングしているように見えますが、Vue.js は内部で多くの作業を行っています。データと DOM はリンクされ、そして全てが**リアクティブ**になっています。しかし、どうやって私達はそれを知ることができるのでしょうか？ブラウザの開発者コンソールを開いて、`exampleData.name` を変更しましょう。上記の更新に応じてレンダリングされるサンプルを確認できるでしょう。

ここで、任意の DOM 操作のコードを書く必要がなかったことに注目してください。バインディングによって拡張された HTML テンプレートは基本的なデータ状態の宣言型マッピングで、それは単なる JavaScript オブジェクトです。その View は完全にデータ駆動型です。

次の例を見てみましょう:

``` html
<div id="example-2">
  <p v-if="greeting">Hello!</p>
</div>
```

``` js
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
```

{% raw %}
<div id="example-2" class="demo">
  <span v-if="greeting">Hello!</span>
</div>
<script>
var exampleVM2 = new Vue({
  el: '#example-2',
  data: {
    greeting: true
  }
})
</script>
{% endraw %}

ここには、何か新しいものがあります。`v-if` 属性は**ディレクティブ**と呼ばれています。ディレクティブは Vue.js によって提供された特別な属性を示すために `v-` が接頭されており、あなたが推測したように、レンダリングされた DOM に特定のリアクティブな振舞いを与えます。次に、コンソールで `exampleVM2.greeting` に `false` を設定します。そうすると、"Hello!" メッセージが非表示になることを確認できるでしょう。

この2つ目の例は、私達が DOM テキストをデータにバインドできるだけではなく、DOM の**構造** にデータをバインドできることを証明しています。さらに Vue.js は、要素が Vue によって挿入/削除されたとき、自動的にトランジション(遷移)エフェクトを適用できるパワフルなトランジションエフェクトシステムも提供します。

Vue.js にはかなりの数のディレクティブがあり、それぞれ独自に特別な機能を持っています。例えば、`v-for` ディレクティブは配列内のアイテムを表示するためのディレクティブで、`v-bind` ディレクティブは HTML 属性をバインディングするためのディレクティブです。完全なデータバインディング構文については、より詳細に、後で説明します。

## コンポーネントシステム

コンポーネントシステムは Vue.js におけるもうひとつの重要なコンセプトです。なぜならコンポーネントシステムは、小さく、自己完結的で、再利用可能なコンポーネントで構成される大規模アプリケーションの構築を可能にする抽象概念だからです。コンポーネントシステムについて考える場合、アプリケーションインタフェースのほぼすべてのタイプは、コンポーネントツリーとして抽象化することができます:

![Component Tree](/images/components.png)

実際、Vue.js で構築された典型的な大規模アプリケーションは、まさに上図右にあるコンポーネントツリーのような形になるでしょう。このガイドの後半ではコンポーネントについてより多くの話をしますが、ここでは、アプリケーションのテンプレートがコンポーネントでどのように見えるか、(架空)の例を示します:

``` html
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```

あなたは、Vue.js のコンポーネントが [Web Components Spec](http://www.w3.org/wiki/WebComponents/) の一部の**カスタム要素 (Custom Element)** にとても似ていることに気づいたかもしれません。実際、Vue.js のコンポーネント構文は仕様に沿って緩くモデル化されています。例えば、Vue コンポーネントは [Slot API](https://github.com/w3c/webcomponents/blob/gh-pages/proposals/Slots-Proposal.md) と `is` という特別な属性を実装しています。しかしながら、いくつか重要な違いがあります:

1. Web Components の仕様はまだまだ進行中で、全てのブラウザにネイティブ実装されているわけではありません。一方、Vue.js コンポーネントはどんなポリフィル (polyfill) も必要とせず、サポートされる全てのブラウザ (IE9 とそれ以上) で同じ動作をします。必要に応じて、Vue.js コンポーネントはネイティブなカスタム要素内でラップ (wrap) することができます。

2. Vue.js コンポーネントは、（もっとも注目すべき）クロスコンポーネントデータフロー、カスタムイベント通信、そしてトランジションエフェクトでの動的コンポーネント切り替えなどの、プレーンなカスタム要素内で利用できない重要な機能を提供します。

コンポーネントシステムは Vue.js で大規模アプリケーションを構築するための基盤となります。さらに Vue.js のエコシステムは高度なツールと、より"フレームワーク"的なシステムを作成するための多くの補助的なライブラリも提供します。
