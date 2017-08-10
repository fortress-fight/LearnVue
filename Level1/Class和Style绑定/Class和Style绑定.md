# Learn Vue

## 1 Class和Style绑定

数据绑定一个常见需求是操作元素的 class 列表和它的内联样式。因为它们都是属性 ，我们可以用v-bind 处理它们：只需要计算出表达式最终的字符串。不过，字符串拼接麻烦又易错。因此，在 v-bind 用于 class 和 style 时， Vue.js 专门增强了它。表达式的结果类型除了字符串之外，还可以是对象或数组。

### 1.1 绑定Class

1. 一般方法：

    ```html
    <div id="example" :class="style"></div>
    <script type="text/javascript">
        var vm = new Vue ({
            el: '#example',
            data: {
                style: 'blue'
            }
        })
    </script>
    ```

2. 对象语法

    ```html
    <div id="example" :class='{blue:onOff, yellow:!onOff}' @click='handleClick'></div>
    <script type="text/javascript">
        var vm = new Vue ({
            el: '#example',
            data: {
                onOff: true
            },
            methods: {
                handleClick(){
                    this.onOff = false
                }
            }
        })
    </script>
    ```
   通过 onOff 是否为真来判断是否添加 相应的class，当 onOff 发生变化的时候，class也会改变
   同样我们也可以将这一部分逻辑放到数据中：

    ```html
    <div id="example" :class='classObj'></div>
    <script type="text/javascript">
        var vm = new Vue ({
            el: '#example',
            data: {
                classObj: {
                    blue: true,
                    yellow: false
                }
            }
        })
    </script>
    ```
   绑定对象的形态可以配合计算属性来使用：
   [DOME1](./html/dome1.html)

    ```js
        var vm = new Vue ({
            el: '#example',
            data: {
                onOff: false
            },
            computed: {
                classObj (){
                    return {
                        yellow: this.onOff,
                        blue: !this.onOff
                    }
                }
            },
            methods: {
                handleClick (){
                    this.onOff = !this.onOff
                }
            }
        })
    ```

3. 数组语法

   如果需要添加一个class 列表的时候，还可以使用数组语法，在数组中可以使用三元表达式，也可以在数组中使用对象语法；

    ```html
        <div id="example" :class='[style2, style1]'></div>
        <div id="example1" :class="classArr"></div>
        <div id="example2" :class="[onOff?style1:style2]" @click='handleClick'></div>
        <script type="text/javascript">
            var vm = new Vue({
                el:'#example',
                data: {
                    style1: 'red',
                    style2: 'yellow'
                }
            })
            var vm1 = new Vue({
                el:'#example1',
                data: {
                    classArr: ['red', 'blue']
                }
            })
            var vm2 = new Vue({
                el: "#example2",
                data: {
                    onOff: true,
                    style1: 'yellow',
                    style2: 'blue'
                },
                methods: {
                    handleClick(){
                        this.onOff = !this.onOff
                    }
                }
            })
        </script>
    ```

4. 用在组件上

   在使用组件时，组件添加的class 会与 设置的class 共存

    ```html
        <div id="example">
            <my-component :class='{yellow:onOff}' v-bind:m='message'></my-component>
        </div>

        <script type="text/javascript">
            Vue.component('my-component', {
                props: ['m'],
                template: '<p class = "big">这里是：{{m}}</p>'
            })
            var vm = new Vue({
                el: example,
                data: {
                    onOff: true,
                    message: '这里是一段话'
                }
            })
        </script>
        <!-- 渲染结构  -->
        <div id="example">
            <p class = "yellow big">这里是：这里是一段话</p>
        </div>
    ```

* 注：可以看出 v-bind:class 具有自己的处理方式，和一般的属性绑定具有差异，比如说对于一般的属性绑定的情况就不会存在对象形式的绑定行为；

### 1.2 绑定式内联样式

1. 对象语法

    ```html
    <div id="dome3" :style='{height:height+"px", background:background}'>
        {{message}}
    </div>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#dome3',
            data: {
                message:'style',
                height: 500,
                background: 'blue'
            }
        })
    </script>
    ```

   或者是这样：

    ```html
    <div id="dome3" :style='styleObj'>
        {{message}}
    </div>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#dome3',
            data: {
                message:'style',
                styleObj: {
                    height: 500 +'px',
                    background: 'blue'
                }
            }
        })
    </script>
    ```

* 注：可以看出 v-bind:style 具有自己的处理方式，和其余的构成方式都有不同；

2. 数组语法

   通过数组语法可以将多个样式对象应用到一个元素上：

    ```html
    <div id="dome3" :style='[styleObj, styleObjFont]'>
        {{message}}
    </div>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#dome3',
            data: {
                message:'style',
                styleObj: {
                    height: 500 +'px',
                    background: 'blue'
                },
                styleObjFont: {
                    fontSize: '100px',
                    color: '#fff'
                }
            }
        })
    <script>
    ```

3. 自动添加前缀

自动添加前缀

当 v-bind:style 使用需要特定前缀的 CSS 属性时，如 transform ，Vue.js 会自动侦测并添加相应的前缀。

'但是经过试验发现没有在chrome 下增加前缀，可能是由于浏览器已经支持了，有待验证'

4. 多重值

    从 2.3.0 起你可以为 style 绑定中的属性提供一个包含多个值的数组，常用于提供多个带前缀的值：
    `<div :style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }">`
    这会渲染数组中最后一个被浏览器支持的值。在这个例子中，如果浏览器支持不带浏览器前缀的 flexbox，那么渲染结果会是 display: flex。