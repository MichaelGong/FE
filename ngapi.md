#	ngapi.js的使用方法

### ngapi.js用于对服务器进行post请求。此模块依赖于 `jQuery`或者 `Zepto`

> 在require引用中此模块的引用代号为 `ngapi` 

#	API

### ngapi(action,params,appid,callback,apiopenid,apitoken,debug)
* action : 必须，请求的接口名称，全部小写
* params ： 可选，请求接口的参数，json格式的对象
* appid ： 必须，对应账号的letwxid
* callback ： 必须，回调方法，返回一个json格式的参数
* apiopenid ： 必须，用户的apiopenid
* apitoken ： 必须，用户的apitoken
* debug : 可选，设置为nf时，代表进入debug模式，系统会不对apiopenid,apitoken进行鉴权

此方法内部采用的是post方式进行服务器请求，并解决了跨域问题。