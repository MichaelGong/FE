#	MHJ.js的使用方法

###	MHJ.js返回了MHJ对象，该对象中封装了对cookie、url参数、post请求、模板的处理方法，该模块依赖于 `jQuery` 或 `Zepto` 。

> 在require引用中此模块的引用代号为 `MHJ` 

#	API

###	MHJ.mlog()
用于输出log信息

### MHJ.post(url,data,callback)
对 `jQuery` 或者 `Zepto` 的post方法的封装，默认接受的是json类型的数据。
* url : 请求的url地址
* data : 需要发往服务器的数据，按照键值对的形式，例如：{name:'test',age:15}
* callback : 请求成功的回调函数

>注意：`请求成功`的意思是：服务器返回了数据，包括error = 0 的情况，也包括 error 等于其他值得情况。

###	MHJ.getUrlParam(url)
获取通过浏览器地址传递的参数。
* url : （可选）需要处理的url，可以为空，为空时默认处理当前页面的url
* 该方法返回一个包含传递信息的对象

>注意：http://ng.letwx.com/app/xxx/index.html?letwxid=1&name=let&age=12
>对于这个链接经过该方法处理后的结果为：{letwxid:1,name:let,age:12}
>为了防止浏览器url中出现的 `#` 对传递数据产生影响，代码对 `#` 做了特殊处理，默认会将链接中的所有`#` 去除掉，然后再做处理。

使用方法示例：
```js
var url = 'http://ng.letwx.com/app/xxx/index.html?letwxid=1&name=let&id=12'
var letwxid = MHJ.getUrlParam('url').letwxid;
var name = MHJ.getUrlParam('url').name;
var age = MHJ.getUrlParam('url').age;
```

###	MHJ.setCookie(name,value,expire_days)
设置cookie
* name : 设置cookie的名字
* value : 设置cookie的值
* expire_days : 设置cookie的有效时间（以天为单位）

###	MHJ.getCookie(name)
获取cookie，不存在返回null
* name : 需要获取的cookie的名字

###	MHJ.delCookie(name)
删除cookie
* name : 需要删除的cookie的名字

### MHJ.tmpl
对html模板进行数据绑定

使用示例
```html
<div id="test"></div>
<textarea id="testTMP" style="display:none;">
	<%
    	if(data.id > 6){
    %>
    	<div>id 大于 6, id等于<%=data.id%></div>
    <%
    	}else{
    %>
    	<div>id 小于 6, id等于<%=data.id%></div>
    <%
    	}
    %>
</textarea>
```
```js
var data = {id:5}; //data必须是个json形式的对象
$('#test').html(MHJ.tmpl($('#testTMP').text(),data));
```
经过上述js可以使id="test"的div展示出我们需要的内容。

