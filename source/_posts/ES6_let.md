---
title: ES6块级作用域
date: 2017-6-14 13:51:45
tags: JavaScript
---

## 块级作用域绑定

> 在ES5之前，不存在块级作用域，在编程的时候很多时候会带来很多的不便，ES6新增了块级作用域，补足了这方面的缺陷。

块级声明指的是该声明的变量无法被代码块外部访问。块作用域，又被称为词法作用域（lexical scopes），可以在如下的条件下创建：

- 函数内部
- 在代码块（即 {  }）内部

块级作用域是很多类C语言的工作机制，ECMAScript 6 引入块级声明的目的是增强 JavaScript 的灵活性，同时又能与其它编程语言保持一致。

<!--more-->

## let声明

> 使用let声明变量的语法和使用var声明的语法是一样的。**但是let声明的变量的作用域会限制在当前的代码块中。这是let与var的最大区别**。

```javascript
<script type="text/javascript">
    let a = 10;
    if(a > 5){
        console.log(b); //用let声明的变量没有声明提前这一特性，所以此处也访问不到（报错）
        let b = 20;
        console.log(b);
    }
    console.log(b); //由于b是在if块中使用let声明的，所以此处无法访问到。（报错）
</script>
```

> 注意：

1. 用 let 声明的变量具有块级作用域，只能在声明的块中访问，在块外面无法访问
2. 用let声明的变量也没有声明提前这一特性。
3. 在同一个块中，let声明的变量也不能重复声明。
4. 在声明变量的时候尽量使用let，慢慢的抛弃var



## const声明(Constant Declarations)

> 在  ES6 使用const来声明的变量称之为常量。这意味着它们不能再次被赋值。由于这个原因，所有的 const 声明的变量都必须在声明处初始化。const声明的常量和let变量一样也是具有块级作用域的特性。

```javascript
<script type="text/javascript">
    var a = 20;
    if (true) {
        const b = 20;
        b = 30;  //错误! 常量不能重新赋值
        const c; //错误！ 常量声明的同时必须赋值。
    }
</script>
```

> 注意：

1. const的特性除了声明的是常量为，其他与let一样。
2. 在let和const声明前的这段区域称之为暂存性死区（**The Temporal Dead Zone** —TDZ)。
3. 使用let和const声明的变量和常量不再是window的属性。  也就是说通过window.a是无法访问到的。



## 循环中的块级绑定

> 使用var声明的循环变量在循环结束后仍然可以访问到。   使用let声明的循环变量，在循环结束之后会立即销毁。

```JavaScript
<script type="text/javascript">
    for(let i = 0; i < 3; i++){ // 循环结束之后会立即销毁 i
        console.log(i);
    }
    console.log(i);  //此处无法访问到 i 。
</script>
```

## 循环中的函数

> 看下面的代码，是输出10个10，而不是0，1，2，...

```javascript
<script type="text/javascript">
    var funcs = [];
    for (var i = 0; i < 10; i++) {
        funcs.push(function () {
            console.log(i);
        });
    }
    funcs.forEach(function (func) {
        func();     // 输出 "10" 共10次
    });
</script>
```

> 解决办法需要使用函数的自执行特性。

```javascript
var funcs = [];
for (var i = 0; i < 10; i++) {
    funcs.push((function(value) {
        return function() {
            console.log(value);
        }
    }(i)));
}
funcs.forEach(function(func) {
    func();     // 输出 0，1，2 ... 9
});
```

**如果使用let声明变量，则完全可以避免前面的问题。 这是ES6规范中专门定义的特性。在for … in和for ... of循环中也适用**

```Javascript
<script type="text/javascript">
    var funcs = [];
    for (let i = 0; i < 10; i++) {
        funcs.push(function () {
            console.log(i);
        });
    }
    funcs.forEach(function (func) {
        func();     // 输出 0，1，2 ... 9
    })
</script>
```

> 说明：

1. let 声明使得每次迭代都会创建一个变量 i，所以循环内部创建的函数会获得各自的变量 i 的拷贝。每份拷贝都会在每次迭代的开始被创建并被赋值。