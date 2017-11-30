# JS基本类型

基本数据类型：Undefined, Null, Boolean, Number, String, Object

## typeof

typeof 操作符可能的返回值：

* "undefined"——如果这个值未定义
* "boolean"——如果这个值是布尔值
* "string"——如果这个值是字符串
* "number"——如果这个值是数值
* "object"——如果这个值是对象或null
* "function"——如果这个值是函数

所以可以用typeof操作符区分函数和对象。

## Undefined

在使用var声明变量但未对其初始化时，该变量的值就是undefined。

对于typeof操作符，未声明或已声明但未初始化的变量，返回的值都是"undefined"。

## Boolean

| 数据类型      | 转换为true        | 转换为false  |
| :-------- | -------------- | --------- |
| Boolean   | true           | false     |
| String    | 任何非空字符串        | ""(空字符串)  |
| Number    | 任何非零数字值(包括无穷大) | 0和NaN     |
| Object    | 任何对象           | null      |
| Undefined | -              | undefined |



## Number

* 如果数值超过了js数值范围，将会转换为Infinity(正无穷)和-Infinity(负无穷)

* 可以使用isFinity()函数判断数值是否超出范围，若在范围内，返回true

* NaN与任何值都不想等，包括它本身

* isNaN()函数会尝试将参数转换为数值，如果不能转换为数值，将返回true

  ```javascript
  isNaN(NaN)	// true
  isNaN(10)	// false
  isNaN("10")	// false，可以转为10
  isNaN("blue")	// true，不能转成数值
  isNaN(true)	// false，可以转为1
  ```

  ​

# 各类型深拷贝

```javascript
    function deepCopy(obj) {
      if(obj instanceof Date) {
        return new Date(obj.valueOf());
      }
      else if(obj instanceof RegExp) {
        return new RegExp(obj.valueOf());
      }
      else if(obj === null || obj === undefined || typeof obj != 'object') {
        return obj;
      }

      var newObj = (obj instanceof Array) ? [] : {};
      for(var k in obj) {
        if(!obj[k] != 'object') {
          newObj[k] = obj[k];
        }
        else {
          newObj[k] = deepCopy(obj[k]);
        }
      }
      return newObj;
    }
```

[代码详情](https://github.com/miraclezys/Notes/blob/master/code/deepCopy.html)



# JS继承

> [JS实现继承的几种方式](http://www.cnblogs.com/humin/p/4556820.html)

首先定义一个父类：

```javascript
function Animal(name) {
  this.name = name || 'Animal';
  this.sleep = function() {
    console.log(this.name + ' is sleeping!');
  }
}

Animal.prototype.eat = function(food) {
  console.log(this.name + ' is eating ' + food +'!');
}
```



## 原型链继承

核心：将父类的实例作为子类的原型

```javascript
// 原型链继承
function Cat() {}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;
var cat = new Cat();

cat instanceof Animal;	// true
cat instanceof Cat; // true
```

缺点：

* 创建子类实例时，无法向父类构造函数传递参数
* 要为Cat类型的原型新增属性和方法，必须要在`new Animal()`之后新建
* 来自原型对象的引用属性是所有实例所共享的（下面会解释）
* 不能多继承



## 构造继承

```javascript
function Cat(name) {
  Animal.call(this, name);
  this.name = name || 'tom';
}

cat instanceof Animal;	// false
cat instanceof Cat; // true
```

优点：

* 创建子类时，可以给父类传递参数
* 解决了原型链继承中子类共享父类引入属性的问题
* 可以实现多继承(可以call多个父类对象)

缺点：

* 实例是子类的实例，不是父类的实例，子类的实例的原型链上不存在父类
* 实例只是继承了父类的属性和方法，没有继承父类原型的属性和方法
* 每个子类都产生父类的属性和方法的副本，影响性能



## 实例继承

```javascript
function Cat(name) {
  var instance = new Animal();
  instance.name = name || 'Tom';
  return instance;
}

cat instanceof Animal;	// true
cat instanceof Cat; // false
```

优点：

* 不限制调用方式

缺点：

* 实例是父类的实例，不是子类的实例
* 不能多继承



## 拷贝继承

```javascript
function Cat(name) {
  var instance = new Animal();
  for(var key in instance) {
    Cat.prototype[key] = instance[key];
  }
  Cat.prototype.name = name || 'Tom';
}

cat instanceof Animal;	// false
cat instanceof Cat; // true
```

优点：

* 支持多继承

缺点：

* 效率低，占用内存高（需要拷贝父类的属性）
* 无法获取父类不可枚举的属性



## 组合继承

```javascript
function Cat(name) {
  Animal.call(this);
  this.name = name || 'Tom';
}

Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

cat instanceof Animal;	// true
cat instanceof Cat; // true
```

优点：

* 实例是子类的实例，也是父类的实例
* 不存在引用属性共享的问题
* 可以传参

缺点：

* 调用了两次父类构造函数（子类实例将子类原型上的相同属性覆盖了）



## 寄生组合继承

```javascript
function Cat(name) {
  Animal.call(this);
  this.name = name || 'Tom';
}

(function() {
  var Super = function(){};
  Super.prototype = Animal.prototype;
  Cat.prototype = new Super();
  Cat.prototype.constructor = Cat;
})();

cat instanceof Animal;	// true
cat instanceof Cat; // true
```

优点：完美

缺点：实现较为复杂



## 原型链继承的缺点(共享属性)



```javascript
// 父类
function Animal(name) {
  this.name = name || 'Animal';
  this.color = ['red'];
  this.sleep = function() {
    console.log(this.name + ' is sleeping!');
  }
}

Animal.prototype.eat = function(food) {
  console.log(this.name + ' is eating ' + food +'!');
}

// 原型链继承
function Cat() {}
Cat.prototype = new Animal();
Cat.prototype.constructor = Cat;

var cat = new Cat('cat');
console.log(cat.color); // red
cat.color.push('yellow');
console.log(cat.color);	// red, yellow
var cat2 = new Cat();
console.log(cat2.color);	// red, yellow
```

从上述代码可以看到，color属性被修改了。如果cat.color = ['black']，那么cat2的color属性就不会被修改。





# ES6





