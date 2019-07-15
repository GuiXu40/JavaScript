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
如果调用元素与该选择符匹配，返回true,否则，返回false
<p id="p2"></p>

## :banana:元素遍历
<a href="#title">:sweet_potato:回到目录</a>
为了解决大多数浏览器都会放回文本节点，导致childNodes和firstChild()等属性行为不一致，增加了5个属性：
+ childElementCount:返回子元素（不包括文本节点和注释）的个数
+ firstElementChild：指向的一个子元素（firstChild的元素版）
+ lastElementChild:指向最后一个元素（lastChild的元素版）
+ previousElementSibling:指向前一个同辈元素（previousSibling的元素版）
+ nextElementSibling：指向后一个同辈元素（nextSibling的元素版）
遍历某元素的子元素：比较<br>
```javascript
var i ,
    len,
    child=element.firstChild;
    while(child!=element.lastChild){
        if(child.nodeType==1){
            processChild(child);  //递归操作
        }
        child=child.nextSibling;
    }
    //对比，使用新加的元素
    var i,
        len,
        child=element.firstElementChild;
        while(child!=element.lastElementChild){
            processChild(child);
            child=child.nextElementSibling;
        }
```
<p id="p3"></p>

## :banana:HTML5
<a href="#title">:sweet_potato:回到目录</a>
#### :corn:与类相关的拓充
随着class的使用广泛，产生了一下方法：<br>
+ getElementsByClassName(一个或多个类名的字符串)，类名先后顺序不重要<br>
```javascript
//取得所有类中包括userame和current的元素
var all=document.getElementByClassName("username current");
//
var selected = document.getElementById("myDiv").getElementByClassName("selected");
```
+ classList属性：在类名中className是一个字符串，所以即使要修改字符串的一部分，也必须每次都设置整个字符串的值比如：<br>
```javascript
<div class="bd user disabled">...</div>
//首先，取得类名字符串并拆分成数组
var classNames=div.className.split(/\s+/);
//找到要删的类名
var pos=-1,
          i,
          len;
    for(i=0;ien=classNames.length;i<len;i++){
        if(classNames[i]=="user"){
            pos=i;
            break;
        }
    }
    //删除类名
    classNames.splice(i,1);
    //把剩下的类名重新组合并重新设置
    div.className=classNames.join(" ");
```
这种方法比较麻烦，所以HTML5重新增加了一种属性classList，有以下方法：<br>
+ add(value):将给定的字符串添加到列表中，如果值存在，就不再添加
+ contains(value):表示列表中是否存在给定的值，存在则返回true，否则false
+ remove(value):从列表中删除给定的字符串
+ toggle(value):如果列表中存在这个值，就删除，不存在就添加<br>
```javascript
div.classList.remove("dis");
```
#### :corn:焦点管理
+ document.activeElement属性，始终会指向引用DOM中当前获取了焦点的元素（元素获取焦点的方式：页面加载，用户输入（通常是按Tab键）,和在代码中调用focus()方法）
```javascript
var button = document.getElementById("button");
button.focus();
alert(document.activeElement===button);
```
注意：文档刚刚加载完成时，document.activeElement中保存的是document.body--文档加载期间，值为null。<br>
+ document.hasFocus()方法，用于确定文档是否获取了焦点，是则返回true
#### :corn:HTMLDocument的变化
+ readyState属性
   + loading:正在加载文档
   + complete：已经加载完成
+ 兼容模式：（区分页面的模式是标准的还是混杂的）document.compatMode
   + 标准模式：值为CSS1Compt
   + 混杂模式：backCompat
+ head属性（补充）
```javascript
var head = document.head  || document.getElementByTagName("head")[0];
```
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

