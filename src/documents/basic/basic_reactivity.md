

# リアクティビティーの基礎

## リアクティブな状態を宣言する

### ref()
```js
import { ref } from 'vue'

const count = ref(0)
```

```js
const count = ref(0)

console.log(count) // { value: 0 }
console.log(count.value) // 0

count.value++
console.log(count.value) // 1
```

- コンポーネントのテンプレート内で ref にアクセスするには、下記に示すように、コンポーネントの setup() 関数で宣言し、それを返します:

```js
import { ref } from 'vue'

export default {
  // `setup` は、Composition API 専用の特別なフックです。
  setup() {
    const count = ref(0)

    // ref をテンプレートに公開します
    return {
      count
    }
  }
}

// observe from html
<div>{{ count }}</div>
```

- テンプレート内で ref を使用する際、.value をつける必要はないことに注意してください。便利なように、ref はテンプレート内で使用されると自動的にアンラップされます（いくつかの注意点があります）。


- イベントハンドラーで直接 ref を変更することもできます:
```js
<button @click="count++">
  {{ count }}
</button>
```
- ⚠️この処理はちゃんと外部に切り出すべきだと思う


- より複雑なロジックの場合、同じスコープで ref を変更する関数を宣言し、状態とともにメソッドとして公開できます:

```js
import { ref } from 'vue'

export default {
  setup() {
    const count = ref(0)

    function increment() {
      // JavaScript 内では .value が必要
      count.value++
    }

    // 関数も公開することを忘れないでください。
    return {
      count,
      increment
    }
  }
}
```

- 公開されたメソッドは、イベントハンドラーとして使用できます:

```html
<button @click="increment">
  {{ count }}
</button>
```

### <script setup>

- setup() で状態やメソッドを手動で公開するのは冗長になりがちです。幸い、単一ファイルコンポーネント（SFC） を使用すれば、これを避けられます。- <script setup> によって使い方を簡略化できます:

⚠️これがテンプレート構文と App.vue で見た一般的な書き方だね
```vue
<script setup>
import { ref } from 'vue'

const count = ref(0)

function increment() {
  count.value++
}
</script>

<template>
  <button @click="increment">
    {{ count }}
  </button>
</template>
```

- <script setup> で宣言されたトップレベルのインポート、変数、関数は、同じコンポーネントのテンプレートで自動的に使用可能になります。
- テンプレートは同じスコープで宣言された JavaScript の関数と同じだと考えれば、当然ながら、一緒に宣言されたすべてのものにアクセスできます。