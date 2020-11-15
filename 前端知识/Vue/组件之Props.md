## [#](https://v3.cn.vuejs.org/guide/component-props.html#prop-类型)Prop 类型

到这里，我们只看到了以字符串数组形式列出的 prop：

```js
props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
```

但是，通常你希望每个 prop 都有指定的值类型。这时，你可以以对象形式列出 prop，这些 property 的名称和值分别是 prop 各自的名称和类型：

```js
props: {
  title: String,				//字符串
  likes: Number,				//数字
  isPublished: Boolean,			//布尔类型
  commentIds: Array,			//数组
  author: Object,				//对象
  callback: Function,			//方法
  contactsPromise: Promise 		// 或任何其他构造函数
}
```

## 传递静态或动态的 Prop

我们可以像这样给 prop 传入一个静态的值：

```html
<blog-post title="My journey with Vue"></blog-post>
```

也知道 prop 可以通过 `v-bind` 或简写 `:` 动态赋值，例如：

```html
<!-- 动态赋予一个变量的值 -->
<blog-post :title="post.title"></blog-post>

<!-- 动态赋予一个复杂表达式的值 -->
<blog-post :title="post.title + ' by ' + post.author.name"></blog-post>
```

在上述两个示例中，我们传入的值都是字符串类型的，但实际上任何类型的值都可以传给一个 prop。

### 传入一个数字

```html
<!-- 即便 `42` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue     -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。             -->
<blog-post :likes="42"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post :likes="post.likes"></blog-post>
```

<iframe height="265" style="width: 100%;" scrolling="no" title="first vue" src="https://codepen.io/xinsixiangyi/embed/qBNLepe?height=265&theme-id=light&default-tab=html,result" frameborder="no" loading="lazy" allowtransparency="true" allowfullscreen="true">
  See the Pen <a href='https://codepen.io/xinsixiangyi/pen/qBNLepe'>first vue</a> by xinsixiangyi
  (<a href='https://codepen.io/xinsixiangyi'>@xinsixiangyi</a>) on <a href='https://codepen.io'>CodePen</a>.
</iframe>

### 传入一个布尔值

```vue
<!-- 包含该 prop 没有值的情况在内，都意味着 `true`。-->
<blog-post is-published></blog-post>

<!-- 即便 `false` 是静态的，我们仍然需要 `v-bind` 来告诉 Vue -->
<!-- 这是一个 JavaScript 表达式而不是一个字符串。-->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 用一个变量进行动态赋值。-->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
```

<iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="300" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/Vue/embed/oNXaoKy?height=300&amp;theme-id=39028&amp;default-tab=result&amp;user=Vue&amp;slug-hash=oNXaoKy&amp;editable=true&amp;pen-title=Component%20basics%3A%20dynamic%20components&amp;name=cp_embed_6" title="Component basics: dynamic components" loading="lazy" id="cp_embed_oNXaoKy" style="width: 740px; overflow: hidden; display: block;"></iframe>

