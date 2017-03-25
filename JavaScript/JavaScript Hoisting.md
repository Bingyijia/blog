### JavaScript 中的提升（Hoisting）

---

##### 前言：

JS中在声明函数与变量的时候是存在变量提升的问题，一不注意，有的时候会导致一些bug，本文主要就是针对JS中的提升问题。JS中的提升，分为变量提升与函数提升，并且函数提升是优先于变量提升的。当然此文讨论的内容不包括ES6。

##### 提升的概念

其实针对这个概念，私以为可以简单的理解为JS解析器将函数的声明与变量的声明**提升到所在作用域的顶部**，以方便使用。注意，只是将声明提前，赋值并没有提前。

##### 实例说明
<!-- more -->
```javascript
console.log(a); // undefined
console.log(b); // function b() {alert(98);}
b(); // 98
function b() {
  console.log(98);
}

var a = 97;
```

上面的例子其实就相当于是：

```javascript
function b() {
  alert(98);
}

var a;
b(); // 98
console.log(a); // undefined
console.log(b); // function b() {alert(98);}
a = 97;
```

##### 块中的提升

在JS的块中声明函数的时候，出现的了比较有趣的现象：

```javascript
console.log(a); // undefined
console.log(b); // undefined
console.log(c); // undefined
console.log(d); // undefined

// b() //不注释掉会报错

if (true) {
  var a = 97;
  b(); // 98
  function b() {
    console.log(98);
  }
} else {
  var c = 99;
  function d() {
    console.log(100);
  }
}
console.log(a, b, c, d);
// a = 97
// b = function b() {console.log(98)}
// c = undefined
// d = undefined
```

现在这个情况就有点意思了：

因为JS是不存在块级作用域的，所以变量 a 和 c 打印出来 undefined 是很好理解的，但是 b 和 d 是什么情况呢？
为什么也是 undefined 呢？这到底是提升了还是没提升呢？

经过查阅资料，我认为这块是浏览器的一种容错机制的结果（未针对所有浏览器的不同版本进行测试，如果有错误，希望留言斧正，谢谢）。因为严格来说，在块中声明函数属于无效语法，JS引擎会尝试修正错误，将其转化为合理的状态（这点在《JavaScript高级程序设计 第三版》176页也是一笔带过）。

我认为，浏览器尝试修正的时候是将函数申明转化为函数表达式，并放在块的最顶部。所以函数的变量名进行了提升，而函数体其实只是一种折中的提升

```javascript
// 上边代码相当于是这样
var a;
var b;
var c;
var d;

if (true) {
  b = function b() {
    console.log(98);
  };
  a = 97;
  b(); // 98
} else {
  d = function () {
    console.log(100);
  }
  c = 99;
}
// 受限于水平，如有错误，欢迎留言斧正，谢谢
```

针对这种情况，强烈不建议在块中出现函数声明，建议改为函数表达式。

##### 总结

只要记住这个关键的点，其他的类似这样题目都会迎刃而解，当然搞懂这个不是为了做题，而是**为了避免一些工作中的bug**，所以推荐以后在写代码的时候，最好按照如下的格式去书写

```javascript
function fun() {
  // 声明各种变量
  // 声明函数
  // 其他语句
}
```



##### 参考链接：

[JavaScript Scoping and Hoisting](http://www.adequatelygood.com/JavaScript-Scoping-and-Hoisting.html)
[Firefox doesn’t hoist function declarations in blocks](http://statichtml.com/2011/spidermonkey-function-hoisting.html)

原创文章，版权所有，转载请注明出处。