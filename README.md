# vue-share
for company internal share，base on vue version 2.2.6

## 什么是vue
一个渐进式js框架。他和react一样，框架本身只负责视图部分，核心功能是一个视图模板引擎。

### fast start
通过两个demo快速了解vue的能力

## 源码目录结构
- build 打包相关的配置文件
- dist 打包之后文件所在位置
- flow 因为Vue源码使用了Flow来进行静态类型检查，这里定义了一些静态类型
- src 主要源码路径
- src/compiler 模板解析相关文件
- src/core 核心代码
- src/entries 入口文件
- src/platforms 平台相关，主要有web、weex

## vue 源码学习
主要根据一些常用用法去了解vue内部实现
### 组件化-合并配置
demo详见mergeOption.html
这个demo控制台会输出什么？可以看到打印了两个'parent created' 然后再打印的'child created';
因为我们mixin了一个created生命周期函数，在实例化的时候会合并配置，如果组件含有created方法，最终会合并成一个created的数组，然后在实例化的时候依次执行

### 在设置data后发生了什么？
vue2.x的版本双向绑定是基于Object.defineProperty的一个观察者模型，在设置data后会触发setter，data的改变会通知观察者更新，然后生成新的虚拟Dom，
然后再和之前的虚拟DOM对比，最后再是dispatch渲染过程。但是在我们平时开发中，我们会同事设置多个data，如果每次都渲染的话，性能和体验都不好了，
看过源码后可以知道，渲染是一个异步的过程，会在主线程执行完毕后才会执行。（这里也可以说一下js的执行机制）

### vue是如何对数组的变化进行监听的？