## Vue 有用的库

### vue-svgicon

矢量图标使用 [例子](https://mmf-fe.github.io/vue-svgicon/v3/)

使用说明

```js
Vue.use(SvgIcon, {
  tagName: 'svg-icon',
  defaultWidth: '1em',
  defaultHeight: '1em'
})


<svg-icon name="arrow" width="50" height="50"></svg-icon>
<svg-icon name="arrow" width="10em" height="10em"></svg-icon>
<svg-icon name="arrow" dir="left"></svg-icon>

```

标签定义的宽高默认是px，暂未找到自适应屏幕的方法，需要js处理。 也可以是em。但是没有px转rem的方法需要js自己处理


### nprogress

加载中，一般使用在路由前后

```js
NProgress.start()

NProgress.done()
```

### vue-class-component

提供了Vue、Component等
可以使用vue-property-decorator代替

```
import Vue from 'vue'
import Component from 'vue-class-component'
 
@Component({
  props: {
    propMessage: String
  }
})
export default class App extends Vue {
  // initial data
  msg = 123
 
  // use prop values for initial data
  helloMsg = 'Hello, ' + this.propMessage
 
  // lifecycle hook
  mounted () {
    this.greet()
  }
 
  // computed
  get computedMsg () {
    return 'computed ' + this.msg
  }
 
  // method
  greet () {
    alert('greeting: ' + this.msg)
  }
}
```

### vue-property-decorator

这个组件完全依赖于vue-class-component.它具备以下几个属性:

- @Component (完全继承于vue-class-component)
- @Emit
- @Inject
- @Provice
- @Prop
- @Watch
- @Model
- Mixins (在vue-class-component中定义)


使用TypeScript时,引入vue-property-decorator，用typescript的方式编辑vue组件

```js
import { Vue, Component, Prop } from 'vue-property-decorator';
import myMixins from '@static/js/mixins';

@Component({
    mixins: [myMixins]
})
export default class myComponent extends Vue{
    created(){
        console.log(this.value) // => Hello
    }
}
```

### register-service-worker

用来控制 Service workers 的注册和卸载，它可以在 生产环境 中，注册一个service worker来为本地缓存提供服务。这使得应用程序在随后的访问中加载更快，并且可以实现离线加载

主要是用了navigator的serviceWorker来实现

#### Service Worker

Service workers 本质上充当Web应用程序与浏览器之间的代理服务器，也可以在网络可用时作为浏览器和网络间的代理。它们旨在（除其他之外）使得能够创建有效的离线体验，拦截网络请求并基于网络是否可用以及更新的资源是否驻留在服务器上来采取适当的动作。他们还允许访问推送通知和后台同步API。

Service worker是一个注册在指定源和路径下的事件驱动worker。运行在worker上下文，因此它不能访问DOM。运行在其他线程中，所以不会造成阻塞。它设计为完全异步。只能由HTTPS承载

### workbox-webpack-plugin

webpack 配置pwa的插件

```js
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const { CleanWebpackPlugin } = require('clean-webpack-plugin');
const WorkboxPlugin = require('workbox-webpack-plugin');

module.exports = {
  entry: {
    app: './src/index.js',
    print: './src/print.js',
  },
  plugins: [
    // new CleanWebpackPlugin(['dist/*']) for < v2 versions of CleanWebpackPlugin
    new CleanWebpackPlugin(),
    new HtmlWebpackPlugin({
      title: 'Progressive Web Application',
    }),
    new WorkboxPlugin.GenerateSW({
      // these options encourage the ServiceWorkers to get in there fast
      // and not allow any straggling "old" SWs to hang around
      clientsClaim: true,
      skipWaiting: true,
    })
  ],
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, 'dist'),
  }
}

// main.js
if ('serviceWorker' in navigator) {
  window.addEventListener('load', () => {
    navigator.serviceWorker.register('/service-worker.js').then(registration => {
      console.log('SW registered: ', registration);
    }).catch(registrationError => {
      console.log('SW registration failed: ', registrationError);
    })
  })
}
```

### clipboard

复制到剪贴板

```js
// 使用方式
var clipboard new ClipboardJS('.btn', {
  container: document.getElementById('modal'), // 复制其他元素用
  text: function(trigger) {                    // 复制属性
      return trigger.getAttribute('aria-label');
  },
  action() { return binding.arg === 'cut' ? 'cut' : 'copy' } //方式
})
 
clipboard.on('success', function(e) {
    console.info('Action:', e.action);
    console.info('Text:', e.text);
    console.info('Trigger:', e.trigger);
 
    e.clearSelection();
});
 
clipboard.on('error', function(e) {
    console.error('Action:', e.action);
    console.error('Trigger:', e.trigger);
});

```

```html
// 或者
<!-- Target -->
<textarea id="bar">Mussum ipsum cacilds...</textarea>
 
<!-- Trigger -->
<button class="btn" data-clipboard-action="cut" data-clipboard-target="#bar">
    Cut to clipboard
</button>

!-- Trigger -->
<button class="btn" data-clipboard-text="Just because you can doesn't mean you should — clipboard.js">
    Copy to clipboard
</button>
```

### vuex-module-decorators

装饰器的方式处理vuex。主要用于typescript

```js
import { Module, VuexModule, Mutation, Action, MutationAction  } from 'vuex-module-decorators'
 
@Module({
  name: 'MyStoreModule',
  namespaced: true,
  stateFactory: true,
})
export default class Counter2 extends VuexModule {
  count = 0
 
  @Mutation
  increment(delta: number) {
    this.count += delta
  }
  @Mutation
  decrement(delta: number) {
    this.count -= delta
  }
 
  // action 'incr' commits mutation 'increment' when done with return value as payload
  @Action({ commit: 'increment' })
  incr() {
    return 5
  }
  // action 'decr' commits mutation 'decrement' when done with return value as payload
  @Action({ commit: 'decrement' })
  decr() {
    return 5
  }

  @MutationAction({ mutate: ['events', 'conferences'] })
  async fetchAll() {
    const response: Response = await getJSON('https://hasgeek.github.io/events/api/events.json')
    return response
  }

  get axles() {
    return this.count / 2
  }
}

// store/index.ts
import MyStoreModule from '~/store/modules/MyStoreModule'
 
export function createStore() {
  return new Vuex.Store({
    modules: {
      MyStoreModule
    }
  }
}

// components/Random.tsx
import { Component, Vue } from 'vue-property-decorator';
import { getModule } from 'vuex-module-decorators';
import MyStoreModule from '~/store/modules/MyStoreModule'
 
@Component
export default class extends Vue {
    public created() {
        const MyModuleInstance = getModule(MyStoreModule, this.$store);
        // Do stuff with module
        MyModuleInstance.increment(1)
    }
}
```

### codemirror

基于javascript的代码编辑器组件。核心库仅提供编辑器组件，不提供附带的按钮，自动完成功能或其他IDE功能。它确实提供了一个丰富的API，在此之上可以直接实现这些功能

```js
import CodeMirror, { Editor } from 'codemirror'

CodeMirror.fromTextArea(dom, options)
```
