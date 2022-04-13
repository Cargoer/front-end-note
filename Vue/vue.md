## Vue

### v-once?

> 只渲染元素和组件**一次**。随后的重新渲染，元素/组件及其所有的子节点将被视为静态内容并跳过。这可以用于优化更新性能。

```vue
<!-- 单个元素 -->
<span v-once>This will never change: {{msg}}</span>
<!-- 有子元素 -->
<div v-once>
  <h1>comment</h1>
  <p>{{msg}}</p>
</div>
<!-- 组件 -->
<my-component v-once :comment="msg"></my-component>
<!-- `v-for` 指令-->
<ul>
  <li v-for="i in list" v-once>{{i}}</li>
</ul>
```

### v-html

让原生html代码不被作为字符串显示，而是显示对应的html元素

```vue
<div>{{rawHtml}}</div>
<div v-html="rawHtml"></div>

export default {
	data(): {
		return {
			rawHtml: "<p style='color:red'>should be red</p>"
		}
	}
}
```

显示结果：

\<p style='color:red'>should be red\</p>

should be red // 是红色的

### v-bind&v-on的动态参数

```vue
<button @[event_name]="doSomething">{{button_name}}</button>

export default {
	data(): {
		return {
			event_name: 'click'
		}
	}
}
```

坑点：event_name不能出现大写，会被当做小写处理，从而找不到对应的变量报错。

### computed

和method区别：

> **计算属性是基于它们的响应式依赖进行缓存的**。只在相关响应式依赖发生改变时它们才会重新求值。

```vue
reversedMessage: function () {
    return this.message.split('').reverse().join('')
}
// 放在computed的话只要message不变就不会再重新执行
```

* 当v-model双向绑定了computed的东西时，需要对computed同时设置get和set，否则当数据发生变更的话会报错：Computed property "listMode" was assigned to but it has no setter.

```javascript
computed: {
    modelData: {
        get() {
            return this.modelData
        }
        set(new_modelData) {
            this.modelData = new_modelData
        }
    }
}
```



### Class绑定

```vue
<div 
     class="static" 
     :class="{active: isActive, 'text-danger':hasError}">
</div>
data: {
    isActive: true,
    hasError: false
}
```

```vue
<div v-bind:class="classObject"></div>
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

```vue
<div v-bind:class="[activeClass, errorClass]"></div>
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

### 绑定内联样式

to be filled...

### v-for相关

```vue
<ul>
    <!--items数组 -->
    <li v-for="(index, item) in items" :key="item.id">{{index}}-{{item}}</li>
</ul>
<ul>
    <!-- object对象 -->
    <li v-for="(index, name, value) in object" :key="name">{{index}}-{{name}}:{{value}}</li>
</ul>
```

为什么要有v-key？

-解释1：指定唯一可标识的key可以防止数据发生改变时的重复渲染，如数组在中间插入，默认用index为key值的话插入位置及以后都要重新渲染

### 事件修饰符

引子

```vue
<input @click="doSomething('arg', $event)">
var vm = new Vue(
	el: "input"
	methods: {
		doSomething: function(arg, event){
			if(event){
				event.preventDefault()
			}
		}
	}
)
```

> 在事件处理程序中调用 `event.preventDefault()` 或`event.stopPropagation()` 是非常常见的需求。尽管我们可以在方法中轻松实现这点，但更好的方式是：方法只有纯粹的数据逻辑，而不是去处理 DOM 事件细节。

```vue
<!-- 阻止单击事件继续传播 -->
<a v-on:click.stop="doThis"></a>

<!-- 提交事件不再重载页面 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 修饰符可以串联 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 只有修饰符 -->
<form v-on:submit.prevent></form>

<!-- 添加事件监听器时使用事件捕获模式 -->
<!-- 即内部元素触发的事件先在此处理，然后才交由内部元素进行处理 -->
<div v-on:click.capture="doThis">...</div>

<!-- 只当在 event.target 是当前元素自身时触发处理函数 -->
<!-- 即事件不是从内部元素触发的 -->
<div v-on:click.self="doThat">...</div>

<!-- 2.1.4 新增 -->
<!-- 点击事件将只会触发一次 -->
<a v-on:click.once="doThis"></a>

<!-- 2.3.0 新增 -->
<!-- 滚动事件的默认行为 (即滚动行为) 将会立即触发 -->
<!-- 而不会等待 `onScroll` 完成  -->
<!-- 这其中包含 `event.preventDefault()` 的情况 -->
<div v-on:scroll.passive="onScroll">...</div>
```

