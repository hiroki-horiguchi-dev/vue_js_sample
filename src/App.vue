<script setup>
import HelloWorld from './components/HelloWorld.vue'
import TheWelcome from './components/TheWelcome.vue'
import { ref } from 'vue'
// write a data for the app like this.
const name = ref('Vue 3 App');
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
