# viewport动态设置的一些理解和方法

> 在制作移动端页面的时候，都会用到一个很重要的 `meta` 属性：viewport,这个属性能够更改页面的显示行为，
简单的说就是我们可以让页面按任意你想要的宽度进行显示，这样有助于页面的快速完成。

例如：设计师在设计页面的时候会按照宽度640px的方式进行设计，但是到了页面层面，
由于不同的手机的宽度都不相同，相同的页面在不同的手机上会有不同的显示效果，
这个时候可能就需要我们去手动调整不同的手机下的样式，会比较麻烦，这个时候我们就可以借助viewport进行统一的设置
或者借助css中的zoom属性进行设置。

### 目前我所看的的形式大概有两种
1.动态设置viewport来满足缩放需求。
2.通过zoom使得页面进行缩放来满足需求。
> 但是这两种方式都需要注意字体大小的设置，建议页面中字体单位统一用rem.

### 动态设置viewport
此方法的思路来自 ：[网易前端](https://github.com/unbug/generator-webappstarter/blob/master/app/templates/app/src/util/MetaHandler.js)
页面需要加上如下 `meta` :
```html
<meta name="viewport" content="width=device-width, target-densitydpi=device-dpi, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```
或者
```html
<meta name="viewport" content="width=640, target-densitydpi=device-dpi, initial-scale=1.0, minimum-scale=1.0, maximum-scale=1.0, user-scalable=no">
```
在<head>中添加js
```js
fixMeta(640);
function fixMeta(metawidth){
    var viewport = document.querySelector('meta[name = viewport]'),
        ua = navigator.userAgent.toLowerCase(),
        android = ua.match(/(android);?[\s\/]+([\d.]+)?/),
        ipad = ua.match(/(ipad).*os\s([\d_]+)/),
        ipod = ua.match(/(ipod)(.*os\s([\d_]+))?/),
        iphone = !ipad && ua.match(/(iphone\sos)\s([\d_]+)/),
        os = {};
    if(android) os.android = true, os.version = android[2];
    if(iphone && !ipod) os.ios = os.iphone = true, os.version = iphone[2].replace(/_/g,'.');
    if(ipad) os.ios = os.ipad = true, os.version = ipad[2].replace(/_/g, '.');
    if(ipod) os.ios = os.ipod = true, os.version = ipod[3] ? ipod[3].replace(/_/g, '.') : null;

    var designWidth = metawidth;

    var iw = window.innerWidth || designWidth,
        ow = window.outerWidth || iw,
        sw = window.screen.width || iw,
        saw = window.screen.availWidth || iw,
        ih = window.innerHeight || designWidth,
        oh = window.outerHeight || ih,
        sh = window.screen.height || ih,
        sah = window.screen.availHeight || ih,
        w = Math.min(iw,ow,sw,saw,ih,oh,sh,sah),
        ratio = w/designWidth,
        dpr = window.devicePixelRatio,
        ratio = Math.min(ratio,dpr);
    os.ratio = ratio;
    if(os.android){
        viewport.setAttribute('content','width=640, minimum-scale = '+ ratio +', maximum-scale = '+ ratio +', target-densitydpi=device-dpi,user-scalable=no');
    }else if(os.ios && !os.android){
        viewport.setAttribute('content','width=640,target-densitydpi=device-dpi,user-scalable=no');
        if(os.ios && parseInt(os.version)<7){
            viewport.setAttribute('content','width=640,target-densitydpi=device-dpi,initial-scale='+ ratio +', user-scalable=no');
        }
    }
}
```

### 使用css属性zoom
> 此方法来自京东微信公众号中的页面

首先你需要在head中添加如下代码：
```html
<style type="text/css" id="M_zoom"></style>
```
接下来你需要在<head>标签中添加如下js代码：
```js
var _zoom = 0;
var _zoomDom = document.getElementById('M_zoom');
var changeSize = function(){
    var _width = document.documentElement.clientWidth || document.body.clientWidth;
    _zoom = _width / 640;
    _zoomDom.innerHTML = '.zoom{zoom:'+ _zoom +'}';
};
changeSize();
window.addEventListener('resize',changeSize,!1);
```