.native?

> 总结： .native - 主要是给自定义的组件添加原生事件。

* 键盘事件的修饰符

参考：[键盘事件修饰符](https://blog.csdn.net/weixin_46071217/article/details/108654509)

```vue
<input @keyup.13="submit">
<input @keyup.enter="submit">
<input @keydown.ctrl.13="submit">
```

### v-model相关

值绑定

```vue
<input type="radio" v-model="pick" v-bind:value="a">
// 当选中时
vm.pick === vm.a
```

```vue
<select v-model="selected">
    <!-- 内联对象字面量 -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
// 当选中时
typeof vm.selected // => 'object'
vm.selected.number // => 123
```

修饰符

.lazy：

在默认情况下，`v-model` 在每次 `input` 事件触发后将输入框的值与数据进行同步 。添加 `lazy` 修饰符，从而转为在 `change` 事件_之后_进行同步

```vue
<!-- 在“change”时而非“input”时更新 -->
<input v-model.lazy="msg">
```

.number：自动将用户的输入值转为数值类型

```vue
<input v-model.number="age" type="number">
```

.trim：自动过滤用户输入的首尾空白字符

```vue
<input v-model.trim="msg">
```

### 插槽相关

传递自定义组件标签之间的内容

```vue
<navigation-link url="/profile">
  Your Profile
</navigation-link>
```

`<navigation-link>` 的模板中可能会写为：

```vue
<a
  v-bind:href="url"
  class="nav-link"
>
  <slot></slot>
</a>
```

可参考：[Vue插槽的理解和使用](https://blog.csdn.net/bobozai86/article/details/105473445)

具名插槽

```vue
// child1
<template>
    <div>
        <div class="header">
            <h1>我是页头标题</h1>
            <div>
                <slot name="header"></slot>
            </div>
        </div>
        <div class="footer">
            <h1>我是页尾标题</h1>
            <div>
                <slot name="footer"></slot>
            </div>
        </div>
    </div>
</template>
```

```vue
<template>
<div>
    <div>slot内容分发</div>
    <child1>
        <template v-slot="header">
            <p>我是页头的具体内容</p>
        </template>
        <template v-slot="footer">
            <p>我是页尾的具体内容</p>
        </template>
    </child1>
</div>
</template>
<!-- 具名插槽的缩写为：v-slot替换成# -->
<template>
<div>
    <div>slot内容分发</div>
    <child1>
        <template #header>
            <p>我是页头的具体内容</p>
        </template>
        <template #footer>
            <p>我是页尾的具体内容</p>
        </template>
    </child1>
</div>
</template>
```

默认插槽

```vue
// child1
<template>
    <div>
        <div class="header">
            <h1>我是页头标题</h1>
            <div>
                <slot name="header"></slot>
            </div>
        </div>
        <div>
            <h1>我是默认插槽的标题</h1>
            <!-- 这里会插入没有v-slot的内容 -->
            <div><slot></slot></div>
    	</div>
        <div class="footer">
            <h1>我是页尾标题</h1>
            <div>
                <slot name="footer"></slot>
            </div>
        </div>
    </div>
</template>
```

```vue
<template>
<div>
    <div>slot内容分发</div>
    <child1>
        <template v-slot="header">
            <p>我是页头的具体内容</p>
        </template>
        <template v-slot="footer">
            <p>我是页尾的具体内容</p>
        </template>
		<template>
			<h1>我是默认插槽具体内容</h1>
		</template>
    </child1>
</div>
</template>
```

作用域插槽

简单理解：父组件可以访问子组件的数据

```vue
<!-- 子组件 -->
<template>
	<span>
        <slot v-bind:user="user">
            <!-- 默认值 -->
            {{ user.lastName }}
    	</slot>
    </span>
</template>

<script>
    export default {
        name: "currentUser",
        data() {
            return {
                user: {
                    firstName: "Kevin",
                    lastName: "Pearson"
                }
            }
        }
    }
</script>
```

```vue
<template>
	<current-user>
        <!-- default对应默认插槽 -->
        <!-- 思考：v-slot:slot-name是不是对应具名插槽 -->
        <template v-slot:default="slotProps">
            {{ slotProps.user.firstName }}
		</template>
    </current-user>
</template>
<!-- 当组件内只有默认插槽时，可以把v-slot直接用在组件上 -->
<template>
	<current-user v-slot:default="slotProps">
        {{ slotProps.user.firstName }}
    </current-user>
</template>
<!-- 再简化，因为只有默认插槽，default可省略 -->
<!-- 但当有具名插槽时不可混用 -->
<template>
	<current-user v-slot="slotProps">
        {{ slotProps.user.firstName }}
    </current-user>
</template>
<!-- 解构插槽Prop -->
<template>
	<current-user v-slot="{ user }">
        {{ user.firstName }}
    </current-user>
</template>
<!-- 可以重命名 -->
<template>
	<current-user v-slot="{ user: person }">
        {{ person.firstName }}
    </current-user>
</template>
<!-- 定义后备内容，用于插槽 prop 是 undefined 的情形 -->
<template>
	<current-user v-slot="{ user = { firstName: 'Jack' } }">
        {{ user.firstName }}
    </current-user>
</template>
```

> 只要出现多个插槽，请始终为*所有的*插槽使用完整的基于 `<template>` 的语法

动态插槽名：类似动态参数

### 生命周期

> onShow在每次打开页面都会加载数据，可以用于数据在需要刷新的环境下
>
> onLoad只是在第一次进入页面会刷新数据，从二级页面回来不会重新加载数据

### 动态组件

<keep-alive>

用来保持组件的状态，以避免反复重渲染导致的性能问题

```vue
<keep-alive>
  <component v-bind:is="currentTabComponent"></component>
</keep-alive>
```

### 异步组件

to be filled

### Prop验证

```vue
Vue.component('my-component', {
  props: {
    // 基础的类型检查 (`null` 和 `undefined` 会通过任何类型验证)
    propA: Number,
    // 多个可能的类型
    propB: [String, Number],
    // 必填的字符串
    propC: {
      type: String,
      required: true
    },
    // 带有默认值的数字
    propD: {
      type: Number,
      default: 100
    },
    // 带有默认值的对象
    propE: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: function () {
        return { message: 'hello' }
      }
    },
    // 自定义验证函数
    propF: {
      validator: function (value) {
        // 这个值必须匹配下列字符串中的一个
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
```

### 访问元素&组件

访问根实例：this.$root.attribute

访问父级组件实例：this.$parent.attribute **慎用** 

访问子组件实例或子元素：this.$refs.childRegisteredName

```vue
<base-input ref="usernameInput"></base-input>

<script>
    this.$refs.usernameInput // 访问这个 <base-input> 实例
</script>
```

### \<transition>相关

to be filled

### mixin

初印象：和组件类似，但是引入后可以作为父组件的扩充，可以存放一些公共的或通用的数据方法

**应用场景**：

有两个非常相似的组件，他们的基本功能是一样的，但他们之间又存在着足够的差异性。可以用mixin存放相似的部分，然后分别引入。

> Mixin对于封装一小段想要复用的代码来讲是有用的。

**选项合并**：

data、methods、components、directives：并集，发生冲突组件优先，同名的取组件的值

钩子函数：先调用mixin再调用组件

自定义选项合并策略：见文档

**实践的坑和总结**：

* mixin的数据更新并不会同步到组件的数据更新，相当于只把一开始的数据给到组件，之后便再无瓜葛了。所以当v-for使用的是在mixin中定义的数据时，无法进行更新渲染。

### 自定义指令

directives，待实践补充

### 渲染函数

render()

和React有点类似

### 过滤器

filters，

使用：

```vue
<!-- 在双花括号中 -->
{{ message | filter1 }}

<!-- 在 `v-bind` 中 -->
<div v-bind:id="rawId | filter2"></div>
```

过滤器可以串联，如下，message为filterA的第一个参数，filterA也可以定义多个参数；filterA的结果为filterB的参数，以此类推，最后一个过滤器的结果为最终显示结果。

```vue
{{ message | filterA[('arg1', arg2,...)] | filterB }}
```

### 路由

to be filled

### setup()

```vue
setup(props, context){
	// 没有this，this变成context
	// 可以存放所有逻辑代码
}
```

### vuex

**使用**

```javascript
import Vue from 'vue'
import Vuex from 'vuex'
Vue.use(Vuex)
const store = new Vuex.Store({
    ...
})
export default store
```

#### state

存放共享数据或状态

```javascript
state: {
    nickname: 'joe',
    age: 17,
    gender: 'boy'
}
```

* mapState

简单理解：简化调用代码

参考：[知乎@全栈前端-vuex：弄懂mapState...](https://zhuanlan.zhihu.com/p/100941659)

```javascript
import {mapState} from 'vuex'
export default {
    name: 'home',
    computed: mapState(['nickname', 'age', 'gender'])
}
// computed里的代码相当于：
nickname() {return this.$store.state.nickname}
age() {return this.$store.state.age}
gender() {return this.$store.state.gender}
// 这样就可以直接在html代码里使用{{ nickname }}了

// 当有自定义计算属性时，用对象展开运算符
computed: {
    ...mapState(['nickname', 'age', 'gender'])
    //...
}
```

#### getter

getter可以调用其他getter方法

```javascript
getters: {
    doneTodos: state => {
        return state.todos.filter(todo => done)
    },
    doneTodosCount: (state, getters) => {
        return getters.doneTodos.length
    }
}
```

getter可以返回一个函数

```javascript
getters: {
    getTodoById: (state) => (id) => {
        return state.todos.find(todo => todo.id === id)
    }
}
// use
store.getters.getTodoById(2)
```

**mapGetters**：类似mapState，可以省去写$store.getters.

```javascript
computed: {
    ...mapGetters([
        'doneTodoCount',
        'anotherGetter',
        // ...
        // 可以对getter重命名
        done: 'doneTodo'
    ])
}
```

#### mutation

> 更改 Vuex 的 store 中的状态的唯一方法是提交 mutation

```javascript
const store = new Vuex.Store({
    state: {count: 1},
    mutations: {
        increment(state) {
            state.count++
        },
        incrementN(state, n) {
            state.count += n
        }
        incrementM(state, payload) {
            state.count += payload.amount
        }
    }
})
```

> 你不能直接调用一个 mutation handler。这个选项更像是事件注册：“当触发一个类型为 `increment` 的 mutation 时，调用此函数。”要唤醒一个 mutation handler，你需要以相应的 type 调用 **store.commit** 方法

```javascript
store.commit('increment')
// 你可以向 store.commit 传入额外的参数，即 mutation 的 载荷（payload）：
store.commit('incrementN', 10)
// 在大多数情况下，载荷应该是一个对象
store.commit('incrementM', {
    amount: 10
})
// 直接使用包含type属性的对象，type即mutation方法名
store.commit({
    type: 'incrementM',
    amount: 10
})
```

**mapMutations**：类似mapGetters

```javascript
methods: {
  ...mapMutations([
    'increment', // 将 `this.increment()` 映射为 `this.$store.commit('increment')`

    // `mapMutations` 也支持载荷：
    'incrementBy' // 将 `this.incrementBy(amount)` 映射为 `this.$store.commit('incrementBy', amount)`
  ]),
  ...mapMutations({
    add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
  })
}
```

Mutation必须是**同步**函数？

to be known

#### action

Action 类似于 mutation，不同在于：

- Action 提交的是 mutation，而不是直接变更状态。
- Action 可以包含任意**异步**操作。

Action 通过 `store.dispatch` 方法触发

**mapAction**：和mapMutation类似

to be completed

#### module

> 当应用变得非常复杂时，store 对象就有可能变得相当臃肿。
>
> 为了解决以上问题，Vuex 允许我们将 store 分割成**模块（module）**。每个模块拥有自己的 state、mutation、action、getter、甚至是嵌套子模块——从上至下进行同样方式的分割

```javascript
const moduleA = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... },
  getters: { ... }
}

const moduleB = {
  state: () => ({ ... }),
  mutations: { ... },
  actions: { ... }
}

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
  }
})

store.state.a // -> moduleA 的状态
store.state.b // -> moduleB 的状态
```

to be filled

### api

* 自定义放于./api文件夹中
* 在入口文件main.js中挂载

```javascript
import Vue from 'vue'
import {dataApi} from '@/api/api'
Vue.prototype.$Api = {
    dataApi: dataApi,
};
```

* 具体使用

```javascript
this.$Api.dataApi.Api_name(para)
```

### 路由

安装使用

```shell
npm install vue-router
```

```javascript
import Vue from 'vue'
import VueRouter from 'vue-router'
Vue.use(VueRouter)
```

简单的例子

```html
<div>
    <router-link to="/foo">Go to Foo</router-link>
    <router-link to="/bar">Go to Bar</router-link>
</div>
<!-- 路由匹配到的组件将渲染在这里 -->
<router-view></router-view>
```

```javascript
// 组件
const Foo = { template: '<div>foo</div>'}
const Bar = { template: '<div>bar</div>'}

// 定义路由
const routes = {
    {path: '/foo', component: Foo},
    {path: '/bar', component: Bar},
    // 重定向，首次进入显示Foo
    {path: '/', redireat: '/foo'}
}

// 创建路由实例
const router = new VueRouter({
    routes
})

// 注入路由
const app = new Vue({
    router
}).$mount('#app')
```

> 通过注入路由器，我们可以在任何组件内通过 `this.$router` 访问路由器，也可以通过 `this.$route` 访问当前路由

#### 动态路由

```javascript
const User = {
  template: '<div>User {{$route.params.id}}</div>'
}

const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { path: '/user/:id', component: User }
  ]
})
```

#### 嵌套路由

```html
<!-- User -->
<template>
    <div>
        <h1>
            User: {{$route.params.id}}
        </h1>
        <route-view></route-view>
    </div>
</template>
```

```javascript
const router = new VueRouter({
  routes: [
    // 动态路径参数 以冒号开头
    { 
        path: '/user/:id', 
        component: User,
        children: [
            {
                path: 'profile',
                component: UserProfile
            }
        ]
    }
  ]
})
```

#### 编程式

* router.push
* router.replace
* router.go

#### 命名路由和视图

to be filled

### bootstrap-table-vue

[bootstrap-table-vue文档](https://www.bootstrap-table.com.cn/doc/vuejs/introduction/)

* 安装

```shell
npm install bootstrap-table
```

* 使用

```vue
<div id="table">
  <bootstrap-table :columns="columns" :data="data" :options="options"></bootstrap-table>
</div>

<script>
  new Vue({
    el: '#table',
    components: {
      'BootstrapTable': BootstrapTable
    },
    data: {
      columns: [
        {
          title: 'Item ID',
          field: 'id'
        },
        {
          field: 'name',
          title: 'Item Name'
        }, {
          field: 'price',
          title: 'Item Price'
        }
      ],
      data: [
        {
          id: 1,
          name: 'Item 1',
          price: '$1'
        }
      ],
      options: {
        search: true,
        showColumns: true
      }
    }
  })
</script>
```

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
    <title>Hello, Bootstrap Table!</title>

    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/css/bootstrap.min.css" integrity="sha384-GJzZqFGwb1QTTN6wy59ffF1BuGJpLSa9DkKMp0DgiMDm4iYMj70gZWKYbI706tWS" crossorigin="anonymous">
    <link rel="stylesheet" href="https://use.fontawesome.com/releases/v5.6.3/css/all.css" integrity="sha384-UHRtZLI+pbxtHCWp1t77Bi1L4ZtiqrqD80Kn4Z8NTSRyMA2Fd33n5dQ8lWUE00s/" crossorigin="anonymous">
    <link rel="stylesheet" href="https://unpkg.com/bootstrap-table@1.15.3/dist/bootstrap-table.min.css">
  </head>
  <body>
    <div id="table">
      <bootstrap-table :columns="columns" :data="data" :options="options"></bootstrap-table>
    </div>

    <script src="https://code.jquery.com/jquery-3.3.1.min.js" integrity="sha256-FgpCb/KJQlLNfOu91ta32o/NMZxltwRo8QtmkMRdAu8=" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.6/umd/popper.min.js" integrity="sha384-wHAiFfRlMFy6i5SRaxvfOCifBUQy1xHdJ/yoi7FRNXMRBu5WHdZYu1hA6ZOblgut" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.2.1/js/bootstrap.min.js" integrity="sha384-B0UglyR+jN6CkvvICOB2joaf5I4l3gm9GU6Hc1og6Ls7i6U/mkkaduKaBhlAXv9k" crossorigin="anonymous"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue"></script>
    <script src="https://unpkg.com/bootstrap-table@1.15.3/dist/bootstrap-table.min.js"></script>
    <script src="https://unpkg.com/bootstrap-table@1.15.3/dist/bootstrap-table-vue.min.js"></script>
    <script>
      // 同上script标签内内容
    </script>
  </body>
</html>
```

作为组件使用的话用import导入即可

### Vue的坑

* npm install失败的解决方法：删掉package-lock.json
