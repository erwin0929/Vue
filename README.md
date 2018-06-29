## Vue 学习总结

> vue介绍省略,安装步骤省略。本文总结vue使用过程中的一些要点和经验分享,全程手打,杜绝复制粘贴敷衍了事,做一个有梦想的程序猿。  

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

## 计算和监听
* computed: 由于插值里的表达式过于简单,且不能复用,computed应运而生,所有复杂的逻辑都可以在这里实现
* methods: 定义一个方法,可以再绑定点击事件,可以进行逻辑运算,可以调用其他方法,做任何想做的
* watch: 监听某个值的改变,如果改变了可以定义函数来进行任何操作。  
函数接受2个参数,1个新值,1个是旧值可以监听路由的变化,store的变化,某些属性的变化
  1. 当监听某个对象的时候,其下的属性发生变化是检测不到的,我们需要设置deep:true来深度监听
  2. vm.$watch 也可以主动调用来监听某个数据
* 每当重新渲染时,computed依赖于某个属性,当属性发生了变化才会重新计算,否则将会使用缓存,而methods不根据依赖属性总会再次执行。


## 生命周期 (非常重要)


## 组件 (这才是vue的精髓所在)

## 组件通讯

## 路由


## 状态管理