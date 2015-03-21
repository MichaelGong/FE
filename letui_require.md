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

	请先仔细阅读上述`资源网址`中的 乐推接口文档中的1,2,3部分，然后再向下阅读。
	
	乐推服务器上的工作目录如下：
		ng.letwx.com
			|
			|------app
			|------styles
					 |-------jsng
	
	开发人员需要svn更新上述`SVN工作目录`中的两个svn目录。
	
	为了使得项目上线和本地开发尽可能一致，且在项目提交时改动的内容最小化，要保证 `app` 文件夹 和 `styles` 文件夹在同一级目录下