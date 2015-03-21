# 漫画君前端使用requirejs的相关规范 #

require.js是遵循AMD（Asynchronous Module Definition）规范的前端模块化的工具。它能够将前端代码划分成一个个的模块，并对模块进行异步加载，以满足前端对模块化的需求。

# Getting Started(请按照如下顺序进行学习)
  * [AMD规范文档](https://github.com/amdjs/amdjs-api/wiki/AMD-(中文版))
  * [require.js官网](http://requirejs.org/)
  * [require.js中文网](http://www.requirejs.cn/)英文不好的童鞋可以看个，不过首页目录上的链接都是错误的，可以直接忽略，直接从上到下看即可。
  * [CMD规范（仅作为了解）](https://github.com/cmdjs/specification/blob/master/draft/module.md)
  * [seajs官网（seajs是遵循CMD规范的，仅作为了解）](http://seajs.org/)

# require.js基础知识点

具体知识点请移步上述`require.js`官网进行学习，这里只讲解最基础的require应用。

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

在上述代码中引入的js都是别人开发好的，有可能都已经写入了`define`的相关内容，那么如果我们的页面上需要一个我们自己写的js，应该如何让他满足AMD规范呢？

示例代码：

```js
define('moduleID',['jquery','swiper'],function($,Swiper){
    $('#swiper-container').text('这是一个defin示例');
    var swiper = new Swiper('swiper-container');
});
```

我么可以像这样来声明一个`define`模块，`define`的作用你可以简单的理解为：`函数声明`，如果你想写一个函数，就必须先声明他，然后再调用它。

`define`默认的有三个参数：`模块的ID`，`当前模块所依赖的js插件`，`一个在所有的插件加载完成之后执行的回调函数`。

一般情况下，我们也可以这样写`define`：

```js
define(['jquery','swiper'],function($,Swiper){
    dosomething();
});
```

第一个参数可以省略。

第二个参数与回调函数的是具有`一一对应`的关系的，对于上述代码，$对应的是jquery插件，Swiper对应的是swiper插件。

但是在回调函数中的参数可以是任意值，比如：

```js
define(['jquery','swiper'],function(a,b){
    dosomething();
});
```

此时，a对应的是jquery插件，b对应的是swiper插件，相当于在你的代码中此时a就起到了`$`或者`jQuery`的作用。
`但是,并不建议你更改插件默认的标识，比如jQuery人们所熟悉的就是$，我们就应该在代码中使用$来代替它，而不要使用其他任何内容`。

### require方法

如果define可以简单的理解成为函数声明的话，那么`require`就可以理解成为`函数调用`。

实例代码：

```js
require(['jquery','swiper'],function($,Swiper){
	dosomething();
});
```

这样你就可以在你的代码中使用jquery和swiper插件了。

注意：`define声明时只是引用了相应的插件但是并没有加载，但是require是将插件加载并写在了页面的<head>标签中`，
具体形式如下：
```html
<script type="text/javascript" charset="utf-8" async="" data-requirecontext="_" data-requiremodule="jquery" src="http://cdnjs.cloudflare.com/ajax/libs/jquery/2.0.0/jquery.min.js"></script>
```

# 例子

下面我们就通过一个例子来了解具体如何进行操作：
	### 新建html文件，在页面中<head></head>中添加require.js的script引用，并指明项目入口
		<script data-main="js/main" src="js/require.min.js"></scrtipt>
	2. 在js文件夹中新建a.js 和 b.js文件，其中的起码分别如下：
		a.js
		```js
		define(function(){
			var a = function(){
				test : function(){
					alert('我是a.js中的test函数');
				}
			}
			return a;
		})
		```
		b.js
		```js
		define(a,function(a){
			var b = function(){
				test : function(){
					alert('我是b.js中的test函数,我的后面会调用a.test函数');
					a.test();
				}
			}
			return b;
		})
		```
	3. 在js文件夹中新建main.js的文件
	4. 在main.js文件中添加require.config的配置,并调用b模块
		```js
		require.config({
			baseUrl: './',
			paths: {
				a: ['js/a.js'],
				b: ['js/b.js']
			}
		});
		require(['b'],function(b){
			b.test();
		});
		```
		执行结果为 ：
				'我是b.js中的test函数,我的后面会调用a.test函数'
				'我是a.js中的test函数'
		
# [乐推前端基于requirejs的工作模式](/letui_require.md)
		
		
	
