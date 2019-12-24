[TOC]

## webpack 简介

Webpack 分为Webpack-cli 和 Webpack 和 Webpack-dev-server

### Webpack 插件的关联

- Webpack

  webpack 是webpack的最终处理

  - bin/webpack.js 仅用于检查webpack-cli的安装和透传。最终调用的是是webpack-cli的相关处理

- Webpack-cli

  目的是方便webpack。是Webpack零配置启动，主要目的就是处理命令行的参数作为webpack的入参。对于自己配置webpack参数，可以不需要。例如vue-cli3中的cli-service。

- Webpack-dev-server

  开发模式运行webpack，使用spdy来启动的服务器

  - bin 主要是处理参数，最终用的是lib/Server, 参数是webpack的编译对象Compiler
