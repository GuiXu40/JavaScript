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
+ 构造函数的问题:每个方法都要在每个实例上重新创建一遍
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
这样:person1和person2对象就共享了全局作用域中定义的同一个sayName()函数,但这样自定义的引用类型就没有封装性了-->解决方法:原型模式
      
### :bomb:1.1.3原型模式
  每个函数都包含prototype(原型)属性，相当于指针，指向一个对象,可以直接将信息添加到原形函数中。.prototype的用途:包含可以有特定类型的所有实例共享的属性和方法(意思就是:prototype就是调用构造函数而创建的那个对象实例的原型对象),好处就是:让所有对象实例共享他所包含的属性和方法-->将属性和方法添加到原型对象中
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
+ 理解原型对象:
只要创建了一个新函数,就会根据一组特定的规则为该函数创建一个prototype属性,这个属性指向函数的原型函数.在默认情况下,所有原型函数都会自动获得一个constructor(构造函数)属性,这个属性是一个指向prototype属性所在函数的指针.<br>
创建了自定义的构造函数之后,其原型对象默认只会取得constructor属性,其他方法,都是从Object继承而来.当调用 构造函数创建一个新实例之后,该实例的内部将包含一个指针(内部属性),指向构造函数的原型函数[[prototype]].
<br>
   + 虽然所有现实中都无法访问到[[prototype]],但可以通过isPrototypeOf()方法来确定对象之间是否存在这种关系
   ```JavaScript
   alert(Person.prototype.isPrototypeOf(person1));  //true
   alert(Person.prototype.isPrototypeOf(person2));  //true
   ```
   因为他们内部都有一个指向Person.prototype的指针<br>
   + Object.getPrototypeOf(),这个方法返回[[prototype]]的值
   ```JavaScript
   alert(Object.getPrototypeOf(person1)==Person.prototype);  //true
   alert(Object.getPrototypeOf(person1).name);  //"guixu"
   ```
   当代码读取到某个对象的某个属性时,都会执行一次搜索,首先从对象实例本身开始,找到了,则返回该属性的值,如果没有,则继续搜索指针指向的原型对象
<br>
虽然可以通过对象实例访问保存在原型中的值,单却不可以通过对象实例重写原型对象中的值,
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
但是,通过delete操作符则可以完全删除实例属性
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
    
    delete person1.name;
    alert(person1.name);  //"fff"  来自原型
  ```
  + 使用hasOwnPrototype()可以检测一个属性是否存在于实例中,还是在原型中
  ```JavaScript
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
    
    alert(person1.hasOwnPrototype("name"));  //false
    
    person1.name="guixu";
    alert(person1.name);  //"guixu" 来自实例
     alert(person1.hasOwnPrototype("name"));  //true
  ```
  + 原型与in操作符:有两种方式使用in操作符:单独使用和for-in循环使用.在单独使用时,in操作符会在通过对象能够访问给定属性时返回true(无论该属性存在于实例或原型中)
  ```JavaScript
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
    
    alert(name in person1);  //true
  ```
  在使用for-in循环时,返回的事能够通过对象访问的,可枚举的(enumerated)属性,屏蔽了原型中不可枚举的属性<br>
  + Object.keys():可以取得所有可枚举的实例属性,接收一个对象为参数,返回一个包含所有可枚举属性的字符串数组
  ```JavaScript
    function Person(){
    }
    Person.prototype.name="fff";
    Person.prototype.age=age;
    Person.prototype.job=job;
    Person.prototype.sayName=function(){
      alert(this.name);
    };
    
    var key=Object.keys(Person.prototype);
    alert(keys);   //"name,age,job,sayName"
    
    var p1=new Person();
    p1.name="gaoju";
    p1.age=18;
    var p1keys=Object.keys(p1);
    alert(p1keys);  //"name,age"
  ```
  + Object.getOwnPropertyNames():得到所有实例属性,无论是否可以枚举
  + 更简单的原型语法:用一个包含所有属性和方法的对象字面量来重写整个原型对象
  ```JavaScript
  function Person(){
        Person.prototype={
            name: "guixu",
            age:18,
            job:"afds",
            sayName: function(){
                alert(this.name);
            }
        }
  }
  ```
  此方法的弊端:constructor属性不在指向Person了,现在指向新的constructor对象(Object函数),尽管instanceof操作符还能返回正确的结果.如果constructor的值很重要,可以先这样:
  ```JavaScript
  function Person(){
        person.prototype={
            constructor:Person,
            name:"guixu",
            age:18
        }
  }
  ```
  但这种方式会导致[[Enumerable]]特性为true
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
  <br>
  + 原生对象的原型,原生对象的引用类型,也是这种方法创建的,通过原生对象的原型,不仅可以取得默认方法的引用,还可以自己定义新方法.例如:
  ```JavaScript
  string.prototype.startsWith=function (text){
        return this.indexOf(text)==0;
  }
  
  var msg="hello world";
  alert(msg.startsWith("hello")); //true
  ```
  + 原型对象的问题:实例共享,没有私有属性
  ```JavaScript
    function Person(){
        Person.prototype={
            constructor:Person,
            name: "guixu",
            age:18,
            job:"afds",
            friends:["hhh","jjj"],
            sayName: function(){
                alert(this.name);
            }
        }
  }
  
  var p1=new Person();
  var p2=new person();
  
  p1.friends.push("kkk");
  
  alert(p1.friends);  //"hhh,jjj,kkk"
  alert(p2.friends);  //"hhh,jjj,kkk"
  ```
### :bomb:1.1.4组合使用构造函数和原型函数
  所有共享的属性都在原型中定义,从而解决原型对象的问题,改进:
```JavaScript
function Person(name,age,job){
    this.name=name;;
    this.age=age;
    this.job=job;
    this.friends = ["hhh","jjj"];
}

