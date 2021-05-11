[参考coderwhy的Vue教程](https://www.bilibili.com/video/BV15741177Eh)

### 1. 基础语法

#### 1. v-for

![v-for](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104258549.png)

![v-for参数](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713113824403.png)

![key属性](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713114252466.png)

![:key属性](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713114316760.png)

#### 2. @click 方法不加括号

![@click](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104333958.png)

#### 3. mustache 语法

![mustache](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104657479.png)

#### 4. v-once 只渲染一次

![v-once](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104744056.png)

#### 5. v-html 解析html

![v-html](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104837067.png)

#### 6. v-text

![v-text](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713104925401.png)

#### 7. v-pre 不渲染它及其子元素

![v-pre](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713105112381.png)

#### 8. v-cloak 防止浏览器显示未编译的mustache

![v-cloak](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713105159585.png)

#### 9. v-bind 动态绑定属性

![v-bind](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713105532945.png)

**语法糖：**

![v-bind语法糖](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713105727461.png)

##### 9.1 动态绑定class

1. **对象语法**

![动态绑定class](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110229959.png)

2. **数组语法**

![动态绑定class](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110249399.png)

##### 9.2 动态绑定style

![css属性写法](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110513910.png)

![动态绑定css](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110522928.png)

#### 10. 计算属性

![计算属性](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110921025.png)

![计算属性的get、set](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713110953936.png)

#### 11. v-on 注意传参问题

![v-on用法](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713111302527.png)

![v-on参数问题](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713111419840.png)

#### 12. v-if、v-else-if、v-else

![条件渲染](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713112106634.png)

![例子](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713112114838.png)

![避免元素复用](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713112139664.png)

#### 13. v-show

![v-show](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713112859529.png)

例如根据登录角色权限不同而渲染不同页面，就使用 v-if。

#### 14. v-model 双向绑定

![v-model](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713114624102.png)

![v-model参数](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713120001167.png)

---

---

### 2. 组件化开发

#### 1. 过程：创建-注册-使用

##### 1. 创建与注册全局或局部组件

![创建-注册全局/局部组件](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713121557978.png)

##### 2. 模板分离写法

![模板分离写法](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713121859604.png)

#### 2. 全局组件与局部组件

区别在于注册方式。

![全局与局部组件](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713120757147.png)

#### 3. 父子组件问题

注意：下面这个例子中，vue实例中不能直接使用子组件标签来引入子组件，因为子组件并没有在vue实例中注册，只注册进了父组件。子组件在注册进父组件时，就已经编译完成作为父组件的一部分了。

![注意父子组件](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713121112322.png)

#### 4. 组件的data域问题

注意：组件无法直接访问vue实例中的data，因为组件也相当于一个vue实例，有自己的data域。并且data必须是一个函数，因为组件可能被多个地方引用，造成并发错误。

![子组件不能直接访问父组件data域](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713122206398.png)

![子组件自己的data域](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713122146531.png)

#### 5. 父子组件通信

##### 1. 父传子 props

![props父传子](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713145631152.png)

##### 2. 子传父 event

![$emit子传父](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713150544430.png)

#### 6. 插槽 Slot

##### 1. 匿名插槽

如果传递了内容，\<slot>中的内容将被**全部替换**，否则显示插槽定义时默认的内容。

![匿名插槽](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713150740637.png)

##### 2. 命名插槽

![具名插槽](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713151018565.png)

##### 3. 编译作用域

![编译作用域](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713151118809.png)

##### 4. 作用域插槽

即在Slot上绑定一些数据，供给父组件直接使用。作用：子组件提供数据，父组件负责控制展示样式的时候可以使用。

![作用域插槽](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713151506797.png)

![获取data](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713151518520.png)

---

---

### 3. 模块化开发

#### 1. 一些导入导出写法

<img src="https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713152014058.png" alt="CommonJS导入导出" style="zoom: 67%;" />

![export导出](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713152200097.png)

![export default](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713152307238.png)

![import导入](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713152408190.png)

#### 2. webpack

#### 3. Vue-Cli

----

---

### 4. vue-router

**本质：将路径和组件映射起来，路径的改变就是组件的切换。**

#### 4.1 入门

##### 4.1.1 两种模式

![hash模式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713153650532.png)

![history模式-push](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713153708986.png)

![history模式-replace](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713153838075.png)

![history模式-go](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713153844224.png)

##### 4.1.2 使用步骤

![使用步骤](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155030655.png)

![创建router](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155110779.png)

![注册router](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155057312.png)

![两个组件](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155211363.png)

![配置映射](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155221195.png)

![使用router](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155233255.png)

![router的属性](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155555671.png)

##### 4.1.3 跳转时执行JS代码

![另一种跳转方式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713155706530.png)

#### 4.2 动态路由、懒加载

##### 4.2.1 动态路由

![动态路由](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713160410719.png)

传参：

```js
<router-link :to="{path: '/applicant/companyDetail/' + item.id}"></router-link>
```



##### 4.2.2 懒加载

![懒加载导入](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713160549797.png)

![简写过程分析](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713160610897.png)

#### 4.3 路由嵌套

**注意：**

- `children[ ]` 内部的组件，path不要带 `/`，否则 url 会映射到根路径。

- 在组件内部使用 `router-view` 标签。

![路由嵌套](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713160835366.png)

![默认路径](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713160934106.png)

#### 4.4 参数传递

##### 4.4.1 传递参数

<img src="https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713161305291.png" alt="两种传参方式" style="zoom:67%;" />

![params方式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713161724292.png)

![query方式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713161732606.png)

##### 4.4.2 获取参数

$route对象：封装了当前路由状态的信息。

![$route对象获取参数](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713162643023.png)

![区别](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713162809458.png)

#### 4.5 导航守卫

**作用：用来监听路由状态的切换。**

如，在路由切换后，更改页面title。

如果是afterEach()方法，不需要调用next()。

![使用导航守卫修改title](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713163450413.png)

#### 4.6 keep-alive

![keep-alive缓存组件](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713163748474.png)

---

---

### 5. Vuex

**作用：响应式的状态管理，方便在多个组件间共享数据。**

<img src="https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713200802029.png" alt="原理" style="zoom:67%;" />

![创建Vuex](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713201855358.png)

![注册Vuex](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713201903953.png)

![使用Vuex](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713201934783.png)

1. State

就是要共享的属性。

2. Getters

可以理解为计算属性。

3. Mutations

更新时使用提交Mutations的方式。

- 携带参数：

![Mutations方法参数问题](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202256028.png)

- 两种提交风格

![另一种提交方式](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202339075.png)

- 响应规则

![state响应式规则](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202439482.png)

4. Actions

![异步->同步](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202703432.png)

![dispatch](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202717616.png)

5. Module

![Vuex划分模块](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713202947366.png)

![举例](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713203036256.png)

![举例](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713203116787.png)

---

---

### 6. axios

#### 6.1 Promise

作用：ES6中异步编程的一种解决方案，是CommonJS为异步编程设计得统一接口。

![Promise](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713203643239.png)

![Promise说明](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713203724552.png)

![Promise三种状态](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713204022539.png)

![链式调用](https://cdn.jsdelivr.net/gh/HenryKang99/blog_img/img/image-20200713204157658.png)

#### 6.2 axios 

> 注意回调。

##### 1. 两种封装方式

**方式一：**传入回调方法，在封装体内部回调。

```js
import axios from 'axios'

//传入回调方法
export function request(config, success, failure) {
  //创建axios实例
  const instance = axios.create({
    baseURL: 'xxx',
    method: "get",
    timeout: 5000
  })
  //发送请求
  instance(config)
    .then(res => {
      success(res);
  })
    .catch(err => {
      failure(err);
  })
}

//---调用---
request({
    config: {},
    success: function(res){},
    failure: function(err){}
})
```

**方式二：**直接return，因为axios返回的就是一个promise。

```js
import axios from 'axios'

//可以给默认的axios实例设置default值
//注意只会在默认的axios实例生效
axios.defaults.baseURL = xxx;
axios.defaults.timeout = xxx;

var baseUrl = 'http://localhost/nqa';

export function get(config) {
  const get = axios.create({
    baseURL: this.baseURL,
    method: "get",
    timeout: 5000,
  })
  return get(config);
}

export function post(config) {
  const post = axios.create({
    baseURL: this.baseURL,
    method: "post",
    headers: {'Content-Type': 'application/json'},
    timeout: 5000
  })
  return post(config);
}
//---调用---
import * as req from 'network/request'

req.get({
    url: "/xxx"
}).then(res => {
    doSth...
}).catch(err => {
    doSth...
});
    
req.post({
    url: '/xxx',
    data: {
		key: value
    },
    headers: {
        //eg...
        'Authorization': myToken,
    }
}).then(res => {
    if (res.data.flag) {
        doSth...
    } else {
        doSth...
    }
}).catch(err => {
    doSth...
});
```

---

