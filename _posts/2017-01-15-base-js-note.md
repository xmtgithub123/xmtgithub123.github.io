---
layout: post
title: "你不知道的javascript学习笔记"
author: "MengTing Xu"
category: js
tags: [documentation,sample]
image:
  feature: cutting.jpg
  teaser: cutting-teaser.jpg
  credit:
  creditlink: ""
---

好记性不如烂笔头，又重温一变你不知道的js，为防止自己记性出现短暂性失忆，哈哈，所以记下相关基础的知识点，知识点有但内容不全，主要是自己觉得自己一看就能理解的重要点，本文章主要参考《你不知道的javascript》,感谢这本书和作者。路过的朋友也可以学习一下！

## 开始

### 目录

1. 作用域是什么
   1. [理解作用域](#理解作用域)
   2. [作用域嵌套](#作用域嵌套)
   3. [会发生的异常](#会发生的异常)
2. 词法作用域
   1. [词法阶段](#词法阶段)
   2. [欺骗词法](#欺骗词法)
 
3. 函数作用域和块作用域
   1. [函数中的作用域](#函数中的作用域)
   2. [隐藏内部实现](#隐藏内部实现)
   3. [函数作用域](#函数作用域)
      1. [匿名和具名](#匿名和具名)
      2. [立即执行函数表达示](#立即执行函数表达示)
   4. [块作用域](#块作用域)
4. [提升](#提升)
   1. [变量提升](#变量提升)
   2. [函数提升](#函数提升)
   3. [函数优先](#函数优先)
5. 关于this
   1. [调用的位置](#调用的位置)
   2. [绑定规则](#绑定规则)
      1. [默认绑定](#默认绑定)
      2. [隐式绑定](#隐式绑定)
      3. [显式绑定](#显式绑定)
      4. [new绑定](#new绑定)
   3. [优先级](#优先级)
   4. [绑定例外](#绑定例外)


### 理解作用域

对程序的处理需要这三个帮手：<br>
1.引擎：负责整个js程序的编译及执行过程<br>
2.编译器：负责语法分析及代码生成等<br>
3.作用域：负责收集并维护由所有声明的标识符组成的理询，确定肖前执行的代码对这些标识符的访问权限

<pre>var a = 2</pre>

例如碰到上以代码的执行时编译器会作如下处理：<br>

1.`var a`,编译器会查询作用域中是否已经有对该变量在于同一个作用域的集合中，如果已有了，编译器会忽略并继续工作;否则会要求作用域在当前作用域的集合中声明一个新的变量，命名为a。<br>

2.之后编译器会为引擎生成运行时所需要的代码，用来处理`a=2`的赋值操作，引擎第一步会问作用域是否在当前作用域集合中有a的变量，若有则使用这个变量，如果没有，引擎则会继续查找该变量。

3.在该例子中，引擎为变量`a` 进行LHS（赋值操作）查询。另一个查找类型 RHS（取到它的源值）

<pre>例1：console.log(a)  //可以看出对`a`的引用是一个RHS引用</pre>

<pre>例2：a = 2 ;  //可以看出对`a`的引用是一个LHS引用</pre>

<pre>例3：function foo(i) {
        console.log(i)  // 查找i并打印出来是对i的源值查找操作 此为RHS操作
    }
    foo(2)  //调用foo函数并为i赋值为2 此为LHS 操作
</pre>

### 作用域嵌套

当一个块或函数嵌套在另一个块或函数中，就会发生了作用域的嵌套。所以在当前作用域中无法找个某个变量时，引擎就会在外层嵌套的作用域中继续查找，直到找到该变量或找到最外层的作用域（全局）为止。

<pre>function foo(a) {
    console.log(a + b) //在这里对a是RHS操作，对b的RHS引用无法在foo函数内完成，这里可以在上一级作用域中完成（此例是在全局中）
}
var b = 2;
foo(1)  => 3   //调用foo()函数 对a是LHS操作</pre>

注：引擎从当前执行作用域开始查找变量，若找不到，则向上一级查找。如果找到最外层即全局作用域时，不管找到与否，过程都将停止。

如图，可作参考
<img src="http://ozc5dgoun.bkt.clouddn.com/%E4%BD%9C%E7%94%A8%E5%9F%9F.png" alt="">

### 会发生的异常

上面提到的LHS 与 RHS，因为在变量还没有声明（其他作用域或全局作用域都无法找到）的情况下，这两种的查询行为是不一样的。

<pre>
    function foo(a) {
        console.log(a + b); // a:执行RHS操作  但是对b的RHS操作无法找到变量，此为`未声明`的变量，在任何作用域下都找不到它。
        b = a;
    }
    foo(2);   // 调用foo()函数，执行LHS操作

    注：如RHS查询（在任何作用域中）找不到变量，引擎会抛出`ReferenceError`的异常。此异常是重要的异常类型。

    不成功的RHS引用会导致ReferenceError异常。不成功的LHS引用会导致自动隐式创建一个全局变量(非严格模式下)，该变量使用LHS引用的目标作为标识符，或者抛出ReferenceError异常(严格模式下)
</pre>


### 词法阶段

词法作用域是定义在词法阶段的作用域。

词法作用域是由你在写代码时将变量和块作用域写在哪里决定的，所以当词法分析器处理代码时会保持作用域不变(大部份情况下)

<pre>
例1: function foo(a) {
    var b = a*2;
    function bar(c) {
        console.log(a,b,c);
    }
    bar(b*3)
    }
    foo(2);
<img src="http://ozc5dgoun.bkt.clouddn.com/%E8%AF%8D%E6%B3%95%E4%BD%9C%E7%94%A8%E5%9F%9F1.png" alt="">
</pre>
<pre>例2：
    function foo(){
        console.log(a);
    }
    function bar(){
        var a = 2;
        foo();
    }
    var a=1;
    bar();  => 1
    //如果javascript具有动态作用域，理论上，会输出2
    <img src="http://ozc5dgoun.bkt.clouddn.com/%E8%AF%8D%E6%B3%95%E4%BD%9C%E7%94%A8%E5%9F%9F2.png" alt="" style="width:300px">
</pre>

> 词法作用域是在写代码或者说定义时确定的，而动态作用域是在运行时确定的(this也是)。词法作用域关注函数是在何处声明的，而动态作用域是关注函数从何处调用。

### 期骗词法

#### 1.eval

js中的Eval()函数可接受一个字符串为参数，并将其中内容视为好像在书写时就存在程程序中这个位置的代码。

#### 2.with

with声明实际是根据你传递给它的对象凭空创建了一个全新的词法作用域。

注：不推荐使用这两个机制


### 函数中的作用域

属于这个函数的全部变量都可以在整个函数的范围内使用和复用。

<pre>例：
    function foo(a){    //foo()的作用域包括了(b,bar(),c)
        var b = 2;
        function bar(){  //bar()的作用域包括了它自己的标识符
            ...
        }
        var c = 3;

    }
    bar(); // 错误   由于bar是foo()作用域包含的，所以不能从全局作用域中访问
    console.log(a,b,c) //错误  同理
</pre>

### 隐藏内部实现

一般来讲，都是先声明一个函数，然后在函数内添加代码;同时，可以用函数声明的方式对代码进行包装，就是把这些代码“隐藏”了的意思。

就是说，可以把变量和函数包裹在一个函数的作用域中，用这个作用域来“隐藏”它们。

<pre>例：
    function foo(a){
        b = a + bar(a *2);
        console.log(b*3);
    }
    function bar(a){
        return a-1;
    }
    var b ;
    foo(2); => 15
    //这里需要注意的是 变量b和bar()应该是foo()内部具体实现的‘私有’内容。放在全局作用域不仅没有必要，而且是‘危险’，所以更‘隐藏’的设计会更加的私有化，例如

    function foo(a){
        function bar(a) {
            return a -1 ;
        }
        var b;
        b = a + bar(a*2);
        console.log(b*3)
    }
    foo(2) => 15

    //这样一来，b和bar()都无法从外部被访问，只被foo()控制。
</pre>

### 函数作用域

先看一个例子:
<pre>
    var a = 2;
    function foo() {
        var a = 3;
        console.log(a); =>3
    }
    foo();
    console.log(a); =>2
</pre>

该例子本身没毛病，但有点不太精致，会导致一些额外的问题

1. 必须声明一个具明函数foo(),foo名子本身‘污染’了所在作用域(全局)

2. 必须是通过一个函数名foo()来调用这个函数才能运行其中代码

后来，js提供了可以解决这两个问题的办法：

<pre>
    var a = 2;
    (function foo(){
        var a = 3;
        console.log(a); => 3
    })();
    console.log(a); => 2
    
    // 自执行匿名函数
    // (function foo(){...})作为函数表达示意味着foo只能在...所代表的位置中访问，外部作用域则不可访问
    // 函数会被当作 <b style="color:red;">函数表达示</b> 而不是一个标准的函数声明来处理
</pre>

<pre>
<b>*区别函数声明 和 表达示的方法</b>
> 如果function 是声明中的第一个词 => 函数声明
> 否则就是一个函数表达示
</pre>

### 立即执行函数表达示

IIFE : 立即执行函数表达示

<pre>
    var a = 2;
    (function foo(){
        var a = 3;
        console.log(a); => 3
    })();
    console.log(a); => 2
    
</pre>

IIFE的进阶用法：把它们当作函数调用并传递参数进去
<pre>
    var a = 2;
    (function foo(global){
        var a = 3;
        console.log(a); => 3 
        console.log(global.a); = >2
    })(window);
    console.log(a);  => 2
</pre>


### 块作用域

常见的javascript代码
<pre>
    for(var i=0;i<10;i++) {
        console.log(i)
    }
    console.log(i); => 10
    //在for循环内部的上下文中使用i，
</pre>

#### let

前面的不是重点，重点是让你心里知道块作用域的用法及其内涵

ES6 改变了现状，引入`let` 关键字，是var以处的另一种变量声明方式 。

对ES6不太理解的可以去看下[ES6的官方文档](http://es6.ruanyifeng.com/)

<pre>
    {
      let a = 10;
      var b = 1;
    }
    a => ReferenceError: a is not defined
    b => 1
</pre>
<pre>
    for (let i = 0; i < 10; i++) {
      // ...
    }

    console.log(i);
    // ReferenceError: i is not defined
</pre>

<b style="color:red;">注：let 命令不存在变量提升</b>

`var` 命令会发生“变量提升”，即变量可以在声明之前使用(输出undefind)

`let` 命令则改变了语法行为，所声明的变量一定是要在声明后使用，否则报错(报错：ReferenceError)。

### const 

>除了let以外,ES6还引入了const,同样也可以用来创建作用域变量，但其值是固定的常量，之后若试图修改值都会报错。

<pre>
    const a = 3;
    a // 3

    a = 4;
    // TypeError: Assignment to constant variable.
</pre>


### 提升

引擎会在解释javascript代码之前首先对其进行编译。编译阶段中的一部份工作就是找到所有的声明，并用合适的作用域将它们关联起来。

>正确说法就是，包括变量和函数在内的所有声明都会在任何代码被执行前首先被处理。

### 变量提升

可以看个简单的例子：

<pre>var a = 1;</pre>

可能会被认为是一个声明，但在之前，javascript会将其看成两个声明 `var a;` 和 `a = 1` ，第一个定义声明是在编译阶段;第二个赋值声明 会被留在原地等待执行阶段 

所以，上面代码会以如下顺序处理：

<pre>
    var a ;
    a = 1;
    console.log(a) =>1
</pre>

### 函数提升

<pre>
    // 函数声明会被提升

    foo();
    function foo() {
        console.log(a);
        var a = 1;
    }

    //以下函数声明可以理解为下面的形式:

    function foo() {
        var a ;
        console.log(a); => undefined
        a = 1;
    }
    foo();
</pre>


> 函数声明会被提升 ，但函数表达示却不会被提升

<pre>
    foo();  // 不是ReferenceError ，而是TypeError
    var foo = function bar() {
        ...
    }
</pre>

<pre>
    foo();  // 不是ReferenceError ，而是TypeError
    bar();  // ReferenceError
    var foo = function bar() {
        ...
    }

    经过提升后的代码：

    var foo;
    foo();
    bar();
    foo = function () {
        var bar = ....
        //...
    }
</pre>


### 函数优先

> 函数声明和变量声明都会被提升，但函数声明优先于变量，函数声明会被提升到普通变量之前。

> 重复的变量声明会被忽略，只剩下赋值操作，多个函数声明可以覆盖前一个。

<pre>
例1：
    var a;
    a = 1;
    foo();
    function foo(){...}

    提升之后的形式如下：
    
    function foo(){...}
    var a ; 
    a = 1;
    foo();
</pre>

<pre>
例2：
    foo();  => 1
    var foo;
    function foo() {
        console.log(1);
    }
    foo = function() {
        console.log(2);
    };

    提升后的代码形式如下：

    function foo(){
        console.log(1);
    }
    /*var foo;*/
    foo(); => 1
    foo = function(){
        console.log(2);
    };

</pre>

var foo 虽然是出现在function foo()...的声明之前，但它是重复声明 (因此被忽略)，因为函数声明会被提升 到普通这变量之前。

<pre>
    foo(); = > 3
    function foo(){
        console.log(1)
    }
    var foo = function () {
        console.log(2)
    }
    function foo(){
        console.log(3)
    }

    提升后的代码形式如下：
    function foo(){console.log(1)}
    function foo(){console.log(3)}
    foo(); => 3
    /*var foo;*/
    foo = function() {console.log(2)}

</pre>


### 调用的位置

this关键字是js中最复杂的机制之一，被定义在所有的函数的作用域中。

当一个函数被调用时，会创建一个活动记录(也称执行上下文)。该记录会包含函数在哪里被调用(调用栈)、函数的调用方法、传入的参数等信息，this就是记录的其中一个属性，会在函数执行的过程中用到。

调用位置：函数在代码中被调用的位置(而不是声明的位置)

### 绑定规则

首先得先找到函数的调用位置，然后再进行以下四条规则的判断，以及多条规则下都可使用的情况下的优先级别是如何进行判断的。

#### 默认绑定

先看如下代码：

<pre>
    function foo() {
        console.log(this.a); => 2
    }
    var a = 2; 
    foo(); //函数调用的位置，在非严格模式下可看作为 window.foo()的调用
</pre>

在这里需要注意的是:这里的this.a最后打印出是2，为什么？因为函数在调用的时候，其实可以看作为全局调用，即window.foo()。所以，函数调用的位置可看作是全局window是调用，这可以看作为默认调用。所以foo()函数里的this就是指向全局，而this.a解析成全局变量a。

注：如果使用严格模式下，那么，this的绑定与foo()的调用位置无关。

<pre>
    functon foo(){
        "use strict";
        console.log(this.a)
    }
    var a = 2;
    foo(); //TypeError: this is undefined
</pre>

#### 隐式绑定

再看看一段代码来理解隐式绑定的意思

<pre>
    function foo() {
        console.log(this.a); =>2
    }
    var obj = {
          a: 2,
          foo:foo  
    };
    obj.foo(); // 调用的位置
</pre>

按照我自己的理解：先看函数的调用位置。再看是否是默认绑定，如果不是就看是谁调用了这个函数，在这里可以显而易见的知道，是obj这个对象调用了foo()，即调用位置会使用obj上下文来引用函数，这样隐匿绑定规则会把函数用中的this绑定到这个上下文对像，

所以调用foo()的时候this会被绑定到obj。结果就是this.a = obj.a

* 对像属性引用链中只有最后一层会影响调用位置。看例子：

<pre>
    functon foo() {
        console.log(this.a)  => 42
    }
    var obj2 = {
        a:42,
        foo:foo 
    };
    var obj1 = {
        a:2,
        obj2:obj2
    };
    obj1.obj2.foo();  // foo函数调用的位置
</pre>

这里和上一个例子不同的是foo调用的位置。这时候肯定一脸蒙蔽，这个怎么判断this是调用到哪个对像的上下文。根据“圆点”号的概念，查找最后调用的那个对象作为this的指向上下文。obj1.obj2.foo()，可以看出最后调用foo()的是obj2，所以this的上下文指向obj2，所以结果输出为 42

#### 显式绑定

可以使用函数的call() 和 apply() 方法来进行显式绑定。

老样子，看例子啦

<pre>
    function foo() {
        console.log(this.a);
    }
    var obj = {a:3};
    foo.call(obj);
</pre>

* 通过使用foo.call(obj)，简单理解为：调用foo()函数并强制将this绑定到obj对象上。

> 从this的绑定角度来讲。call() 和 apply() 的操作是一样，但区别在于 传参的的情况下，foo.apply('这里是数组')

##### 硬绑定

显示绑定仍然无法解决丢失绑定的问题

<pre>
    function foo(){
        console.log(this.a);
    }
    var obj = {
        foo.call(obj);
    };
    bar(); =>2
    setTimeout(bar,100); =>2

    //硬绑定的bar，不可能再修改它的this
    bar.call(window); => 2
</pre>

创建bar()函数，并手动foo.call(obj),强制把foo绑定到obj上。之后无论如何调用函数bar，都会手动在obj上调用foo。所以这种绑定是一种显式的强制绑定，即硬绑定

### new绑定

> Javascript中的“构造函数”只是一些使用new操作符时被调用的函数。它们并不属于某个类，也不会实例化一个类(即js中没有类的概念)。

使用new来调用函数，或者是在构造函数调用的时候，有如下操作：

1. 创建(构造)一个全新的对象 

2. 这个新对象会被执行(原型)连接

3. 新对象会绑定到函数调用的this

4. 如果函数没有返回其他对象，new表达式中的函数调用会自动返回这个新对象 

例：
<pre>
    function foo(a) {
        this.a = a;
    }
    var bar = new foo(3);
    console.log(bar.a ); => 2
</pre>

new 调用foo()时，会构造一个新对象并绑定到foo()调用中的this上。

### 优先级

<!-- new绑定 > 显式绑定 > 隐式绑定 > 默认绑定 -->

> new 和 call/apply无法一起使用

以上四种绑定规则的使用先后推断如下：
 1、函数是否在 new 中调用（new 绑定）？如果是的话 this 绑定的是新创建的对象。

`var bar = new foo(); `

2、函数是否通过 call、apply （显示绑定）或者硬绑定？如果是的话，this 绑定的是指定的对象。

` var bar = foo.call(obj2); `

3、函数是否在某个上下文对象中调用（隐式绑定）？如果是的话，this 绑定的是那个上下文对象。

` var bar = obj1.foo(); `

4、如果都不是的话，使用默认绑定。如果在严格模式下，就绑定到 undefined，否则绑定到全局对象。

` var bar = foo(); `

### 绑定例外

> 若把`null`或`undefined` 作为`this`的绑定对象传入`call`,`applh`,或`bind`,这些值在调用的时候会被忽略，实际应用的是默认绑定规则

<pre>
    function foo(){
        console.log(this.a); => 2
    }
    var a = 2;
    foo.call(null)
</pre>

什么情况下会在绑定下传入`null`？

常见的做法是使用apply(...)来“展开”一个数组，并当作参数传入一个函数。类似的，bind(...)可对参数进行柯里化（预先设置一些参数）

<pre>
    function foo(a,b) {
        console.log("a:" + a + ",b:" + b);
    }
    //把数组“展开”成参数 
    foo.apply(null,[2,3]); //a:2,b:3
    
    //使用bind(...)进行柯里化
    var bar = foo.bind(null ,2);
    bar(3);  // a:2,b:3
</pre>

这两种方法都需要传入一个参数当this的绑定对象。若函数并不关心this的话，你仍需要传入一个占位符，null会是一个不错的选择

#### 更安全的this

一种更安全的做法是传入一个特殊的对象，把this绑定到这个对象不会对你的程序产生任何副作用。
在js中创建一个空对象最简单的方法 `Object.create(null)`,`Object.create(null)`和`{}`很像，但并不会创建Object.prototype这委托，所以比`{}`更空

<pre>
    function foo(a,b) {
        console.log("a:" + a + ",b:" + b);
    }

    var & = Object.create(null);  //空对象 
    
    //把数组“展开”成参数 
    foo.apply(&,[2,3]); //a:2,b:3
    
    //使用bind(...)进行柯里化
    var bar = foo.bind(& ,2);
    bar(3);  // a:2,b:3
</pre>

> 