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

[代码详情](https://github.com/miraclezys/Notes/blob/master/code/inherit.html)

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

核心：使用父类构造函数增强子类实例，等于将父类的实例属性复制到子类

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




## 原型式继承

```javascript
function object(o) {
  function F(){};
  F.prototype = o;
  return new F();
}

var animal = new Animal();
var cat = object(animal);	// 核心
cat.color = 'black';
```

原型式继承必须有一个对象作为另一个对象的基础。如果有这么一个对象，可以把它传递给object()函数，然后再根据具体需求对得到的对象加以修改。

可以使用Object.create()替代上面的object()函数：

```javascript
var animal = new Animal();
var cat = Object.create(animal);	// 核心
cat.color = 'black';
```



## 寄生式继承

```javascript
function createAnother(original) {
  var clone = object(original);
  clone.sayHi = function() {
    console.log('hi');
  };
  return clone;
}
```

寄生式继承是与原型式继承紧密相连的一种思路。寄生式继承是创建一个仅用于封装继承过程的函数，该函数内部以某种方式来增强对象，最后再像真地使它做了所有工作一样返回对象。



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



# 作用域链

每个函数都有自己的执行环境，当执行流进入一个函数时，函数的环境就会被推入一个环境栈中。代码执行完后，栈将其环境弹出，把控制权返回之前的执行环境。

当代码在一个环境执行时，就会创建一个与之关联的作用域链。作用域链的前端，是当前执行的代码所在环境的变量对象（环境中定义的所有变量和函数）。如果当前的环境是一个函数，那么作用域链的前端就是定义这个函数参数和局部变量的对象，作用域链中下一个变量对象来自包含（外部）环境，而再下一个变量对象则来自下一个包含环境，以此类推，最后是全局执行环境的变量对象。

所以在执行环境中查找标识符（变量）时，会先从作用域链的顶端查找，接着逐渐往上级查询，直到查找到为止。

> 执行环境：定义了变量或函数有权访问的数据
>
> 作用域链：指向变量对象的指针列表



# 闭包

闭包是指有权访问另一个函数作用域的变量的函数。

```javascript
function add(a) {
  return function(b) {
    return a + b;
  }
}
```

全局环境的变量对象始终存在，而add()函数这样的局部环境的变量对象，只有在函数执行时才存在。在创建add()函数时，会创建一个预先包含全局变量对象的作用域链，这个作用域链保存在内部的([Scope])属性中。当调用add()函数时，会为函数创建一个执行环境，然后通过赋值函数的([Scope])属性中的对象构建起执行环境的作用域链。接着会有一个活动对象被创建并推入作用域链的前端。所以对于add()函数而言，作用域链包含两个变量对象：本地活动对象和全局变量对象。

add()函数的内部函数的作用域链因为包含add()函数的作用域，所以能够访问add()函数的变量。

> 由于闭包会携带包含它的函数的作用域，所以会比其他函数占用更多的内存，谨慎使用闭包。

## this对象

每个函数在调用时会自动取得两个特殊的变量：this和arguments。内部函数在搜索这两个变量时，只会搜索到活动对象为止，不能直接访问这两个变量。所以内部函数的this通常为window。如果想使用外部函数的这两个变量，可以先使用一个变量保存下来即可。

## 内存泄漏

```javascript
var a = add('Amy');
var result = a('Bob');
```

当add()函数执行完毕后，它的活动对象不会被销毁，因为其内部函数的作用域链仍然在引用这个活动对象。换句话说，add()函数返回后，其执行环境的作用域链会被销毁，但它的活动对象仍然在内存中，直到内部函数被销毁，add()函数的活动对象才能被销毁。

```javascript
var a = add('Amy');
var result = a('Bob');

// 解除对内部函数的引用（以便释放内存）
a = null;
```

通过将a设置为null，解除该函数的引用，等于通知垃圾回收例程将其清除。随着匿名函数的作用域链被销毁，其他的作用域链（除了全局函数）也会被安全的销毁。

## 应用

闭包通常用来创建内部变量，使得这些变量不能被外部随意修改，同时又可以通过指定的函数接口来操作。 

* 可以读取函数内部的变量
* 让这些变量的值始终保持在内存中

```javascript
function foo() {
	var b = 1;
	return function() {
		b += 1;
		return b;
    }
}

var a = foo();
a();	// 2
a();	// 3;
```



# 原型链

基本思想：利用原型让一个引用类型继承另一个引用类型的属性和方法。

每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个指向原型对象的内部指针。假如让原型对象等于另一个类型的实例，那么此时的原型对象将包含一个指向另一个原型的指针，假如另一个原型又是另一个类型的实例，一次类推，就构成了原型链。



# MV*

> [界面之下：还原真实的MV*模式——戴嘉华](https://github.com/livoras/blog/issues/11)

## MVM

- 视图（View）：用户界面。
- 控制器（Controller）：业务逻辑
- 模型（Model）：数据保存

![](https://raw.githubusercontent.com/miraclezys/Notes/master/image/mvm01.png)

用户的对View操作以后，View捕获到这个操作，会把处理的权利交移给Controller（Pass calls）；Controller会对来自View数据进行预处理、决定调用哪个Model的接口；然后由Model执行相关的业务逻辑；当Model变更了以后，会通过观察者模式（Observer Pattern）通知View；View通过观察者模式收到Model变更的消息以后，会向Model请求最新的数据，然后重新更新界面。

看似没有什么特别的地方，但是由几个需要特别关注的关键点：

1. View是把控制权交移给Controller，Controller执行应用程序相关的应用逻辑（对来自View数据进行预处理、决定调用哪个Model的接口等等）。
2. Controller操作Model，Model执行业务逻辑对数据进行处理。但不会直接操作View，可以说它是对View无知的。
3. View和Model的同步消息是通过**观察者模式**进行，而同步操作是由View自己请求Model的数据然后对视图进行更新。

### MVC的优缺点

优点：

1. 把业务逻辑和展示逻辑分离，模块化程度高。且当应用逻辑需要变更的时候，不需要变更业务逻辑和展示逻辑，只需要Controller换成另外一个Controller就行了（Swappable Controller）。
2. 观察者模式可以做到多视图同时更新。

缺点：

1. Controller测试困难。因为视图同步操作是由View自己执行，而View只能在有UI的环境下运行。在没有UI环境下对Controller进行单元测试的时候，应用逻辑正确性是无法验证的：Model更新的时候，无法对View的更新操作进行断言。
2. View无法组件化。View是强依赖特定的Model的，如果需要把这个View抽出来作为一个另外一个应用程序可复用的组件就困难了。因为不同程序的的Domain Model是不一样的



## MVP

![](https://raw.githubusercontent.com/miraclezys/Notes/master/image/mvp01.png)

和MVC模式一样，用户对View的操作都会从View交移给Presenter。Presenter会执行相应的应用程序逻辑，并且对Model进行相应的操作；而这时候Model执行完业务逻辑以后，也是通过观察者模式把自己变更的消息传递出去，但是是传给Presenter而不是View。Presenter获取到Model变更的消息以后，**通过View提供的接口更新界面**。

关键点：

1. View不再负责同步的逻辑，而是由Presenter负责。Presenter中既有应用程序逻辑也有同步逻辑。
2. View需要提供操作界面的接口给Presenter进行调用。（关键）

对比在MVC中，Controller是不能操作View的，View也没有提供相应的接口；而在MVP当中，Presenter可以操作View，View需要提供一组对界面操作的接口给Presenter进行调用；Model仍然通过事件广播自己的变更，但由Presenter监听而不是View。

### MVP（Passive View）的优缺点

优点：

1. 便于测试。Presenter对View是通过接口进行，在对Presenter进行不依赖UI环境的单元测试的时候。可以通过Mock一个View对象，这个对象只需要实现了View的接口即可。然后依赖注入到Presenter中，单元测试的时候就可以完整的测试Presenter应用逻辑的正确性。这里根据上面的例子给出了Presenter的[单元测试样例](https://github.com/livoras/MVW-demos/tree/master/test/mvp)。
2. View可以进行组件化。在MVP当中，View不依赖Model。这样就可以让View从特定的业务场景中脱离出来，可以说View可以做到对业务完全无知。它只需要提供一系列接口提供给上层操作。这样就可以做到高度可复用的View组件。

缺点：

1. Presenter中除了应用逻辑以外，还有大量的View->Model，Model->View的手动同步逻辑，造成Presenter比较笨重，维护起来会比较困难。

## MVVM

MVVM代表的是Model-View-ViewModel，这里需要解释一下什么是ViewModel。ViewModel的含义就是 "Model of View"，视图的模型。它的含义包含了领域模型（Domain Model）和视图的状态（State）。 在图形界面应用程序当中，界面所提供的信息可能不仅仅包含应用程序的领域模型。还可能包含一些领域模型不包含的视图状态，例如电子表格程序上需要显示当前排序的状态是顺序的还是逆序的，而这是Domain Model所不包含的，但也是需要显示的信息。

可以简单把ViewModel理解为页面上所显示内容的数据抽象，和Domain Model不一样，ViewModel更适合用来描述View。

![](https://raw.githubusercontent.com/miraclezys/Notes/master/image/mvvm01.png)

MVVM的调用关系和MVP一样。但是，在ViewModel当中会有一个叫Binder，或者是Data-binding engine的东西。以前全部由Presenter负责的View和Model之间数据同步操作交由给Binder处理。你只需要在View的模版语法当中，指令式地声明View上的显示的内容是和Model的哪一块数据绑定的。当ViewModel对进行Model更新的时候，Binder会自动把数据更新到View上去，当用户对View进行操作（例如表单输入），Binder也会自动把数据更新到Model上去。这种方式称为：Two-way data-binding，双向数据绑定。可以简单而不恰当地理解为一个模版引擎，但是会根据数据变更实时渲染。

也就是说，MVVM把View和Model的同步逻辑自动化了。以前Presenter负责的View和Model同步不再手动地进行操作，而是交由框架所提供的Binder进行负责。只需要告诉Binder，View显示的数据对应的是Model哪一部分即可。

### MVVM的优缺点

优点：

1. 提高可维护性。解决了MVP大量的手动View和Model同步的问题，提供双向绑定机制。提高了代码的可维护性。
2. 简化测试。因为同步逻辑是交由Binder做的，View跟着Model同时变更，所以只需要保证Model的正确性，View就正确。大大减少了对View同步更新的测试。

缺点：

1. 过于简单的图形界面不适用，或说牛刀杀鸡。
2. 对于大型的图形应用程序，视图状态较多，ViewModel的构建和维护的成本都会比较高。
3. 数据绑定的声明是指令式地写在View的模版当中的，这些内容是没办法去打断点debug的。



# 同源策略





# ES6





