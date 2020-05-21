# Vuex

实现组件全局状态（数据）管理的一种机制，可以方便组件之间数据的共享。

**优点：**

能够在vuex中集中管理共享的数据，易于开发和后期维护；

能够高效地实现组件之间的数据共享，提高开发效率；

存储在vuex中的数据是响应式的，能够实时保持数据与页面的同步。

### 基本使用

1、安装vuex依赖包

```shell
npm install vuex --save
```

2、导入vuex包

```javascript
import Vuex from 'vuex'
Vue.use(Vuex)
```

3、创建store对象

```javascript
const store = new Vuex.Store({
	// state 中存放的就是全局共享数据
	state: {count: 0}
})
```

4、将store对象挂载到vue实例中

```javascript
new Vue({
	el: '#app',
    render: h => h(app),
	router,
    // 将创建的共享数据对象，挂载到Vue实例中
    // 所有的组件可以直接从store中获取全局数据
    store
})
```

5、主要核心概念

**State** 

提供唯一的公共数据源，所有共享的数据都要统一放到Store的State中进行存储。

![image-20200323222322011](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200323222322011.png)

![image-20200323230315714](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200323230315714.png)

**Mutations**

用于变更Store中的数据

注意：在mutations中不要执行异步操作

![image-20200323233659154](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200323233659154.png)

![image-20200323235127381](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200323235127381.png)

**Actions**

用于处理异步任务：如果通过异步操作变更数据，必须通过Action，而不能使用Mutation，但是在Action中还是要通过触发Mutation的方式间接变更数据。

![image-20200324001121492](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200324001121492.png)

![image-20200324002414828](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200324002414828.png)

**Getters**

用于对Store中的数据进行加工处理形成新的数据，且不会影响State中原数据

![image-20200324003129129](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200324003129129.png)

![image-20200324003435279](C:\Users\15740\AppData\Roaming\Typora\typora-user-images\image-20200324003435279.png)













