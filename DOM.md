<p id="title"><p>
  
# :gem:标题：DOM

<a href="#a1">:diamonds:节点层次</a><br>
<a href="#a2">:diamonds:DOM操作技术</a><br>
Dom:Document object model是针对HTML和XML文档的一个API(应用程序编程接口)
<p id="a1"></p>

# :bulb:节点层次
<a href="#title">:+1:回到目录</a>
```html
<html>
  <head>
    <title></title>
  </head>
  <body>
    <p>hello,world</p>
  </body>
</html>
```
文档节点是每个文档的根节点。在这个例子中，文档节点只有一个子节点，既html元素，我们称之为文本元素(是最外层元素)<br>
元素通过元素节点来描述，特征（attribute）通过特征节点来描述.文档类型通过文档类型节点来描述，注释通过注释节点。总共有12总节点。。
## :snowflake:Node类型
定义了一个接口node，除了IE外都可以访问到这个接口，JavaScript中所有节点都继承自Node节点，因此所有节点类型都共享相同的基本属性和基本方法。<br>
每一个节点都有nodeType属性，用于表示节点的类型，由12个数值常量来表示。。<br>
通过比较这些常量，可以知道节点的类型。
```javascriptt
if(someNode.nodeType == Node.ELEMENT_NODE){
  alert("Node is an element.");
}

```
在IE浏览器中报错，保证兼容性，将其与数值比较：
```javascript
if(someNode.nodeType == 1)
{
  alert("Node is an element.");//适用与所有浏览器
}
```
### nodeName和nodeValue属性：
了解节点的具体信息，使用这两个属性，，使用之前最好先检测节点的属性：
```javascript
if(someNode.nodeType == 1)
{
  value = someNode.nodeName;
}
```
对于元素节点，nodeName是元素的标签名，nodeValue的值为null.
### 节点关系
每个节点都有childNodes属性，保存这一个NodeList对象(是一种类数组对象，用法同数组)
```javascript
var firstChild = someNode.childNodes[0];
var secondChild = someNode.childNodes.item(1);
var length = someNode.childNodes.length;
```
也可以将NodeList对象转换为数组：
```javascript
var arrayofNodes = Array.prototype.slice.call(someNode.childNode,0);
```
在IE8之前不可使用，需要手动枚举所用成员：
```javascript
function convertToArray(node){
  var array = null;
  try
  {
    array = Array.prototype.slice.call(someNode.childNode,0);
  }
  catch(ex)
  {
    array = new Array();
    for(var i =0,len=nodes.length;i<len;i++)
    {
      array.push(nodes[i]);
    }
    
  }
  return array;
}
```
每个节点都有一个parentNode属性，指向父节点，，包含在childNodes列表中的每个节点之间都是同胞节点。通过使用列表中的每个节点的previousSibling和nextSibling属性，可以访问同一列表中的其他节点。<br>
所有节点都有的最后一个节点是ownerDocument,指向表示整个文档的文档节点。
### 操作节点
+ 增加节点：appendChild();
```javascript
var returnNode = someNode.appendChild(newNode);
alert(returnNode == newNode);  //true
alert(someNode.lastChild == newNode);  //true
```
如果传入到appendChild()的节点已经是文档的一部分了，那就是将该节点从原来的位置移到新位置。
```javascript
//someNode 有多个子节点
var returnNode = someNode.appendChild(someNode.firstChild);
alert(returnNode == someNode.firstChild);    //false
alert(returnNode == someNode.lastChild);
```
+ 插入节点： insertBefore():接受两个参数（要插入的节点和作为参照的节点）
```javascript
//插入后成为最后一个子节点
returnNode = someNode.insertBefore(newNede,null);
alert(newNode == someNode.lastChild);  //true

//插入后成为第一个子节点
var returnNode = someNode.insertBefore(newNode,someNode,lastChild);

//插入到最后一个子节点前面
var returnNode = someNode.insertBefore(newNode,someNode.lastChild);
```
+ 改变节点：replaceChild(要插入的节点，要替换的节点)
```javascript
//替换第一个子节点
var returnNode = someNode.replaceChild(newNode,someNode.firstChild);

//替换最后一个子节点
var returnNode = someNode.replaceChild(newNode,someNode.lastChild);
```
被替换的节点仍然还在文档中，当他在文档中没有了自己的位置。
+ 移除节点：removeChild()
```javascript
var former = someNode.removeChild(someNode.firstChild);
```
被移除的节点仍然还在文档中，当他在文档中没有了自己的位置。<br>
这4种方法都是对莫个节点的子节点进行操作。必须先取得父节点（parentNode）
+ 其他方法
   + cloneNode(布尔值)用于创建调用这个方法的节点的一个副本。，若为true：执行深复制（复制节点以及整个子节点树），false:执行浅复制（只复制节点本身）。
   此方法不会复制添加到DOM节点的JavaScript属性，例如事件处理程序。但在IE中，会，因此复制之前最好先移除事件处理程序。。
