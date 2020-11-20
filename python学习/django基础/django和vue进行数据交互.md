### **一、前端请求的封装**

1.将请求地址封装起来，以便日后修改，在src/assets/js目录下创建getPath.js文件

```
export default function getUrl(str) {
　　let url = 'http://localhost:8000/' + str;
　　return url;
}
```

2.在同一个目录下创建axios.js文件

我的前端数据交互使用的模块使用的是axios

```
import axios from 'axios'
import getUrl from './getPath'
export default {
    Get: (config) => {
        axios({
            methods: "get",
            url: getUrl(config.url),
            params: config.params
        }).then((res) => {
            stateDetection(res);
            config.callback && config.callback(res);
        })

    },
    Post: () => {
        axios({
            methods: "post",
            url: getUrl(config.url)
        }).then((res) => {
            stateDetection(res);
            config.callback && config.callback(res);
        })
    }

}
//状态检测
let stateDetection = (data, callback) => {
    let status = data.status_code;
    switch (status) {
        case 102:
            break;
        case 103:
            alert(data.content);
            break;
        case 404:
            window.location.href = data.url;
            break;
    }
}
```

### **二、前端Get请求使用**

1.在src/store/目录下的ArchiveStore.js文件引入axios模块

```
import axios from '../assets/js/axios'
```

2.在src/store/目录下的ArchiveStore.js文件里的state添加文章详情的数据结构

```
 specific: {
            browse: 0,
            content: '',
            title: '',
            date: '',
            tags: []
        }, //文章详情
```



3.在src/store/目录下的ArchiveStore.js文件里创建一个action方法

```
        getArticlesSpecific({ commit, state }, id) { //得到指定文章详情
            axios.Get({
                url: 'get_article_specific',
                params: {
                    id: id
                },
                callback: (res) => {
                    // console.log(res);
                    let data = res.data
                    state.specific = {
                        browse: data['browse'],
                        content: data['content'],
                        title: data['title'],
                        date: data['date'],
                        tags: data['tags']
                    }
                    state.loading = false;
                    //   specific
                }
            })

        }
```



4.在文章详情页面Specificartical.vue(src/components)下执行getArticlesSpecific方法即可



```
<template>
    <div class="specificartical borderStyle container" >
        <h1 class='title'>{{specific.title}} </h1>
        <div class='content'>
             <div><span class='annotation'><i></i>{{specific.date}}</span>/
            <span class='annotation'><i></i>{{specific.browse}}</span>/
            <div>{{specific.content}}</div>
            <div class='attention'><i>@</i></div>
            <div class='lable'><i>*</i><a v-for="(tag,index) in specific.tags" :key="index">{{index!=0?',':''}}{{tag}}</a></div>
        </div>
       
    </div>
</template>

<script>
import {mapState,  mapActions} from 'vuex'
export default {
  name: 'specificartical',
   computed: {
    ...mapState({
        specific:state=>state.ArchiveStore.specific,
    })
  },
  methods:{
    ...mapActions([
       'getArticlesSpecific'
    ]), 
  },
  activated:function(){this.getArticlesSpecific(this.$route.params.id);
  }
}
</script>
```

在这里要注意的是使用activated生命周期函数，该函数会在keep-alive，组件被激活时调用

### **三、后端Get请求使用**

1.在urls.py(djangoBlog)文件下面引入views.py里面的方法

```python
from blog.views import *
```

2.注册url

```python
from blog.views import *
urlpatterns = [
    url(r'^get_article_specific/$', getArticleSpecific, name='get_article_specific'),
]
```

3.在views.py里面导入需要用到的模块和models

 

```python
from blog.models import *
from django.http import JsonResponse
from django.db.models.functions import TruncDate
```

 

4.在views.py里面添加getArticleSpecific方法

```python
#得到文章详情
def getArticleSpecific(request):
    results={}
    #得到标签数组
    temp=list(Article.objects.get(id=request.GET['id']).tag.values_list('name')  )
    results['tags']=[]
    #处理标签数组的格式
    for value in temp:
        results['tags'].append(value[0])
    #得到文章详情
    data=Article.objects.annotate(date=TruncDate('create_time')).values('title','content','browse','date').get(id=request.GET['id'])
    results['browse']=data['browse']
    results['title']=data['title']
    results['content']=data['content']
    results['date']=data['date']
    results['status_code']=102
    return JsonResponse(results, safe=False)
```

