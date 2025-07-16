<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import { ref } from 'vue'
// write a data for the app like this.
const name = ref('Vue 3 App');
const isButtonDisabled = ref(false);
</script>

<template>
  <header>
    <img alt="Vue logo" class="logo" src="./assets/logo.svg" width="125" height="125" />

    <div class="wrapper">
      <HelloWorld msg="You did it!" />
      <!-- 一言で言えば、Vue.js は、リアクティブなデータバインディングと仮想DOM(木構造なので DFS, BFS で更新)を使って、データの変化に応じてUIを効率的に再描画するフロントエンドフレームワーク -->
       <!-- {{}}: マスタッシュ構文 -->
      <h4>"Hellow World {{name}}"<br /></h4>
      <!-- v-html: ディレクティブ、はレンダリングされるDOMに特別なリアクティブな振る舞いを割り当てる -->
      <!-- この例では、現在アクティブなインスタンスが持つ rawHtml というプロパティをこの要素の inner HTML に適用して最新に保つ、が書かれている -->
      <!-- ええと、pタグ？が持つ rawHtml というプロパティをってことかな -->
      <!-- span の中身は rawHtml プロパティが持つ値に置き換えられ、プレーンな HTML として解釈される -->
      <!-- データバインディングは無視される -->
      <!-- v-html は、テンプレートの断片を組み立てるのには利用できない、Vue が文字列ベースのテンプレートエンジンではないから-->
      <!-- それに代わり、UI の再利用や組み立ての基本単位として推奨されているのが「コンポーネント」 -->
       <!-- ⚠️ ウェブサイト上で任意の HTML を動的にレンダリングすることは、クロスサイトスクリプティング（XSS）脆弱性を招きやすく、非常に危険、
        v-html は信頼できるコンテンツにのみ使用し、ユーザーから渡されるコンテンツには決して使用してはいけない -->
      <p>Using v-html directive: <span v-html="rawHtml"></span></p>
      <!-- attribute binding -->
        <!-- HTML 属性の中ではマスタッシュ構文が使えません。代わりに、以下の v-bind ディレクティブを使用 -->
        <!-- この v-bind ディレクティブは、要素の id という属性を、コンポーネントが持つ dynamicId というプロパティと同期させるよう Vue に指示している -->
        <!-- バインドされた値が null または undefined の場合、その属性はレンダリングされる要素から除外される -->
        <div v-bind:id="dynamicId"></div>
      <!-- 省略記法 -->
        <!-- v-bind は使用頻度が非常に高いため、以下の専用の省略記法がある -->
        <div :id="dynamicId"></div>
        <!-- : で始まる属性は、普通の HTML の記法とは少し異なるように見えますが、実際には属性名として有効な文字 -->
        <!-- Vue をサポートするすべてのブラウザーは、これを正しく解析することができる -->
        <!-- なお、これは最終的にレンダリングされるマークアップには現れない -->
        <!-- この省略記法を使うかどうかは任意ですが、その使い方を後ほど詳しく知れば、良さがわかるはず.⚠️以降のガイドでは省略記法 -->
      <!-- 同名省略記法 -->
        <!-- 3.4 以上でのみサポート -->
        <!-- vue_js_sample@0.0.0 /Users/hiroki.horiguchi/WorkSpace/Vue_js/vue_js_sample
          ├─┬ @vitejs/plugin-vue@6.0.0
          │ └── vue@3.5.17 deduped
          ├─┬ vite-plugin-vue-devtools@7.7.7
          │ └─┬ @vue/devtools-core@7.7.7
          │   └── vue@3.5.17 deduped
          └─┬ vue@3.5.17
            └─┬ @vue/server-renderer@3.5.17
              └── vue@3.5.17 deduped -->
        <!-- 現在の環境は 3.5 なので使える -->
        <!-- バインドされている JavaScript の値の変数名と属性が同じ名前を持っている場合、属性値を省略するために構文をさらに短くすることができる -->
        <!-- :id="id" と同じ -->
        <div :id></div>
        <!-- 同じように動きます -->
        <div v-bind:id></div>
        <!-- これは、JavaScript でオブジェクトを宣言するときのプロパティ省略記法に似ている -->
        <!-- この機能は Vue 3.4 以上でのみ使用できる -->
      <!-- Boolean 属性 -->
        <!-- 例えば、disabled は最も一般的に用いられるブーリアン属性の 1 つ -->
        <!-- 以下のケースでは、v-bind は少し特別な動作をします: -->
        <button :disabled="isButtonDisabled">Button</button>
        <!-- この disabled という属性は、isButtonDisabled が 真値（truthy value） である場合に要素に含まれます -->
        <!-- また、<button disabled=""> との一貫性を保つため、値が空の文字列である場合にも含まれます -->
        <!-- それ以外の偽値（falsy values） の場合には、属性が要素から取り除かれます -->
        <!-- ⚠️実験: JS の言語仕様も抑えないと、この辺の制御でバグらせる可能性があるなぁ
          :disabled="0" -> true, 
          :disabled="1" -> false, 
          :disabled="null" -> true (これ厄介), 
          :disabled="" ->  error [plugin:vite:vue] v-bind is missing expression.   
          -->
      <!-- 複数の属性を動的にバインドさせる -->
       <!-- const objectOfAttrs = {
           id: 'container',
           class: 'wrapper',
           style: 'background-color:green'
        } -->
        <!-- 何これ、クラス？リスト？と思ったが、オブジェクトらしい。key, value を持つ JavaScript オブジェクトだそうで -->
        <!-- 辞書型と理解して良さそう -->
        <!-- 以下のように v-bind を引数なしで用いると、これらの複数の属性を 1 つの要素にバインドすることができます: -->
        <!--  -->
        <div v-bind="objectOfAttrs"></div>
      <!-- JavaScript の式を用いる -->
        <!-- {{ number + 1 }}
          {{ ok ? 'YES' : 'NO' }}
          {{ message.split('').reverse().join('') }}
          <div :id="`list-${id}`"></div> -->
        <!-- これらの式は、現在のコンポーネントインスタンスのデータスコープ内で、JavaScript の式として評価されます。 -->
        <!-- Vue のテンプレートでは、以下の場所で JavaScript の式を使用することができます: -->
          <!-- テキスト展開の内部（マスタッシュ構文内）-->
          <!-- 任意の Vue ディレクティブ（v- で始まる特殊な属性）の属性値の中身  -->
          <!-- インクリメントとか意味不明な目ジックナンバーなんだからやっちゃダメだろ、なんで表示側でロジック組むんだよ: まあ三項演算子くらいならいいか -->
        <!-- 式に限られる -->
          <!-- それぞれのバインディングには、単一の式しか含めることができません。式とは、ある値に対して評価されるコードの一部分です。簡単なチェックは、return の後に使えるかどうかです。 -->
          <!--  これは文であり、式ではありません: 
            {{ var a = 1 }}
            フロー制御も動作しません。代わりに三項演算子を使用してください。
            {{ if (ok) { return message } }} 
          -->
        <!-- 関数の呼び出し -->
          <!-- コンポーネントから公開されているメソッドであれば、以下のようにバインディングの式の内部で呼び出すことができます -->
          <!-- 
            <time :title="toTitleDate(date)" :datetime="date">
              {{ formatDate(date) }}
            </time> 
          -->
          <!-- バインディングの式の内部で呼び出される関数は、コンポーネントが更新されるたびに呼び出されます。そのため、データの変更や非同期処理をトリガーするような副作用を持たせてはいけません。 -->
          <!-- 純粋関数を使っとけば問題ないっていう話 -->
        <!-- グローバルへのアクセスの制限 -->
          <!-- テンプレートで用いる式はサンドボックス内で実行され、限定的なグローバルのリストにのみアクセスできます -->
          <!-- このリストには、Math や Date などのよく使われる組み込みグローバルが含まれています -->
          <!-- ユーザーが window に付与したプロパティなど、このリストに明示的に含まれていないグローバルには、テンプレート内の式からアクセスすることができません -->
          <!-- ただし、app.config.globalProperties に追加することにより、Vue のあらゆる式で利用できるグローバルを明示的に定義することができます  -->
          <!-- 詰まるところ、グローバル変数にはアクセスできませんよと、JS でグローバル変数は windows.xxx みたいに定義できるらしい、これにアクセスできないよって話 -->
        <!-- ディレクティブ -->
          <!-- 動的、リアクティブにDOM操作をすることが目的、メインで書いてあるのは記法の話なので本質的ではないかも、、ボイラープレートコードを省けることくらい -->
          <!-- ディレクティブは、v- という接頭辞を持つ特別な属性です。Vue では、上で紹介した v-html や v-bind をはじめ、数々の組み込みディレクティブが用意されています。
               ディレクティブの属性値は、JavaScript の単一の式であることが期待されます（ただし v-for、v-on、v-slot は例外であり、後ほどそれぞれのセクションで説明します）。ディレクティブの役割は、式が示す値が変化したとき、リアクティブに更新を DOM に適用することです。例えば、v-if を取り上げてみます: -->
            <!-- DOM って何？:  -->
              <!-- この辺参考: https://ja.vuejs.org/guide/extras/rendering-mechanism.html#virtual-dom -->
              <!-- HTML, XML を木構造で表現し、JavaSctipt から操作できるようにしたもの -->
             <!-- <div id="app">
                    <p>Hello</p>
                  </div>
              -->
              <!-- なるほどね、値の更新をする際に BFS, DFS を木構造の走査で使うんだな -->
              <!-- 仮想DOM は DOM を直接操作するんじゃなくて、間に diff をおいて、差分だけ本物の DOM を更新するみたいな処理にするんやなめちゃ理解した -->
            <p v-if="seen">Now you see me</p>
            <!-- この例では v-if ディレクティブが、式 seen の値の真偽に基づいて <p> 要素を削除または挿入します。 -->
            <!-- ⚠️結構前の、isButtonDisabled こいつを ref しているのがそもそもディレクティブの一つで、DOM 操作をしていることになる -->
          <!-- 引数 -->
            <!-- 一部のディレクティブは引数を取ることができます。引数は、ディレクティブ名の後にコロンで示します。以下は、v-bind ディレクティブを使って HTML 属性の 1 つをリアクティブに更新する例です: -->
            <!-- 
              <a v-bind:href="url"> ... </a>
              省略記法 
              <a :href="url"> ... </a> -->
            <!-- 何をしているかっていうと、この a タグが url を参照するようになっていて、動的にリンクの向き先を変えられるよって話ね -->
            <!-- 
              <a v-on:click="doSomething"> ... </a>
              省略記法
              <a @click="doSomething"> ... </a> 
            -->
            <!-- これも同様に、onClick の際に発火する doSomething メソッドを動的に変えられることに意味があるんだろう -->
          <!-- 動的引数 -->
           <!--
              引数で使用できる式には、いくつか制約があります。詳細は以下の
            「動的引数の値に関する制約」および「動的引数の構文上の制約」セクションで説明します。
            -->
            <!-- 
              <a v-bind:[attributeName]="url"> ... </a>
              省略記法 
              <a :[attributeName]="url"> ... </a> 
            -->
            <!-- この例では、attributeName が JavaScript の式として動的に評価され、そこで評価された値が最終的な引数を指す値として使用されます。
             例えば、コンポーネントのインスタンスが attributeName というデータプロパティを持ち、その値が "href" のとき、このバインディングは v-bind:href と同等になります。 -->
            <!-- 同じように、動的引数を用いてハンドラーを動的なイベント名にバインドすることもできます: -->
              <!-- <a v-on:[eventName]="doSomething"> ... </a> -->
              <!-- 省略記法 -->
              <!-- <a @[eventName]="doSomething"> ... </a> -->
            <!-- v-on 系は onFocus, onClick, key.enter みたいなイベントハンドリング系だな -->
            <!-- この例では、eventName の値が "focus" のとき、v-on:[eventName] が v-on:focus と同等になります。 -->
          <!-- 動的引数の値に関する制約 -->
              <!-- 動的引数は、評価結果が null または文字列のいずれかになることが期待されます。null は特別な値で、バインディングを削除することを明示的に表します。それ以外の非文字列の値を指定すると、警告が発生します。 -->
          <!-- 動的引数の構文上の制約 -->
              <!-- 動的引数の式には、構文上の制約がいくつかあります。これは、スペースや引用符など特定の文字が HTML の属性名の中では無効となるためです。例えば、次のようなものは無効となります: -->
              <!-- この場合、コンパイラーで警告が発生します。 -->
              <!-- <a :['foo' + bar]="value"> ... </a> -->
              <!-- 複雑な動的引数を渡す必要がある場合は、後ほど取り上げる算出プロパティを使用するとよいでしょう。 -->
              <!-- また、HTML ファイルに直接記述する DOM 内テンプレートを使用する場合、ブラウザーでは属性名が小文字であることが求められるため、以下のように大文字のキー名を使用することは避ける必要があります: -->
              <!-- <a :[someAttr]="value"> ... </a> -->
              <!-- 上のコードは、DOM 内テンプレートでは :[someattr] に変換されます。もしコンポーネントに someattr ではなく someAttr というプロパティしかなければ、このコードは動作しません。単一ファイルコンポーネント内のテンプレートは、この制約の対象外です。 -->
          <!-- 修飾子 -->
              <!-- 修飾子は、ドット（.）で示される特別な接頭辞で、ディレクティブを何らかの特別な方法でバインドすることを表します。例えば、以下に示す .prevent という修飾子は、イベントがトリガーされたときに event.preventDefault() を呼び出すことを v-on ディレクティブに伝えます: -->
              <!-- <form @submit.prevent="onSubmit">...</form> -->
              <!-- この後、v-on 向けや v-model 向けの修飾子の例を、その機能のページで見ることになるでしょう。 -->
              <!-- ⚠️全くよくわからないが、prevent を指定すると
                  1. event.preventDefault() を Vue が自動で呼び出してくれる
                  2. ページリロードが起こらない
                  3. Vue アプリの状態がそのまま保持される
                  4. onSubmit() 内で、API を叩くなどのカスタム処理ができる

                  methods: {
                    onSubmit(event) {
                      // Vue がこれを自動でやってくれてる ↓
                      event.preventDefault();

                      // 自分のロジック
                      this.sendFormData();
                    }
                  }
              -->
              <!-- 要するに、画面遷移ないしページロードをさせずに自分の書いた処理を実行させたい時に書く -->
              <!-- prevent は修飾子というらしい(修飾子ってアクセス修飾子以外で初めて聞いたぞ) -->
    </div>
  </header>

  <main>
    <TheWelcome />
  </main>
</template>

<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
}
</style>