## :snowflake:Document类型
Document类型表示文档,document对象是window对象的一个属性，因此可作为全局变量来访问。<br>
其特征：
+ nodeType为9；
+ nodeName为"#document";
+ nodeValue为null;
+ ownerDocument 为null
### 文档的子节点
两个内置访问子节点的方式：1.documentElement：该属性始终指向 HTML页面中的<html>元素。2.childNodes列表访问文档元素。
```javascript
<html>
  <body></body>  
</html>
  
var html = document.documentElement;   //取得对html的引用
alert(html == docement.childNodes[0]);  //true
alert(heml == docement.firstChild);  //true
```
document对象还有一个body属性，直接指向body属性
```javascript
var body = document.body;
```
### 文档信息
还具有一些标准的Document对象所没有的属性
+ title，包含着<tltle>元素中的文本(可修改)
  ```javascript
  var originalTitle = document.title;
  document.title = "New page title"
  ```
+ URL:包含网页完整的URL
+ domain:只包含页面的域名
+ referer: 保存着链接到当前页面的URL,在没有来源页面的时候，可能会包含空字符串。
注意：只有domain是可以设置的，而且所有浏览器对domain还有一个限制，既如果域名一开始是松散的“loose”，就不能将其设置为紧绷的。("wro.com"->"pp.wro.com" false )
### 查找元素
  + getElementById()<br>
  ID必须与页面中的id特性全面匹配，包括大小写。
  ```javascript
  <div id="myDiv">some text</div>
  //取得这个元素
  var div = document.getElementById("myDiv");  //正确的用法
  var div = document.getElementById("mydiv");  //错误的用法
  ```
  ```javascript
  <input type="text" name="myElement" value="Text field">
  <div id="myElement">A div</div>
  ```
  再此代码中调用document.getElementById("myElement");返回的是input标签
  + getElementByTagName();<br>
  返回的是包含0，或多个元素的NodeList,在html文档中会返回一个HTMLCollection.
  ```javascript
  var images = document.getElementsByTagName("img");
  alert(images.length);
  alert(images[0].src);  //输出第一个图像元素的src特性
  alert(images.item(0).src);// 输出第一个元素的src特性。
  ```
  + HTMLCollection还有一个方法：namedItem(),通过元素的name特性
  ```javascript
  <img src="" name="myImage">
  var image = images.nameItem("myImage");
  ```
  还支持名称访问项
  ```javascript
  var image = images["myImage"];
  ```
  要取得文档中的所有元素，可以向getElementByTagName()中传入*号。<br>
  + getElementByName():只有HTMLDocument才具有的方法
### 特殊集合
+ document.anchors:包含文档中所有带name的<a>元素
+ document.forms:包含文档中所有的form元素。
  + document.images 包含所有图片
  + document.link 带href特性的<a>元素
### DOM一致性检测
  







## :snowflake:Element类型
## :snowflake:Text类型
## :snowflake:Command类型
## :snowflake:CDATASection类型
## :snowflake:DocumentType类型
## :snowflake:DocumentFragment类型
## :snowflake:
<p id="a2"></p>

# :bulb:DOM操作技术
<a href="#title">:+1:回到目录</a>
## :snowflake:动态脚本
## :snowflake:动态样式
## :snowflake:操作表格
## :snowflake:使用NodeList


