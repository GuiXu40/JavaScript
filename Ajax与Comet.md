<p id="title"></p>

# :strawberry:Ajax与Comet

<a href="#p1">:peach:XMLHttpRequest对象</a><br>
<a href="#p2">:peach:XMLHttpRequest 2级</a><br>
<a href="#p3">:peach:进度事件</a><br>
<a href="#p4">:peach:跨域源资源分享</a><br>
<a href="#p5">:peach:其他跨域技术</a><br>
<a href="#p6">:peach:安全</a><br>
Ajax的核心技术是XMLHttpRequests对象(简称XHR),XHR为向服务器发送请求和解析服务器响应提供了流畅的接口,能够异步的方式从服务器取得更多的信息,这种技术就是无需刷新页面即可从服务器取得数据,但不一定是XHR数据
<p id="p1"></p>

## :banana:XMLHttpRequest对象
<a href="#title">:sweet_potato:回到目录</a><br>
IE5是第一款引入XHR对象的浏览器,是通过MSXML库中的一个ActiveX对象实现的,在IE中,会遇到三种不同版本的XHR对象,要使用MSXML库中的XHR对象,需要编写一个函数
```JavaScript
function createXHR(){
    if(typeof arguments.callee.activeXString != "string"){
        var versions = ["MSXML2.XMLHttp.6.0","MSXML2.XMLHttp.3.0","MSXML2.XMLHttp"],
            i,len;
            
        for(i=0,len=versions.length;i<len;i++){
            try{
                new ActiveXObject(version[i]);
                arguments.callee.activeXString = version[i];
                break;
            }catch(ex){
                //跳过
            }
        }
    }
    
    return new ActiveXObject(arguments.callee.activeXString);
}
```
#### :corn:XHR的用法 
#### :corn:HTTP头部信息
#### :corn:GET请求
#### :corn:POST请求
<p id="p2"></p>

## :banana:XMLHttpRequest 2级
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:FromData
#### :corn:超时设定
#### :corn:overrideMimeType
<p id="p3"></p>

## :banana:进度事件 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:load事件
#### :corn:progress事件
<p id="p4"></p>

## :banana:跨域源资源分享 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:IE对CORS的实现
#### :corn:其他浏览器对CORS的实现
#### :corn:Preflighted Requests
#### :corn:带凭据的请求
#### :corn:跨浏览器的CORS
<p id="p5"></p>

## :banana:其他跨域技术 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:图像Ping
#### :corn:JSONP
#### :corn:Comet
#### :corn:服务器发送事件
#### :corn:Web Scokets
#### :corn:SSE与Web 
<p id="p6"></p>

## :banana:安全 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn: 
