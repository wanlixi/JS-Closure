# JS-Closure
深入了解js中的闭包

<br>
##定义：
<br>当内部函数 在定义它的作用域 的外部 被引用时,就创建了该内部函数的闭包 ,如果内部函数引用了位于外部函数的变量,当外部函数调用完毕后,这些变量在内存不会被 释放,因为闭包需要它们.
<br>
##闭包的两大好处：
<br>1.可以读取到其他函数内部的变量
<br>2.可以将变量保存在内存中
<br>
让我们说的更透彻一些。所谓“闭包”，就是在构造函数体内定义另外的函数作为目标对象的方法函数，而这个对象的方法函数反过来引用外层函数体中的临时变量。这使得只要目标 对象在生存期内始终能保持其方法，就能间接保持原构造函数体当时用到的临时变量值。尽管最开始的构造函数调用已经结束，临时变量的名称也都消失了，但在目 标对象的方法内却始终能引用到该变量的值，而且该值只能通这种方法来访问。即使再次调用相同的构造函数，但只会生成新对象和方法，新的临时变量只是对应新 的值，和上次那次调用的是各自独立的。
<br><br><br>
For example:
```function a() { 
 var i = 0; 
 function b() { alert(++i); } 
 return b;
}
var c = a();
c();```

<br>这段代码有两个特点：
<br>1、函数b嵌套在函数a内部；
<br>2、函数a返回函数b。
<br>引用关系如图：
![](http://files.jb51.net/upload/201007/20100703001016918.jpg)
 <br>这样在执行完var c=a()后，变量c实际上是指向了函数b，再执行c()后就会弹出一个窗口显示i的值(第一次为1)。这段代码其实就创建了一个闭包，为什么？因为函数a外的变量c引用了函数a内的函数b，就是说：
　　<br>当函数a的内部函数b被函数a外的一个变量引用的时候，就创建了一个闭包。
　　<br>让我们说的更透彻一些。所谓“闭包”，就是在构造函数体内定义另外的函数作为目标对象的方法函数，而这个对象的方法函数反过来引用外层函数体中的临时变量。这使得只要目标 对象在生存期内始终能保持其方法，就能间接保持原构造函数体当时用到的临时变量值。尽管最开始的构造函数调用已经结束，临时变量的名称也都消失了，但在目 标对象的方法内却始终能引用到该变量的值，而且该值只能通这种方法来访问。即使再次调用相同的构造函数，但只会生成新对象和方法，新的临时变量只是对应新 的值，和上次那次调用的是各自独立的。
  
##闭包有什么作用
<br>简而言之，闭包的作用就是在a执行完并返回后，闭包使得Javascript的垃圾回收机制GC不会收回a所占用的资源，因为a的内部函数b的执行需要依赖a中的变量。这是对闭包作用的非常直白的描述，不专业也不严谨，但大概意思就是这样，理解闭包需要循序渐进的过程。
在上面的例子中，由于闭包的存在使得函数a返回后，a中的i始终存在，这样每次执行c()，i都是自加1后alert出i的值。
<br>那 么我们来想象另一种情况，如果a返回的不是函数b，情况就完全不同了。因为a执行完后，b没有被返回给a的外界，只是被a所引用，而此时a也只会被b引 用，因此函数a和b互相引用但又不被外界打扰(被外界引用)，函数a和b就会被GC回收。
  
##闭包的应用场景
<br>保护函数内的变量安全。以最开始的例子为例，函数a中i只有函数b才能访问，而无法通过其他途径访问到，因此保护了i的安全性。
在内存中维持一个变量。依然如前例，由于闭包，函数a中i的一直存在于内存中，因此每次执行c()，都会给i自加1。
通过保护变量的安全实现JS私有属性和私有方法（不能被外部访问）
私有属性和方法在Constructor外是无法被访问的
<br>function Constructor(...) {  
<br>  var that = this;  
<br>  var membername = value; 
<br>  function membername(...) {...}
<br>}
<br>以上3点是闭包最基本的应用场景，很多经典案例都源于此。

##Javascript的垃圾回收机制
<br>在Javascript中，如果一个对象不再被引用，那么这个对象就会被GC回收。如果两个对象互相引用，而不再被第3者所引用，那么这两个互相引用的对象也会被回收。因为函数a被b引用，b又被a外的c引用，这就是为什么函数a执行后不会被回收的原因。
