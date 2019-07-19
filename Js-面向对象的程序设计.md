# :sun_with_face:面向对象
---
<p id="title">目录</p>

:mag_right:<a href="#a1">理解对象</a><br>
:mag_right:<a href="#a2">创建对象</a><br>
:mag_right:<a href="#a3">继承</a><br>

<p id="a1"></p>

## :unlock:1.1理解对象
<a href="#title">:bulb:返回目录</a><br>
创建对象最简单的方法就是创建一个Object的实例,然后再为它添加属性和方法
```JavaScript
var person = new Object();
person.name="guixu";
person.age=18;
person.job="fasdafs";
person.sayHello=function(){
    alert("hello,world");
}
```
这是早期的方法,几年后对象字面量成为创建这种方式的首选模式,改进如下:
```JavaScript
var person = {
    name: "guixu",
    age: 18.
    job: "asdf",
    sayHello: function(){
        alert("hello,world");
    }
}
```
### :bomb:1.1.1属性类型
在定义只有在内部才能使用的特征(attribute)时,描述了属性(property)的各种特征,为了表示特征是内部值,将他们放在两队方括号中,例如[[Enumerable]],ECMAScript中有两种属性:数据属性和访问器属性
#### :exclamation:数据属性
  【1】[[Configurable]]:能否痛过delete删除属性而重新定义属性。默认：true。<br>
  【2】[[Enumerable]]:能否通过for-in循环返回属性。默认：true。<br>
  【3】[[Writeable]]:能否修改属性的值.默认：true。<br>
  【4】[[Value]]：属性的数据值。默认：undefined<br>
  ```JavaScript
  var person = {
       name: "guixu"
       //这里创建了一个名为name 的属性,值为"guixu",-->[[value]]特性被设置为"guixu"
  }
  ```
  object.defineProperty(属性对象，属性名字，描述符对象),可以修改属性默认的特征,描述符对象的属性必须是:configurable,enumerable,writable,value.设置其中的一或多个值
  
  ```javascript
    var person = {};
    Object.defineProperty(
      person,"name",{
        writeable: false,   //说明这个属性是只读的,不可以被修改
        value: "fff";
        }
    );
    alert(person.name); //fff
    person.name="grd";
    alert(person.name); ///fff
  ```
  
#### :exclamation:访问器属性
访问器属性不包含数据值,他们包含一对儿getter和setter函数(都不是必须的),在读取访问器属性时,会调用getter函数,返回有效的值,在写入访问器属性时,会调用setter函数并传入新值
    1.四个特性:<br>
      【1】Configerable: 能否通过delete删除属性<br>
      【2】Eunumerable: 能否通过for-in循环返回属性。<br>
      【3】Get: 在读取属性是调用的函数<br>
      【4】set： 在写入属性的调用的函数。<br>
    2.必须使用Object.defineProperty()来定义访问器属性。
  ```javascript
    var book={
      _year:2004,  //下划线是一种标志,用于表示只能通过对象方法访问的属性
      edition: 1
    };
    Object.defineProperty(book,"year",{
      get: function(){
        return this._year;
      },
      set:function(newValue){
        if(newValue>2004){
          this._year=newValue;
          this.edition+=newValue-2004;
        }
      }
    });
    book.year=2005;
    alert(book.edition);   //2
  ```
  这是使用访问器属性的常用方式,设置一个属性的值会导致其他属性的改变.<br>
  在 Object.defineProperty()方法之前,都使用两个非标准的方法:__defineGetter__()和__defineSetter__(),将上面的例子改写:
 ```JavaScript
 var book={
        _year:2004,
        edition: 1
 };
 book.__defineGetter__("year",function(){
        return this._year;
 });
 
 book.__defineSetter__("year",function(newValue){
        if(newValue>2004){
            this._year=newValue;
            this.edition+=newValue-2004;
        }
 });
 book.year=2005;
 alert(book.edition);   //2
 ```
### :bomb:1.1.2定义多个属性
  可通过Object.defineProperties(修改的对象，修改的属性)。,通过这个方法可以通过描述符一次定义多个属性.
```JavaScript
var book={};
Object.defineProperties(book,{
    _year: {
        writable: true,
        value: 2004
    },
    
    edition: {
        writable: true,
        value: 1
    },
    
    year: {
        get: function(){
            return this._year;
        },
        
        set: function(newValue){
            if(newValue>2004){
                this._year=newValue;
                this.edition+=newValue-2014
            }
        }
    }
});
```
这段代码定义了两个数据属性(_year和edition)和一个访问器属性(year)
### :bomb:1.1.3读取属性的特征
Object.getPropertyDescriptor(属性所在的对象，属性名称)  返回值是一个对象,如果是访问器属性.这个对象的属性有configurable,enumerable,get,set;如果是数据属性,这个对象的属性有configurable,enumerable,writable,value.
```javascript
var book={};
Object.defineProperties(book,{
    _year: {
        writable: true,
        value: 2004
    },
    
    edition: {
        writable: true,
        value: 1
    },
    
    year: {
        get: function(){
            return this._year;
        },
        
        set: function(newValue){
            if(newValue>2004){
                this._year=newValue;
                this.edition+=newValue-2014
            }
        }
    }
});

var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value);  //2004
alert(descriptor.configurable);  //false
```
<p id="a2">创建对象</p>

