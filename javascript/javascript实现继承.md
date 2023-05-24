javascript如何实现继承？

# 【什么是继承？】

* 子类拥有父类的***特征***和***行为***

  > * 特征==属性
  > * 行为==方法

# 【怎么做？】

````javascript
//父类
function Person(name){//给构造函数添加了参数
    this.name = name;
    this.msg = function(){
        alert(this.name);
    }
}
Person.prototype.age = 10;//给构造函数添加了原型属性
````

## 方法一：原型链继承

![](https://gitee.com/lao-jiawei/photo-gallery/raw/master/images/20230524-2.jpg)

==步骤1：创建一个子类，并把子类的原型对象指向父类的实例**（关键点）**==

````javascript
//创建子类
function Sub(){
    this.name = 'roy';
}
//子类的原型对象指向父类实例
Sub.prototype = new Person();
````

步骤2：创建子类实例，此时子类实例已经完成继承

````javascript
var sub1 = new Sub();
````

步骤3：验证是否继承成功（其实真正实现继承仅上面两步即可，这一步属于调用父类，检验是否继承成功）

````javascript
console.log(sub1.age);//10
//instanceof 判断元素是否在另一个元素的原型链上
//sub1继承了Person的属性，返回true
console.log(sub1 instanceof Person);//true
````

【分析】

* 优点
  * 实现了继承
    * √  实例的构造函数的属性（即子类的构造函数属性）
    * √  父类的构造函数的属性
    * √  父类原型的属性
* 缺点
  * 新实例无法向父类构造函数传参
    * 【原因】步骤二时，是通过子类来创建新实例，而不是通过父类创建实例
  * 继承单一
    * 【原因】如图（1）一个子类仅继承一个父类
  * 修改子类实例的属性会改到父类的属性。
    * 【原因】如图（1）

<br>

## 方法二：构造函数继承

![](https://gitee.com/lao-jiawei/photo-gallery/raw/master/images/20230524-4.jpg)

==步骤1：在子类构造函数内回调父类构造函数（关键点）==

````javascript
function Sub(){
    Person.call(this,'roy');
    this.age = 12;
}
````

步骤2：创建子类实例

````javascript
var sub1 = new Sub();
````

步骤3：检验是否继承成功

````javascript
console.log(sub1.name);//roy
console.log(sub1.age);//12
console.log(sub1 instanceof Person);//false
````

【分析】

* 优点
  * 实现了继承：
    * √  继承父类**构造函数**的属性
    * ✕ 继承父类**原型上**的属性（如图（3））
  * 可以向父类构造函数传参
* 缺点
  * 无法实现**父类**构造函数的复用
    * 【原因】用一次，重新调用父类构造函数一次
  * 每个新实例都有父类构造函数的副本，臃肿
    * 【原因】子类构造函数中产生了闭包，闭包导致内存泄露
  * 无法继承父类原型上的属性
    * 【原因】如图（3）

<br>

## 方法三：组合式继承

![](https://gitee.com/lao-jiawei/photo-gallery/raw/master/images/20230524-5.jpg)

==步骤1：在子类构造函数内回调父类构造函数**（关键点）**==

````javascript
function Sub(name){
    Person.call(this,name);//构造函数继承
}
````

==步骤2：创建一个子类，并把子类的原型对象指向父类的实例**（关键点）**==

````javascript
Sub.prototype = new Person();//原型链继承
````

步骤3：创建子类实例，检验是否继承成功

````javascript
var sub = new Sub('roy');
console.log(sub.name);//roy 继承了构造函数属性
console.log(sub.age);//10 继承了父类原型的属性
````

【分析】

* 优点：
  * 
* 缺点：

## 方法四：ES6 Class类继承

# 好文推荐

[1] [贪吃的猫,《JS继承》,稀土掘金，2021-04-29](https://juejin.cn/post/6956581082460487710)