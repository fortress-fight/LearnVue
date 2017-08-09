# Learn Vue

## 1. vue 实例

1. Vue 构造器

    `var vm = new Vue()`
    所有的组件都是其实都是被扩展的vue实例；

2. 属性和方法

    [DOME1](./html/dome1.html)
    每一个vue实例都会代理其 data 对象的所有属性（下文中的 vm 都将代表 vue 实例）

    ```html
    <script type="text/javascript">
        var vm = new Vue({
            data: {
                'a': '1'
            }
        });
        console.log(vm.a == vm._data.a);
    </script>
    ```

    只有被代理的属性在修改的时候才会触发视图的更新；而在声明后加入的数据并不会触发视图的更新

    Vue 实例暴露了一些有用的实例**属性**与**方法**。这些属性与方法都有前缀 $，以便与代理的 data 属性区分。也可以当做参数传入vue的实例中（这种情况不需要前缀）例如：

    ```html
    <div id="dome1"></div>
    <script type="text/javascript">
        var vm = new Vue({
            el: '#dome1',
            data: {
                'a': '1'
            }
        });
        console.log(vm.$el); //  <div id="dome1"></div>
        console.log(vm.el); // undefined
        console.log(vm.a == vm.$data.a);
        vm.$watch('a', function(nowValue,  oldValue){
            // 这个函数会在data的a进行修改的时候才会触发
        })
    </script>
    ```

    注: 不要在实例属性或者回调函数中（如 vm.$watch('a', newVal => this.myMethod())）使用箭头函数。因为箭头函数绑定父上下文，所以 this 不会像预想的一样是 Vue 实例，而是 this.myMethod 未被定义。

3. 实例的声明周期

    一个vue实例被创建的时候都要经历一系列的创建过程，在这个过程中实例会产生一些生命周期的钩子，例如`created`就会在实例被创建的时候触发

    ```html
    <div id="dome2">
        {{message}}
    </div>
    <script type="text/javascript">
        var vm = new Vue ({
            el: '#dome2',
            data: {
                message: 'hello'
            },
            created: function (){
                console.log('create');
            }
        })
    </script>
    ```

    实例的生命周期还包括：mounted(安装好)、 updated(更新好) 、destroyed(销毁后)

    生命周期图示：
    ![生命周期](./img/生命周期.png)

4. 详解生命周期

    文章：
    [VUE-生命周期](http://www.cnblogs.com/fly_dragon/p/6220273.html)

    VUE 提供了一系列的生命周期的事件钩子，辅助我们进行对整个Vue实例生成、编译、挂载、销毁等过程进行js控制。

    Vue实例有一个完整的生命周期，也就是从开始创建、初始化数据、编译模板、挂载Dom、渲染→更新→渲染、卸载等一系列过程，我们称这是Vue的生命周期。通俗说就是Vue实例从创建到销毁的过程，就是生命周期。

    在这些事件响应方法中的this直接指向的是vue的实例。

    ![VUE-生命周期](./img/生命周期2.png);

    简单的说一下：
    图中红色框内的就是可以挂载的钩子，共6个：
    创建前后，插入前后，更新前后，销毁前后

    ```html
        <!DOCTYPE html>
        <html lang="en">
        <head>
            <meta charset="UTF-8">
            <meta name="viewport" content="width=device-width, initial-scale=1.0">
            <meta http-equiv="X-UA-Compatible" content="ie=edge">
            <title>Document</title>
            <script src="../../../vue.js"></script>
        </head>
        <body>
            <div id="app">
                {{message}}
            </div>
            <script>
                var app = new Vue({
                    el: "#app",
                    data: {
                        message: 'this is a apple'
                    },
                    beforeCreate: function (){
                        console.log('new 一开始但是没有传入数据 beforeCreate')
                    },
                    created: function (){
                        console.log('拿到数据并且完成事件绑定 created')
                    },
                    beforeMount: function (){
                        console.log('挂载DOM前 beforeMount')
                    },
                    mounted: function (){
                        console.log('挂载DOM后 mounted 如果没有在初始化的时候表明挂载的 DOM 就会观察是否具有 app.$mount(el)')
                        this.message = "this is a org"
                    },
                    beforeUpdate: function (){
                        console.log('数据发生变化但是还没有完成页面刷新 beforeUpdate')
                    },
                    updated: function (){
                        console.log('数据发生变化并且完成页面刷新 updated')
                    },
                    beforeDestroy: function (){
                        console.log('如果调用了 app.$destroy() 在还没有真正卸载之前会触发 beforeDestroy 事件')
                    },
                    destroyed: function (){
                        console.log('完成卸载 整个生命周期结束 destroyed')
                    }
                })
                app.$watch ('message', function (){
                    console.log('数据已经发生了修改');
                })
            </script>
        </body>
        </html>
    ```