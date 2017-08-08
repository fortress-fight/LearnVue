# Learn Vue

## 1. Render函数

### 1.1 基础

VUE 推荐使用 template 创建 HTML，但是在一些场景中使用模板会造成代码的冗长；

[dome1](./html/dome.html)

```html
<div id="app">
    <my-template :level="1">Hello world!</my-template>
</div>
<script>
    Vue.component("my-template", {
        template: `<div>
            <h1 v-if="level === 1">
                <slot></slot>
            </h1>
            <h2 v-if="level === 2">
                <slot></slot>
            </h2>
            <h3 v-if="level === 3">
                <slot></slot>
            </h3>
            <h4 v-if="level === 4">
                <slot></slot>
            </h4>
            <h5 v-if="level === 5">
                <slot></slot>
            </h5>
            <h6 v-if="level === 6">
                <slot></slot>
            </h6>
        </div>`,
        props: {
            level: {
                type: Number
            }
        }
    });
    var app = new Vue({
        el: '#app'
    })
</script>
```

简单的说一下：这里表达的是 通过使用不同的 level 引用不同的标签将内容包裹

这种方式使用 template 并不是很好的方式，因为会显得代码冗长；

这种情况下就引入了 render 函数的概念。

render 比 template 更接近编译器：将编译的工作完全交给 JavaScript；

这里使用 render 函数，简单的改写这个示例：

```html
<div id="app">
    <my-template :level="1">Hello world!</my-template>
</div>
<script>
    Vue.component('my-template', {
        render: function(createElement) {
            return createElement(
                'h' + this.level,
                this.$slots.default
            )
        },
        props: {
            level: {
                type: Number
            }
        }
    });
    var app = new Vue({
        el: "#app"
    })
</script>
```

### 1.2 render函数

#### 1.2.1 createElement 参数

render 函数是通过 createElement 来生成的模板，这里简单的介绍一下

```js
// @returns {VNode}
createElement(
  // {String | Object | Function}
  // 一个 HTML 标签，组件选项，或一个函数
  // 必须 Return 上述其中一个
  'div',
  // {Object}
  // 一个对应属性的数据对象
  // 您可以在 template 中使用.可选项.
  {
    // (下一章，将详细说明相关细节)
  },
  // {String | Array}
  // 子节点(VNodes). 可选项.
  [
    createElement('h1', 'hello world'),
    createElement(MyComponent, {
      props: {
        someProp: 'foo'
      }
    }),
    'bar'
  ]
```

例如：

```html
<div id="app">
    <my-template></my-template>
</div>
<script>
    Vue.component('my-template', {
        render: function(createElement) {
            return createElement("div", [
                createElement('em', 'one'),
                createElement('h1', 'two'),
            ])
        }
    })
    var app = new Vue({
        el: "#app"
    })
</script>
```

生成的结构：

```html
<div><em>one</em><h1>two</h1></div>
```

#### 1.2.3 完整的数据对象

在 templates 中，存在许许多多的对象：

简单的介绍一下：
class -- 绑定class 属性：使用的时候需要加引号
style -- 绑定样式，可以通过对象的形式添加样式属性
attrs -- 添加 attribute 属性
props -- 组件
domPors -- DOM 属性：例如使用innerHTML
on -- 绑定事件
nativeOn -- 在调用模板的时候绑定原生事件
directives -- 暂时没有接触到
scopedSlots -- 暂时没有接触到
slot -- 暂时没有接触到
key -- 暂时没有接触到
ref -- 提取元素

```js
{
  // 和`v-bind:class`一样的 API
  'class': {
    foo: true,
    bar: false
  },
  // 和`v-bind:style`一样的 API
  style: {
    color: 'red',
    fontSize: '14px'
  },
  // 正常的 HTML 特性
  attrs: {
    id: 'foo'
  },
  // 组件 props
  props: {
    myProp: 'bar'
  },
  // DOM 属性
  domProps: {
    innerHTML: 'baz'
  },
  // 事件监听器基于 "on"
  // 所以不再支持如 v-on:keyup.enter 修饰器
  // 需要手动匹配 keyCode。
  on: {
    click: this.clickHandler
  },
  // 仅对于组件，用于监听原生事件，而不是组件使用 vm.$emit 触发的事件。
  nativeOn: {
    click: this.nativeClickHandler
  },
  // 自定义指令. 注意事项：不能对绑定的旧值设值
  // Vue 会为您持续追踨
  directives: [
    {
      name: 'my-custom-directive',
      value: '2'
      expression: '1 + 1',
      arg: 'foo',
      modifiers: {
        bar: true
      }
    }
  ],
  // Scoped slots in the form of
  // { name: props => VNode | Array<VNode> }
  scopedSlots: {
    default: props => h('span', props.text)
  },
  // 如果子组件有定义 slot 的名称
  slot: 'name-of-slot'
  // 其他特殊顶层属性
  key: 'myKey',
  ref: 'myRef'
}
```

使用实例：

[dome4](./html/dome4.html)

```html
<div id="app">
    <my-template :level="2">
        <h1>This.is.a.tempalte</h1>
    </my-template>
</div>
<script>
    var getChildrenTextContent = function(children) {
        return children.map(function(item) {
            return item.children ? getChildrenTextContent(item.children) : item.text
        }).join('')
    };
    Vue.component('my-template', {
        render: function(createElement) {
            var headingId = getChildrenTextContent(this.$slots.default)
                .toLowerCase()
                .replace(/\W+/g, '-')
                .replace(/(^\-|\-$)/g, '');
            return createElement(
                'h' + this.level, [
                    createElement('a', {
                        attrs: {
                            name: headingId,
                            href: '#' + headingId,
                        }
                    }, this.$slots.default)
                ]
            )
        },
        props: {
            level: {
                type: Number
            }
        }
    })
    var app = new Vue({
        el: "#app"
    })
</script>
```

渲染成：

```html
    <div id="app">
        <h2>
            <a name="this-is-a-tempalte" href="#this-is-a-tempalte">
                <h1>This.is.a.tempalte</h1>
            </a>
        </h2>
    </div>
```

简单的解释一下：
这个实例是将：模板标签的内容变成一个 a 标签的内容，其中 name 和 href 也都是有模板内容转换而成的

#### 1.2.4 约束

注：源文档所下面这种方式，是无法渲染的，经过试验发现是可以的：

```js
Vue.component('my-template', {
    render: function(createElement) {
        var t1 = createElement('p', 'title');
        return createElement('div', [
            t1,
            t1
        ])
    }
})
```

如果需要重复渲染一个标签可以使用：

```js
Vue.component('my-template', {
    render: function(createElement) {
        return createElement('div',
            Array.apply(null, {
                length: 10
            }).map(function() {
                return createElement('p', 'title');
            })
        )
    }
});
var app = new Vue({
    el: '#app'
})
```

#### 1.2.5 使用 JavaScript 代替模板功能

1. vue 的 render 函数中不会提供 vue 的方法，例如 v-if || v-for || v-model 如果需要就需要使用 js 来实现其功能；

2. Event & Key Modifiers

    在事件中vue提供了一些可以使用的前缀，

    - .capture	!
    - .once	~
    - .capture.once or
    - .once.capture	~!

    例如：

     ```js
        on: {
            '!click': this.doThisInCapturingMode,
            '~keyup': this.doThisOnce,
            `~!mouseover`: this.doThisOnceInCapturingMode
            }
     ```

    而对于其余事件需要使用js进行模拟

3. Slots

<!--后面的部分没有看懂-->