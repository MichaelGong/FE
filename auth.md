#	auth.js的使用方法

### auth.js用于对用户的身份进行验证，并获取到apitoken、apiopeni。该模块依赖于 `MHJ` 这个模块。

> 在require引用中此模块的引用代号为 `oAuth` 

#	API

### oAuth.cfg(letwxid,isdebug,scope)
对获取auth信息的操作进行相应的配置，配置了authurl等信息。此方法要配合 `oAuth.checkToken`方法。
* letwxid ：  必须，[查询地址](http://wewiki.sinaapp.com/NG平台letwxid对应关系表)
* isdebug ：  是否为调试模式，非调试模式（正式环境）下会跳转到微信相关页面获取用户信息再跳转回来。
* scope ： 是否获取用户头像、昵称等个人信息。

### oAuth.checkToken(cb,errcb)
获取用户的 `apiopenid` 、 `apitoken`等身份信息。
* cb : 成功后的回调函数，包括2个参数：`apiopenid` 、 `apitoken`
* errcb : 执行错误的回调函数。

示例：
```js
function check(MHJ,oAuth,cb){
	var letwxid = MHJ.getUrlParam().letwxid;
	if (letwxid) {
		oAuth.cfg(letwxid, isDebug);
		oAuth.checkToken(function(apiopenid, apitoken) {
			cb && cb(letwxid, apiopenid, apitoken);
		}, function() {
			alert('checktoken错误！');			
		});
	} else alert('应用ID错误');
}
```
### oAuth.getAuthId()
从cookie中获取用户的 `apiopenid`

### oAuth.getAuthToken()
从cookie中获取用户的 `apitoken`

### oAuth.clear()
删除cookie中的 `apiopenid`、`apitoken`信息。
>在请求中出现 `error=1002`时，需要执行此方法，并重新加载页面。