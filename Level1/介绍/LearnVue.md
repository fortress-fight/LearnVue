# Learn Vue

## 1. 基础介绍

Vue 是一种渐进式MVVM框架，其核心在于相应的数据绑定和组合视图组件

### 1.1 起步
在el 下的节点都会被检测，遇到`{{}}` 或者是指令的时候进行解析；

1） 绑定插入的文本内容
[DOME1](./html/dome1.html)
采用简洁模板语法来声明式的将数据渲染进去
```
<div id="app">
    {{message}}
</div>
<script type="text/javascript">
    var app = new Vue({
        el: '#app',
        data: {
            message: 'Hello Vue'
        }
    })
</script>
```

经过这样的声明，数据message 就和 DOM 绑定到一起了，通过在控制台修改 app.message ，浏览器就会重新渲染数据；

2） 绑定DOM 元素的元素属性
[DOME2](./html/dome2.html)
通过指令 `v-bind` 来实现，这种绑定也是动态的
`v-bind:attrName='key'`

```
<div id="app-2" v-bind:title='attr'>
    {{message}}
</div>
<script type="text/javascript">
    var app2 =  new Vue({
        el: '#app-2',
        data: {
            message: 'hello vue',
            attr: new Date()
        }
    })
</script>
```

3） 条件与循环 绑定DOM结构到数据

1. 通过 `v-if` 来实现条件：
`v-if = 'key'`

[DOME3](./html/dome3.html)

```
<div id="app-3" v-bind:title='attr' v-if = 'onOff'>
    {{message}}
</div>
<script type="text/javascript">
    var app3 = new Vue({
        el: '#app-3',
        data: {
            message: 'hello',
            attr: 'title',
            onOff: true
        }
    })
</script>
```

在这里就将 如果 v-if 为真，才会渲染div，如果为假就不渲染了；

2. 通过 `v-for` 指令，实现绑定数据到数组来渲染 一个列表
`v-for = 'val in key'`
从数据的key中循环，循环的每一项赋值于val
[DOME4](./html/dome4.html)

```
<div id="app-4" v-bind:title='val' v-if='onOff'>
    <div  v-for='todo in todos'>
        {{todo.text}}
    </div>
</div>
<script type="text/javascript">
    var app4 = new Vue ({
        el: '#app-4',
        data: {
            val: new Date(),
            onOff: true,
            message: 'hello FF',
            todos: [
                {text: 'name1'},
                {text: 'name2'},
                {text: 'name3'},
                {text: 'name4'}
            ]
        }
    })
</script>
```

在这里，同样也是动态的，可以通过在控制台里输入 app4.todos.push({text: 'wo'});的方式动态添加到DOM中；

4) 处理用户输入

通过 `v-on` 指令绑定事件，调用我们vue中实例的方法(方法存放在 vue 对象的methods下)；
`v-on:eventName=key`

[DOME5](./html/dome5.html)

```
<div id="app-5">
    <p>{{message}}</p>
    <button v-on:click = 'handleClick'>{{message}}</button>
</div>
<script type="text/javascript">
    var app5 = new Vue ({
        el: '#app-5',
        data: {
            message: 'abcdefghijklmnopqrstuvwxyz'
        },
        methods: {
            handleClick: function (){
                this.message = this.message.split('').reverse().join('')
            }
        }
    })
</script>
```

5) 表单的双向数据绑定：

[DOME6](./html/dome6.html)
使用`v-model` 指令，可以将表单输入和数据进行双向数据绑定

```
<div id="app-6">
    <p>{{message}}</p>
    <input type="text" v-model='message'/>
</div>
<script type="text/javascript">
    var app6 = new Vue({
        el: '#app-6',
        data: {
            message: '双向绑定v-model'
        }
    })
</script>
```

6) 使用组件构建应用：

在vue 中，一个vue的实例就相当于一个组件：

```
<div id="app-7">
    <ol>
        <todo-item v-for="item in todoList" v-bind:todo="item"></todo-item>
    </ol>
</div>
<script type="text/javascript">
    Vue.component('todo-item', {
        props: ['todo'],
        template: '<li>{{todo.text}}</li>'
    })
    var app7 = new Vue ({
        el: '#app-7',
        data: {
            todoList: [
                {text: 'name1'},
                {text: 'name2'},
                {text: 'name3'},
                {text: 'name4'}
            ]
        }
    })
</script>
```
注：
1. 使用 component（组成）创建一个组件，第一个参数是 组件的名称，第二个参数就是组件的内容
2. porps 储存了标签的属性, 必须要将需要使用的属性以 ['todo'] 的方式引用，以供模板使用
3. template 指的就是模板，也就是 todo-item 引入的东西
4. 将 data 引入的标签的属性中，然后建立起 component 与 数据之间的链接

原文如下：
这只是一个假设的例子，但是我们已经将应用分割成了两个更小的单元，子元素通过 props 接口实现了与父亲元素很好的解耦。我们现在可以在不影响到父应用的基础上，进一步为我们的 todo 组件改进更多复杂的模板和逻辑。
在一个大型应用中，为了使得开发过程可控，有必要将应用整体分割成一个个的组件。在后面的教程中我们将详述组件，不过这里有一个（假想）的例子，看看使用了组件的应用模板是什么样的：
```
<div id="app">
  <app-nav></app-nav>
  <app-view>
    <app-sidebar></app-sidebar>
    <app-content></app-content>
  </app-view>
</div>
```
