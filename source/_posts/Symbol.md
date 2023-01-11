---
title: Symbol
date: 2023-01-11 15:12:51
tags:
---
### [](#Symbol "Symbol")Symbol

> 概念  
> ES5的对象属性名都是字符串，这容易造成属性名的冲突。比如，你使用了一个他人提供的对象，但又想为这个对象添加新的方法（mixin模式），新方法的名字就有可能与现有方法产生冲突。如果有一种机制，保证每个属性的名字都是独一无二的就好了，这样就从根本上防止属性名的冲突。这就是ES6引入Symbol的原因。

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br></pre></td><td class="code"><pre><span class="line">eg:var a = { name: 'lucy'};</span><br><span class="line"></span><br><span class="line">a.name = 'lili';</span><br><span class="line">这样就会重写属性</span><br></pre></td></tr></tbody></table>

> Undefined、Null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）  
> ES6引入了一种新的原始数据类型Symbol，表示独一无二的值。

**Symbol值通过Symbol函数生成,不能用new命令，基本上，它是一种类似于字符串的数据类型。**

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br></pre></td><td class="code"><pre><span class="line">let s = Symbol();</span><br><span class="line"></span><br><span class="line">typeof s</span><br><span class="line">// "Symbol"</span><br></pre></td></tr></tbody></table>

> Symbol可以接收字符串 为参数 Symbol值可以显示转为字符串 下面提到

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br></pre></td><td class="code"><pre><span class="line">var s1 = Symbol('foo');</span><br><span class="line">s1 // Symbol(foo)</span><br><span class="line">s1.toString() // "Symbol(foo)"</span><br></pre></td></tr></tbody></table>

> Symbol 接收参数是一个对象的话 就会调用该对象的toString方法，将其转为字符串，然后才生成一个 Symbol 值。

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br></pre></td><td class="code"><pre><span class="line">const obj = {</span><br><span class="line">toString() {</span><br><span class="line">    return 'abc';</span><br><span class="line">}</span><br><span class="line">};</span><br><span class="line">const sym = Symbol(obj);</span><br><span class="line">sym // Symbol(abc)</span><br></pre></td></tr></tbody></table>

**Symbol函数是为了独一无二，相同参数的Symbol函数的返回值是不相等的**

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br></pre></td><td class="code"><pre><span class="line">// 没有参数的情况</span><br><span class="line">var s1 = Symbol();</span><br><span class="line">var s2 = Symbol();</span><br><span class="line"></span><br><span class="line">s1 === s2 // false</span><br><span class="line"></span><br><span class="line">// 有参数的情况</span><br><span class="line">var s1 = Symbol('foo');</span><br><span class="line">var s2 = Symbol('foo');</span><br><span class="line"></span><br><span class="line">s1 === s2 // false</span><br></pre></td></tr></tbody></table>

*++++++s1和s2是两个Symbol值。如果不加参数，它们在控制台的输出都是Symbol()，不利于区分。有了参数以后，就等于为它们加上了描述，输出的时候就能够分清，到底是哪一个值。*

**Symbol值不能与其他类型的值进行运算，会报错。**

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br></pre></td><td class="code"><pre><span class="line">var sym = Symbol('My Symbol');</span><br><span class="line"></span><br><span class="line">"your Symbol is " + sym</span><br><span class="line">// TypeError: can't convert Symbol to string</span><br><span class="line">`your Symbol is ${sym}`</span><br><span class="line">// TypeError: can't convert Symbol to string</span><br></pre></td></tr></tbody></table>

**Symbol值可以显式转为字符串,也可以转为布尔值**

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br></pre></td><td class="code"><pre><span class="line">var sym = Symbol('My Symbol');</span><br><span class="line"></span><br><span class="line">String(sym) // 'Symbol(My Symbol)'</span><br><span class="line">sym.toString() // 'Symbol(My Symbol)'</span><br><span class="line"></span><br><span class="line"></span><br><span class="line">var sym = Symbol();</span><br><span class="line">Boolean(sym) // true</span><br><span class="line">!sym  // false</span><br><span class="line"></span><br><span class="line">if (sym) {</span><br><span class="line">// ...</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line">Number(sym) // TypeError: Cannot convert a Symbol value to a number</span><br><span class="line">sym + 2 // TypeError: Cannot convert a Symbol value to a number</span><br></pre></td></tr></tbody></table>

* * *

#### [](#作为属性名的Symbol "作为属性名的Symbol")作为属性名的Symbol

> 由于每一个Symbol值都是不相等的，这意味着Symbol值可以作为标识符，用于对象的属性名，就能保证不会出现同名的属性。这对于一个对象由多个模块构成的情况非常有用，能防止某一个键被不小心改写或覆盖。

