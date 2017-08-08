# Learn Vue

## 1. 基础介绍

Vue 是一种渐进式MVVM框架，其核心在于相应的数据绑定和组合视图组件;
Vue 仅仅关注于视图层，可以方便的与第三方库或者机油项目进行整合；
Vue 具有自己的生态系统 -- awesome-vue;
Vue 没有真正的 html 结构，而是通过数据配合 js 渲染出页面，所以需要以 js 的视角观察 html，并且这种方式并不易于 SEO

### 1.1 起步

在 el 下的节点都会被检测，遇到 `{{}}` 或者是指令的时候进行解析；

- 绑定插入的文本内容

    [DOME1](./html/dome1.html)
    采用简洁模板语法来声明式的将数据渲染进去

    ```js
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

    经过这样的声明，数据message 就和 DOM 绑定到一起了，通过在控制台修改 `app.message`; 浏览器就会重新渲染数据到标签中；(首先利用了对于属性定义的函数，在修改的使用调用了执行函数进行了页面的重新渲染；)

- 绑定DOM 元素的元素属性

    [DOME2](./html/dome2.html)
    通过指令 `v-bind` 来实现，这种绑定也是动态的
    `v-bind:attrName='key'`

    ```js
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
    对属性进行绑定，会检测 `v-bind` 后面的语句，在 data 中查找字符串对应的属性，然后进行赋值替换；同第一个示例这里的数据同样可以在控制台修改以后及时渲染； `:` 后面的 title 可以理解为表示要绑定的 key，最终输出到这个标签中

- 条件与循环 绑定DOM结构到数据

    1. 通过 `v-if` 来实现条件：

        `v-if = 'key'`
        [DOME3](./html/dome3.html)

        ```js
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
        这里就很好理解了，在判断 `v-if` 后面的字符串所对应的属性值是真是假，来决定是否__渲染__（这里不是隐藏了, 而是移出标签）

    2. 通过 `v-for` 指令，实现绑定数据到数组来渲染 一个列表

        `v-for = 'val in key'`
        从数据的key中循环，循环的每一项赋值于val
        [DOME4](./html/dome4.html)

        ```js
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
        通过对于 `v-for` 后面的字符串进行查询，利用 `item in list` 的方式表示循环行为；可以看出一个细节：指令后的赋值代表着数的 key；而数据是在声明 Vue 对象时放入 data 中的信息；在标签中使用 `name=value` 的方式更加符合常识

- 处理用户输入

    通过 `v-on` 指令绑定事件，调用我们vue中实例的方法(方法存放在 vue 对象的methods下)；
    `v-on:eventName=key`

    [DOME5](./html/dome5.html)

    ```js
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
    这里出现了新的信息 -- methods 其对应着 `v-on` 指令；
    `v-on:name="value"` 其中 name 代表着对应的事件 例如：'click ...'; 而value 就是要在 methods 中查找其对应的执行函数，然后进行绑定；  
    
- 表单的双向数据绑定：

    [DOME6](./html/dome6.html)
    使用`v-model` 指令，可以将表单输入和数据进行双向数据绑定

    ```js
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
    这个事件稍稍复杂了一些，`v-model` 会将 input 的输入 value 和 其绑定的数据，进行关联；然后关联的数据有控制着 data 从而导致 message 进行的重新渲染；

- 使用组件构建应用：

    Vue 中一个比较重要的概念就是组件系统，在 vue 中，任何类型的应用界面都是一个小组件，然后通过组合可以构建大型应用
    
    在vue 中，一个vue的实例就相当于一个组件：

    ```js
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

    这里的概念比较多一个个解释：

    - 使用 Vue 的方法 `Vue.component` 创建一个组件, 需要两个参数 1. 名称（控制的注册什么名字的标签） 2. option （基础：template 定义这个标签对应的真实标签样式，最终会以这个标签样式进行真实渲染， `prop:[dataName]` -- 接受的数据（这个数据来源于其创建标签时绑定的自定义属性（不是很准确的叫法）`props:name="data"`）） 

        为什么需要使用 props ？  
        这个组件是相对独立的（可以理解为在父级盒子下一个封闭的盒子），无法应用父级数据；这时如果需要使用父级的参数，就要建立数据传输的同道，在子组件内部引入自己标签的自定义属性（`props:[name]`）,而父级就要将子级需要的数据注入到他的自定义属性中完成交流（v-bind:name="value"）；  
        个人理解为：

            ```js
            v-bind: value;
            props: [name];
            // 相当于
            data: {
                name: value
            }
            ```
        
    - 对于 `v-for` 而言，item 相当于局部变量，可以在当前作用域中使用;
    
- 多组件组合实例

    这里有一个（假想）的例子，看看使用了组件的应用模板是什么样的：

    ```html
    <div id="app">
    <app-nav></app-nav>
    <app-view>
        <app-sidebar></app-sidebar>
        <app-content></app-content>
    </app-view>
    </div>
    ```

## 总结

- Vue 目前更像是一个具有特殊规则的模板编译器，通过书写指定格式的模板配个 js 编译成为一套完整且具有 MVVM 性质的文件；
- Vue 实例 

        ```js
        var app = {
            el: '数据绑定的标签选择器'，
            data: {
                name: value
                // 对象个数，容器内部可以使用的对象信息；
            },
            methods: {
                name: value
                // 事件对应的执行信息
            }
        }
        ```
- 基础命令： `v-bind:name="value"`、`{{name}}`、`v-if="name"`、`v-model="name"`、`v-on:eventName="methodsName"`
- 创建组件 -- Vue.component

        ```js
            Vue.componet({
                props: ['name'], //数据对应的属性名
                template: "<li>组件引入的真实代码段</li>"
            })
        ```

