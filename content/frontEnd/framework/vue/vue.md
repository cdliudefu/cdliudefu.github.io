<div class="article fmt article-content" data-id="1190000016344599" data-license="cc" deep="6">
                                
<p>看看面试题，只是为了查漏补缺，看看自己那些方面还不懂。切记不要以为背了面试题，就万事大吉了，最好是理解背后的原理，这样面试的时候才能侃侃而谈。不然，稍微有水平的面试官一看就能看出，是否有真才实学还是刚好背中了这道面试题。<br data-filtered="filtered">（都是一些基础的vue面试题，大神不用浪费时间往下看）</p>
<h2 id="item-1">一、对于MVVM的理解？</h2>
<p>MVVM 是 Model-View-ViewModel 的缩写。<br data-filtered="filtered"><strong>Model</strong>代表数据模型，也可以在Model中定义数据修改和操作的业务逻辑。<br data-filtered="filtered"><strong>View</strong> 代表UI 组件，它负责将数据模型转化成UI 展现出来。<br data-filtered="filtered"><strong>ViewModel</strong> 监听模型数据的改变和控制视图行为、处理用户交互，简单理解就是一个同步View 和 Model的对象，连接Model和View。<br data-filtered="filtered">在MVVM架构下，View 和 Model 之间并没有直接的联系，而是通过ViewModel进行交互，Model 和 ViewModel 之间的交互是双向的， 因此View 数据的变化会同步到Model中，而Model 数据的变化也会立即反应到View 上。<br data-filtered="filtered"><strong>ViewModel</strong> 通过双向数据绑定把 View 层和 Model 层连接了起来，而View 和 Model 之间的同步工作完全是自动的，无需人为干涉，因此开发者只需关注业务逻辑，不需要手动操作DOM, 不需要关注数据状态的同步问题，复杂的数据状态维护完全由 MVVM 来统一管理。</p>
<h2 id="item-2">二、Vue的生命周期</h2>
<p><strong>beforeCreate</strong>（创建前）    在数据观测和初始化事件还未开始<br data-filtered="filtered"><strong>created</strong>（创建后）    完成数据观测，属性和方法的运算，初始化事件，$el属性还没有显示出来<br data-filtered="filtered"><strong>beforeMount</strong>（载入前）    在挂载开始之前被调用，相关的render函数首次被调用。实例已完成以下的配置：编译模板，把data里面的数据和模板生成html。注意此时还没有挂载html到页面上。<br data-filtered="filtered"><strong>mounted</strong>（载入后）    在el 被新创建的 vm.$el 替换，并挂载到实例上去之后调用。实例已完成以下的配置：用上面编译好的html内容替换el属性指向的DOM对象。完成模板中的html渲染到html页面中。此过程中进行ajax交互。<br data-filtered="filtered"><strong>beforeUpdate</strong>（更新前）    在数据更新之前调用，发生在虚拟DOM重新渲染和打补丁之前。可以在该钩子中进一步地更改状态，不会触发附加的重渲染过程。<br data-filtered="filtered"><strong>updated</strong>（更新后）    在由于数据更改导致的虚拟DOM重新渲染和打补丁之后调用。调用时，组件DOM已经更新，所以可以执行依赖于DOM的操作。然而在大多数情况下，应该避免在此期间更改状态，因为这可能会导致更新无限循环。该钩子在服务器端渲染期间不被调用。<br data-filtered="filtered"><strong>beforeDestroy</strong>（销毁前）    在实例销毁之前调用。实例仍然完全可用。<br data-filtered="filtered"><strong>destroyed</strong>（销毁后）    在实例销毁之后调用。调用后，所有的事件监听器会被移除，所有的子实例也会被销毁。该钩子在服务器端渲染期间不被调用。<br data-filtered="filtered">1.什么是vue生命周期？<br data-filtered="filtered">答： Vue 实例从创建到销毁的过程，就是生命周期。从开始创建、初始化数据、编译模板、挂载Dom→渲染、更新→渲染、销毁等一系列过程，称之为 Vue 的生命周期。</p>
<p>2.vue生命周期的作用是什么？<br data-filtered="filtered">答：它的生命周期中有多个事件钩子，让我们在控制整个Vue实例的过程时更容易形成好的逻辑。</p>
<p>3.vue生命周期总共有几个阶段？<br data-filtered="filtered">答：它可以总共分为8个阶段：创建前/后, 载入前/后,更新前/后,销毁前/销毁后。</p>
<p>4.第一次页面加载会触发哪几个钩子？<br data-filtered="filtered">答：会触发 下面这几个beforeCreate, created, beforeMount, mounted 。</p>
<p>5.DOM 渲染在 哪个周期中就已经完成？<br data-filtered="filtered">答：DOM 渲染在 mounted 中就已经完成了。</p>
<h2 id="item-3">三、 Vue实现数据双向绑定的原理：Object.defineProperty（）</h2>
<p>vue实现数据双向绑定主要是：采<strong>用数据劫持结合发布者-订阅者模式</strong>的方式，通过<strong>Object.defineProperty（）</strong>来劫持各个属性的setter，getter，在数据变动时发布消息给订阅者，触发相应监听回调。当把一个普通 Javascript 对象传给 Vue 实例来作为它的 data 选项时，Vue 将遍历它的属性，用 Object.defineProperty 将它们转为 getter/setter。用户看不到 getter/setter，但是在内部它们让 Vue 追踪依赖，在属性被访问和修改时通知变化。</p>
<p>vue的数据双向绑定 将MVVM作为数据绑定的入口，整合Observer，Compile和Watcher三者，通过Observer来监听自己的model的数据变化，通过Compile来解析编译模板指令（vue中是用来解析 {{}}），最终利用watcher搭起observer和Compile之间的通信桥梁，达到数据变化 —&gt;视图更新；视图交互变化（input）—&gt;数据model变更双向绑定效果。</p>
<p><strong>js实现简单的双向绑定</strong></p>
<div class="widget-codetool" style="display: none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="<body>
    <div id=&quot;app&quot;>
    <input type=&quot;text&quot; id=&quot;txt&quot;>
    <p id=&quot;show&quot;></p>
