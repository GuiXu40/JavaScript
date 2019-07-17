<p id="title"></p>

# :strawberry:在HTML中使用JavaScript

<a href="#p1">:peach:script元素</a><br>
<a href="#p1">:peach:嵌入代码和外部文件</a><br>
<a href="#p1">:peach:文档模式</a><br>
<a href="#p1">:peach:noscript元素</a><br>
<p id="p1"></p>

## :banana:script元素 
<a href="#title">:sweet_potato:回到目录</a><br>
向页面中插入JavaScript的方法,就是使用<script>元素,它具有6个属性:<br>
+ async:可选,表示应该立即下载脚本,但不妨碍页面中的其他操作,只对外部文件有效
+ charset:可选,表示通过src属性指定的代码的字符集(很少使用)
+ defer:可选,表示脚本可以延迟到文档完全被解析和显示之后在执行
+ language:废弃
+ src:可选,表示包含要执行代码的外部文件
+ type:可选一直使用的是text/JavaScript,实际上,服务器再传送JavaScript文件时使用的MIME(多用途互联网邮件拓展类型)类型一般是application/x-javascript(这个属性不是必须的,默认是text/JavaScript)<br>
使用<script>的方式:1.直接在页面中嵌入JavaScript代码 2.包含外部JavaScript文件.<br>
注意:在使用<script>嵌入JavaScript代码时,代码中任何地方不能出现</script>字符串,会报错
```JavaScript
<script>
    function satHello(){
        alert("</script>");
    }
</script>
//会被当做是结束的标致.
```
改进:使用转义字符
```JavaScript
<script>
    function satHello(){
        alert("</\script>");
    }
</script>
```
引入外部JavaScript文件,src属性是必须的.
```JavaScript
<script type="text/javascript" src="aaa.js"></script>
```
注意:带有src属性的<script>元素不应该在<script></script>标签之间包含多余的代码,如果包含了,只会下载并执行外部脚本文件,无论如何包含代码,只有不存在defer和async,就只会按照script在页面出现的顺序执行

#### :corn:标签的位置
传统做法:将脚本放在<head>元素中,等全部JavaScript代码都被下载.解析,执行完成之后,才开始呈现页面的内容(页面呈现会出现延迟)<br>
现在的做法:放在<body>元素内容的后面
#### :corn:延迟脚本
给script添加defer属性,也可以起到上面一样的用法
#### :corn:异步脚本
async属性:不让页面等待两个脚本的下载和执行,从而异步加载页面其他内容(建议异步脚本不要在加载期间修改DOM)<br>
注意:在XHTML文档中,要把async设置为async="async"
#### :corn:在XHTML中的语法
XHTML(Extensible HyperText ,Markup Language可拓展超文本标记语言):是将HTML作为XML的应用而重新定义的一个标准(编写比HTML严格),例如:<br>
```JavaScript
<script>
    function compare(){
        if(a<b){
            alert("a小于b");
        }else if(a>b){
            alert("a大于b");
        }else{
            alert("a等于b");
        }
    }
</script>
```
上面这段代码中(<)号不被正确解析,会被当做一个新标签来解析,解决方法:<br>
+ 使用相应的实体(&lt)替换掉(<)
```JavaScript
<script>
    function compare(){
        if(a &lt b){
            alert("a小于b");
        }else if(a>b){
            alert("a大于b");
        }else{
            alert("a等于b");
        }
    }
</script>
```
+ 用一个CData片段包含JavaScript代码(这个区域可以包含不需要解析的任意格式的文本内容,但有浏览器兼容问题)
```JavaScript
<script ><![CDATA[
    function compare(){
        if(a<b){
            alert("a小于b");
        }else if(a>b){
            alert("a大于b");
        }else{
            alert("a等于b");
        }
    }]]>
</script>
//对于浏览器不兼容的问题,可以通过JavaScript注释将标记去掉
<script >
//<![CDATA[
    function compare(){
        if(a<b){
            alert("a小于b");
        }else if(a>b){
            alert("a大于b");
        }else{
            alert("a等于b");
        }
    }
    //]]>
</script>
```
#### :corn:不推荐使用的语法
<p id="p2"></p>

## :banana: 嵌入代码和外部文件
<a href="#title">:sweet_potato:回到目录</a><br>
两种方式使用JavaScript代码都可以:但引用外部文件的优点:<br>
+ 可维护性
+ 可缓存
+ 适应未来
<p id="p3"></p>

## :banana: 文档模式
<a href="#title">:sweet_potato:回到目录</a><br>
<p id="p4"></p>

## :banana:noscript元素 
<a href="#title">:sweet_potato:回到目录</a><br>
如果浏览器不支持脚本,或者浏览器支持脚本,但脚本被禁用<noscript>元素就会显现出来
