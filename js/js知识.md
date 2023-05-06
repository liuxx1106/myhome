# js知识点

### 1. 闭包

- 定义：如果在一个内部函数里，对在外部作用域(但不是全局作用域)的变量进行引用，那么内部函数就被认为是闭包(closure)。

- 通常我们会使用函数嵌套的方式做闭包，这是因为我们需要把变量放在函数内部，否则变量就成为一个全局变量。

- 为什么return?

  return是为了闭包能被使用，并属于闭包的一部分，把内部函数直接挂到window也是可以的。

- 作用使用场景：

  隐藏访问变量，使之只能间接访问。

  比如，一段游戏代码，玩家有30条命。如果代表他生命的变量是全局变量，那么就有被修改的风险。我们可以构建一个访问器函数，在函数内部定义该变量，然后通过内部函数间接操作玩家生命数的加减。

- 关于闭包的谣言

  闭包会造成内存泄露？

  错。

  说这话的人根本不知道什么是内存泄露。内存泄露是指你用不到（访问不到）的变量，依然占居着内存空间，不能被再次利用起来。

  闭包里面的变量明明就是我们需要的变量（lives），凭什么说是内存泄露？

  这个谣言是如何来的？

  因为 IE。IE 有 bug，IE 在我们使用完闭包之后，依然回收不了闭包里面引用的变量。

### 2. 浏览器跨域问题

- a、后端设置响应头的 Access-Control-Allow-Origin 属性

  ```tsx
  // 全局拦截所有请求，做相关的操作
  app.all('*', (req, res, next) => {
    if (req.method === 'OPTIONS') {
      return false;
    }
    
    // 支持跨域
    res.header('Access-Control-Allow-Origin', '*');
    res.header('Access-Control-Allow-Headers', 'Content-Type');
    res.header('Access-Control-Allow-Methods', '*');
    res.header('Content-Type', 'application/json;charset=utf-8');
  
    next();
  });
  ```

- 前端通过代理的方式请求接口

  ```tsx
  devServer: {
      before: function (app, server, compiler) {
        /* mock 接口 */
        app.get('/api/demo', (req, res) => {
          setTimeout(() => {
            res.send({
              data: 'something...'
            })
          }, 5000)
        })
      },
      proxy: {
        '/api': {
          /*
           * 将来自 http://localhost:8000/api
           * 的请求资源全部转发为 /api开头的请求
           */
          target: 'http://localhost:8000/api',
          changeOrigin: true,
          secure: false,
          pathRewrite: {
            '^/api': ''
          }
        },
        '/tools': {
          /*
           * 将来自 http://www.tanglihe.cn/tools 
           * 的请求资源全部转发为 /tools开头的请求
          */
          target: 'http://www.tanglihe.cn/tools',
          changeOrigin: true,
          secure: false,
          pathRewrite: {
            '^/tools': ''
          }
        }
      }
    }
  ```

- nginx代理

- window.postMessage

### 3. es5怎么面向对象编程

 - 构建构造函数，new 构造函数生成对象

   new执行过程，对象转为数组-》取出构造函数-》创建空对象继承构造函数的属性-》执行构造函数，返回的是对象则直接返回，否则返回context

 - **Object.create()创建实例对象** 

   ```js
   let obj1={
    name:'张三',
    age:18,
    greeting:function(){
        console.log('hi! I\'m'+ this.name +  '.')
   }
   }
   let obj2=Object.create(obj1)
   obj2.name //张三
   obj2.greeting()//hi!I'm 张三.
   ```

### 4.vue中的$变量

$开头的变量`只是`Vue`的命名规则，为了`区分普通变量属性`，避免我们自己声明或者添加`自定义`属性导致`覆盖。

- vue的实例属性`$data`是用于获取`data`里数据的相当于用`this`获取。

- `$watch`函数实际上和配置项`watch`的作用是一样的，都是用来监听一个变量，区别在于：

  - `$watch`函数更加灵活，可以随时随地添加监听
  - `$watch`函数需要手动销毁，配置项`watch`会随组件的生命周期结束而自动销毁

- $el

  获取`Vue`实例关联的`DOM`元素，`$el`指向当前组件`template`模板中的根标签。

- $options

  vue`实例属性`$options`是一个`对象`,可以调用`vue`的各个组件下的`方法`和`数据`，即`new vue({})`大括号内的东西，统称为`options`。

- $event有下面两个作用：

  - 获取原生DOM事件的事件对象
  - 事件注册所传的参数(子组件向父组件传值

- $emit实际上是发送了一个广播事件，$emit调用时可以传递两个参数$emit('eventname', data)：

  eventname：事件名称
  data：跟随事件一起传递的数据
  对于如何接受$emit的广播事件有两种方案：

  使用$on方法接收广播事件：vm.$on( 'eventname', fn )
  使用自定义事件：<CT @eventname="fn($event)"></CT>。实际上自定义事件就是vue自动帮我们调用了一下vm.$on( 'eventname', fn )函数

### 5. prototype