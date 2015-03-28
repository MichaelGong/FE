#	loading.js的使用方法

### loading.js展示loading效果。此模块会自动依赖于 `styles/css/loading.css` 文件

> 在require引用中此模块的引用代号为 `M` 

#	API

### M.loading(type,bgType,bgColor)
产生loading效果
* type : loading的样式id，整型，目前有6种形式，支持传入1-6的数字，默认值为1
* bgType : loading层的背景形式，1：纯白色背景，2：半透明背景，默认值为半透明形式
* bgColor : 背景颜色，支持正常的所有形式的颜色值

loading效果会在body中第一个子元素前加上一个id= `Mpop`的div

一般情况下直接调用 `M.loading(1,1);`即可。

### M.loadingShow()
让之前生成的loading展示出来（仅仅只是展示而已），如果之前页面上没有id=`Mpop`的loading元素，此方法将无效。

### M.loadingHide()
隐藏loading效果。