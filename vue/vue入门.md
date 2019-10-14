## vue入门

### vue引用

```html
<!-- 开发环境版本，包含了有帮助的命令行警告 -->
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>

<!-- 生产环境版本，优化了尺寸和速度 -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script>
```

### vue基本代码结构

```html
<div id="app">
	{{msg}}
</div>
<script>
var vm = new Vue({
    //非常重要,如果不在此区域定义的属性不会生效
	el: '#app',
    data: {
   		msg: 'LOGIN IN'
    }
})
</script>
```

#### v-text,v-cloak,v-html

```javascript
<div id="app">
    <div v-cloak>{{ msg }}</div>
	<div v-text="msg"></div>
	<div v-html="msg2"></div>
</div>

<script>
	var vue = new Vue({
		el: '#app',
		data : {
			msg: 'Hello World',
			msg2: '<h2>Hi</h2>'
		}
	})
</script>
```

> 三者区别说明:
>
> v-cloak:用于在网速较慢的时候,vue还未完成渲染,会显示{{msg}},此属性用于对{{msg}}进行隐藏
>
> v-text:作用与{{msg}}相同,但是在网速较慢的时候,直接不进行显示
>
> v-html:可以对html进行渲染

#### v-bind数据绑定

```html
<input type="button" value="按钮" v-bind:title="title"/>
<!-- 简写 :title等同于v-bind:title,v-bind里面可以写合法的js表达式 -->
<input type="button" value="按钮" :title="title + '123'"/>
<script>
    var vue = new Vue({
        el: '#app',
        data : {
            title: 'Title'
        }
    })
</script>
```

#### v-on事件绑定

```html
<div id="app">
    <!-- v-on:后面跟事件类型,如click,keyups -->
	<input type="button" value="按钮" v-on:click="show"/>
    <!-- 缩写@click -->
    <input type="button" value="按钮" @click="show"/>
</div>

<script>
    var vue = new Vue({
        el: '#app',
        methods: {
            show:function() {
                alert("hello");
            }
        }
    })
</script>
```

#### 跑马灯效果实现

> 需求:点击"浪起来"实现跑马灯效果,点击"低调"停止跑马灯效果

```html
<div id="app">
    <input type="button" value="浪起来" @click="lang"/>
    <input type="button" value="低调" @click="stop"/>
    <h4>{{msg}}</h4>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            msg: '猥琐发育,别浪~~',
            intervalId: null
        },
        methods: {
            lang() {
                if(this.intervalId != null) return;
                this.intervalId = setInterval(() => {
                    var start = this.msg.charAt(0);
                    var end = this.msg.substr(1);
                    this.msg = end + start;
                },400)
            },
            stop() {
                clearInterval(this.intervalId);
                this.intervalId = null;
            }
        }
    })
</script>
```

#### 事件修饰符

> - .stop 阻止冒泡
> - .prevent 阻止默认事件
> - .capture 添加事件侦听器时使用事件捕获模式
> - .self 只当事件在该元素本身(比如不是子元素)触发时触发回调
> - .once 事件只触发一次

```html
<!-- 事件修饰符可以合并使用:如@click.stop.prevent -->
<div id="app">
    <div id="area" @click="divClick" style="background: darkcyan">
        <!-- 阻止冒泡事件 -->
        <input type="button" value="按钮" @click.stop="btnClick"/>
        <a href="https://www.baidu.com/" @click.prevent="linkClick">阻止默认行为</a>
    </div>
</div>

<script>
    var vm = new Vue({
        el: '#app',
        methods: {
            divClick() {
                alert("div_click");
            },
            btnClick() {
                alert("btn_click");
            },
            linkClick() {
                alert("触发点击事件")
            }
        }
    })
</script>
```

#### v-model实现双向数据绑定

> 实现V修改同步到M,M修改同步到V,v-model只能运用在表单元素中,如下
>
> <input(radio,text,address,email...)> select checkbox textarea

```html
<div id="app">
    <!-- 修改input,span内容也会改变 -->
    <span>{{msg}}</span><br/>
    <input type="text" v-model="msg">
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'this is a msg'
        }
    })
</script>
```

#### v-model实现简易计算器效果

