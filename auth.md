#	auth.js的使用方法

### auth.js用于对用户的身份进行验证，并获取到apitoken、apiopeni。该模块依赖于 `MHJ` 这个模块。

> 在require引用中此模块的引用代号为 `oAuth` 

#	API

### oAuth.cfg(letwxid,isdebug,scope)
对获取auth信息的操作进行相应的配置，配置了authurl等信息。
* letwxid ：  必须，[查询地址](http://wewiki.sinaapp.com/NG平台letwxid对应关系表)
* isdebug ：  是否为调试模式，非调试模式（正式环境）下会跳转到微信相关页面获取用户信息再跳转回来。
* scope ： 是否获取用户头像、昵称等个人信息。