> 下面代码通过方括号结构和Object.defineProperty，将对象的属性名指定为一个Symbol值。

mySymbol

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br><span class="line">16</span><br></pre></td><td class="code"><pre><span class="line"></span><br><span class="line">// 第一种写法</span><br><span class="line">var a = {};</span><br><span class="line">a[mySymbol] = 'Hello!';</span><br><span class="line"></span><br><span class="line">// 第二种写法</span><br><span class="line">var a = {</span><br><span class="line">[mySymbol]: 'Hello!'</span><br><span class="line">};</span><br><span class="line"></span><br><span class="line">// 第三种写法</span><br><span class="line">var a = {};</span><br><span class="line">Object.defineProperty(a, mySymbol, { value: 'Hello!' });</span><br><span class="line"></span><br><span class="line">// 以上写法都得到同样结果</span><br><span class="line">a[mySymbol]  // "Hello!"</span><br></pre></td></tr></tbody></table>

还有一种是点运算符 赋值 先看下

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br></pre></td><td class="code"><pre><span class="line"> var mySymbol = Symbol();</span><br><span class="line"> var a = {};</span><br><span class="line"> a.mySymbol = 'Hello!';</span><br><span class="line"> console.log(a.mySymbol)</span><br><span class="line"> console.log(a[mySymbol])</span><br><span class="line"> console.log(a['mySymbol'])</span><br><span class="line">// VM642:5 Hello!</span><br><span class="line">// undefined</span><br><span class="line">// VM642:6 Hello!</span><br></pre></td></tr></tbody></table>

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br></pre></td><td class="code"><pre><span class="line">var mySymbol = Symbol();</span><br><span class="line">var a = {};</span><br><span class="line">a.mySymbol = 'Hello me!';</span><br><span class="line">a[mySymbol] = 'Hello you!';</span><br><span class="line">console.log(a.mySymbol)   //</span><br><span class="line">console.log(a[mySymbol])  //  你猜</span><br><span class="line">console.log(a['mySymbol'])  //</span><br></pre></td></tr></tbody></table>

**因为点运算符后面总是字符串，所以不会读取mySymbol作为标识名所指代的那个值，导致a的属性名实际上是一个字符串，而不是一个Symbol值。**

