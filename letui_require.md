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
<script data-main="js/index" src="//ngcdn.letwx.com/styles/jsng/require.min.js"></script>
```
3.require.config.js的服务器路径为：http://ng.letwx.com/styles/jsng/require.config.js

4.js中需要自动判断是服务器还是本地环境（为了减少代码提交时的手动改动）：
>此方法和之前的方式不一样，请注意修改

```js
var config = (function(){
    var baseurl = 'http://ng.letwx.com/';
    var arr = window.location.hostname.split('.')[0];
    var isDebug = (arr=='192')||(arr=='127')||(arr=='localhost')||(arr=='');
    return {
        touch : 'touchstart',
        isDebug : isDebug,
        baseUrl : isDebug ? '../../../' : baseurl,
		baseCDNUrl : 'http://ngcdn.letwx.com/',
        urlConfig : (isDebug ? '../../../' : baseurl)+'styles/jsng/require.config.js'
    }
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
	
>页面中的所有内容都要被id="content"的元素包含，且在html中就将content元素隐藏，
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

8.在测试地址下，`微信分享`是无法正常调用的，因为需要在微信后台设置相应的域名才可以，所以建议`微信分享`的代码在其他逻辑都完成之后再加入到页面中去。

9.开发人员开发的任何一个项目的url上都需要带有 `letwxid` 这个字段，形式如下： 
```
http://ng.letwx.com/app/xxx/index.html?letwxid=12
```
目的是为了便于管理、修改相关应用，以及依赖于 `letwxid` 会做一些其他的操作。开发人员开发时，如果一个应用中有多个页面，应该保证 `letwxid` 都是从首页传递下去的，即只需要在首页的url上加上 `letwxid` ,其他页面的 `letwxid` 都是通过首页传递下去的形式添加的。
> `letwxid` 的查询地址：http://wewiki.sinaapp.com/NG平台letwxid对应关系表

10.微信分享引用 `wxshare` 插件，在验证了身份信息后调用方法如下：
```js
wxshare.initWx(shareInfo,gameid,apiopenid,apitoken); //可能会有变动
```
在没有验证身份信息的时候调用方法如下：
```js
wxshare.initWxAuth(share, letwxid);
```
具体调用方式请参考下方的 `wxshare.js的使用方法`。
	
# jsng文件夹中的插件说明

jsng文件夹中存放着项目中可能会用到的插件和 `require.config.js`文件，

下面是jsng中各插件的的作用和使用说明：


| 文件名称     |使用代号	| 说明		| 使用方法  	|
| :-------- |:---------|:---------| :-------- |
| require.config.js  | 无 | requirejs的配置文件，其中规定了各种插件	|     		|
| MHJ.js     | MHJ  | 包含了一些常用的基础js方法 	|  [MHJ.js使用方法](MHJ.md)  		|
| auth.js	| oAuth	| 用于进行乐推的oAuth认证	|	[auth.js的使用方法](auth.md)	|
| ngapi.js      | ngapi   | 对服务器的请求（封装了跨域请求和post请求） 	|  [ngapi.js的使用方法](ngapi.md)  		|
| imgpreload.js		| imgpreload	| 用于进行图片预加载	|	[imgpreload.js的使用方法](imgpreload.md)	|
| wxshare.js	| wxshare	| 对微信分享进行了封装	| [wxshare.js的使用方法](wxshare.md) |
| loading.js	| M	| 用于页面进行请求时的loading效果	|	[loading.js的使用方法](loading.js)	|
| css.min.js	| css	| 用于加载css文件	|	[css.min.js的使用方法](https://github.com/guybedford/require-css)	|
| hammer.min.js	| Hammer	| 一个触摸插件	|	[hammer.min.js的使用方法](https://github.com/hammerjs/hammer.js)	|
| idangerous.swiper.min.js | Swiper	| swiper单页滑动插件	|	[swiper.min.js的使用方法](https://github.com/nolimits4web/swiper/)	|
| idangerous.swiper.hashnav.js	|  Swiper	|	依赖于swiper.min.js的添加hash的插件	|	[hashnav.js的使用方法](hashnav.md)	|
| idangerous.swiper_progress.js	|  Swiper	|	依赖于swiper.min.js的progress插件	|	[progress.js的使用方法](progress.md)	|
| qrcode.js	| QRCode	| 二维码生成插件	| [qrcode的使用方法](https://github.com/kazuhikoarase/qrcode-generator)	|
| shake.js	| Shake 	|摇一摇插件	|	[shake.js的使用方法](https://github.com/alexgibson/shake.js)	|
| jquery.min.js | $		| jquery插件 | [jquery中文文档](http://www.hemin.cn/jq/) |
| zepto.min.js 	| $		| zepto插件（jQuery的简化版）	| [zepto中文文档](http://mweb.baidu.com/zeptoapi/) |
| wx	| wx 	| 微信官方出的微信分享插件	| [使用方法](http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html) |


```js
var config = (function(){
	var baseurl = 'http://lppz.letwx.com/app/eat/';
	var arr = window.location.hostname.split('.')[0];
	var isDebug = (arr=='192')||(arr=='127')||(arr=='localhost')||(arr=='');
	return {
		touch : 'touchstart',
		isDebug : isDebug,
		baseUrl : isDebug ? '../../../lppz/eat' : baseurl,
		urlConfig : isDebug ? 'js/libs/require.config.js':baseurl+'js/libs/require.config.js'
	}
}());
```

