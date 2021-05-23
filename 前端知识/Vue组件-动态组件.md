## 动态组件

有的时候，在不同组件之间进行动态切换是非常有用的，比如在一个多标签的界面里：

<iframe allowfullscreen="true" allowpaymentrequest="true" allowtransparency="true" class="cp_embed_iframe " frameborder="0" height="300" width="100%" name="cp_embed_6" scrolling="no" src="https://codepen.io/Vue/embed/oNXaoKy?height=300&amp;theme-id=39028&amp;default-tab=result&amp;user=Vue&amp;slug-hash=oNXaoKy&amp;editable=true&amp;pen-title=Component%20basics%3A%20dynamic%20components&amp;name=cp_embed_6" title="Component basics: dynamic components" loading="lazy" id="cp_embed_oNXaoKy" style="width: 740px; overflow: hidden; display: block;"></iframe>

上述内容可以通过 Vue 的 `<component>` 元素加一个特殊的 `is` attribute 来实现：

```html
<!-- 组件会在 `currentTabComponent` 改变时改变 -->
<component :is="currentTabComponent"></component>
```

在上述示例中，`currentTabComponent` 可以包括

- 已注册组件的名字，或
- 一个组件的选项对象