> **\***在对象的内部，使用Symbol值定义属性时，Symbol值必须放在方括号之中。 \[Symbol(‘你想要的’)\]：‘你想要的’ 取值一直没找到问题所在

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br></pre></td><td class="code"><pre><span class="line">let s = Symbol();</span><br><span class="line">let obj = {</span><br><span class="line">  [s](arg) { ... }</span><br><span class="line">};</span><br><span class="line"></span><br><span class="line"></span><br><span class="line">let myobj = {</span><br><span class="line">    [Symbol('name')]:'yongge'</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line">// 输出yonge</span><br></pre></td></tr></tbody></table>

#### [](#属性名的遍历 "属性名的遍历")属性名的遍历

> Symbol 作为属性名，该属性不会出现在for…in、for…of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。但是，它也不是私有属性，有一个Object.getOwnPropertySymbols方法，可以获取指定对象的所有 Symbol 属性名。

> Object.getOwnPropertySymbols方法返回一个数组，成员是当前对象的所有用作属性名的 Symbol 值。

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br></pre></td><td class="code"><pre><span class="line">var obj = {};</span><br><span class="line">var a = Symbol('a');</span><br><span class="line">var b = Symbol('b');</span><br><span class="line"></span><br><span class="line">obj[a] = 'Hello';</span><br><span class="line">obj[b] = 'World';</span><br><span class="line"></span><br><span class="line">obj // {Symbol(a): "Hello", Symbol(b): "World"}</span><br><span class="line"></span><br><span class="line">var objectSymbols = Object.getOwnPropertySymbols(obj);</span><br><span class="line"></span><br><span class="line">objectSymbols</span><br><span class="line">// [Symbol(a), Symbol(b)]</span><br></pre></td></tr></tbody></table>

> 另一个例子，Object.getOwnPropertySymbols方法与for…in循环、Object.getOwnPropertyNames方法进行对比的例子。

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br><span class="line">12</span><br><span class="line">13</span><br><span class="line">14</span><br><span class="line">15</span><br></pre></td><td class="code"><pre><span class="line">var obj = {};</span><br><span class="line"></span><br><span class="line">var foo = Symbol("foo");</span><br><span class="line"></span><br><span class="line">Object.defineProperty(obj, foo, { value: "foobar"});</span><br><span class="line"></span><br><span class="line">for (var i in obj) {</span><br><span class="line">  console.log(i); // 无输出</span><br><span class="line">}</span><br><span class="line"></span><br><span class="line">Object.getOwnPropertyNames(obj)</span><br><span class="line">// []</span><br><span class="line"></span><br><span class="line">Object.getOwnPropertySymbols(obj)</span><br><span class="line">// [Symbol(foo)]</span><br></pre></td></tr></tbody></table>

> 使用Object.getOwnPropertyNames方法得不到Symbol属性名，需要使用Object.getOwnPropertySymbols方法。另一个新的API，Reflect.ownKeys方法可以返回所有类型的键名，包括常规键名和 Symbol 键名

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br></pre></td><td class="code"><pre><span class="line">let obj = {</span><br><span class="line">  [Symbol('my_key')]: 1,</span><br><span class="line">  enum: 2,</span><br><span class="line">  nonEnum: 3</span><br><span class="line">};</span><br><span class="line"></span><br><span class="line">Reflect.ownKeys(obj)</span><br><span class="line">// [Symbol(my_key), 'enum', 'nonEnum']</span><br></pre></td></tr></tbody></table>

#### [](#Symbol-for-，Symbol-keyFor "Symbol.for()，Symbol.keyFor()")Symbol.for()，Symbol.keyFor()

> Symbol.for() 首先在全局中搜索有没有以该参数作为名称的Symbol值，如果有，就返回这个Symbol值，否则就新建并返回一个以该字符串为名称的Symbol值。和直接的Symbol就点不同了。  
> Symbol.for()与Symbol()这两种写法，都会生成新的Symbol。它们的区别是，前者会被登记在全局环境中供搜索，后者不会。Symbol.for()不会每次调用就返回一个新的 Symbol 类型的值，而是会先检查给定的key是否已经存在，如果不存在才会新建一个值。比如，如果你调用Symbol.for(“cat”)30次，每次都会返回同一个 Symbol 值，但是调用Symbol(“cat”)30次，会返回30个不同的Symbol值。

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br><span class="line">6</span><br><span class="line">7</span><br><span class="line">8</span><br><span class="line">9</span><br><span class="line">10</span><br><span class="line">11</span><br></pre></td><td class="code"><pre><span class="line">var s1 = Symbol.for('foo');</span><br><span class="line">var s2 = Symbol.for('foo');</span><br><span class="line"></span><br><span class="line">s1 === s2 // true</span><br><span class="line"></span><br><span class="line"></span><br><span class="line">Symbol.for("bar") === Symbol.for("bar")</span><br><span class="line">// true</span><br><span class="line"></span><br><span class="line">Symbol("bar") === Symbol("bar")</span><br><span class="line">// false</span><br></pre></td></tr></tbody></table>

> Symbol.keyFor方法返回一个已登记的Symbol类型值的key。实质就是检测该Symbol是否已创建

<table><tbody><tr><td class="gutter"><pre><span class="line">1</span><br><span class="line">2</span><br><span class="line">3</span><br><span class="line">4</span><br><span class="line">5</span><br></pre></td><td class="code"><pre><span class="line">var s1 = Symbol.for("foo");</span><br><span class="line">Symbol.keyFor(s1) // "foo"</span><br><span class="line"></span><br><span class="line">var s2 = Symbol("foo");</span><br><span class="line">Symbol.keyFor(s2) // undefined</span><br></pre></td></tr></tbody></table>

#### [](#内置Symbol "内置Symbol")内置Symbol

除了定义自己使用的Symbol值以外，ES6还提供了11个内置的Symbol值，指向语言内部使用的方法

　　1、Symbol.haslnstance

　　一个在执行instanceof时调用的内部方法，用于检测对象的继承信息

　　2、Symbol.isConcatSpreadable

　　一个布尔值，用于表示当传递一个集合作为Array.prototype.concat()方法的参数时，是否应该将集合内的元素规整到同一层级

　　3、Symbol.iterator

　　一个返回迭代器的方法

　　4、Symbol.match

　　一个在调用String.prototype.match()方法时调用的方法，用于比较字符串

　　5、Symbol.replace

　　一个在调用String.prototype.replace()方法时调用的方法，用于替换字符串的子串

　　6、Symbol.search

　　一个在调用String.prototype.search()方法时调用的方法，用于在字符串中定位子串

　　7、Symbol.species

　　用于创建派生类的构造函数

　　8、Symbol.split

　　一个在调用String.prototype.split()方法时调用的方法，用于分割字符串

　　9、Symbol.toprimitive

　　一个返回对象原始值的方法

　　10、Symbol.ToStringTag

　　一个在调用Object.prototype.toString()方法时使用的字符串，用于创建对象描述

　　11、Symbol.unscopables

　　一个定义了一些不可被with语句引用的对象属性名称的对象集合