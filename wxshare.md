#	wxshare.js的使用方法

### wxshare.js用于微信分享。此模块依赖于 `wx`、`auth`、`ngapi`模块。

> 在require引用中此模块的引用代号为 `wxshare` 

#	API

### wxshare.initWx(share, letwxid, apiopenid, apitoken, succCb, cancelCb, errorCb)
微信分享
* share	: 必须，share是一个json形式的对象，格式如下
	```
	{
		title:'title',
		desc:'desc',
		link:'link',
		imgUrl:'imgUrl'
	}
	```
	* title : 分享的标题
	* desc : 分享的描述
	* link : 分享的链接
	* imgUrl : 分享的图标
* letwxid ： 必须， 对应账号的letwxid
* apiopenid ： 必须，用户的apiopenid
* apitoken ：  必须，用户的apitoken
* succCb ： 可选， 分享成功的回调函数
* cancelCb ： 可选， 取消分享的回调函数
* errorCb : 可选，分享失败的回调函数

### wxshare.initWxAuth(share,letwxid,succCb,cancelCb, errorCb)
微信分享，此方法与 `wxshare.initWx` 的区别在于在此方法内部会进行auth鉴权来回去用户的 `apiopenid` 和 `apitoken`，而 `wxshare.initWx`不会进行鉴权，因为	 `wxshare.initWx`的参数中传入了用户信息

此方法中的各参数和 `wxshare.initWx`中一致。

### wxshare.initWxCfg(share, wxconfig, succCb, cancelCb, errorCb)
用于注册微信分享事件。
>一般情况下，此方法只用于wxshare.js内部使用，并不需要外部调用。

* share : 分享的信息，同上。
* wxconfig : wx.config的配置信息
* succCb ： 可选， 分享成功的回调函数
* cancelCb ： 可选， 取消分享的回调函数
* errorCb : 可选，分享失败的回调函数

想了解更多有关微信JS-SDK的相关信息，请参考：[微信JS-SDK官方文档](http://mp.weixin.qq.com/wiki/7/aaa137b55fb2e0456bf8dd9148dd613f.html)