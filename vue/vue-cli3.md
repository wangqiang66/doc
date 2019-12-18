## vue-cli3 打包流程

通过@vue/cli-service/bin 将bin注册到node_modules命令下，可以通过

node_modules/.bin/vue-cli-service -h 查看帮助

### vue-service 如何注册commands

通过源码可以看到constructor的commands对象是空的，并且该文件没有commands相关的加入内容。

其实是在PluginAPI中注册的。通过registerCommand进行加入命令

整个过程是：

1. 读取我们项目的packages
   1. 默认读取项目中的packages.json
   2. 如果希望读取自定义目录的json。可以在package.json中添加{vuePlugins: {resolveFrom: 指定相对的路径}}
2. 通过正则式/^(@vue\/|vue-|@[\w-]+(\.)?[\w-]+\/vue-)cli-plugin-/读取pkg的devDependencies和dependencies  将匹配的加入到plugins里面。
   - 如果希望自定plugin 可以配置{optionalDependencies: {}}
  
3. 执行plugin的apply方法（即 对于plugin的export方法）。如果是command 增加对应的plugin 为 (api, options) => api.registerCommand(name, func)将对应的命名注入进去

### vue-service 加载vue.config.js

在 Service.js 里面 loadUserOptions 方法中，会读取配置文件vue.config.js

1. 配置文件可以是function或者是对象
2. 会将配置文件的chainWebpack加载到webpackChainFns里面。
   - 回掉函数带的参数为 webpack-chain里面的Config对象
3. configureWebpack 加载到webpackRawConfigFns
   - Object: 回和Config对象合并
   - Function: Config作为参数执行后在于Config对象合并

### mode处理环境变量

会加载.env{.$mode}?{.local}?文件的变量

### minimist

处理cli参数转换

```js
var argv = require('minimist')(process.argv.slice(2));
console.dir(argv);
$ node example/parse.js -a beep -b boop
{ _: [], a: 'beep', b: 'boop' }
$ node example/parse.js -x 3 -y 4 -n5 -abc --beep=boop foo bar baz
{ _: [ 'foo', 'bar', 'baz' ],
  x: 3,
  y: 4,
  n: 5,
  a: true,
  b: true,
  c: true,
  beep: 'boop' }
```

