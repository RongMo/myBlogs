---
title: ES6函数新增的特性
date: 2017-06-14 14:09:53
tags: JavaScript
---



## 带默认参数的函数

> JavaScript函数的最大的一个特点就是在传递参数的时候，参数的个数不受限制的。为了健壮性考虑，一般在函数内部需要做一些默认值的处理。

```javascript
function makeRequest(url, timeout, callback) {
    timeout = timeout || 2000;
    callback = callback || function() {};
}
```

<!--more-->

其实上面的默认值方法有个bug：当timeout是0的时候也会当做假值来处理，从而给赋值默认值2000.

> ES6从语言层面面上增加了 **默认值的** 支持。看下面的代码：

```javascript
//这个函数如果只传入第一个参数，后面两个不传入，则会使用默认值。如果后面两个也传入了参数，则不会使用默认值。
function makeRequest(url, timeout = 2000, callback = function() {}) {

    // 其余代码

}
```

## 默认参数对 arguments 对象的影响	

> 在非严格模式下，arguments总是能反映出命名参数的变化。看下面的代码：

```javascript
<script type="text/javascript">
    function foo(a, b) {
        //非严格模式
        console.log(arguments[0] === a); //true
        console.log(arguments[1] === b); //true
        a = 10;
        b = 20;
        console.log(arguments[0] === a); //true
        console.log(arguments[1] === b); //true
    }
    foo(1, 2);
</script>
```

> **在ES5的严格模式下**，arguments只反映参数的初始值，而不再反映命名参数的变化！

```Javascript
<script type="text/javascript">

    function foo(a, b) {
        //严格模式
        "use strict"
        console.log(arguments[0] === a); //true
        console.log(arguments[1] === b); //true
        a = 10;
        b = 20;
        console.log(arguments[0] === a); //false。  修改a的值不会影响到arguments[0]的值
        console.log(arguments[1] === b); //false
    }
    foo(1, 2);
</script>
```

> 当使用ES6参数默认值的时候，不管是否是在严格模式下，都和ES5的严格模式相同。看下面的代码：

```Javascript
<script type="text/javascript">

    function foo(a, b = 30) {
        console.log(arguments[0] === a); //true
        console.log(arguments[1] === b); //true
        a = 10;
        b = 20;
        console.log(arguments[0]  === a); //false。  由于b使用了默认值。虽然a没有使用默认值，但是仍然表现的和严格模式一样。
        console.log(arguments[1] === b); //false。  b使用了默认值，所以表现的和严格模式一样。
    }
    foo(1, 2);
</script>
```

> 注意：如果这样调用foo(1),则 a == 1， b == 30， arguments[0] == 1, arguments[1] == undefined。也就是说默认值并不会赋值给arguments参数。

## 默认参数表达式 (**Default Parameter Expressions**)

> 参数的默认值，也可以是一个表达式或者函数调用等。看下面的代码

```javascript
<script type="text/javascript">
    function getValue() {
        return 5;
    }

    function add(first, second = getValue()) { //表示使用getValue这个函数的返回值作为second的默认值。
        return first + second;
    }

    console.log(add(1, 1));     // 2.  调用add函数的时候，传入了第二个参数，则以传入的参数为准。
    console.log(add(1));        // 6。 调用add函数的时候，没有传入第二个参数，则会调用getValue函数。
</script>
```

> 有一点需要要注意：getValue()只会在调用add且不传入第二个参数的时候才会去调用。不是在解析阶段调用的。

```javascript
<script type="text/javascript">
    let value = 5;
    function getValue() {
        return value++;
    }

    function add(first, second = getValue()) {  //
        return first + second;
    }

    console.log(add(1, 1));     // 2
    console.log(add(1));        // 6。 
    console.log(add(1));        // 7
    console.log(add(1));        // 8
</script>
```

> 由于默认值可以表达式，所以我们甚至可以使用前面的参数作为后面参数的默认值。

```javascript
function add(first, second = first) {  // 使用第一个参数作为第二个参数的默认值
        return first + second;
 }

```

> 注意：可以把前面的参数作为后面参数的默认值，但是不能把后面的参数作为第一个参数的默认值。这可以前面说的let和const的暂存性死区一个意思。

```javascript
function add(first = second, second)) {  // 这种写法是错误的

        return first + second;
}
```

## 未命名参数问题

> Javascript并不限制传入的参数的数量。在调用函数的时候，传入的实参的个数超过形参的个数的时候，超过的部分就成为了未命名参数。在ES5之前，我们一般可以通过arguments对象来获取到未命名参数的值。但是罗显繁琐。

```javascript
<script type="text/javascript">
    function foo(a) {
        console.log(a);
        console.log(arguments[1])  //取得传入的多余的参数。
    }
    foo(2, 3);
</script>
```

> ES6，提供了一种更加优雅处理未命名参数的问题：**剩余参数**( **Rest Parameters** )
>
> 语法：function a(a, … b){ }   
>
> 剩余参数使用三个点( … )和变量名来表示。

```javascript
<script type="text/javascript">
    function foo(a, ...b) {
        console.log(a);
        console.log(b instanceof Array);  //true  .多余的参数都被放入了b中。b其实就是一个数组。
    }
    foo(2, 3, 4, 6);
</script>
```

> 注意：

1. 函数最多只能有一个剩余参数b。而且这个剩余参数必须位于参数列表的最后位置。
2. 虽然有了剩余参数，但是arguments仍然存在，但是arguments完全无视了剩余参数的存在。
3. 剩余参数是在函数声明的时候出现的。



## 函数中的扩展运算符

> 例如:Math中的max函数可以返回任意多个参数中的最大值。但是如果这些参数在一个数组中，则没有办法直接传入。以前通用的做法是使用applay方法。
>
> 看下面的代码：

