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
大多数浏览器都支持原生的XHR对象,可以直接使用XHR的XMLHttpRequest()构造函数
```
var xhr=new XMLHttpRequest();
```
想支持IE的早期版本,就可以在createXHR()加入对原生对象的支持
```JavaScript
function createXHR(){
    if(typeof XMLHttpRequest != "undefined"){
        return new XMLHttpRequest();
    }else if(typeof arguments.callee.activeXString != "string"){
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
}
```
#### :corn:XHR的用法 
在使用XHR对象时,先调用open()方法
<font color=gray size=72>open()方法并不会真正发送请求</font>
第二步:调用send()方法.<br>

**open()**:接受3个参数--要发送请求的类型(get,POST等),请求的URL,表示是否异步发送请求的布尔值<br>
**只能向同一个域中使用相同端口和协议的URL发送请求**<br>
```JavaScript
xhr.open("get","example.php",false);
xhr.send(null);
```
send()方法接收一个参数,就是请求主体发送的数据.如果不需要请求主体发送数据,则必须传入null<br>
由于这次响应是同步的,在收到响应后,响应的数据会自动填充XHR对象的属性,相关属性介绍如下:<br>

属性|作用
---|:--:
responseText|作为响应主体被返回的文本
responseXML|如果响应的内容是"text/xml"或 "application/xml",这个属性中将保存包含着响应数据的的XML DOM文档
status|响应的HTTP状态
statusText|HTTP状态的说明

在接收到响应之后,想检查status属性,,将HTTP状态码为200作为成功的标志,此时responseText属性的内容已经就绪,在内容正确的情况下,responseXML也可以访问了,状态资源码**304**表示请求的资源没有被修改.<br>
检查代码:
```JavaScript
xhr.open("get","example.php","false");
xhr.sned(null);
if(xhr.status>=200 && xhr.status<300 || xhr.status==304){
    alert(xhr.responseText);
}else{
    alert("没有很成功");
}
```
在更多的情况下,使用异步请求,此时,可以检测xhr对象的readyState属性[表示请求/响应过程的当前活动阶段],属性可取的值为:

属性可取的值|阶段信息
---|:--:
0:未初始化|尚未调用open()方法
1:启动|已经调用open()方法,但未调用send()方法
2:发送|已经调用send()方法,但尚未接收响应
3:接收|已经接收到部分响应数据
4:完成|已经接收到全部数据

只要readyState属性值变化一次,就会触发一次readystatehange()事件
```JavaScript
var xhr = createXHR();
xhr.onreadystatechange=function(){
    if(xhr.readyState==4){
        if(xhr.status>=200 && xhr.status<300 || xhr.status==304){
            alert(xhr.responseText);
        }else{
            alert("没有很成功");
        }
    }
};
xhr.open("get","example.php","true");
xhr.send(null);
```
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