</div>
</body>
<script type=&quot;text/javascript&quot;>
    var obj = {}
    Object.defineProperty(obj, 'txt', {
        get: function () {
            return obj
        },
        set: function (newValue) {
            document.getElementById('txt').value = newValue
            document.getElementById('show').innerHTML = newValue
        }
    })
    document.addEventListener('keyup', function (e) {
        obj.txt = e.target.value
    })
</script>" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs xml"><code><span class="hljs-tag">&lt;<span class="hljs-name">body</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"app"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"text"</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"txt"</span>&gt;</span>
    <span class="hljs-tag">&lt;<span class="hljs-name">p</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"show"</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">p</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">body</span>&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">script</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"text/javascript"</span>&gt;</span><span class="javascript">
    <span class="hljs-keyword">var</span> obj = {}
    <span class="hljs-built_in">Object</span>.defineProperty(obj, <span class="hljs-string">'txt'</span>, {
        <span class="hljs-attr">get</span>: <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params"></span>) </span>{
            <span class="hljs-keyword">return</span> obj
        },
        <span class="hljs-attr">set</span>: <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">newValue</span>) </span>{
            <span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">'txt'</span>).value = newValue
            <span class="hljs-built_in">document</span>.getElementById(<span class="hljs-string">'show'</span>).innerHTML = newValue
        }
    })
    <span class="hljs-built_in">document</span>.addEventListener(<span class="hljs-string">'keyup'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-params">e</span>) </span>{
        obj.txt = e.target.value
    })
