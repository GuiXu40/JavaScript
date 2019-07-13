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
每个节点都有一个parentNode属性，指向父节点，，包含在childNodes列表中的每个节点之间都是同胞节点。通过使用列表中的每个节点的previousSibling和nextSibling属性，可以访问同一列表中的其他节点,列表中的第一个节点的previousSibling属性和列表中的最后一个节点的nextSibling属性也为null。<br>
所有节点都有的最后一个节点是ownerDocument,指向表示整个文档的文档节点（意思是任何节点都属于他所在的文档--》作用：不必在节点层次中通过层层回溯到顶端，而是可以直接访问文档节点）。
```javascript
if(someNode.nextSilbling === null)
{
	alert("这是最后一个节点");
}else if(someNode.previousSibling === null)
{
	alert("这是第一个节点");
}
```
-----
注意点：虽然所有节点类型都继承自Node，但不是每种节点都有子节点。
-----
### 操作节点
+ 增加节点：appendChild();
```javascript
var returnNode = someNode.appendChild(newNode);
alert(returnNode == newNode);  //true
alert(someNode.lastChild == newNode);  //true
```
如果传入到appendChild()的节点已经是文档的一部分了，那就是将该节点从原来的位置移到新位置。
```javascript
//例如：调用appendChild()时传入父节点的第一个子节点，那么这个节点就会成为父节点的最后一个节点
//someNode 有多个子节点
var returnNode = someNode.appendChild(someNode.firstChild);
alert(returnNode == someNode.firstChild);    //false
alert(returnNode == someNode.lastChild);     //true
```
+ 插入节点： insertBefore():接受两个参数（要插入的节点和作为参照的节点）<br>
被插入的节点会变成参照节点的同胞节点（previousSibling）and （如果参照节点是null，和appendChild()一样的操作）
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
被替换的节点仍然还在文档中，当他在文档中没有了自己的位置。（所有关系指针都会被从它替代的节点复制过来）
+ 移除节点：removeChild()
```javascript
var former = someNode.removeChild(someNode.firstChild);
```
被移除的节点仍然还在文档中，当他在文档中没有了自己的位置。<br>
这4种方法都是对莫个节点的子节点进行操作。必须先取得父节点（parentNode）
+ 其他方法
   + cloneNode(布尔值)用于创建调用这个方法的节点的一个副本。，若为true：执行深复制（复制节点以及整个子节点树），false:执行浅复制（只复制节点本身）。--》复制后返回的节点副本属于文档所有，并没有父节点，要通过其他操作将其添加到文档中。<br>
   此方法不会复制添加到DOM节点的JavaScript属性，例如事件处理程序。但在IE中，会，因此复制之前最好先移除事件处理程序。。
   + normalize()--处理文档书中的文档节点--若后代节点出现空文本节点，则删除他，若找到相邻的文本节点，则合并成一个。
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
	```javascript
	//取得完整的URL
	var url = document.URL;
	／／取得域名
	var domain = document.domain;
	//取得来源页面的URL
	var referrer = document.referrer;
	```
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
  + namesItem() 通过元素的name特性取得集合中的项
	```javascript
	<img src="myimage#" name="myImage">
	
	var myImage=images.namedItem("myImage");
	//或者直接获取
	var myImage= images["myImage"]
	```
  + getElementByName():只有HTMLDocument才具有的方法   最常使用此方法的情况是取得单选按钮(保证单选按钮必须具有相同的name特征)
  ```javascript
  <fieldset>
  	<legend>Which color do you prefer?</legend>
	<ul>
		<li><input type="radio" value="red" name="color" id="colorRed">
			<lable for="colorRed">Red</lable></li>
		<li><input type="radio" value="green" name="color" id="colorGreen">
			<lable for="colorGreen">green</lable></li>
		<li><input type="radio" value="blue" name="color" id="colorBlue">
			<lable for="colorBlue">blue</lable></li>
	</ul>
  </fieldset>
  //获取所有单选按钮
  var radius = document.getElementsByName("color");//返回一个NodeList，nameItem()只会取得第一项
  ```
