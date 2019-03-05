# :sun_with_face:面向对象
---
<p id="title">目录</p>
:mag_right:<a href="a1">理解对象</a><br>
:mag_right:<a href="a2">创建对象</a><br>
:mag_right:<a href="a3">继承</a><br>
<p id="a1"></p>

## :unlock:1.1理解对象
<a href="title">:bulb:返回目录</a>
### :bomb:1.1.1属性类型
#### :exclamation:数据属性
  【1】Configurable:能否痛过delete删除属性而重新定义属性。默认：true。<br>
  【2】Enumerable:能否通过for-in循环返回属性。默认：true。<br>
  【3】Writeable:能否修改属性的值.默认：true。<br>
  【4】Value：属性的数据值。默认：undefined<br>
  object.defineProperty(属性对象，属性名字，描述符对象)
  
  ```javascript
    var person = {};
    Object.defineProperty(
      person,"name",{
        writeable: false,
        value: "fff";
        }
    );
    alert(person.name); //fff
    person.name="grd";
    alert(person.name); ///fff
  ```
  
#### :exclamation:访问器属性
    1.四个特性:<br>
      【1】Configerable: 能否通过delete删除属性<br>
      【2】Eunumerable: 能否通过for-in循环返回属性。<br>
      【3】Get: 在读取属性是调用的函数<br>
      【4】set： 在写入属性的调用的函数。<br>
    2.必须使用Object.defineProperty()来定义访问器属性。
  ```javascript
    var book={
      _year:2004,
      edition: 1
    };
    Object.defineProperty(book,"year",{
      get: function(){
        return this._year;
      },
      set;function(newValue){
        if(newValue>2004){
          this._year=newValue;
          this.edition+=newValue-2014;
        }
      }
    });
    book._year=2005;
    alert(book.edition);
  ```
### :bomb:1.1.2定义多个属性
  可通过Object.defineProperties(修改的对象，修改的属性)。
### :bomb:1.1.3读取属性的特征
  Object.getPropertyDescriptor(属性所在的对象，属性名称)。
<p id="a2">创建对象</p>

## :unlock:创建对象
<a href="title">返回目录</a>
### :bomb:1.1.1工厂模型
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

### :bomb:1.1.2构造函数模式
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
  创建Person实例：注意点：必须要new操作符<br>
  person1，person2都包含constructor（构造函数）属性。指向Person。（可通过instanceof来判定person1的类型）<br>
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
  原型的动态性<br>
  尽管可以随时为原形添加属性和方法，但如果重写整个原型对象，就等于切断了构造函数与最初原型的关系。
  ```javascript
  
  ```
### :bomb:1.1.4
### :bomb:1.1.5
### :bomb:1.1.6