</span><span class="hljs-tag">&lt;/<span class="hljs-name">script</span>&gt;</span></code></pre>
<h2 id="item-4">四、Vue组件间的参数传递</h2>
<p><strong>1.父组件与子组件传值</strong><br data-filtered="filtered">父组件传给子组件：子组件通过props方法接受数据;<br data-filtered="filtered">子组件传给父组件：$emit方法传递参数<br data-filtered="filtered"><strong>2.非父子组件间的数据传递，兄弟组件传值</strong><br data-filtered="filtered">eventBus，就是创建一个事件中心，相当于中转站，可以用它来传递事件和接收事件。项目比较小时，用这个比较合适。（虽然也有不少人推荐直接用VUEX，具体来说看需求咯。技术只是手段，目的达到才是王道。）</p>
<h2 id="item-5">五、Vue的路由实现：hash模式 和 history模式</h2>
<p><strong>hash模式：</strong>在浏览器中符号“#”，#以及#后面的字符称之为hash，用window.location.hash读取；<br data-filtered="filtered">特点：hash虽然在URL中，但不被包括在HTTP请求中；用来指导浏览器动作，对服务端安全无用，hash不会重加载页面。<br data-filtered="filtered">hash 模式下，仅 hash 符号之前的内容会被包含在请求中，如 <a href="http://www.xxx.com" rel="nofollow noreferrer" target="_blank">http://www.xxx.com</a>，因此对于后端来说，即使没有做到对路由的全覆盖，也不会返回 404 错误。</p>
<p><strong>history模式：</strong>history采用HTML5的新特性；且提供了两个新方法：pushState（），replaceState（）可以对浏览器历史记录栈进行修改，以及popState事件的监听到状态变更。<br data-filtered="filtered">history 模式下，前端的 URL 必须和实际向后端发起请求的 URL 一致，如 <a href="http://www.xxx.com/items/id" rel="nofollow noreferrer" target="_blank">http://www.xxx.com/items/id</a>。后端如果缺少对 /items/id 的路由处理，将返回 404 错误。<strong>Vue-Router 官网里如此描述：</strong>“不过这种模式要玩好，还需要后台配置支持……所以呢，你要在服务端增加一个覆盖所有情况的候选资源：如果 URL 匹配不到任何静态资源，则应该返回同一个 index.html 页面，这个页面就是你 app 依赖的页面。”</p>
<h2 id="item-6">六、Vue与Angular以及React的区别？</h2>
<p>（版本在不断更新，以下的区别有可能不是很正确。我工作中只用到vue，对angular和react不怎么熟）<br data-filtered="filtered"><strong>1.与AngularJS的区别</strong><br data-filtered="filtered">相同点：<br data-filtered="filtered">都支持指令：内置指令和自定义指令；都支持过滤器：内置过滤器和自定义过滤器；都支持双向数据绑定；都不支持低端浏览器。</p>
<p>不同点：<br data-filtered="filtered">AngularJS的学习成本高，比如增加了Dependency Injection特性，而Vue.js本身提供的API都比较简单、直观；在性能上，AngularJS依赖对数据做脏检查，所以Watcher越多越慢；Vue.js使用基于依赖追踪的观察并且使用异步队列更新，所有的数据都是独立触发的。</p>
<p><strong>2.与React的区别</strong><br data-filtered="filtered">相同点：<br data-filtered="filtered">React采用特殊的JSX语法，Vue.js在组件开发中也推崇编写.vue特殊文件格式，对文件内容都有一些约定，两者都需要编译后使用；中心思想相同：一切都是组件，组件实例之间可以嵌套；都提供合理的钩子函数，可以让开发者定制化地去处理需求；都不内置列数AJAX，Route等功能到核心包，而是以插件的方式加载；在组件开发中都支持mixins的特性。<br data-filtered="filtered">不同点：<br data-filtered="filtered">React采用的Virtual DOM会对渲染出来的结果做脏检查；Vue.js在模板中提供了指令，过滤器等，可以非常方便，快捷地操作Virtual DOM。</p>
<h2 id="item-7">七、vue路由的钩子函数</h2>
<p>首页可以控制导航跳转，beforeEach，afterEach等，一般用于页面title的修改。一些需要登录才能调整页面的重定向功能。</p>
<p><strong>beforeEach</strong>主要有3个参数to，from，next：</p>
<p><strong>to</strong>：route即将进入的目标路由对象，</p>
<p><strong>from</strong>：route当前导航正要离开的路由</p>
<p><strong>next</strong>：function一定要调用该方法resolve这个钩子。执行效果依赖next方法的调用参数。可以控制网页的跳转。</p>
<h2 id="item-8">八、vuex是什么？怎么使用？哪种功能场景使用它？</h2>
<p>只用来读取的状态集中放在store中； 改变状态的方式是提交mutations，这是个同步的事物； 异步逻辑应该封装在action中。<br data-filtered="filtered">在main.js引入store，注入。新建了一个目录store，….. export 。<br data-filtered="filtered">场景有：单页应用中，组件之间的状态、音乐播放、登录状态、加入购物车<br data-filtered="filtered"><span class="img-wrap"><img referrerpolicy="no-referrer" src="/img/bVOAAF?w=701&amp;h=551" alt="图片描述" title="图片描述"></span></p>
<p><strong>state</strong><br data-filtered="filtered">Vuex 使用单一状态树,即每个应用将仅仅包含一个store 实例，但单一状态树和模块化并不冲突。存放的数据状态，不可以直接修改里面的数据。<br data-filtered="filtered"><strong>mutations</strong><br data-filtered="filtered">mutations定义的方法动态修改Vuex 的 store 中的状态或数据。<br data-filtered="filtered"><strong>getters</strong><br data-filtered="filtered">类似vue的计算属性，主要用来过滤一些数据。<br data-filtered="filtered"><strong>action</strong> <br data-filtered="filtered">actions可以理解为通过将mutations里面处里数据的方法变成可异步的处理数据的方法，简单的说就是异步操作数据。view 层通过 store.dispath 来分发 action。</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="const store = new Vuex.Store({ //store实例
      state: {
         count: 0
             },
      mutations: {                
         increment (state) {
          state.count++
         }
          },
      actions: { 
         increment (context) {
          context.commit('increment')
   }
 }
})" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs pf"><code>const store = new Vuex.Store({ //store实例
      <span class="hljs-keyword">state</span>: {
         count: <span class="hljs-number">0</span>
             },
      mutations: {                
         increment (<span class="hljs-keyword">state</span>) {
          <span class="hljs-keyword">state</span>.count++
         }
          },
      actions: { 
         increment (context) {
          context.commit('increment')
   }
 }
})</code></pre>
<p><strong>modules</strong><br data-filtered="filtered">项目特别复杂的时候，可以让每一个模块拥有自己的state、mutation、action、getters,使得结构非常清晰，方便管理。</p>
<div class="widget-codetool" style="display: none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="const moduleA = {
  state: { ... },
  mutations: { ... },
  actions: { ... },
  getters: { ... }
 }