### 特殊集合
+ document.anchors:包含文档中所有带name的<a>元素
+ document.forms:包含文档中所有的form元素。
  + document.images 包含所有图片
  + document.link 带href特性的<a>元素
### DOM一致性检测
检测DOM的功能及其版本号：
```javascript
var hasXmlDom = document.implementation.hasFeature("XML","1.0");	
```

### 文档写入
将输出流写入网页的能力：
  + write(),，原样写入，writeln()：加一个‘\n’,都只接受一个参数，可以使用这两个方法向页面动态添加内容<br>
  ```javascript
  <script>
    document.write("<strong>"+(new Date()).toString()+ "<strong>");
  </script>
  ```
  可以动态包含外部资源：
  ```javascript
  <script>
    document.write("<script type=\"text\javascript\" scr=\"file.js\">" + "</script>");  //错误表示 ,</script>将被解释为外部的script标签 
     document.write("<script type=\"text\javascript\" scr=\"file.js\">" + "<\/script>"); //添加转义字符
  </script>
  ```
  如果在文档加载结束后再调用document.write()，那么输出的内容会重写整个页面
  + open(),close()：用于打开和关闭网页的输出流。如果在页面加载期间使用write和writeln方法，则不需要用到这个方法。
    
## :snowflake:Element类型
Element类型用于表现XML和HTNL元素，提供了对元素标签名，子节点及特性的访问。<br>
特性：
+ nodeType: 1;
+ nodeName: 元素的标签名
+ nodeValue: null
+ parentNode: document或Element
要访问元素的标签名：nodeName和tagName都可以
```javascript
<div id="myDiv"></div>
//取得标签
var div = document.getElementByTagName("myDiv");
alert(div.tagName);   //"Div"在HTML中标签名始终大写
alert(div.tagName == div.nodeName);   //true
```
在HTML中，标签名始终是大写，所以再比较之前将标签名转换为相同的大小写形式，
```javscript
if(element.tarName == "div"){}
//这样比较容易出错

if(element.tagName.toLowerCase() == "div"){
	//这样最好
}
```
### HTML元素
所以HTNL元素都有HTMLElement类型表示<br>
标准特性
+ id
+ title
+ lang
+ dir 语言的方向
+ className
### 取得特性
每个元素都有一个或多个特性，特性的用途是给出相应元素或内容的附加信息
+ getAttribute()
```javascript
var div = document.getElementById("myDiv");

alert(div.getAttribute("id"));   //"myDiv"
alert(div.getAttribute("class"));  //"bd"
alert(div.getAttribute("title"));  //"Body text"
alert(div.getAttribute("lang"));  //"en"
alert(div.getAttribute("dir"));  //"ltr"
```
注意：传递给getAttribute()的特姓名与实际的特性名相同。因此要获取class的特性值，要传入class而不是className(只有再通过对象属性访问特性时才用，)若不存在返回null。<br>
可以取得自定义特性
```javascript
<div id="myDiv" my_special_attribute="hello"></div>

//可以像其他特性一样取得这个值
var value = div.getAttribute("my_special_attribute");
```
特性的名称不区分大小写，根据HTML5规范，自定义特性应该加上data-前缀以便验证。<br>
特性：
+ style 再通过getAttribute()返回的是包含css文本的，而通过属性访问却是一个对象
+ onclick这样的事件处理程序 返回的是代码字符串。
由于这些差距，再通过JavaScript以编程方式操作DOM时，开发人员不使用getAttribute()，而使用对象的属性
### 设置特性-setAttribute(要设置的特姓名，值)
如果特性存在，会替代现有的值，不存在，则创建该属性并设置相应的值。
```javascript
div.setAttribute("id","someOtherId");
div.setAttribute("class","ft");
div.setAttribute("tltle","some other text");
```
setAttribute()方法既可以操作HTML特性也可以操作自定义特性。
### 删除元素特性removeAttribute()
```javascript
div.removeAttribute("class");
```
### attributes（操作子节点，打印出子节点的相关特征）
Element类型是使用attributes属性的唯一一个DOM节点。attributes属性中包含一个NamedNodeMap,与NodeList类似，是一个动态的集合，NameNodeMap对象拥有下列方法：
+ getNamedItem(name):返回nodeName属性等于name的节点
+ removeNamedItem(name):从列表移除nameNode属性等于name的节点
+ setNamedItem(node):向列表中添加节点，以节点的nodeName属性为索引
+ item（pos):返回位于数字pos位置处的节点
<br>
特别注意：attributes属性包括一系列节点，每个节点的nodeName就是特性的名称，而节点的nodeName就是特征的值。例如，要取得元素的id特征：
```javascript
var id = element.attributes.getNameItem("id").nodeValue;
```
开发人员更多会使用getAttribute(),removeAttribute(),setAttribute()，但想要遍历元素的特征，这个可以派上用场（在需要将DOM结构序列化为XML或HTML字符串时，多数会遍历元素的特征），可以将特征构造成name="value"的形式。
```javascript
function outputAttributes(element){
	var pairs= new Array(),
		attrName,
		attrValue,
		i,
		len;
	for(i=0,len=element.attributes.length;i<len;i++){
		attrName=element.attributes[i].nodename;
		attrValue=element.attributes[i].nodeValue;
		pairs.push(attrName+"=\""+ attrValue+ "\"");
	}
	return pairs.join(" ");
}
```
这是序列化长字符串时的一种常用技巧。但这个方法具有一些bug（浏览器兼容问题）改进：（使用每个节点都具有的specified属性）
```javascript
function outputAttributes(element){
	var pairs= new Array(),
		attrName,
		attrValue,
		i,
		len;
	for(i=0,len=element.attributes.length;i<len;i++){
		attrName=element.attributes[i].nodename;
		attrValue=element.attributes[i].nodeValue;
		if(element.attributes[i].specified){
			pairs.push(attrName+"=\""+ attrValue+ "\"");
		}
	}
	return pairs.join(" ");
}
```
### 创建元素
document.creatElement(元素的标签名)
```javascript
var div = docement.creatElement("div");
```
在创建新元素的同时，也为元素设置了ownerDocument属性。可以操作元素的特性
```javascript
div.id="myDiv";
div.className="box";
```
在新元素上设置这些属性只是赋予了相应的信息，由于新元素没有添加到文档树中，这些属性不会再浏览器中显示出来，可以使用appendChild(),insertBefore(),replaceChild().
<br>
也可以传入完整的元素标签
```javascript
var div = document.creatElement("<div id=\"myDiv\" calss=\"box\"></div>");
```
### 元素的字节点
元素的childNodes属性包含了他的所有子节点，浏览器看待子节点有明显不同。
<br>
注意点：不同的浏览器对于子节点有不同的看法：例如：<br>
```javascript
<ul>
	<li></li>
	<li></li>
	<li></li>
</ul>
```
IE对次有7个节点（还包含4个文本节点），其他的只有3个<br>
注意点：如果需要通过childNodes属性来遍历子节点，就不能忘记之一区别，通常都要先检查一下nodeType属性
```javascript
for(var i=0,len=element.childNodes.length;i<len;i++){
	if(element.childNodes[i].nodeType == 1){   //判断是否为元素节点
	 	//执行什么操作
	}
}
```
## :snowflake:Text类型
纯文本可以包含转义的HTML字符，但不包含HTML代码。
<br>
特性：
+ nodeType:3
+ nodeName:"#text"
+ nodeValue:节点包含的文本
+ parentNode:Element
+ 没有子节点
<br>
可以通过nodeValue属性或Text属性访问text节点包含的文本
操作文本节点的文本的方法 ：
+ appendData(text): 将text添加到节点的末尾
+ deleteData(offset,count):从offset指定的位置开始删除count个字符
+ insertData(offset,text)：从offset指定的位置插入text
+ replaceData(offset,count,text):用text替代offset到offset+count位置的文本
+ splitText(offset)：从指定位置将文本分为两个部分
+ substringData(offset,count)：提取offset到offset+count位置的文本
文本节点具有length属性，保存着节点中字符的数目，每个可以包含内容的元素最多只有一个文本节点，而且必须有内容存在。
```javascript
//没有内容，也没有文本节点
<div></div>

//有空格，因而有一个节点
<div> </div>

//有内容，因而有一个文本节点
<div>hello,world</div>
```
第三个div有一个文本节点---nodeValue为hello，world
<br>
访问文本子节点
```javascript
var textNode = div.firstChild;
```
修改内容：
```javascript
div.firstChild.nodeVvalue = "some other message";
```
注意：字符串会经过HTML编码
```javascript
div.firstChild.nodeValue = "some <strong>other</strong>message"
//输出结果是：some &lt;strong&gt;...
```
### 创建文本节点
document.createTextNode(插入节点的文本)创建文本节点
```javascript
var element = document.createElement("div");  //创建一个元素div
element.className = "message";  //给元素设置class特征
var text = document.createTextNode("hello,world");  //创建新文本节点
element.appendChild(text);  //将文本节点添加到div中
document.body.appendChild(element);   //将div添加到body中
```
一般每个元素只有一个文本节点，但可以包含多个子节点：
```javascript
var element = document.createElement("div");
element.className = "message";
var text = document.createTextNode("hello,world");
element.appendChild(text);

var another = document.createTextNode("guixu");
element.appendChild(another);
document.body.appendChild(element);
```
如果两个文本节点是相邻的同胞节点，这两个节点的文本就会连起来，中间不会有空格。
### 规范化文本节点
同胞节点容易导致混乱，使用normalize()，可以将所有文本节点合并成一个
```javascript
var element = document.createElement("div");
element.className = "message";
var text = document.createTextNode("hello,world");
element.appendChild(text);

var another = document.createTextNode("guixu");
element.appendChild(another);
document.body.appendChild(element);

alert(element.childNodes.length);  //2

element.normalize();   //使用此方法可以将文本同胞节点连在一起
alert(element.childNodes.length);  //1
alert(element.firstChild.nodeValue);  //hello worldguixu
```
### 分割文本节点
splitText(),将文本节点分为2个节点
```javascript
var element = document.createElement("div");
element.className = "message";
var text = document.createTextNode("hello,world");
element.appendChild(text);
document.body.appendChild(element);

var newNode = element.firstChild.splitText(5);
alert(element.firstChild.nodeValue);  //hello
alert(newNode.nodeValue);  //world
```
分割文本节点是从文本中获取数据的一种很常用的DOM解析技术

