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
  