const moduleB = {
  state: { ... },
  mutations: { ... },
  actions: { ... }
 }

const store = new Vuex.Store({
  modules: {
    a: moduleA,
    b: moduleB
})" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs dts"><code>const moduleA = {
<span class="hljs-symbol">  state:</span> { ... },
<span class="hljs-symbol">  mutations:</span> { ... },
<span class="hljs-symbol">  actions:</span> { ... },
<span class="hljs-symbol">  getters:</span> { ... }
 }
const moduleB = {
<span class="hljs-symbol">  state:</span> { ... },
<span class="hljs-symbol">  mutations:</span> { ... },
<span class="hljs-symbol">  actions:</span> { ... }
 }

const store = new Vuex.Store({
<span class="hljs-symbol">  modules:</span> {
<span class="hljs-symbol">    a:</span> moduleA,
<span class="hljs-symbol">    b:</span> moduleB
})</code></pre>
<h2 id="item-9">九、vue-cli如何新增自定义指令？</h2>
<p>1.创建局部指令</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="var app = new Vue({
    el: '#app',
    data: {    
    },
    // 创建指令(可以多个)
    directives: {
        // 指令名称
        dir1: {
            inserted(el) {
                // 指令中第一个参数是当前使用指令的DOM
                console.log(el);
                console.log(arguments);
                // 对DOM进行操作
                el.style.width = '200px';
                el.style.height = '200px';
                el.style.background = '#000';
            }
        }
    }
})
" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs processing"><code>var app = <span class="hljs-keyword">new</span> Vue({
    el: <span class="hljs-string">'#app'</span>,
    data: {    
    },
    <span class="hljs-comment">// 创建指令(可以多个)</span>
    directives: {
        <span class="hljs-comment">// 指令名称</span>
        dir1: {
            inserted(el) {
                <span class="hljs-comment">// 指令中第一个参数是当前使用指令的DOM</span>
                console.<span class="hljs-built_in">log</span>(el);
                console.<span class="hljs-built_in">log</span>(arguments);
                <span class="hljs-comment">// 对DOM进行操作</span>
                el.style.<span class="hljs-built_in">width</span> = <span class="hljs-string">'200px'</span>;
                el.style.<span class="hljs-built_in">height</span> = <span class="hljs-string">'200px'</span>;
                el.style.<span class="hljs-built_in">background</span> = <span class="hljs-string">'#000'</span>;
            }
        }
    }
})
</code></pre>
<p>2.全局指令</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="Vue.directive('dir2', {
    inserted(el) {
        console.log(el);
    }
})
" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs less"><code><span class="hljs-selector-tag">Vue</span><span class="hljs-selector-class">.directive</span>(<span class="hljs-string">'dir2'</span>, {
    <span class="hljs-selector-tag">inserted</span>(el) {
        <span class="hljs-selector-tag">console</span><span class="hljs-selector-class">.log</span>(el);
    }
})
</code></pre>
<p>3.指令的使用</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="<div id=&quot;app&quot;>
    <div v-dir1></div>
    <div v-dir2></div>
</div>
" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs applescript"><code>&lt;<span class="hljs-keyword">div</span> <span class="hljs-built_in">id</span>=<span class="hljs-string">"app"</span>&gt;
    &lt;<span class="hljs-keyword">div</span> v-dir1&gt;&lt;/<span class="hljs-keyword">div</span>&gt;
    &lt;<span class="hljs-keyword">div</span> v-dir2&gt;&lt;/<span class="hljs-keyword">div</span>&gt;
&lt;/<span class="hljs-keyword">div</span>&gt;
</code></pre>
<h2 id="item-10">十、vue如何自定义一个过滤器？</h2>
<p>html代码：</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="<div id=&quot;app&quot;>
     <input type=&quot;text&quot; v-model=&quot;msg&quot; />
     {{msg| capitalize }}
