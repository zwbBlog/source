---
title: vue.js
date: 2015-2-01 14:07:12
---

``` bash
### vue1.0和2.0
v-on:click  可以简写为 @click
v-bind      可以简写成 :
v-cloak/v-text/v-html     防止闪烁  [v-cloak]{display:none}
事件对象为:$event
       阻止冒泡:
               a) e.cancelBubble=true;
               b) @click.stop="fun()";
       阻止默认行为:
               a) e.preventDefault();
               b) @contextmenu.prevent
键盘事件:
               按下:@keydown=show($event);  e.keyCode
               抬起:@keyup=show($event);    e.keyCode
               回车(13,enter)、上(up)、下(down)、左(left)、右(right)
               回车键示例:@keydown.13=fun()||@keydown.enter=fun()
属性:
       v-bind
                <img src="{{url}}" alt="">      效果能出来，但是会报错
                <img v-bind:src="url" alt="">   效果能出来，不报错
       :class和:style属性注意点
                :class={red:true/false,blue:true/false,...}
                :style="a" --> data:{a:{color:"red",backgroundColor:"blue"}}
模版:
       {{msg}}    数据与模版同时更新
       {{*msg}}   数据只绑定一次
       {{{msg}}}  html转意输出
过滤器:
       内置过滤器:
           {{msg|filterA (参数)|filterB (参数)}}
           uppercase  转大写
           lowercase  转小写
           capitalize 首字母大写
           currency   钱币符号
           debounce   延迟
           ...
       与数组配合使用的过滤器:
           limitBy  参数一取几个 参数二从哪个开始取
           filterBy 过滤数据
           orderBy  排序   1正序  -1倒序
       双向过滤器:
           自定义过滤器
交互:
       引入 vue-resource
       get:
           get:function () {
              this.$http.get("data/1.txt").then(function (res) {
                      alert("请求成功"+" "+res.status+" "+res.data)
                  },function (res) {
                      alert("请求失败"+" "+res.status+" "+res.data)
                         })
                 },
       get2:function () {
              this.$http.get("data/1.php",{
                       //传参给服务器
                           a:1,
                           b:2
                  }).then(function (res) {
                       alert("请求成功"+" "+res.status+" "+res.data)
                   },function (res) {
                       alert("请求失败"+" "+res.status+" "+res.data)
                          })
                  }
       post:
           post:function () {  //三个参数 （url,参数,配置）
                 this.$http.post("data/2.php",{a:1,b:50},{emulateJSON:true}).then(function (res) {
                       alert("请求成功"+" "+res.status+" "+res.data)
                 },function (res) {
                       alert("请求失败"+" "+res.status+" "+res.data)
                            })
                 }
       jsonp:
            jsonp: function () {
                   this.$http.jsonp("https://www.baidu.com/his";,
                   {wd: "a"},  //第二个参数是上传给服务器的数据
                   {jsonp: "callback"})  //回调函数的名字,默认为callback
                   .then(function (res) {
                       console.log(("请求成功" + " " + res.status + " " + res.data.s));
                    },function (res) {
                       alert("请求失败" + " " + res.status + " " + res.data)
                               })
                 }
        this.$http({
            url:"https://www.baidu.com/his";,
            method:"jsonp"/"get"/"post",
            jsonp:"cb",     //cbName
            data:{a:1,b:2}
        }).then(function(res){"成功"},function(res){"失败"})
Vue生命周期:
        钩子函数
        init:
        created:function(){"实例已经创建"}
        beforeCompile:function(){"编译之前"}
        compiled:function(){"编译之后"}
        ready:function(){"插入到文档中"}
        beforeDestroy:function(){"销毁之前"}
        destroyed:function(){"销毁之后"}
计算属性:
        computed:{}
内置属性
       vm.$el           -->绑定的元素
       vm.$data         -->数据对象
       vm.$mount        -->手动挂载  new Vue().$mount("body")
       vm.$options      -->自定义属性
       vm.$destroy()    -->销毁对象
       vm.$log()        -->查看数据状态
循环数据重复  track-by="key"
自定义指令:
      属性指令:
          Vue.directive("Name",function(color){})  //this.el-->指令所对应的元素
          v-red -->red           v-red="color"
          拖拽
      元素指令:
          Vue.elementDirective("Name",{bind:function(){}})
      键盘信息:
          Vue.directive("on").keyCodes.ctrl=17
监听事件:
      $watch
        vm.$watch("a",function (){})   //浅度监听
        vm.$watch("json",function (){},{deep:true})   //深度监听
结合animate.css做动画:
Vue组件:
      全局组件:
             var title=Vue.extend({
                 data(){      //组件中放数据
                    return {
                        msg:"我是自定义的标题组件"
                    }
                 },
                 methods:{
                    change(){
                      this.msg="改变数据"
                    }
                 }
                 template:"<h2>{{msg}}</h2>"
             });
             Vue.component("aaa",title);
      局部组件:
            去掉 Vue.component("aaa",title),把它写在根组件上(components);
            new Vue({
             components:{
                aaa:title
                }
            })
      另一种写法(全局):
            Vue.component("you-aaa",{
                 template:"<b>简洁写法</b>"
             });
             new Vue({
                       components:{
                         aaa:{
                            template:"简洁的局部组件"
                         }
                      }
               })
      动态组件:
        <component :is="a"></component>
      组件之间的通信:
         子获取父的数据 父模版中调用的子组件绑定属性记录父组件的数据 子组件props展示出数据
         父获取子的数据
      slot占位  类似ng中的transclude
vue-router SPA单页面应用
    html
          跳转 v-link={path:"/home"}   v-link={path:"/news"}
          显示视图  <router-view></router-view>
    js
         1.创建一个根组件
            var App=Vue.extend()
         2.创建home和news组件
            var Home=Vue.extend({
              template:"<h1>主页内容</h1>"
            })
            var News=Vue.extend({
              template:"<h1>主页内容</h1>"
            })
         3.创建路由模块
            var router=new VueRouter()
         4.配置路由
            router.map({
              "/home":{component:Home},
              "/news":{component:News}
            })
         5.开启路由
           router.start(App,"#box")
         6.设置默认显示
           router.redirect({
                "/":"home"
           })
vue-cli脚手架(用cmd的cd找文件路径-->c:或者e:找到路径)
        1  npm install vue-cli -g    验证是否安装成功   vue --version
        2  生成项目模版  vue init 模版名称 本地文件名   vue init simple vuePro
               模板有
               simple
               webpack     多了代码规范和单元检测
               webpack-simple  （推荐使用）
        3 进入到生成目录里面
               npm install
        4 运行 npm run dev
Vue2.0
        1.不支持模版中多个代码片段了
        2.组件的写法有所不同   不推荐在使用Vue.extend()
            新写法:
                  var home={
                      template:"#a1",
                      data(){
                        return {
                            a:12342351513

                            }
                       },
                      methods:{}
                   };
                  Vue.component("aa",home);     <aa></aa>
            精简写法:Vue.component("bb",{template:"<h1>模版信息</h1>"})    <bb></bb>
        3.生命周期
           beforeCreate   实例刚创建,身上没有属性
           created        实例已经创建,属性已经绑定
           beforeMount    模版编译之前
           mounted        模版编译完成  相当于ready
           beforeUpdate   组件更新之前
           updated        组件更新之后
           beforeDestroy  组件销毁之前
           destroyed      组件销毁之后
        4.v-for循环中的索引
              v-for="(val,index) in arr"   与1.0相反
              默认可以添加重复数据  提高性能写法 :key="index"    与1.0 track-by="$index"差不多
        5.自定义键盘事件
              Vue.config.keyCodes.ctrl=17   1.0的写法 Vue.directive("on").keyCodes.ctrl=17
        6.内置过滤器删除了,需要自己自定义过滤器,传参方式也改变了
              Vue.filter("toDou",function(input,a,b){return ...})
              {{msg | toDou(a,b)}}
        7.组件间的通信
              子组件获取父组件的数据  props
              之前子组件可以修改父组件的数据.并且可以数据同步   :msg.sync="a"
              现在子组件获取父组件的数据并且修改数据会报错
                   解决:a)同步  父组件传递一个对象类型的数据
                        b)子组件自己定义数据,通过mounted再把父组件的数据赋值给子组件的数据
              *单一事件管理组件通信
        8.动画-transition
            1.<transition name="fade">
               html元素、路由、组件...
            </transition>
            2.配合animate使用
                单个元素动画  transition
                多个元素动画  transition-group
        9.路由
            html:
                <router-link to="/home"></router-link>
                <router-view></router-view>
            js:
                    //准备组件
                    var Home={
                        template:"<h3>我是主页内容</h3>"
                    };
                    var News={
                        template:"<h3>我是新闻页内容</h3>"
                    };
                    //配置路由
                    var routes=[
                        {path:"/home",component:Home},
                        {path:"/news",component:News},
                        {path:"*",redirect:"/home"}
                    ];
                    //创建路由实例对象
                    var router=new VueRouter({routes});
                    //挂载路由到Vue对象
                    new Vue({
                        router,
                        el:"#box"
                    })
            路由嵌套及传参
                    var routes=[
                        {path:"/home",component:Home},
                        {path:"/user",component:User,
                            children:[{path:"/user/:username/age/:age",component:Username}]   *核心
                        },
                        {path:"*",redirect:"/home"}
                    ];
                    <p>我是{{$route.params.username}}用户,今年{{$route.params.age}}岁</p>
Vue2.0脚手架
            vue init webpack-simple vuePro
            cnpm install
            npm run dev
            在src文件夹下创建自己的components文件夹存在组件
elementUI -->PC端  (http://element.eleme.io)
    1.新建项目  vue init webpack-simple element-demo
    2.下载element-ui框架 npm i element-ui -D (i-->install  D--> --save-dev  S--> --save)
    3.引入框架 2种方式
        a)在index.html中link引入
        b)在main.js中引入 inport ElementUI from 'element-ui'
        *下载style-loader再配置  cnpm install style-loader --save-dev
    组件可以添加事件
    自己定义的组件不能直接添加事件 需要这样写 @click.native='get'
mint-ui  -->移动端 (http://mint-ui.github.io/#!/zh-cn)
vuex：集中式管理数据 (https://vuex.vuejs.org)
      cnpm install vuex -save   保存在dependencies中
      mapActions    管理事件(行为)
      mapGetters    获取数据

点击跳转路由
元素内     @click="$router.push(term.path)                    参数形式
               @click="$router.push('/home/index/data1')   手写形式
方法内     this.$router.push({ path:'/home/index/data1' });
属性
说明
$route.path
当前路由对象的路径，如'/view/a'
$rotue.params
关于动态片段（如/user/:username)的键值对信息,如{username: 'paolino'}
$route.query
请求参数，如/foo?user=1获取到query.user = 1
$route.router
所属路由器以及所属组件信息
$route.matched
数组，包含当前匹配的路径中所包含的所有片段所对应的配置参数对象。
$route.name
当前路径名字

Vue.set(listArr,1,修改内容)

vue路由传参
<router-link :to="'/index/data/'+params" tag="li"></router-link>
import axios from 'axios'
Vue.prototype.$http = axios
get(){
    var self=this;
    self.$http.get("./src/a.json",{params:{a:1,b:2}}).then(function(res){
        self.myMessage=res.data;
        console.log(res);
        alert("请求成功get");
        }).catch(function(err){
            alert("请求失败");
        });

    var data={c:3,d:4};
    self.$http({url:"./src/b.txt",method:"get",params:data}).then(function(res){
        console.log(res);
        alert("请求成功");
        }).catch(function(err){
            alert("请求失败");
        });

//也使用jquery的请求方式
        $.ajax({
            type:"post",
            url: "./src/a.json",
            data: {c:3,d:4},
            dataType:"json",
            success:function(res){
            alert("请求成功")
                }
            });

//jquery请求并发问题解决
$.when($.ajax("./src/a.json"),$.ajax("./src/b.txt")).done(function(res1,res2){
            console.log(res1[0]);
            console.log(res2[0]);
})

//axios请求并发问题解决
function ajax1(){
        return self.$http.get("./src/a.json")
}
function ajax2(){
        return self.$http.get("./src/b.txt")
}
self.$http.all([ajax1(),ajax2()]).then(self.$http.spread(function(res1,res2){
        console.log(res1);
        console.log(res2);
    }))
}
//保证DOM元素加载完成
methods:{
       toggle(){
            var self=this;
            if(self.isShow==true){
                 self.isShow=false
             }else{
                 self.isShow=true;
                //保证DOM元素加载完成
                 self.$nextTick(()=>{
                       self.getFocus()
                 })
            }
       },
       getFocus(){
             this.$refs.inp.focus();
       }
 }	  
```