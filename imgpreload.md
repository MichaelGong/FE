#	imgpreload.js的使用方法

### imgpreload.js用于进行图片预加载。

> 在require引用中此模块的引用代号为 `imgpreload` 

#	API

### imgpreload.load(imgs,onComplete,onReady)
进行图片预加载
* imgs : 图片路径数组
* onComplete : 图片加载完成后的回调函数，有2个参数：
	* loaded : 成功加载的图片数组
	* imgs ： 返回传入的图片数组
* onReady : 每加载完一张图片都会执行该方法，有2个参数：
	* n : 被加载图片在数组中的序号
	* img : 传入的图片的路径
	