</div>
" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs django"><code><span class="xml"><span class="hljs-tag">&lt;<span class="hljs-name">div</span> <span class="hljs-attr">id</span>=<span class="hljs-string">"app"</span>&gt;</span>
     <span class="hljs-tag">&lt;<span class="hljs-name">input</span> <span class="hljs-attr">type</span>=<span class="hljs-string">"text"</span> <span class="hljs-attr">v-model</span>=<span class="hljs-string">"msg"</span> /&gt;</span>
     </span><span class="hljs-template-variable">{{msg| capitalize }}</span><span class="xml">
<span class="hljs-tag">&lt;/<span class="hljs-name">div</span>&gt;</span>
</span></code></pre>
<p>JS代码：</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="var vm=new Vue({
    el:&quot;#app&quot;,
    data:{
        msg:''
    },
    filters: {
      capitalize: function (value) {
        if (!value) return ''
        value = value.toString()
        return value.charAt(0).toUpperCase() + value.slice(1)
      }
    }
})" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs vim"><code>var <span class="hljs-keyword">vm</span>=<span class="hljs-keyword">new</span> Vue({
    <span class="hljs-keyword">e</span><span class="hljs-variable">l:</span><span class="hljs-string">"#app"</span>,
    dat<span class="hljs-variable">a:</span>{
        ms<span class="hljs-variable">g:</span><span class="hljs-string">''</span>
    },
    <span class="hljs-built_in">filter</span><span class="hljs-variable">s:</span> {
      capitalize: <span class="hljs-function"><span class="hljs-keyword">function</span> <span class="hljs-params">(value)</span> {</span>
        <span class="hljs-keyword">if</span> (!value) <span class="hljs-keyword">return</span> <span class="hljs-string">''</span>
        value = value.toString()
        <span class="hljs-keyword">return</span> value.charAt(<span class="hljs-number">0</span>).toUpperCase() + value.slice(<span class="hljs-number">1</span>)
      }
    }
})</code></pre>
<p>全局定义过滤器</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="Vue.filter('capitalize', function (value) {
  if (!value) return ''
  value = value.toString()
  return value.charAt(0).toUpperCase() + value.slice(1)
})" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs livecodeserver"><code>Vue.<span class="hljs-built_in">filter</span>(<span class="hljs-string">'capitalize'</span>, <span class="hljs-function"><span class="hljs-keyword">function</span> (<span class="hljs-title">value</span>) {</span>
  <span class="hljs-keyword">if</span> (!<span class="hljs-built_in">value</span>) <span class="hljs-literal">return</span> <span class="hljs-string">''</span>
  <span class="hljs-built_in">value</span> = <span class="hljs-built_in">value</span>.toString()
  <span class="hljs-literal">return</span> <span class="hljs-built_in">value</span>.charAt(<span class="hljs-number">0</span>).toUpperCase() + <span class="hljs-built_in">value</span>.slice(<span class="hljs-number">1</span>)
})</code></pre>
<p>过滤器接收表达式的值 (msg) 作为第一个参数。capitalize 过滤器将会收到 msg的值作为第一个参数。</p>
<h2 id="item-11">十一、对keep-alive 的了解？</h2>
<p><strong>keep-alive</strong>是 Vue 内置的一个组件，可以使被包含的组件保留状态，或避免重新渲染。<br data-filtered="filtered">在vue 2.1.0 版本之后，keep-alive新加入了两个属性: include(包含的组件缓存) 与 exclude(排除的组件不缓存，优先级大于include) 。</p>
<p>使用方法</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="<keep-alive include='include_components' exclude='exclude_components'>
  <component>
    <!-- 该组件是否缓存取决于include和exclude属性 -->
  </component>
</keep-alive>" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs xml"><code><span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span> <span class="hljs-attr">include</span>=<span class="hljs-string">'include_components'</span> <span class="hljs-attr">exclude</span>=<span class="hljs-string">'exclude_components'</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">component</span>&gt;</span>
    <span class="hljs-comment">&lt;!-- 该组件是否缓存取决于include和exclude属性 --&gt;</span>
  <span class="hljs-tag">&lt;/<span class="hljs-name">component</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span></code></pre>
<p>参数解释<br data-filtered="filtered">include - 字符串或正则表达式，只有名称匹配的组件会被缓存<br data-filtered="filtered">exclude - 字符串或正则表达式，任何名称匹配的组件都不会被缓存<br data-filtered="filtered">include 和 exclude 的属性允许组件有条件地缓存。二者都可以用“，”分隔字符串、正则表达式、数组。当使用正则或者是数组时，要记得使用v-bind 。</p>
<p>使用示例</p>
<div class="widget-codetool" style="display:none;">
        <div class="widget-codetool--inner">
        <span class="selectCode code-tool" data-toggle="tooltip" data-placement="top" title="" data-original-title="全选"></span>
        <span type="button" class="copyCode code-tool" data-toggle="tooltip" data-placement="top" data-clipboard-text="<!-- 逗号分隔字符串，只有组件a与b被缓存。 -->