## :snowflake:Commend（注释）类型
特性：
+ nodeType:8
+ nodeName:#comment
+ nodeValue:注释的内容
+ 没有子节点
其他用法同text节点（可以通过nodeValue或data属性来取得注释的内容）
## :snowflake:CDATASection类型
只基于XML文档，继承自Text类型，有除splitText()之外的所有方法
+ nodeType: 4

## :snowflake:DocumentType类型
在Web服务器中不常用，所以不过多介绍
## :snowflake:DocumentFragment类型
（文档片段）
在文档中没有对应的标记。可以包含和控制节点，但不会像完整的文档那样占用额外的资源。
+ nodeType: 11
+ nodeName: "#document-fragment"
+ nodeValue: null
+ parentValue: null
虽然不能把文档片段直接添加到文档中，但可以把它当作一个仓库，既可以保存在里面保存将来会添加到文档中的节点。创建文档片段：document.cteateDocumentFragment()
```javascript
var fragment = document.createDocumentFragment();

```
可以通过appendChild(),insertBrfore()将文档片段内容添加到文档中，实际上只会将文档片段中的所有子节点添加到相应位置，永远不会成为文档树的一部分。
```javascript
<ul id="myList"></ul>
//为这个ul添加3个列表项，如果逐个添加列表项，导致浏览器反复渲染（呈现）新信息
var fragment = document.createDocumentFragment();
var ul = document.getElementById("myList");
var li = null;
for( vai i=0; i<3;i++)
{
  li = document.createElemrnt("li");
  li.appendChild(document.createTextNode("Item" + (i+1)));
  fargment.appendChild(li);
}
ul.appendChild(fragment);
```