```html
<div id="app">
    <input type="text" v-model="n1"/>
    <select v-model="opt">
        <option value="+">+</option>
        <option value="-">-</option>
        <option value="*">*</option>
        <option value="/">/</option>
    </select>
    <input type="text" v-model="n2"/>
    <button @click="calc">=</button>
    <input type="text" v-model="result"/>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            n1: 0,
            n2: 0,
            result: 0,
            opt: '-'
        },
        methods: {
            calc() {
                //只是demo,不要过于纠结细节
                var codeStr = 'this.n1' + this.opt + this.n2;
                this.result = eval(codeStr);
            }
        },
    })
</script>
```

### 在vue中使用样式

#### 使用class样式

1. 数组

   ```html
   <h1 :class="['red','thin']">this is a eval h1</h1>
   ```

2. 数组中使用三元表达式

   ```html
   <h1 :class="['red','thin',isactive?'active':'']">this is a eval h1</h1>
   ```

3. 数组中嵌套对象

   ```html
   <h1 :class="['red','thin',{'active':isactive}]">this is a eval h1</h1>
   ```

4. 直接使用对象

   ```html
   <h1 :class="{red:true,italic:true,active:true,thin:true}">this is a eval h1</h1>
   ```

#### 使用内联样式

1. 直接在元素上通过<code>:style</code>的形式,书写样式对象

   ```html
   <h1 :style="{color:'red','font-size':'40px'}">this is a eval h1</h1>
   ```

2. 将样式对象,定义到<code>data</code>中,并直接引用到<code>:style</code>中

   - 在data上定义样式(注意:如果属性存在'-',必须添加单引号)

   ```js
   data: {
   	h1StyleObj: {color:'red','font-size': '40px','font-weight': '200'}
   }
   ```

   - 在元素中,通过属性绑定的形式,将样式对象应用到元素中:

   ```html
   <h1 :style="h1StyleObj">this is a eval h1</h1>
   ```

3. 在<code>:style</code>中通过数组,引用多个<code>data</code>上的样式对象

   - 在data上定义样式(注意:如果属性存在'-',必须添加单引号)

   ```javascript
   data: {
   	h1StyleObj: {color:'red','font-size': '40px','font-weight': '200'},
   	h1StyleObj2: {font-style: 'italic'}
   }
   ```

   - 在元素中,通过属性绑定的形式,将样式对象应用到元素中

   ```html
   <h1 :style="[h1StyleObj,h1StyleObj2]">this is a eval h1</h1>
   ```

### vue指令

#### v-for与key属性

> 2.20+的版本中,当在组件中使用v-for时,key现在是必须的
>
> 当Vue.js用v-for正在更新已渲染过的元素列表时,它默认用"就地复用"策略,如果数据项的顺序被改变,Vue将不是移动DOM元素来匹配数据项的顺序,而是简单复用此处每个元素,并且确保它在特定索引下显示已被渲染过的每个元素。
>
> 为了给Vue一个提示,以便它能追踪每个节点的身份,从而复用和重新排序现有元素,你需要单独为每项提供一个唯一key属性

1. 迭代数组

   ```html
   <ul>
   	<li v-for="(item,i) in list">索引:{{i} --- 姓名:{{item.name}} --- 年龄:{{item.age}}}</li>
   </ul>
   ```

2. 迭代对象中的属性(顺序为:值,键,索引)

   ```html
   <!-- 循环遍历对象身上的属性 -->
   <div v-for="(val,ke,i) in userInfo">{{{val}} --- {{key}} --- {{i}}}</div>
   ```

3. 迭代数字(i从1开始)

   ```html
   <p v-for="i in 10">{{i}}</p>
   ```

#### v-if和v-show

><code>v-if</code>每次都会重新删除或创建元素;<code>v-for</code>每次不会重新进行DOM的删除和创建操作,只是切换display:none样式;
>
><code>v-if</code>有较高的切换性能消耗,<code>v-for</code>有较高的初始渲染消耗;如果元素涉及到频繁的切换,最好不要使用v-if

```html
<div id="app">
    <input type="button" @click="flag=!flag" value="切换">
    <h3 v-if="flag">这是v-if控制的元素</h3>
    <h3 v-show="flag">这是v-show控制的元素</h3>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            flag: true
        }
    })
</script>
```

### 案例: 使用vue实现品牌新增,删除以及关键字搜索和时间处理