<keep-alive include=&quot;a,b&quot;>
  <component></component>
</keep-alive>

<!-- 正则表达式 (需要使用 v-bind，符合匹配规则的都会被缓存) -->
<keep-alive :include=&quot;/a|b/&quot;>
  <component></component>
</keep-alive>

<!-- Array (需要使用 v-bind，被包含的都会被缓存) -->
<keep-alive :include=&quot;['a', 'b']&quot;>
  <component></component>
</keep-alive>
" title="" data-original-title="复制"></span>
        </div>
        </div><pre class="hljs xml"><code><span class="hljs-comment">&lt;!-- 逗号分隔字符串，只有组件a与b被缓存。 --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span> <span class="hljs-attr">include</span>=<span class="hljs-string">"a,b"</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">component</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">component</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span>

<span class="hljs-comment">&lt;!-- 正则表达式 (需要使用 v-bind，符合匹配规则的都会被缓存) --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span> <span class="hljs-attr">:include</span>=<span class="hljs-string">"/a|b/"</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">component</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">component</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span>

<span class="hljs-comment">&lt;!-- Array (需要使用 v-bind，被包含的都会被缓存) --&gt;</span>
<span class="hljs-tag">&lt;<span class="hljs-name">keep-alive</span> <span class="hljs-attr">:include</span>=<span class="hljs-string">"['a', 'b']"</span>&gt;</span>
  <span class="hljs-tag">&lt;<span class="hljs-name">component</span>&gt;</span><span class="hljs-tag">&lt;/<span class="hljs-name">component</span>&gt;</span>
