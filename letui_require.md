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
			|----app               项目的工作目录
			|----styles			 
					 |----css	   项目中可能会用到的插件所依赖的css文件
					 |----jsng    项目中可能会用到的插件 和 require.config.js文件
	
开发人员需要svn更新上述 `SVN工作目录` 中的两个svn目录。
	
	为了使得项目上线和本地开发尽可能一致，在项目提交时改动的内容最小化，且便于团队之间相互合作，
	要保证 `app` 文件夹 和 `styles` 文件夹在同一级目录下，并且保证你的本地服务器可以直接访问到app所在的目录，
	因此你的本地工作路径应该是这样的：http://192.168.1.2:8080/app/xxx/xxx.html
	
# 开发规则

	1.开发人员所做的项目放在 `app` 文件夹中，在其中创建项目文件夹，并在文件夹中创建自己的项目，
	例如，test项目的形式如下：
		app
		 |----test                 项目目录
		 		|----index.html    项目页面
		 		|----css		        存放项目中css文件的文件夹
		 		|----images		        存放项目中图片文件的文件夹
		 		|----js			        存放项目中js文件的文件夹
		 		|----resource 	        存放项目中其他文件的文件夹，例如音频文件
	2.require.min.js的路径
	3.require.config.js的路径
	4.如何自动话判断服务器和本地环境
	5.验证token信息
	6.loading
	7.主逻辑
	
	
	