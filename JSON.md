<p id="title"></p>

# :strawberry:JSON

<a href="#p1">:peach:语法</a><br>
<a href="#p2">:peach:解析与系列化</a><br>
是一种数据格式而不是一种编程语言,可以表示3种类型的值<br>
+ 简单值  不支持undefined
+ 对象  一组无序的键值对
+ 数组  有序值的列表
<p id="p1"></p>

## :banana:语法 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:简单值
下列都是有效的JSON简单值:
```JavaScript
5
"hello,world"
```
#### :corn:对象
JSON中的对象与JavaScript中的字面量有一些不同
```JavaScript
//对象字面量
var person = {
    name:"guixu",
    age : 18
};
//JSON
{
    "name":"guixu",
    "age": 18
}
```
有两个不同的地方:(1.)没有声明变量(2.)没有末尾的分号<br>
>> 注意:在JSON中,对象的属性必须加双引号
<br>
属性的值可以是简单值,也可以是复杂类型的值
```JavaScript
{
    "name":"guixu",
    "age":18,
    "school":{
        "name": "123 school",
        "localtion": "afsdfas"
    }
}
```
#### :corn:数组
JSON数组采用的就是JavaScript中的数组字面量形式
```JavaScript
//数组字面量
var values = [25,"hi",true];
//JSON
[25,"hi",true]
```
JSON数组也没有变量和分号,可以把数组和对象结合起来
[
    {
        "title": "gggggggg",
        "author": [
            "fad"
        ],
        "edition": 3,
        "year": 2009
    },
    {
        "title": "gggggggg",
        "author": [
            "fad"
        ],
        "edition": 3,
        "year": 2009
    },
    {
        "title": "gggggggg",
        "author": [
            "fad"
        ],
        "edition": 3,
        "year": 2009
    }
]
<p id="p2"></p>

## :banana:解析与系列化 
<a href="#title">:sweet_potato:回到目录</a><br>
#### :corn:JSON对象
JSON对象有两个方法:stringify()和parese(),作用是把JavaScript对象序列化为JSON字符串和把JSON字符串解析为原生JavaScript值
```JavaScript
var book = {
    title:"hhh",
    auther: [
        "guixu"
    ],
    edition: 3,
    year: 2019
};

var jsonText = JSON.stringify(book);
alert(jsonText);   //{"title":"hhh","auther":["guixu"],"edition":3,"year":2019}  此方法不包含任何空格和缩进
var bookCopy=JSON.parse(jsonText);
```
在序列化JavaScript对象时,所有函数原型及原型成员都会被忽略,虽然book和bookCopy有相同的属性,但他们两个是独立的,没有任何关系的对象
#### :corn:序列化选项
JSON.stringify()除了要序列化的JavaScript对象外,还可以接收两个参数,第一个参数是过滤器,可以是数组,也可以是函数,第二个参数是一个选项,表示是否在JSON字符串中保留缩进
<br>
+ 过滤结果
   + 如果参数是数组,返回值中只包含数组中列出来的项
   ```JavaScript
   var book={
        "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019
   };
   var jsonText = JSON.stringify(book,["title","edition"]);
   alert(jsonText);   //{"title":"sffd","edition":3}
   ```
   + 如果是函数的话,有所不同,传入的函数接收两个参数,属性名和属性值,如果函数返回了undefined,那么相应的属性会被忽略
   ```JavaScript
      var book={
        "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019
   };
   
   var jsonText = JSON.stringify(book,function(key,value){
        switch(key){
            case "auther":
                return value.join(",");
            case "year":
                return 5000;
            case "edition":
                return undefined;
            default:
                return value;
        }
   });
   alert(jsonText);  //{"title":"sffd","auther":"guixu","year":5000}
   ```
+ 第三个参数用于控制结果中的缩进和空白符,如果参数是一个数值,表示每个级别缩进的空格数
```JavaScript
      var book={
        "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019
   };
   var jsonText=JSON.stringify(book,null,4);
   alert(jsonText);
   ////////////////运行结果:
   {
      "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019
   }
```
也在结果字符串中插入了换行符
,最大缩进空格数为10,所有大于10的都会转换为10,如果参数是一个字符串而非数值,则这个字符串将被当做缩进字符(不在使用空格)
```JavaScript
      var book={
        "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019
   };
   var jsonText=JSON.stringify(book,null,"--");
   alert(jsonText);
      ////////////////运行结果:
   {
    --"title": "sffd",
    --"auther":[
    ----   "guixu"
    -- ],
    --edition: 3,
    -- year: 2019
   }
```
+ toJSON()方法
#### :corn:解析选项
JSON.parse()方法也可以接收另一个参数,是一个函数(被称作还原函数)
```JavaScript
      var book={
        "title": "sffd",
        "auther":[
            "guixu"
        ],
        edition: 3,
        year: 2019,
        releaseDate: new Date(2019,7,18)
   };
   var jsonText=JSON.stringify(book);
   alert(jsonText);   //{"title":"sffd","auther":["guixu"],"edition":3,"year":2019,"releaseData":"2019-08-17T16:00:00.000Z"}
   
   var bookCopy=JSON.parse(jsonText,function(key,value){
      if(key=="releaseDate"){
          return new Date(value);
      }else{
          return value;
      }
   });
   alert(bookCopy.releaseDate.getFullYear());   //2019
```