```html
<div id="app">
    <div class="panel panel-primary">
        <div class="panel-heading">
            <h3 class="panel-title">添加品牌</h3>
        </div>
        <div class="panel-body form-inline">
            <label>
                id:
                <input type="text" class="form-control" v-model="id">
            </label>
            <label>
                name:
                <input type="text" class="form-control" v-model="name">
            </label>
            <input type="button" value="添加" class="btn btn-primary" @click="add">
            <label>
                搜索名称关键字
                <!-- 在vue中的所有指令,在调用的时候都以v-开头 -->
                <input type="text" class="form-control" v-model="keywords">
            </label>
        </div>
    </div>

    <table class="table table-hover table-bordered table-striped">
        <thead>
            <tr>
                <th>id</th>
                <th>name</th>
                <th>ctime</th>
                <th>operation</th>
            </tr>
        </thead>
        <tbody>
            <tr v-for="item in search(keywords)" :key="item.id">
                <td>{{item.id}}</td>
                <td>{{item.name}}</td>
                <td>{{item.ctime | dateFormat('yyyy-MM-dd')}}</td>
                <td>
                    <a href="#" @click.prevent="del(item.id)">删除</a>
                </td>
            </tr>
        </tbody>
    </table>
</div>

<script>
    //全局过滤器
    Vue.filter('dateFormat',function(dateStr,pattern = '') {
        //根据给定字符串获取特定时间
        var dt = new Date(dateStr)
        //yyyy-mm-dd
        var y = dt.getFullYear()
        var m = dt.getMonth() + 1
        var d = dt.getDate()
        if(pattern && pattern.toLowerCase() == 'yyyy-mm-dd') {
            return `${y}-${m}-${d}`
        } else {
            var hh = dt.getHours()
            //padStart:开始填充;padEnd():末尾填充(如两位填充数字1: 01 和 10)
            var mm = (dt.getMinutes()).toString().padStart(2,'0')
            var ss = (dt.getSeconds()).toString().padStart(2,'0')
            return `${y}-${m}-${d} ${hh}:${mm}:${ss}`
        }
    })
    var vm = new Vue({
        el: '#app',
        data: {
            id:null,
            name:null,
            keywords:'',
            list: [
                {id : 1,name: '奔驰',ctime: new Date()},
                {id : 2,name: '宝马',ctime: new Date()},
                {id : 3,name: '保时捷',ctime: new Date()}
            ]
        },
        methods: {
            add() {
                var car = {id: this.id,name: this.name,ctime: new Date()};
                this.list.push(car);
                this.id = this.name = null;
            },
            del(id) {
                // this.list.some((item,i) => {
                //     if(item.id == id) {
                //         this.list.splice(i,1);
                //         return true;
                //     }
                // })

                var index = this.list.findIndex(item => {
                    if(item.id == id) {
                        return true;
                    }
                })
                this.list.splice(index,1);
            },
            search(keywords) {
                //"测试".indexOf('')等于0
                // var newList = [];
                // this.list.forEach(item => {
                //     if(item.name.indexOf(keywords) != -1) {
                //         newList.push(item);
                //     }
                // })
                // return newList;

                return this.list.filter(item => {
                    if(item.name.includes(keywords)) {
                        return true;
                    }
                })
            }
        },
        filters: { //定义私有过滤器【过滤器名称和处理函数】
            //如果同名,
            dateFormat:function(dateStr,pattern) {
                return "私有过滤器"
            }
        }
    })
</script>
```

### vue-devtools安装

> 推荐翻墙安装,搜索<code>Vue.js devtools</code>
>
> 注意事项:
>
> vue扩展程序需要勾选允许访问文件网址和收集各项错误;同时引入的vue.js不能使用min.js

### 过滤器 

> vue.js允许自定义过滤器,可被用作一些常见文本格式化.过滤器可以用在两个地方:<code>mustache插值</code>和<code>v-bind表达式</code>.过滤器应该被添加在js表达式的尾部,由"管道"符指示

#### 私有过滤器

> 过滤器调用: Vue.filter('过滤器名称',function(){})
>
> //过滤器中的function,第一个参数已经规定死了,永远都是过滤器管道符前面传递过来的数据

1. html元素

```html
<td>{{item.ctime | dataFormat('yyyy-mm-dd')}}</td>
```

2. 私有filter定义方式

```javascript
filters: {
	dataFormat(input,pattern = '') {
        ...
    }
}
```

- 替换'单纯'为'邪恶'

