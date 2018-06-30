## Vue 学习总结

> vue介绍省略,安装步骤省略。本文总结vue使用过程中的一些要点和经验分享,下面所说的全是开发过程中使用到的,可能不完全,毕竟我不是写Api。全程手打,杜绝复制粘贴敷衍了事,做一个有梦想的程序猿。  

* 常用开发环境介绍: 
  1. node版本: v8.11.3。
  2. npm版本: 6.1.0。
  3. vue版本: 2.x。
  4. 编辑器: webstorm。
  5. 代码管理工具: github。
  6. 系统环境: macOs 10.12.6
  7. 使用vue-cli脚手架建立模板,单文件开发模式。

## 双向绑定
 
Vue实例中的data属性与其渲染的dom保持一致,无论谁被改变,另一方也会更新为相同的数据。  
使用插值来显示数据`{{}}`,使用指令`v-model="message""`来实现双向绑定,当用户编辑该输入框时,会将更改的值实时同步到实例的`data`属性中。  
而指令`v-model`不过是一种语法糖,展开写法为`v-bind:value="message"`,`v-on:input="message = $event.target.value"`,将元素的value属性
与message互相映射,为元素绑定input事件,触发该事件时,将输入的值更新到message中。

## 指令

* {{}}: 称为插值,又称为Mustache 语法,将对应属性解析为普通文本
* 在可以绑定属性的地方,也支持简单的单个表达式,语句和流程控制语句不生效。
* v-once: 一次性绑定,没有响应式功能
* v-html: 解析为HTML代码
* v-text: 解析为普通文本,性能较插值更好
* v-bind: 由于插值不能用在元素属性上,这种情况下,我们使用v-bind:id来绑定元素属性,简写格式为 :id, :title, :src
* v-if: 根据绑定的值来判断元素是否被渲染,
* v-else: 必须紧跟在v-if之后
* v-esle-if: 同上,可连续使用
* v-show: 根据绑定的值来判断元素是否显示(切换display属性),无论值为真假,该元素都被成功渲染
* v-on: 事件绑定,v-on:click,v-on:keydown,v-on:keyup,简写格式@click,@keyup  
@click.stop:阻止事件传播,@click.prevent:阻止浏览器默认事件
* v-for: v-for="(item, index) in items" 遍历items数组,item为值,index为键,也可遍历对象
* :key 使用key设定不同的值告知vue不要复用该元素
* :class: 为元素设定样式
  1. :class="{ active: isActive } 对象写法,当isActive为true,则设定样式为active,该对象可以有多个键值对
  2. :class="[activeClass, errorClass]" 定了2个实例属性,会将2个属性都渲染成对应的值作为元素的class
  3. :class="[isActive ? activeClass : '', errorClass]" 也可以使用三元判断
  4. :class="[{ active: isActive }, errorClass] 先判断isActive是否为真来决定是否设置active,再设置errorClass对应的值为class
  5. <my-component class="baz boo"></my-component> 用在组件上时,将会作为组件根元素的样式,而不是覆盖
* :style 为元素设定行内样式  同样有数组和对象写法,与class类似

## Vue实例

* 每一个组件都是创建一个Vue实例时,我们可以定义一个对象存入到实例的data属性中,当我们改变这个对象的值时,视图也会产生响应,更新为最新的值,而我们修改视图上的值时,也会更新到实例的data属性中,当data属性中的值改变,视图会重新渲染。
* 只有在Vue实例化时,data中存在的属性才会拥有响应式功能,也就是想让某个属性完成响应功能,必须在data属性中提前定义,实例化完成后再添加新属性是不具响应功能的。
* Object.freeze() 该方法会冻结对象的修改。
* Vue实例中暴露了许多$开头的属性,这是为了和用户定义的属性区分开
* $data: 存放了所有定义的数据对象
* $el: 存放了挂载的根元素
* $children: 存放了调用的子组件的实例,数组类型,可以使多个实例
* $props: 存放了父组件传递的数据,对象类型
* $parent: 存放了父组件的实例,对象类型,只能有1个父组件
* $root: 存放了组件树的根组件实例,如果没有父组件,实例则是自己
* $options: 存放了实例化组件时传递的选项对象
* $slot: 数组类型,存放了插槽替换的内容,可以是多个
* $attr: 存放了不被props获取的传值,class和style除外

