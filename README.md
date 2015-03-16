# 漫画君前端使用requirejs的相关规范

require.js是遵循AMD（Asynchronous Module Definition）规范的前端模块化的工具。它能够将前端代码划分成一个个的模块，并对模块进行异步加载，以满足前端对模块化的需求。

# Getting Started(请按照如下顺序进行学习)
  * [AMD规范文档](https://github.com/amdjs/amdjs-api/wiki/AMD-(中文版))
  * [require.js官网](http://requirejs.org/)
  * [require.js中文网](http://www.requirejs.cn/)英文不好的童鞋可以看个，不过首页目录上的链接都是错误的，可以直接忽略，直接从上到下看即可。
  * [CMD规范（仅作为了解）](https://github.com/cmdjs/specification/blob/master/draft/module.md)
  * [seajs官网（seajs是遵循CMD规范的，仅作为了解）](http://seajs.org/)

# require.js基础知识点

具体知识点请移步上述require.js官网进行学习，这里只讲解最基础的require应用。

### require配置

按照标准规范，在使用require.js的时候需要自己写`require.config`进行配置（当然这不是必须的），可以通过`require.confi`g配置需要使用的js的目录，甚至以包的形式引入，使用`require.config`有助于程序员管理需要使用的js文件和有效的利用require进行相关操作。

下面举一个`require.config`的例子：

```js
require.config({
    baseUrl: './',                      //项目的baseUrl
    paths:{
        jquery:[                        //jquery插件,默认情况下不需要`.js`，写了可能会出错
                'http://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.0/jquery.min',
                'js/jquery.min'
                ],                      //这里的数组表示第一个路径中的插件
                                        //没有加载完成时加载第二个插件
        swiper:'js/swiper.min',         //swiper插件
        backbone:'js/backbone',         //backbone插件
        underscore:'js/underscore',     //underscore插件
    },
    shim:{                              //shim的作用是为那些没有定义define的插件
                                        //做兼容处理，使require能够正常引用它
        backbone:{
            deps: [ 'underscore' ],     //backbone插件依赖underscore插件
            exports: 'Backbone'
        }
        underscore:{
            exports: '_'
        }
    }
});
```
require.js配置好了就可以使用以上信息进行开发了。

### define

在上述代码中引入的js都是别人开发好的，有可能都已经写入了define