```html
<div id="app">
    <p> {{msg | msgFormat('邪恶') | str}}</p>
</div>
<script>
    Vue.filter('msgFormat',function(msg,arg) {
        return msg.replace(/单纯/g,arg);
    })
    Vue.filter('str',function(msg) {
        return msg+"---";
    })
    var vm = new Vue({
        el: '#app',
        data: {
            msg: '曾经,我也是一个单纯的少年,单纯的我,傻傻的问,谁是世界上最单纯的男人'
        }
    })
</script>
```

### 自定义按键修饰符

- 1.x中自定义键盘修饰符(了解不推荐)

```javascript
Vue.directive('on').keyCodes.f2 = 113;
```

- 2.x中自定义键盘修饰符

1. 通过<code>Vue.config.keyCodes.名称 = 按键值</code>来自定义按键修饰符的别名

```javascript
Vue.config.keyCodes.f2 = 113;
```

2. 使用自定义的按键修饰符

```html
<input type="text" v-model="name" @keyup.f2="add">
```

### 自定义指令

> 参数1：指令的名称,在定义的时候不需要加v-前缀,在调用的时候,必须加上v-前缀
>
> 参数2：一个对象,对象身上有一些指令相关的函数,函数可以在特定的阶段,执行相关的操作

- 使用Vue.directive()定义全局指令 

```html
<input id="search" type="text" class="form-control" v-model="keywords" v-focus v-color="'blue'">
<script>
    //自动获取焦点
    Vue.directive('focus',{
        //在每个函数中,第一个参数永远是el,表示被绑定了指令的那个元素,这个el参数是一个原生js对象
        bind:function(){}, //每当指令绑定在元素上的时候,会立即执行这个bind函数,只执行一次
        inserted:function(el){
            el.focus();
        },//元素插入到dom中的时候会执行inserted函数,触发一次
        updated:function(){}//当VNode更新的时候,会执行updated,可能会触发多次
    });
    
    //写法一:全局
    //修改字体颜色
    Vue.directive('color',{
        bind: function(el,expression) {
            el.style.color = binding.value;
            console.log(binding.name);//color
            console.log(binding.value);//blue
            console.log(binding.expression);//'blue'
        }
    })
    
    //写法二:私有
    var vue = new Vue({
        el: '#app',
        directives: {
            'color': {
                bind: function(el,binding) {
                    el.style.color = binding.value;
                    ...
                }
            }
        }
    })
</script>
```

#### 函数简写

> 在很多时候，你可能想在 `bind` 和 `update` 时触发相同行为，而不关心其它的钩子。比如这样写:

```javascript
Vue.directive('color-swatch', function (el, binding) {
  el.style.backgroundColor = binding.value
})
```

### vue实例的生命周期

- 什么是生命周期:从vue实例创建,运行到销毁期间,总是伴随着各种各样的事件,这些事件统称为生命周期
- 生命周期钩子: 生命周期事件的别名
- 主要的生命周期函数分类
  - 创建期间的生命周期函数
    - beforeCreate: 实例刚在内存中被创建出来,此时,还没有初始化好data 和 methods属性
    - created: 实例已经在内存中创建ok,此时data和Methods已经创建ok,此时还没有开始编译模板
    - beforeMount: 此时已经完成模板的编译,但是还没有挂在到页面中
    - mounted: 此时,已经将编译好的模板,挂在到了页面指定的容器中显示
  - 运行期间的生命周期函数
    - beforeUpdate: 状态更新之前执行此函数,此时data中的状态值是最新的,但是界面上显示的数据还是旧的,因为此时还没有开始渲染DOM节点
    - updated: 实例更新完毕之后调用此函数,此时data中的状态值和页面上显示的数据,都已经完成了更新,界面已经被重新渲染好了
  - 销毁期间的生命周期
    - beforeDestory: 实例销毁之前调用,在这一步,实例仍然完全可用
    - destoryedL Vue实例销毁后调用,调用后,vue实例指示的所有东西都会解除绑定,所有的事件监听器都会移除,所有的子实例也会被销毁

