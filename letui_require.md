# 乐推微信前端基于requirejs的工作模式

# 资源网址：
  * 乐推官网：[http://ng.letwx.com](http://ng.letwx.com)
  * 乐推wiki网址：[http://wewiki.sinaapp.com](http://wewiki.sinaapp.com)
  * 乐推接口文档：[http://wewiki.sinaapp.com/Ng.letwx.com接口文档](http://wewiki.sinaapp.com/Ng.letwx.com接口文档)
  * 协作平台worktile：[https://worktile.com/](https://worktile.com/)
  * SVN工作目录：
  	* app文件夹： http://203.195.189.40/svn/ng/app
  	* styles文件夹： http://203.195.189.40/svn/ng/styles
  
# 如何构建本地工作环境

请先仔细阅读上述 `资源网址`中的 `乐推接口文档` 中的1,2,3部分，然后再向下阅读。
	
	乐推服务器上的工作目录如下：
		ng.letwx.com
			|
			|----app               	项目的工作目录
			|----styles			 
					 |----css	  	项目中可能会用到的插件所依赖的css文件
					 |----jsng    	项目中可能会用到的插件 和 require.config.js文件
	
开发人员需要svn更新上述 `SVN工作目录` 中的两个svn目录。
	
	为了使得项目上线和本地开发尽可能一致，在项目提交时改动的内容最小化，且便于团队之间相互合作，
	要保证 `app` 文件夹 和 `styles` 文件夹在同一级目录下，并且保证你的本地服务器可以直接访问到app所在的目录，
	因此你的本地工作路径应该是这样的：http://192.168.1.2:8080/app/xxx/xxx.html
	
# 开发规则

###开发人员需要遵守如下规则：

1.开发人员所做的项目放在 `app` 文件夹中，在其中创建项目文件夹，并在文件夹中创建自己的项目，
例如，test项目的形式如下：

```
	app
	 |----test					项目目录
			|----index.html		项目页面
	 		|----css			存放项目中css文件的文件夹
	 		|----images			存放项目中图片文件的文件夹
	 		|----js				存放项目中js文件的文件夹
	 		|----resource		存放项目中其他文件的文件夹，例如音频文件
```

2.页面中引用require.min.js的方式：
```html
<script data-main="js/index" defer="defer" async="async" src="//ngcdn.letwx.com/styles/jsng/require.min.js"></script>
```
3.require.config.js的服务器路径为：http://ng.letwx.com/styles/jsng/require.config.js

4.js中需要自动判断是服务器还是本地环境（为了减少代码提交时的手动改动）：
```js
var isDebug = (function(){
	var hostnameIndex = window.location.hostname.split('.')[0];
	return (hostnameIndex == '192') || (hostnameIndex == '127') || (hostnameIndex == 'localhost') || (hostnameIndex == '');
}());
```

5.应用需要验证token信息的，代码如下：
```js
require(['MHJ', 'auth'],function(MHJ,oAuth){
	check(MHJ,oAuth,init);
	function init(letwxid,apiopenid,apitoken){
		dosomething();
	}
});
function check(MHJ,oAuth,cb){
	var letwxid = MHJ.getUrlParam().letwxid;
	if (letwxid) {
		oAuth.cfg(letwxid, isDebug);
		oAuth.checkToken(function(apiopenid, apitoken) {
			cb && cb(letwxid, apiopenid, apitoken);
		}, function() {
			//alert('checktoken错误！');
		});
	} else alert('应用ID错误');
}
```

6.页面中需要有loading页面，请引用‘loading’这个组件，可以这样引用：
```js
require(['loading','x','xxx'],function(M,x,xx){
	M.loading(1,1);
	dosomething();
	M.loadingHide();
});
```
使用loading时，最好是将页面中的其他元素隐藏，为此约定如下：
	
	页面中的所有内容都要被id="content"的元素包含，且在html中就将content元素隐藏，
	等到需要加载的元素或进行的操作进行完毕后，再将content元素显示。

7.主逻辑部分：

主逻辑部分应该在`loading`、`checktoken`之后进行。<br>
主逻辑部分的代码应该保证代码的`简洁性`和`可读性`。

>注意：在代码中会涉及到服务器请求，你需要判断服务器传过来的数据中的error信息是否等于1002，如果等于1002就需要清除`token`信息（error等于1002代表token信息过期），具体代码如下：
>
		var error = data.error - 0;
		if(error == 1002){
			alert('您的身份信息已过期，点击确定重新加载页面！');
			oAuth.clear();
			window.location.reload();
		}
	
# jsng文件夹中的插件说明

jsng文件夹中存放着项目中可能会用到的插件和 `require.config.js`文件，

下面是jsng中各插件的的作用和使用说明：


| 文件名称     	| 说明		| 使用方法  	|
| :-------- |:---------| :-------- |
| require.config.js  | requirejs的配置文件，其中规定了各种插件	|     		|
| MHJ.js     |   包含了一些常用的基础js方法 	|  [MHJ.js使用方法](MHJjs.md)  		|
| auth.js	|	用于进行乐推的oAuth认证	|	[auth.js的使用方法](auth.md)	|
| ngapi.js      |    对服务器的请求（封装了跨域请求和post请求） 	|  [ngapi.js的使用方法](ngapi.md)  		|
| imgpreload.js		|	用于进行图片预加载	|	[imgpreload.js的使用方法](imgpreload.md)	|
| wxshare.js	|	对微信分享进行了封装	| [wxshare.js的使用方法](wxshare.md) |
| loading.js	|	用于页面进行请求时的loading效果	|	[loading.js的使用方法](loading.js)	|
| css.min.js	|	用于加载css文件	|	[css.min.js的使用方法](https://github.com/guybedford/require-css)	|
| hammer.min.js	|	一个触摸插件	|	[hammer.min.js的使用方法](https://github.com/hammerjs/hammer.js)	|
| idangerous.swiper.min.js |	swiper单页滑动插件	|	[swiper.min.js的使用方法](https://github.com/nolimits4web/swiper/)	|
| idangerous.swiper.hashnav.js	|	依赖于swiper.min.js的添加hash的插件	|	[hashnav.js的使用方法](hashnav.md)	|
| idangerous.swiper_progress.js	|	依赖于swiper.min.js的progress插件	|	[progress.js的使用方法](progress.md)	|
| qrcode.js	|	二维码生成插件	|	[qrcode的使用方法](https://github.com/kazuhikoarase/qrcode-generator)	|
| shake.js	|	摇一摇插件	|	[shake.js的使用方法](https://github.com/alexgibson/shake.js)	|


