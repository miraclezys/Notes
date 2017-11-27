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