```html
<div id="app">
    <input type="button" value="触发change" @click="msg='this is not a msg'">
    <h3 id="h3">{{msg}}</h3>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            msg: 'this is a msg'
        },
        methods: { 
            show() {
                console.log("this is a function")
            }
        },
        //实例化周期
        beforeCreate() { // first life cycle function
            //在beforeCreate生命周期函数执行的时候,data和methos中的数据都还没有初始化
            console.log(this.msg)//undefined
        },
        created() { //second life cycle function
            //如果要调用methods中的方法,或操作data中的数据,最早只能在created中操作
            console.log(this.msg)//this is a msg
        },
        beforeMount() { //third life cycle function
            //模板已经在内存中编辑完成,尚未把模板渲染到页面中去,页面中的元素还未替换过来
            console.log(document.getElementById("h3").innerText)//{{msg}}
        },
        mounted() { //fourth life cycle function
            //内存中的模板,已经真实的挂载到了页面中,用户已经可以看到渲染好的页面("实例创建"的最后一个生命周期函数)
            console.log(document.getElementById("h3").innerText)//this is a msg
        },
        //运行中周期
        beforeUpdate() {
            //数据被改变触发,此时页面中显示的数据还未与最新数据同步,data数据已经最新
            console.log(document.getElementById("h3").innerText)//页面数据:this is a msg
            console.log(this.msg)//data中的msg数据:this is not a msg
        },
        updated() {
            //此时,页面和data数据已经保持同步,都是最新的
            console.log(document.getElementById("h3").innerText)//页面数据:this is not a msg
            console.log(this.msg)//data中的msg数据:this is not a msg
        },
    })
</script>
```



![](lifecycle.png)

### vue-resource实现get,post,jsonp请求

> 除了vue-resource外,还可以使用axios实现数据的请求
>
> 用于测试请求的api(有兴趣的小伙伴可以自己写api接口测试)
>
> 1. get请求: https://jsonplaceholder.typicode.com/users
> 2. post请求: https://jsonplaceholder.typicode.com/users
> 3. jsonp请求: https://jsonplaceholder.typicode.com/users

