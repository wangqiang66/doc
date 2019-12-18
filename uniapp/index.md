## UniApp

支持H5, 支付宝、微信、百度、头条、QQ、APP 7中方式

对于App生成方式需要HBuilderX工具支持

### UniApp 微信小程序初体验

1. 通过vue-cli3安装

2. 运行yarn build:mp-weixin 报错：vue-loader和vue-compile-template版本不一致

3. @dcloudio/uni-template-compiler 在packages里面用一个vue-template-compiler 固定了版本号2.6.10 版本需要固定vue版本2.6.10
