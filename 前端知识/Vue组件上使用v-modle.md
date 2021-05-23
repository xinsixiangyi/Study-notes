### 在组件上使用 v-model

自定义事件也可以用于创建支持 `v-model` 的自定义输入组件。记住：

```html
<input v-model="searchText" />
```

1

等价于：

```html
<input :value="searchText" @input="searchText = $event.target.value" />
```

1

当用在组件上时，`v-model` 则会这样：

```vue
<!DOCTYPE html>
<html>
	<head>
		<meta charset="utf-8">
		<title></title>
		<link rel="stylesheet" href="../src/css/index.css">
		<script src="https://unpkg.com/vue@next"></script>
	</head>
	<body>
		<div id="components-a">
			<custom-input
			  :model-value="searchText"
			  @update:model-value="searchText = $event"
			></custom-input>{{searchText}}
			<hr>
			<input v-model="searchText" />{{searchText}}
		</div>
		<script type="text/javascript">
			const app=Vue.createApp({
				data(){
					return{
					searchText:"hello"
				}
				}	
			})
			 app.component("custom-input",{
				
				template:`<input
							:value="modelValue"
							@input="$emit('update:modelValue', $event.target.value)"
				>
				`,
				props: ['modelValue'],
			}
			).mount("#components-a")
		</script>

	</body>
</html>

```

