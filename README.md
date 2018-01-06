# vue-demo-Search Github Users


----------




----------


## 在线搜索githup用户

[在线预览][1]

[GitHub源码][2]


----------


----------

技术选型

 1. 使用Vuejs全家桶+ES6+Webpack开发
 2. 采用模块化、组件化、工程化的模式开发
 3. 选用iview ui 组件库
 4. 调用githup api  https://api.github.com/search/users?q=${}


----------
###实现效果如下


![此处输入图片的描述][3]


 


----------
关键代码

```
//实现后生成 html标签
//更新元素的 innerHTML 。内容按普通 HTML 插入 - 不会作为 Vue 模板进行编译
   <p class="card-text" v-html="user.login" >{{user.login}}</p>
//处理数据

for (var i = 0; i < items.length; i++) {
    if (items[i].login.indexOf(this.searchName) == 0) {
    items[i].login = items[i].login.replace(this.searchName, '<span style="background: #ffff00;">' + this.searchName + '</span>');
        }
}


```

  
  


  [1]: https://qaqxiyangyang.github.io/Search-Github-Users/dist/index.html
  [2]: https://github.com/QAQXiYangYang/Search-Github-Users
  [3]: http://img.blog.csdn.net/20170908142145017?