## 计算和监听 ,这些属性将被混入到Vue实例中
* computed: 由于插值里的表达式过于简单,且不能复用,computed应运而生,所有复杂的逻辑都可以在这里实现
* methods: 定义一个方法,可以再绑定点击事件,可以进行逻辑运算,可以调用其他方法,做任何想做的
* watch: 监听某个值的改变,如果改变了可以定义函数来进行任何操作。  
函数接受2个参数,1个新值,1个是旧值可以监听路由的变化,store的变化,某些属性的变化
  1. 当监听某个对象的时候,其下的属性发生变化是检测不到的,我们需要设置deep:true来深度监听
  2. vm.$watch 也可以主动调用来监听某个数据
* 每当重新渲染时,computed依赖于某个属性,当属性发生了变化才会重新计算,否则将会使用缓存,而methods不根据依赖属性总会再次执行。
* 使用场景: computed 一般用于计算某个属性的值,比如根据数据的gender来判断显示男或女,只要数据没有发生变化,就会依赖缓存不再重新计算,多使用return将结果返回。methods: 一般多用于事件绑定,或者读取数据的方法也可以写在里面,不使用return,进行一些具体的操作。watch一般用于观测某个值的变化,可以监听路由的变化来判断用户是否登录。


## 生命周期 (非常重要)
* 声明周期是我比较感兴趣的话题,vue中有许多声明周期函数,他赋予了我们在某个阶段执行某些方法的能力
* 共有8个阶段:实例创建前后,挂载前后,更新前后,销毁前后。
  1. beforeCreate 组件刚被创建,数据观测未执行,data属性不可见
  2. created 实例创建完成,data属性可见,dom未生成,$el不可见,此时可以读取远端数据,并给出loading动画
  3. beforeMounted 元素挂载之前,dom被渲染,data属性可见,
  4. mounted 元素挂载之后,将data里对应的属性渲染进dom,即插值生成普通文本,$el可见
  5. beforeUpdate 在data下的值更新之前,dom未重新渲染
  6. updated 在data下的值更新之后,dom重新被渲染
  7. beforeDestroy 实例销毁之前
  8. destoryed Vue实例中的所有事件监听器被移除，所有的子实例也被销毁,数据也被销毁


## 组件 (这才是vue的精髓所在)

* 使用Vue.component方法来创建全局组件,这样在任何组件中都可以使用。
* 但是如果使用了webpack构建工具,这样注册是浪费性能的行为,多数情况下,我们使用局部注册。
* 单文件开发模式下,我们首先创建一个components目录,将组件文件Abc.vue放进去  
在需要使用的文件中import myComonent from './components/myComonent'引入后,再在选项对象中,  
我们定义一个components属性, components: {myComponent},以上完成注册后,可以在模板中使用了。  
注意使用kebab-case的写法<my-component>。
* 组件通信: 大多情况下,我们要为子组件传递数据,<my-component post-msg="我是传递的数据">(静态传递,不使用变量),其中post-msg使用kebab-case命名,在子组件的选线对象中,新增props属性,以字符串数组形式接受 props: ['postMsg'],也可以使用限定传递的数据类型  
```
props: {
  title: String,
  likes: [String, Number], // 多个可能的数据类型
  isPublished: Boolean,
  commentIds: {
    type: Number,   // 必须是数值类型
    default: 100,   // 默认为100
    required: true  // 必须传递该数据
  },
  author: Object
}
```
* 该传递方式为单向,父级修改数据,子级也更新,这就意味着,不应更改子组件的值,否则将会发出warning。
* 子组件可以使用$emit向父组件传值,该方法接受2个参数,第一个是监听器名,第二个是子组件向父组件传的数据
* slot插槽会替换组件标签里的内容,当多处使用一个组件时,某一个组件下要添加一张图片或一段文字,那么此时slot变得非常有用。
有时我们要用到多个插槽,可以使用具名插槽。 slot='home' 与 <slot name='home'></slot> 形成替换,如果没有找到对应的name值,则不做替换。
也可以保留一个未命名的插槽用作默认显示。
可以在实例中找到插槽,访问方式: vm.$slots。
* 父子组件间的访问还可以使用 $root, $parent, $children这些属性来访问,不过这些属性使用起来会不利于维护
* keepalive 使用该方法时如果切换到其他组件时将会缓存不活动的组件,不进行销毁,不再重新渲染。
当组件被切换时,会有2个生命周期函数触发 activated 组件激活时调用 和 deactivated组件失去活动时调用
使用场景: 当我们首次进入列表页时读取数据,加缓存,从详情页回到列表页时不再读取数据,直接读缓存


