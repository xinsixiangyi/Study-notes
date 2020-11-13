```html

	<div id="app" class="demo">
		{{message}}
	</div>
	
```


​			
```jsx
	<script type="text/javascript">
	//第一种方式注册
	// const App={
	// 	data(){
	// 		return{
	// 			message: "HelloVue"
	// 		}
	// 	}
	// }
	// Vue.createApp(App).mount("#app")
	
	//第二种方式注册
	// const App=Vue.createApp({
	// 	data(){
	// 		return{
	// 			message:"Hello Vue"
	// 		}
	// 	}
	// })
	// const vm=App.mount("#app")
	
	//第三种方式
		Vue.createApp({
			data(){
				return{
					message:"Hello Vuex"
				}
			}
		}
		).mount("#app")
	
	</script>
```