[vue-resource github docs](https://github.com/pagekit/vue-resource)

```html
<!-- 依赖于vue.js -->
<script src="https://cdn.jsdelivr.net/npm/vue-resource@1.5.1"></script>

<div id="app">
    <input type="button" value="get请求" @click="getInfo">
    <input type="button" value="post请求" @click="postInfo">
    <input type="button" value="jsonp请求" @click="jsonpInfo">
</div>

<script>
    var vm = new Vue({
        el: '#app',
        data: {},
        methods: {
            getInfo() {
                var get_url = 'https://jsonplaceholder.typicode.com/users'
                this.$http.get(get_url).then(response => {
                    console.log(response.body)
                })
            },
            postInfo() {
                //emulateJSON:Send request body as application/x-www-form-urlencoded content type
                var post_url = 'https://jsonplaceholder.typicode.com/users'
                this.$http.post(post_url,{},{emulateJSON:true}).then(response => {
                    console.log(response.body)
                })
            },
            jsonpInfo() {
                var jsonp_url = "https://jsonplaceholder.typicode.com/users"
                this.$http.jsonp(jsonp_url).then(response => {
                    console.log(response.body)
                })
            }
        }
    })
</script>
```

### vue-resource

#### 结合node手写jsonp

> 前提:已经安装了node.js,参考[node.js安装配置](https://www.runoob.com/nodejs/nodejs-install-setup.html)
>
> vscode本地服务器启动: 下载Live Server插件> open with live server
>
> nodejs app启动: vscode > debug > start debugging

1. JSONP的实现原理
   - 由于浏览器的安全限制,不允许ajax访问协议不同,域名不同,端口不同的数据接口,浏览器认为这种访问不安全
   - 可以通过动态创建script标签的形式,把script标签的src属性,指向数据接口的地址,因为script标签不存在跨域限制,这种数据获取方式,称作jsonp(根据jsonp的实现原理,jsonp只支持get请求)
   - 具体实现过程
     - 先在客户端定义一个回调方法,预定义对数据的操作
     - 再把这个回调方法的名称,通过url传参的形式,提交到服务器的数据接口
     - 服务器数据接口组织好要发送给客户端的数据,再拿到客户端传过来的回调方法命名成,拼接出一个调用这个方法的字符串,发送给客户端去解析执行
     - 客户端拿到服务器返回的字符串之后,当作script脚本去解析执行,这样就能够拿到jsonp的数据了

client_jsonp.html

```html
<script>
    function showInfo(data) {
        console.log(data) //{name: "zhangsan", age: 18, gender: "male"}
    }
</script>

<script src="http://127.0.0.1:3000/getscript?callback=showInfo"></script>
```

node_module/app.js

```js
//导入http内置模板
const http = require('http')
//这个核心模块,能够帮我们解析url地址,从而拿到pathname query
const url_module = require('url')

//创建一个http服务器
const server = http.createServer()
//监听http服务器的request请求
server.on('request',function(req,res) {
    //const url = req.url
    const {pathname:url,query} = url_module.parse(req.url,true)
    if(url == '/getscript') {
        //拼接一个合法的js脚本
        //var scriptStr = 'show()'
         var data = {
            name: 'zhangsan',
            age: 18,
            gender: 'male'
        }

        var scriptStr = `${query.callback}(${JSON.stringify(data)})`
        //res.end发送给客户端,客户端拿到后当作js代码解析执行
        res.end(scriptStr)
    } else {
        res.end('404')
    }
})

//指定端口号并启动服务器监听
server.listen(3000,function() {
    console.log('server listen at http://127.0.0.1:3000')
})
```

此时<code>http://127.0.0.1:5500/client_jsonp.html</code> (live server默认5500端口),控制台此时输出 ok

#### 案例:vue-resource实现品牌列表改造

前端代码:

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>vue resource改造品牌列表</title>
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" 
        integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/vue-resource@1.5.1"></script>
</head>
<body>
    <div id="app">
        <div class="panel panel-success">
            <div class="panel-heading">
                <h3 class="panel-title">添加品牌</h3>
            </div>
            <div class="panel-body form-inline">
                <label>
                    NAME
                    <input type="text" class="form-control" v-model="name" >
                </label>
                <input type="button" class="btn btn-success" value="新增" @click="add" >
                <input type="button" class="btn btn-success" value="获取品牌列表" >
            </div>
        </div>

        <table class="table table-bordered table-hover table-striped">
            <thead>
                <tr>
                    <th>id</th>
                    <th>name</th>
                    <th>ctime</th>
                    <th>operation</th>
                </tr>
            </thead>
            <tbody>
                <tr v-for="item in list" :key="item.id">
                    <td>{{item.id}}</td>
                    <td>{{item.name}}</td>
                    <td>{{item.cname}}</td>
                    <td>
                        <a href="" @click.prevent="remove(item.id)">删除</a>
                    </td>
                </tr>
            </tbody>
        </table>
    </div>
    <script>
        const global_url = "http://127.0.0.1:8080";
        var vm = new Vue({
            el: '#app',
            data: {
                name:'',
                list: {}
            },
            created() {
                this.$http.get(`${global_url}/api/getprodList`).then(response => {
                    var data = response.body
                    if(status == 0) {
                        this.list = data.msg
                    } else {
                        alert("数据获取失败")
                    }
                })
            },
            methods: {
                add() {
                    this.$http.post(`${global_url}/api/addproduct`,{name:this.name},{emulateJSON:true}).then(response => {
                        var data = response.body
                        if(status == 0) {
                            this.list = data.msg
                            this.name = null
                        } else {
                            alert("添加失败")
                        }
                    })
                },
                remove(id) {
                    //this.list.splice(this.list.findIndex(item => item.id === id), 1)
                    this.$http.delete(`${global_url}/api/removeproduct/${id}`).then(response => {
                        var data = response.body
                        if(status == 0) {
                            this.list = data.msg
                        } else {
                            alert("删除失败")
                        }
                    })
                }
            }
        })
    </script>
</body>
</html>
```

> 因为视频的api链接失效了,所以自己按照api文档手写了一个(凑合使用),熟悉后端的可以自己写一个

[后端api代码](https://github.com/helloworld-liushijie/api)

#### vue-resource全局配置数据接口的根域名

```javascript
//Note that for the root option to work, the path of the request must be relative. This will use this the root option: Vue.http.get('someUrl') while this will not: Vue.http.get('/someUrl').
Vue.http.options.root = '/root';
//such as: api/getprodList不能写成/api/getprodList => 不能使用绝对路径
Vue.http.options.root = 'http://127.0.0.1:8080'
this.$http.get(`api/getprodList`).then(response => {})
```

#### 全局启用emulateJSON

```javascript
Vue.http.options.emulateJSON = true;
```

### [vue动画](https://cn.vuejs.org/v2/guide/transitions.html)

> 动画能够提高用户的体验,帮助用户更好的理解页面中的功能

#### 自定义动画

```html
<style>
    /* v-enter 是进入之前,元素的起始状态,还没开始进入;v-leave-to: 是动画离开之后,离开的终止状态 */
    .v-enter,
    .v-leave-to {
        opacity: 0;
        transform: translateX(200px);
    }

    /* v-enter-active:入场动画的时间段;v-leave-active:离场动画的时间段 */
    .v-enter-active,
    .v-leave-active {
        transition: all .8s ease;
    }

    .my-enter,
    .my-leave-to {
        opacity: 0;
        transform: translateY(200px);
    }

    /* v-enter-active:入场动画的时间段;v-leave-active:离场动画的时间段 */
    .my-enter-active,
    .my-leave-active {
        transition: all .8s ease;
    }
</style>
<div id="app">
    <input type="button" value="toggle" @click="flag = !flag"><br/>
    <input type="button" value="toggle" @click="flag1 = !flag1">
    <!-- 需求: 点击按钮,让h3显示,再点击让h3隐藏 -->
    <!-- 使用transition元素(vue官方提供),把需要控制的元素包裹起来 -->
    <transition> 
        <h3 v-if="flag">this is h3</h3>
    </transition>
    <!-- 自定义动画,分别设置效果,将默认v改为name值即可 -->
    <transition name="my"> 
        <h6 v-if="flag1">this is h6</h6>
    </transition>
</div>
<script>
    var vm = new Vue({
        el: '#app',
        data: {
            flag: false,
            flag1: false
        },
        methods: {}
    })
</script>
```

#### 使用第三方动画库

```html
<link href="https://cdn.bootcss.com/animate.css/3.7.2/animate.min.css" rel="stylesheet">
	<!-- 
		使用transition元素(vue官方提供),把需要控制的元素包裹起来
		如果动画不生效可以加上animated bounceIn
		使用:duration设置入场和离场动画时长(ms) => 分开设置动画时间 :duration="{enter:200,leave:400}"
	-->
	<transition enter-active-class="bounceIn" leave-active-class="bounceOut" :duration="400"> 
    <h3 v-if="flag">
        this is h3
    </h3>
</transition>
```

#### 钩子函数实现半场动画

```html
<transition
  v-on:before-enter="beforeEnter"
  v-on:enter="enter"
  v-on:after-enter="afterEnter"
  v-on:enter-cancelled="enterCancelled"

  v-on:before-leave="beforeLeave"
  v-on:leave="leave"
  v-on:after-leave="afterLeave"
  v-on:leave-cancelled="leaveCancelled"
>
```



```html
<style>
    .ball {
        background-color: red;
        width: 20px;
        padding-top:20px;
        border-radius: 50%;
    }
</style>
<div id="app">
    <input type="button" value="快到碗里来" @click="flag=!flag">
    <transition
                @before-enter="before_enter"
                @enter="enter"
                @after-enter="after_enter">
        <div class="ball" v-show="flag"></div>        
    </transition>
</div>
<script>
    var vm = new Vue({
        el:'#app',
        data: {
            flag: false
        },
        methods: {
            //注意: 动画钩子函数的第一个参数el,表示要执行动画的那个dom元素,是个原生的js dom对象
            before_enter(el){
                //before-enter:表示动画入场之前,此时动画尚未开始,可以在before-enter中,设置元素开始动画之前的起始样式
                el.style.transform = 'translate(0,0)'
            },
            enter(el,done){
                //一定要设置el.style.transition的时间（如果只使用js钩子的话）,不然没有过渡效果。
                el.offsetWidth
                //enter:表示动画开始之后的样式,设置完成动画后的结束状态
                el.style.transform = 'translate(150px,450px)'
                el.style.transition = 'all 1s ease'
                //这里的done其实就是after_enter函数的引用
                done()
            },
            after_enter(el){
                this.flag = !this.flag
                console.log('ok')
            }
        }
    })
</script>
```

#### 列表动画

```html
<ul>
    <!-- 在实现列表过渡的时候,如果需要过渡的元素,是通过v-for循环渲染出来的,
		不能使用transition包裹,需要使用trnasitionGroup,其他的没变化 -->
    <transition-group enter-active-class="animated rollIn" leave-active-class="animated rollOut" 					:duration="1500">
           	<li v-for="item in list" :key="item.id">
                {{item.id}}=>{{item.name}}
            </li>
    </transition-group>
</ul>
```



资料参考

[vue.js guide](https://cn.vuejs.org/v2/guide/)

[keycode键盘 按键 - 键码 对应表](https://www.cnblogs.com/yiven/p/7118056.html)

[animate.css](https://daneden.github.io/animate.css/)