## 路由

* 通过在Vue根实例中传入router实例,从而注入到每个子组件中

* 先介绍下钩子函数
  1. 全局前置导航守卫 beforeEach(to, from, next) 每次路由跳转之前触发,接受3个参数,to: 目标路由,from:起始路由,next跳转的方法。可以设定页面加载提示
  2. 全局后置导航守卫 afterEach 监听每次路由跳转之后,可以关闭页面加载提示
  3. 路由独享守卫, 定义在路由文件里, beforeEnter(to, from, next) 进入该路由之前触发,与全局守卫接受的参数一样
  4. 组件守卫 定义在组件里, beforeRouteEnter(to, from, next) 路由跳转确认之前调用,不能获取当前实例,组件还未创建
  5. beforeRouteUpdate(to, from, next) 路由改变,当组件被复用时调用,比如 /foo/1 和 /foo/2 之间跳转的时候 
  6. beforeRouteLeave(to, from, next) 离开路由之前调用

* 路由元信息meta, 定义在路由内,可以定义一些自定义属性,比如为路由设置title,来修改页面title,
可以设置keepAlive来判断是否要缓存当前路由, 可以这是scrollToTop来决定路由跳转之后滚动的位置
* 使用 <transition name="fade" mode="out-in"></transition>完成动画过渡,name决定使用哪个动画样式,mode决定先渲染再进行动画过渡,有很多钩子函数
* 何时读取数据,我的习惯是在created中读取数据,同时展示loading动画
* 滚动行为 scrollBehavior (to, from, savedPosition) 可以设置每次路由跳转滚动的位置,to,from接受路由对象,
savedPosition该参数通过浏览器前进或后退才会触发
* 路由懒加载,import('./Foo.vue'),使用import来按需加载组件而不是一次性加载所有组件
* 动态路由,可以设置path:'detail/:id'来根据传递的id值判断显示哪个商品的详情页,可以有多个动态参数
* 嵌套路由,定义一个children属性  
```
children: {
  path: 'list',
  component: List
}
```
该子组件会被渲染在父组件的router-view中。
* 可以为路由命名name: 'user',从而push使用该名字就可完成跳转,简便快捷
* router-link组件的属性
  1. to:路由跳转,等同于$route.push,有诸多写法  
  ```
  :to="{ path: 'home' }"  跳转至path为home的路由
  :to="{ name: 'user', params: { userId: 123 }}" 跳转到名为user的路由,传递动态参数userId为123
  :to="{ path: 'register', query: { plan: 'private' }} 跳转到path为register的路由,添加查询参数/register?plan=private
  ```
  2. replace: 添加该属性导航不会留下历史记录
  3. append: 在当前路由后追加路径
  4. tag: 将元素渲染成指定的标签
  5. active-class: 设置激活该链接时的样式
* router-view组件的属性
  1. name 会根据components下定义的name形成比较,如果匹配成功则渲染
* 路由对象$route.path存放了当前路径
* $route.params存放了当前路由的动态参数 'user/2'
* $route.query 存放了当前路由的查询参数 /register?plan=private
* $route.fullPath 包含了查询的参数的完整路径
* $route.name 存放了当前路由的名称
* $route.matched 包含了当前路由所有的嵌套关系,包括父路由,子路由,数组类型
* $route.redirectedFrom 存放了重定向来源的名字
* $route.meta 存放了路由元信息
* $router对象
  1. push 用于路由跳转
  2. go 用于路由跳转至前后的历史
  3. back 用于跳转至上一个路由
  4. forwaord 前进
  5. 该对象存放了所有的路由钩子函数
  6. app 存放了Vue根实例
  7. mode 路由模式
  8. options.route 存放了定义的所有路由记录


## 状态管理

vuex为我们提供了状态管理,不使用官方的解释,我的理解是把需要共享的变量存储在一个变量里,在根Vue实例化时作为参数,将store注入进去,这样所有的组件都能共享到其中的变量,也意味2个非亲非故的组件之间也能进行通信。

以下是我的常用解构。