```javascript
<script type="text/javascript">
    let values = [25, 50, 75, 100]	
    console.log(Math.max.apply(Math, values));  // 100
</script>
```

> 上面这种方法虽然可行，但是总是不是那么直观。
>
> 使用ES6提供的扩展运算符可以很容易的解决这个问题。在数组前加前缀 … (三个点)。

```Javascript
<script type="text/javascript">
    let values = [25, 50, 75, 100]
    console.log(Math.max(...values));  //使用扩展运算符。相当于拆解了数组了。
	console.log(Math.max(...values, 200));  //也可以使用扩展运算符和参数的混用，则这个时候就有 5 个数参与比较了。
</script>
```

> **注意：剩余参数和扩展运算符都是 使用三个点作为前缀。但是他们使用的位置是不一样的。**
>
> 1. ****剩余参数是用在函数的声明的时候的参数列表中，而且必须在参数列表的后面
> 2. 扩展运算符是用在函数调用的时候作为实参来传递的，在实参中的位置没有限制。

# 全新的函数：箭头函数（=>）

> ECMAScript 6 最有意思的部分之一就是箭头函数。正如其名，箭头函数由 “箭头”（=>）这种新的语法来定义。
>
> 其实在别的语言中早就有了这种语法结构，不过他们叫拉姆达表达式。

## 箭头函数语法

> 基本语法如下：

```javascript
(形参列表)=>{
  //函数体
}
```

---

> 箭头函数可以赋值给变量，也可以像匿名函数一样直接作为参数传递。

- 示例1：

```javascript
<script type="text/javascript">
    var sum = (num1, num2) =>{
        return num1 + num2;
    }
    console.log(sum(3, 4));
    //前面的箭头函数等同于下面的传统函数
    var add = function (num1, num2) {
        return num1 + num2;
    }
    console.log(add(2, 4))
</script>
```

---

> 如果函数体内只有一行代码，则包裹函数体的 **大括号** ({ })完全可以省略。如果有return，return关键字也可以省略。
>
> 如果函数体内有多条语句，则 {} 不能省略。

- 示例2：

```javascript
<script type="text/javascript">
    var sum = (num1, num2) => num1 + num2;
    console.log(sum(5, 4));
    //前面的箭头函数等同于下面的传统函数
    var add = function (num1, num2) {
        return num1 + num2;
    }
    console.log(add(2, 4));

	//如果这一行代码是没有返回值的，则方法的返回自也是undefined
	var foo = (num1, num2) => console.log("aaa");
	console.log(foo(3,4));  //这个地方的返回值就是undefined
</script>
```

---

> 如果箭头函数只有一个参数，则包裹参数的小括号可以省略。其余情况下都不可以省略。**当然如果不传入参数也不可以省略**

- 示例3：

```javascript
<script type="text/javascript">
    var foo = a=> a+3; //因为只有一个参数，所以()可以省略
    console.log(foo(4)); // 7
</script>
```

---

> 如果想直接返回一个js对象，而且还不想添加传统的大括号和return，则必须给整个对象添加一个**小括号 ()**

- 示例4：

```Javascript
<script type="text/javascript">
    var foo = ()=>({name:"lisi", age:30});
    console.log(foo());
	//等同于下面的；
	var foo1 = ()=>{
      	return {
          	name:"lisi",
          	age : 30
      	};
	}
</script>
```

## 使用箭头函数实现函数自执行

```Javascript
<script type="text/javascript">
    var person = (name => {
            return {
                name: name,
                age: 30
            }
        }
    )("zs");
    console.log(person);
</script>
```

## 箭头函数中无this绑定(No this Binding)

> 在ES5之前this的绑定是个比较麻烦的问题，稍不注意就达不到自己想要的效果。因为this的绑定和定义位置无关，只和调用方式有关。
>
> **在箭头函数中则没有这样的问题，在箭头函数中，this和定义时的作用域相关，不用考虑调用方式**
>
> 箭头函数没有 this 绑定，意味着 this 只能通过查找作用域链来确定。**如果箭头函数被另一个不包含箭头函数的函数囊括，那么 this 的值和该函数中的 this 相等，否则 this 的值为 window。**

```Javascript
<script type="text/javascript">
    var PageHandler = {
        id: "123456",
        init: function () {
            document.addEventListener("click",
                event => this.doSomething(event.type), false); // 在此处this的和init函数内的this相同。
        },

        doSomething: function (type) {
            console.log("Handling " + type + " for " + this.id);
        }
    };
    PageHandler.init();
</script>
```

看下面的一段代码：

```javascript
<script type="text/javascript">

    var p = {
        foo:()=>console.log(this)   //此处this为window
    }
    p.foo();  //输出为 window对象。   并不是我想要的。所以在定义对象的方法的时候应该避免使用箭头函数。
//箭头函数一般用在传递参数，或者在函数内部声明函数的时候使用。
</script>
```

> 说明：

1. 箭头函数作为一个使用完就扔的函数，不能作为构造函数使用。也就是不能使用new 的方式来使用箭头函数。
2. 由于箭头函数中的this与函数的作用域相关，所以不能使用call、apply、bind来重新绑定this。但是虽然this不能重新绑定，但是还是可以使用call和apply方法去执行箭头函数的。



## 无arguments绑定

> 虽然箭头函数没有自己的arguments对象，但是在箭头函数内部还是可以使用它外部函数的arguments对象的。

```javascript
<script type="text/javascript">
    function foo() {
        //这里的arguments是foo函数的arguments对象。箭头函数自己是没有 arguments 对象的。
        return ()=>arguments[0]; //箭头函数的返回值是foo函数的第一个参数
    }
    var arrow = foo(4, 5);
    console.log(arrow()); // 4
</script>
```