<span class="hljs-tag">&lt;/<span class="hljs-name">keep-alive</span>&gt;</span>
</code></pre>
<h2 id="item-12">十二、一句话就能回答的面试题</h2>
<p><strong>1.css只在当前组件起作用</strong><br data-filtered="filtered">答：在style标签中写入<strong>scoped</strong>即可 例如：&lt;style scoped&gt;&lt;/style&gt;</p>
<p><strong>2.v-if 和 v-show 区别</strong><br data-filtered="filtered">答：v-if按照条件是否渲染，v-show是display的block或none；</p>
<p><strong>3.<mjx-container class="MathJax CtxtMenu_Attached_0" jax="SVG" tabindex="0" ctxtmenu_counter="0"><svg xmlns="http://www.w3.org/2000/svg" width="7.911ex" height="2.262ex" role="img" focusable="false" viewBox="0 -750 3496.8 1000" xmlns:xlink="http://www.w3.org/1999/xlink" style="vertical-align: -0.566ex;"><g stroke="currentColor" fill="currentColor" stroke-width="0" transform="matrix(1 0 0 -1 0 0)"><g data-mml-node="math"><g data-mml-node="mi"><use xlink:href="#MJX-TEX-I-72"></use></g><g data-mml-node="mi" transform="translate(451, 0)"><use xlink:href="#MJX-TEX-I-6F"></use></g><g data-mml-node="mi" transform="translate(936, 0)"><use xlink:href="#MJX-TEX-I-75"></use></g><g data-mml-node="mi" transform="translate(1508, 0)"><use xlink:href="#MJX-TEX-I-74"></use></g><g data-mml-node="mi" transform="translate(1869, 0)"><use xlink:href="#MJX-TEX-I-65"></use></g><g data-mml-node="mo" transform="translate(2612.8, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">和</text></g></g></g></svg></mjx-container>router的区别</strong><br data-filtered="filtered">答：<mjx-container class="MathJax CtxtMenu_Attached_0" jax="SVG" tabindex="0" ctxtmenu_counter="1"><svg xmlns="http://www.w3.org/2000/svg" width="113.645ex" height="2.262ex" role="img" focusable="false" viewBox="0 -750 50231.2 1000" xmlns:xlink="http://www.w3.org/1999/xlink" style="vertical-align: -0.566ex;"><g stroke="currentColor" fill="currentColor" stroke-width="0" transform="matrix(1 0 0 -1 0 0)"><g data-mml-node="math"><g data-mml-node="mi"><use xlink:href="#MJX-TEX-I-72"></use></g><g data-mml-node="mi" transform="translate(451, 0)"><use xlink:href="#MJX-TEX-I-6F"></use></g><g data-mml-node="mi" transform="translate(936, 0)"><use xlink:href="#MJX-TEX-I-75"></use></g><g data-mml-node="mi" transform="translate(1508, 0)"><use xlink:href="#MJX-TEX-I-74"></use></g><g data-mml-node="mi" transform="translate(1869, 0)"><use xlink:href="#MJX-TEX-I-65"></use></g><g data-mml-node="mo" transform="translate(2612.8, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">是</text></g><g data-mml-node="mo" transform="translate(3774.6, 0)"><use xlink:href="#MJX-TEX-N-201C"></use></g><g data-mml-node="mo" transform="translate(4274.6, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">路</text><text data-variant="normal" transform="translate(884, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">由</text><text data-variant="normal" transform="translate(1768, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">信</text><text data-variant="normal" transform="translate(2652, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">息</text><text data-variant="normal" transform="translate(3536, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">对</text><text data-variant="normal" transform="translate(4420, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">象</text></g><g data-mml-node="mo" transform="translate(9578.6, 0)"><use xlink:href="#MJX-TEX-N-201D"></use></g><g data-mml-node="mo" transform="translate(10356.3, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text><text data-variant="normal" transform="translate(884, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">包</text><text data-variant="normal" transform="translate(1768, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">括</text></g><g data-mml-node="mi" transform="translate(13286.1, 0)"><use xlink:href="#MJX-TEX-I-70"></use></g><g data-mml-node="mi" transform="translate(13789.1, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(14318.1, 0)"><use xlink:href="#MJX-TEX-I-74"></use></g><g data-mml-node="mi" transform="translate(14679.1, 0)"><use xlink:href="#MJX-TEX-I-68"></use></g><g data-mml-node="mo" transform="translate(15532.9, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(16694.7, 0)"><use xlink:href="#MJX-TEX-I-70"></use></g><g data-mml-node="mi" transform="translate(17197.7, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(17726.7, 0)"><use xlink:href="#MJX-TEX-I-72"></use></g><g data-mml-node="mi" transform="translate(18177.7, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(18706.7, 0)"><use xlink:href="#MJX-TEX-I-6D"></use></g><g data-mml-node="mi" transform="translate(19584.7, 0)"><use xlink:href="#MJX-TEX-I-73"></use></g><g data-mml-node="mo" transform="translate(20331.4, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(21493.2, 0)"><use xlink:href="#MJX-TEX-I-68"></use></g><g data-mml-node="mi" transform="translate(22069.2, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(22598.2, 0)"><use xlink:href="#MJX-TEX-I-73"></use></g><g data-mml-node="mi" transform="translate(23067.2, 0)"><use xlink:href="#MJX-TEX-I-68"></use></g><g data-mml-node="mo" transform="translate(23921, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(25082.8, 0)"><use xlink:href="#MJX-TEX-I-71"></use></g><g data-mml-node="mi" transform="translate(25528.8, 0)"><use xlink:href="#MJX-TEX-I-75"></use></g><g data-mml-node="mi" transform="translate(26100.8, 0)"><use xlink:href="#MJX-TEX-I-65"></use></g><g data-mml-node="mi" transform="translate(26566.8, 0)"><use xlink:href="#MJX-TEX-I-72"></use></g><g data-mml-node="mi" transform="translate(27017.8, 0)"><use xlink:href="#MJX-TEX-I-79"></use></g><g data-mml-node="mo" transform="translate(27785.6, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(28947.3, 0)"><use xlink:href="#MJX-TEX-I-66"></use></g><g data-mml-node="mi" transform="translate(29497.3, 0)"><use xlink:href="#MJX-TEX-I-75"></use></g><g data-mml-node="mi" transform="translate(30069.3, 0)"><use xlink:href="#MJX-TEX-I-6C"></use></g><g data-mml-node="mi" transform="translate(30367.3, 0)"><use xlink:href="#MJX-TEX-I-6C"></use></g><g data-mml-node="mi" transform="translate(30665.3, 0)"><use xlink:href="#MJX-TEX-I-50"></use></g><g data-mml-node="mi" transform="translate(31416.3, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(31945.3, 0)"><use xlink:href="#MJX-TEX-I-74"></use></g><g data-mml-node="mi" transform="translate(32306.3, 0)"><use xlink:href="#MJX-TEX-I-68"></use></g><g data-mml-node="mo" transform="translate(33160.1, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(34321.9, 0)"><use xlink:href="#MJX-TEX-I-6D"></use></g><g data-mml-node="mi" transform="translate(35199.9, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(35728.9, 0)"><use xlink:href="#MJX-TEX-I-74"></use></g><g data-mml-node="mi" transform="translate(36089.9, 0)"><use xlink:href="#MJX-TEX-I-63"></use></g><g data-mml-node="mi" transform="translate(36522.9, 0)"><use xlink:href="#MJX-TEX-I-68"></use></g><g data-mml-node="mi" transform="translate(37098.9, 0)"><use xlink:href="#MJX-TEX-I-65"></use></g><g data-mml-node="mi" transform="translate(37564.9, 0)"><use xlink:href="#MJX-TEX-I-64"></use></g><g data-mml-node="mo" transform="translate(38362.7, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">，</text></g><g data-mml-node="mi" transform="translate(39524.4, 0)"><use xlink:href="#MJX-TEX-I-6E"></use></g><g data-mml-node="mi" transform="translate(40124.4, 0)"><use xlink:href="#MJX-TEX-I-61"></use></g><g data-mml-node="mi" transform="translate(40653.4, 0)"><use xlink:href="#MJX-TEX-I-6D"></use></g><g data-mml-node="mi" transform="translate(41531.4, 0)"><use xlink:href="#MJX-TEX-I-65"></use></g><g data-mml-node="mo" transform="translate(42275.2, 0)"><text data-variant="normal" transform="matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">等</text><text data-variant="normal" transform="translate(884, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">路</text><text data-variant="normal" transform="translate(1768, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">由</text><text data-variant="normal" transform="translate(2652, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">信</text><text data-variant="normal" transform="translate(3536, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">息</text><text data-variant="normal" transform="translate(4420, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">参</text><text data-variant="normal" transform="translate(5304, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">数</text><text data-variant="normal" transform="translate(6188, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">。</text><text data-variant="normal" transform="translate(7072, 0) matrix(1 0 0 -1 0 0)" font-size="884px" font-family="serif">而</text></g></g></g></svg></mjx-container>router是“路由实例”对象包括了路由的跳转方法，钩子函数等。</p>
<p><strong>4.vue.js的两个核心是什么？</strong><br data-filtered="filtered">答：数据驱动、组件系统</p>
<p><strong>5.vue几种常用的指令</strong><br data-filtered="filtered">答：v-for 、 v-if 、v-bind、v-on、v-show、v-else</p>
<p><strong>6.vue常用的修饰符？</strong><br data-filtered="filtered">答：.prevent: 提交事件不再重载页面；.stop: 阻止单击事件冒泡；.self: 当事件发生在该元素本身而不是子元素的时候会触发；.capture: 事件侦听，事件发生的时候会调用</p>
<p><strong>7.v-on 可以绑定多个方法吗？</strong><br data-filtered="filtered">答：可以</p>
<p><strong>8.vue中 key 值的作用？</strong><br data-filtered="filtered">答：当 Vue.js 用 v-for 正在更新已渲染过的元素列表时，它默认用“就地复用”策略。如果数据项的顺序被改变，Vue 将不会移动 DOM 元素来匹配数据项的顺序， 而是简单复用此处每个元素，并且确保它在特定索引下显示已被渲染过的每个元素。key的作用主要是为了高效的更新虚拟DOM。</p>
<p><strong>9.什么是vue的计算属性？</strong><br data-filtered="filtered">答：在模板中放入太多的逻辑会让模板过重且难以维护，在需要对数据进行复杂处理，且可能多次使用的情况下，尽量采取计算属性的方式。好处：①使得数据处理结构清晰；②依赖于数据，数据更新，处理结果自动更新；③计算属性内部this指向vm实例；④在template调用时，直接写计算属性名即可；⑤常用的是getter方法，获取数据，也可以使用set方法改变数据；⑥相较于methods，不管依赖的数据变不变，methods都会重新计算，但是依赖数据不变的时候computed从缓存中获取，不会重新计算。</p>
<p><strong>10.vue等单页面应用及其优缺点</strong><br data-filtered="filtered">答：优点：Vue 的目标是通过尽可能简单的 API 实现响应的数据绑定和组合的视图组件，核心是一个响应的数据绑定系统。MVVM、数据驱动、组件化、轻量、简洁、高效、快速、模块友好。<br data-filtered="filtered">缺点：不支持低版本的浏览器，最低只支持到IE9；不利于SEO的优化（如果要支持SEO，建议通过服务端来进行渲染组件）；第一次加载首页耗时相对长一些；不可以使用浏览器的导航按钮需要自行实现前进、后退。</p>
<p><strong>11.怎么定义 vue-router 的动态路由? 怎么获取传过来的值</strong><br data-filtered="filtered">答：在 router 目录下的 index.js 文件中，对 path 属性加上 /:id，使用 router 对象的 params.id 获取。</p>
<p><a href="/img/bVOAAF">Vue面试中，经常会被问到的面试题/Vue知识点整理</a><br data-filtered="filtered"><a href="https://segmentfault.com/a/1190000016481101">532道前端真实大厂面试题</a><br data-filtered="filtered"><a href="https://segmentfault.com/a/1190000016068235">学习ES6笔记──工作中常用到的ES6语法</a></p>

                            </div>