```
── store
    ├── index.js           我们组装模块并导出 store 的地方
    ├── state.js           跟级别的 state
    ├── getters.js         跟级别的 getter
    ├── mutation-types.js  根级别的 mutations名称（官方推荐mutions方法名使用大写）
    ├── mutations.js       根级别的 mutation
    ├── actions.js         根级别的 action
    └── modules
        ├── m1.js         # 模块1
        └── m2.js  
```

最终,$store会被混入到Vue实例中,我们可以以简单粗暴的方式this.$store.state.someThing来获取某个"全局"变量(为了简单记忆,暂且叫他全局变量好了)。  

> state.js

我们可以在state.js中定义"全局"变量,定义判断用户是否登录和登录用户的信息

```
const state = {
  isLogin: true,
  userInfo: {}
}
```

> mutations-types.js

在该文件里定义mutations的名称,官方的推荐是使用大写方式命名。注意该文件只是命名,不做逻辑操作。  

```
// 获取购物车里的商品
export const GET_CART_INFO = 'GET_CART_INFO' 
// 获取用户信息
export const GET_USERINFO = 'GET_USERINFO' 
```

> mutations.js

* 不推荐手动$store.state.someThing = 'XXXX'来修改状态  
* 一般情况下,我们提交一个mutations来修改store中的状态
* mutations只能使用同步函数,这是官方限定,具体原因我还不是很懂  

```
const mutations = {
  // 获取购物车内的商品,接受2个参数,state就是存放数据的仓库,payLoad载荷接受提交过来的数据
  [GET_CART_INFO] (state, payLoad) {
    // 将载荷中的数据存入state中
    state.someThing = payLoad.data
  } 
}
```

> actions.js

* 通过分发actions来提交一个mutations
* 这里可以定义异步函数
* 这里存放了获取数据的主要逻辑,或者说actions是激活vuex的核心代码
* 我们来通过axios来获取用户信息

```
const actions = {
  // 这里形参接受一个与store实例具有相同属性的context对象,这里形参使用了解构赋值接受
  // 展开语法糖为: {commit} = context
  // 这里默认读者拥有es6解读能力,不做解释了
  getUserInfo ({ commit }) {
    // 使用axios调用接口,axios使用了es6中的promise写法,避免回调嵌套过深
    // actions允许异步操作
    axios.post('Api/getUser')
      .then((response) => {
        // 解构赋值得到list
        let { data: { list } } = response
        // 提交到mutations,list作为载荷传递
        commit({
          type: GET_USERINFO,
          list
        })
      })
  }
}
```

至此,一个简易的数据读取和保存就完成了,注意这不过是一个简单的流程,实际开发中,我们会用更复杂的逻辑来做。  
好了,数据是获取到了,怎么让他传递到dom层并渲染呢?

> mapState

我们可以使用$store.state.someThing来获取到state存储的数据,但是随着模块的增多,使用的频率变高,这种写法未免太过冗余,官方提供了mapState方法

```
// 首先引入mapState方法
import { mapState } from 'vuex'

computed: {
  // 使用对象展开运算符将此对象混入到外部对象中
  ...mapState({
     //定义一个data属性someThing其值是后续箭头函数的返回值,而箭头函数接受一个参数就是store下的state
     // 那么 this.someThing将映射为this.$store.state.someThing
     someThing: state => state.someThing,
     // 如果定义了模块
     anyThing: state => state.moduleA.antThing
  })
}
最终我们接收到了store中的state,那么将其与绑定至dom中,触发重新渲染,数据成功展示。
```

> mapMutations

有时我们不想通过分发actions来提交mutations

```
// 引入mapMutations方法
import { mapMutations } from 'vuex'

methods: {
  ...mapMutations({
    add: 'increment' // 将 `this.add()` 映射为 `this.$store.commit('increment')`
  })
}
这样就可以不通过actions直接提交一个mutations,以解某些紧急之需。
```

> mapActions

我们需要通过分发actions来执行里面的读取数据操作  
可以通过this.$store.dispatch('actions')来触发某个actions,如果觉得这样的写法过于冗余,vuex同样提供了简化方法

```
import { mapActions } from 'vuex'

methods: {
  ...mapActions({
    add: 'increment' // 将 `this.add()` 映射为 `this.$store.dispatch('increment')`
  })
}
```

以上是我的vuex个人经验,随着项目的壮大,代码会更加复杂,此时vuex就提供了很好的维护性  
在一些小项目中简单的组件就能完成。