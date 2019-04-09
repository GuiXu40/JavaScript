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
每个节点都有一个parentNode属性，指向父节点，，包含在childNodes列表中的每个节点之间都是同胞节点

















## :snowflake:Document类型
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