Person.prototype={
    constructor: Person,
    sayName:function(){
        alert(this.name);
    }
}

var p1=new Person("guixu",18,"fasd");
var p2=new Person("gaoju",22,"fasdasd");

p1.frends.push("kkk");
  alert(p1.friends);  //"hhh,jjj,kkk"
  alert(p2.friends);  //"hhh,jjj"
```
### :bomb:1.1.5动态原型模式
  将所有信息都封装在构造函数中，通过检查莫个应该存在的方法是否有效，来决定是否需要初始化原型。
  ```javascript
    function Person(name,age,job){
      this.name=name;
      this.age=age;
      this.job=job;
      //下面的代码只会在初次调用构造函数时才会执行.此后,原型已经完成初始化,不需要在做什么修改
      if(typeof this.sayName != "function"){
        Person.prototype.sayName=function(){
          alert("this.name");
        }
      }
    }
  ```
不能使用对象字面量重写原型(会切断现有实例与新原型之间的联系)
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
 不能用instanceof来确定对象类型<br>
 + 稳妥构建函数模型,和寄生差不多
<p id="a3"></p>
 
## :unlock:1.3继承
<a href="#title">:bulb:返回目录</a><br>
许多语言都支持两种继承方式:接口继承和实现继承,接口继承只继承方法签名,而实现继承则继承实际的方法.ECMAScript只支持实现继承,主要是依靠原型链来实现的
#### :bomb:1.3.1原型链
基本思路:利用原型让一个引用类型继承另一个引用类型的属性和方法,基本模式如下:
```JavaScript
function SuperType(){
    this.property = true;
}

SuperType.prototype.getSuperValue = function(){
    return this.property;
};

function SubType(){
    this.subproperty=false;
}

//继承了SuperType
SubType.prototype=new SuperType();

SubType.prototype.getSubValue=function(){
    return this.subproperty;
};

var instance = new SubType();
alert(instance.getSuperValue());  // true
```
instance指向SubType的原型,SubType的原型又指向SuperType的原型,要注意instance.constructor现在指向的是SuperType<br>
+ 别忘记了默认的原型,所有的引用类型都继承了Object,所有函数的默认原型都是Object的实例,因此默认原型都会包含一个内部指针,指向Object.prototype
+ 确定原型和实例的关系,两种方法:使用instanceof操作符,第二种使用isPrototypeOf()方法
```JavaScript
alert(instance instanceof Object);  //true
alert(instance instanceof SuperType);  //true
alert(instance instanceof Subtype);  //true

alert(Object.prototype.isPrototypeOf(instance));  //true
alert(SuperType.prototype.isPrototypeOf(instance));  //true
alert(SubType.prototype.isPrototypeOf(instance));  //true
```
+ 谨慎的定义方法,子类型有时候需要覆盖
#### :bomb:1.3.2借用构造函数
#### :bomb:1.3.3组合继承
#### :bomb:1.3.4原型式继承
#### :bomb:1.3.5寄生式继承
#### :bomb:1.3.6寄生组合式继承
