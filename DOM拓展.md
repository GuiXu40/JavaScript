<p id="title"></p>

# :strawberry:DOM拓展
<a href="#p1">:peach:选择符API</a><br>
<a href="#p1">:peach:元素遍历</a><br>
<a href="#p1">:peach:HTML5</a><br>
<a href="#p1">:peach:专有拓展</a><br>
对DOM的两个重要的拓展是SelectorAPI和HTML5
<p id="p1"></p>

## :banana:选择符API 
<a href="#title">:sweet_potato:回到目录</a>
就是根据css选择符选择与某个模式匹配的DOM元素（jquery 就是如此）
#### :corn:querySelector()方法
```javascript
//取得body元素
var body=document.querySelector("body");
//取得ID为mydiv的元素
var myDiv=document.querySelector("#mydiv");
```
css选择符可以简单也可以复杂，视情况而定
#### :corn:querySelectorAll()方法
这个方法返回的是一个NodeList
```javascript
//  取得某div中的所有em元素
var ems=document.getElementById("myDiv").querySelectorAll("em");
```
#### :corn:matchesSelectAll()方法
如果
#### :corn:match，返回
#### :corn:matchesSelectAll()方法
<p id="p2"></p>

## :banana:元素遍历
<a href="#title">:sweet_potato:回到目录</a>
<p id="p3"></p>

## :banana:HTML5
<a href="#title">:sweet_potato:回到目录</a>
#### :corn:与类相关的拓充
#### :corn:焦点管理
#### :corn:HTMLDocument的变化
#### :corn:字符集属性
#### :corn:自定义数据类型
#### :corn:插入标记
#### :corn:scorllIntoView()方法
<p id="p4"></p>

## :banana:专有拓展
<a href="#title">:sweet_potato:回到目录</a>
#### :corn:文档模式
#### :corn:children属性
#### :corn:contains()方法
#### :corn:插入文本
#### :corn:滚动