## :unlock:创建对象
<a href="#title">返回目录</a><br>
Object构造函数和对象字面量都可以创建单个对象,但使用同一个接口创建很多对象,会产生大量的重复代码-->所以开始使用工厂模式
### :bomb:1.1.1工厂模型
ECMAScript无法创建类,所以
  用函数来封装以特定接口创建对象。<br>
  ```javascript
    function creatperson(name,age,job){
      var o = new object();
      o.name="fff";
      o.age=age;
      o.job=job;
      o.sayname(){
        alert(this.name);
      };
      return o;
    }
  ```
但这个方法没有解决对象识别的问题(即怎么知道一个对象的类型)->从而产生了构造函数模型
### :bomb:1.1.2构造函数模式
像Object,Array这样的原生构造函数,在运行时会自动出现在执行环境中,也可以自己自定义构造函数,从而自定义对象类型的属性和方法,
  构造函数模式将上一步重写<br>
  ```javascript
    function Person(name,age,job){
      this.name=name;
      this.age=age;
      this.job=job;
      this.sayName=function(){
        alert(this.name);
      };
    }
    
    var person1=new Person("fff",22,"Software,Engineer");
    var person2=new Person("hhh",25,"Doctor");
  ```
与工厂模式相比较:<br>
+ 没有显示的创建对象
+ 直接将属性和方法赋给this对象
+ 没有return语句<br>
  创建Person实例：注意点：必须要new操作符,这种方式会经历4个步骤<br>
+ 创建一个新对象
+ 将构造函数的作用域赋给新对象(因此this指向了这个新对象)
+ 执行构造函数中的代码
+ 返回新对象<br>
  person1，person2都包含constructor（构造函数）属性。指向Person。（可通过instanceof来判定person1的类型）<br>
```JavaScript
alert(person1.constructor == Person);  //true
alert(person2.constructor == Person);  //true
```
对象的constructor属性最初是用来标识对象类型的,,但是instanceof操作符更可靠
```JavaScript
alert(person1 instanceof Object);  //true
alert(person1 instanceof Person);  //true
alert(person2 instanceof Object);  //true
alert(person2 instanceof Person);  //true
```
创建自定义的构造函数意味着可以将他的实例标识作为一种特定的类型(比工厂模式好的地方),在这个地方person1和person2都是Object的实例,是因为所有对象都继承自Object
<br>
+ 构造函数当做函数:任何函数只要通过new操作符来调用,那么他就可以作为构造函数,而不通过new来调用,于普通函数也没有区别
```JavaScript
//当做构造函数使用
var person = new Person("guixu",18,"afdfa");
person.sayName();  //"guixu"

//作为普通函数
Person("gaoju",22,"adfa");   //添加到window   因为是全局作用域
window.sayName();  //"gaoju"

//在另一个作用域调用
var o=new Object();
Person.call(o,"guixu",18,"asdf");
o.sayName();  //"guixu"
```
  :flashlight:注意点：<br>
      每个方法都要在每个实例上重新创建一遍。
      ```javascript
        alert(person1.sayName == person2.sayName);  //false
      ```
      解决办法： 将sayName()函数的定义转移到构造函数外部。<br>
      ```javascript
        function Person(name,age,job){
          this.name=name;
          this.age=age;
          this.job=job;
          this.sayName=sayName;
        }
        function sayName(){
          alert(this.name);
        }
      ```
### :bomb:1.1.3原型模式
#### :exclamation:原型
  每个函数都包含prototype(原型)属性，相当于指针，可以直接将信息添加到原形函数中。
  ```javascript
    function Person(){
    }
    Person.prototype.name="fff";
    Person.prototype.age=age;
    Person.prototype.job=job;
    Person.prototype.sayName=function(){
      alert(this.name);
    };
  ```
  如果在实例中创建该属性，该属性将会屏蔽原型中的属性
  ```javascript
    function Person(){
    }
    Person.prototype.name="fff";
    Person.prototype.age=age;
    Person.prototype.job=job;
    Person.prototype.sayName=function(){
      alert(this.name);
    };
    
    var person1=new Person;
    var person2=new Person;
    person1.name="hhh";
    alert(person1.name);  //"hhh"来自实例
    alert("person2.name");//"fff"来自原型
  ```
  #### :exclamation:原型的动态性
  尽管可以随时为原形添加属性和方法，但如果重写整个原型对象，就等于切断了构造函数与最初原型的关系。
  ```javascript
    function Person(){
    }
    var friend=new Person();
    Person.prototype={
      constructor:Person,
      name:"fff",
      age:22,
      job:"asd",
      sayName:function(){
        alert("this.name");
      }
    };
    friend.sayName();  //error
  ```
  先创造一个Person的一个实例，然后又重写了其原型对象，但调用时发生了错误，因为friend指定的原型不包含以该名字命名的属性。
### :bomb:1.1.4组合使用构造函数和原型函数
  所有共享的属性都在原型中定义
### :bomb:1.1.5动态原型模式
  将所有信息都封装在构造函数中，通过检查莫个应该存在的方法是否有效，来决定是否需要初始化原型。
  ```javascript
    function Person(name,age,job){
      this.name=name;
      this.age=age;
      this.job=job;
      if(typeof this.sayName != "function"){
        Person.prototype.sayName=function(){
          alert("this.name");
        }
      }
    }
  ```
### :bomb:1.1.6寄生构造函数模型
 ```javascript
 function Person(name,age,job){
  var o =new object();
  o.name=name;
  o.age=age;
  o.job=job;
  o.sayName(){
    alert(this.name);
  };
  return o;
 }
 ```
 <p id="#a3"></p>
 
## :unlock:1.3继承

### :bomb:1.3.1原型链