## :snowflake:Attr类型
在所有浏览器中，都可以通过Attr类型访问构造函数和原型
+ nodeType:2
+ nodeValue: 是特征的值
+ nodeNmae： 特征的名字

Attr对象有3个属性：name(特性名称),value(特性的值),specified（布尔值：区分特性是在代码中指定的还是默认的）
```javascript
//为元素添加align属性
var attr = document.createAttribute("align");
attr.value = "left";
element.setAttributeNode(attr);
alert(element.attributes["align"].value);  //"left"
alert("");
```
<p id="a2"></p>

# :bulb:DOM操作技术
<a href="#title">:+1:回到目录</a>
## :snowflake:动态脚本
方式：插入外部文件，直接插入JavaScript代码
<br>
加载外部js文件的函数：
```javascript
function loadScript(url){
	var script = document.createElement("script");
	script.type="text/javascript";
	script.src = url;
	document.body.apendChild(script);
}
```

## :snowflake:动态样式
使用DOM代码可以很容易大的动态创建这个元素
```javascript
var link = document.createElement("link");
link.rel="stylesheet";
link.type="text/css";
link.href="style.css";
var head= document.getElementsByTagName("head")[0];
head.appendChild(link);
```
必须将link标签添加到head中。
<br>
通过下面的代码可以实时的向页面中添加样式（马上就可以看到）
```javascript
function loadStyleStringa(css){
	var style = document.createElement("style");
	style.type="text/css";
	try{
		style.appendChild(document.createTextNode(css));
	}catch(ex){
		style.styleSheet.cssText = css;
	}   //使用try catch解决IE的兼容性问题
	var head= document.getElementsByTagName("head")[0];
	head.appendChild(style);
}
```
## :snowflake:操作表格
表格行，单元格，表头
```javascrript
		<script type="text/javascript">
			//创建table
			var table=document.createElement("table");
			table.border=1;
			table.width="%100";
			//创建tbody
			var tbody=document.createElement("tbody");
			table.appendChild(tbody);
			//创建第一行
			var row1=document.createElement("tr");
			tbody.appendChild(row1);
			var cell1_1=document.createElement("td");
			cell1_1.appendChild(document.createTextNode("Cell 1,1"));
			row1.appendChild(cell1_1);
			var cell2_1=document.createElement("td");
			cell2_1.appendChild(document.createTextNode("Cell 2,1"));
			row1.appendChild(cell2_1);
			//创建第二行
			var row2=document.createElement("tr");
			tbody.appendChild(row2);
			var cell1_2=document.createElement("td");
			cell1_2.appendChild(document.createTextNode("call 1,2"));
			row2.appendChild(cell1_2);
			var cell2_2=document.createElement("td");
			cell2_2.appendChild(document.createTextNode("call 2,2"));
			row2.appendChild(cell2_2);
			//把表格添加到文档主体中
			document.body.appendChild(table);
		</script>
```
P282
## :snowflake:使用NodeList
NodeList,NameNodeMap,HTMLCollrction，都是动态的，当文档结构发生改变的时候，都会发生改变，从本质上讲，所有NodeList对象都是在访问DOM实时进行的查询
，以下代码会无限循环（因为这个集合是动态的）
```javascript
var divs = document.getElementsByTagName("div"),i,div;
for(i=0;i<divs.length;i++)
{
  div = document.createElement("div");
  document.body.appendChild(div);
}
```
length会一直改变
```javascript
var divs = document.getElementsByTagName("div"),i,div,len;
for(i=0,len=divs.length;i<len;i++)
{
  div = document.createElement("div");
  document.body.appendChild(div);
}
```


