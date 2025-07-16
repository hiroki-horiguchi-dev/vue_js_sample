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
          <!-- これやっちゃダメだろ、なんで表示側でロジック組むんだよ -->
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
