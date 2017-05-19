js深入理解

一切（引用类型）都是对象，对象是属性的集合

四种基本类型：undefined ，string，number，boolean         （undefined，null，string，number，boolean，object-复杂对象，前面5种为简单对象）与String,Number,
Boolean,Object,Function等不同。

其他引用对象：function，数组，对象，null，new Number（10）NaN 参与任何数值计算的结构都是NaN，而且 NaN != NaN   Infinity / Infinity = NaN 

对象里的都是属性，判断一个变量是不是对象，通过typeof

*对象的属性就是键值对。
*函数的属性不是键值对。

"定义式"
function dof(){
	alert('1');
}
"变量式"
var dof  = function(){
	alert('1');
}
dof.func = function(){
	alert(2);
}
dof.func2 = function(){
	alert(3);	
}

函数创建对象，函数也是对象，怎么理解的这句话？
var arr  = new Array();
arr[0]=10;//typeof(arr[0])  number
arr[1]='10';//typeof(arr[1]) string
arr[2]=function(){};//typeof(arr[2]) function
函数创建对象，每个函数都有prototype属性，这个prototype属性也是一个Object对象（属性的集合），这个prototype的constructor属性指向该函数本身。

**概括为： object 和 function 的内部实现就是一个字典结构，但这种字典结构却通过严谨而精巧的语法表现出了丰富的外观。正如
量子力学在一些地方用粒子来解释和处理问题，而在另一些地方却用波来解释和处理问题。

每个对象都有一个隐式属性 __proto__ ，该属性引用了创建该函数的对象prototype。这个 __proto__是对象的“隐式原型”
所以每个对象的__proto__隐形原型指向创造这个对象的函数的prototype属性
	搞明白这个js的作用域链和闭包的原理之后你再去看webkit技术内幕和深入理解jquery和angular2都会是一种进阶，当然这个过程中都要结合手写代码和
不断的脑回路更新。
	不要怀疑自己，你可以做到的比你想的更多，你需要做的只是放空自己，然后不断坚持梦的方向，那些说你不行的人都是alias.
	
	var foo = new Foo();
	foo.__proto__ ===Foo.prototype;---->Foo.prototype 的constructor指向 function Foo(),而 function Foo()的prototype指向的是 Foo.prototype
	Foo.prototype.username='jodge';
	Foo.prototype.password='pass';
	foo.username ='deage';
	alert(foo.password);
	foo.password 如果在foo的基本属性中没有，就foo.__proto__ 的指针 Foo.prototype 的 constructor指向这个函数的prototype，即function Foo() 属性 prototype 
	
对象的函数的原型链中是否包含这个属性通过的方法就是通过hasownProperty()方法。
那么foo.hasOwnProperty()方法是通过Foo()函数的__proto__查找原型链找到Object.prototype ,和java中的继承类似，但是和java的继承还是有区别的。

疑问：Object.__proto__ ===Function.prototype ,function继承了object的属性（函数），object也继承了function的属性（函数）。


【执行上下文】
1.
alert(foo1);//undefined
var foo1 = function(){
};
	
2.
alert(foo2);//function foo2(){};
function foo2(){
};

3.
alert(demo);//undefined
var demo =10;

4.
var demo =10;
alert(demo);//10

5.
alert(this);//window{top:window,window:window}
执行上下文是啥？
函数在每次被调用时候，都会产生一个新的执行上下文。

变量、函数表达式——变量声明，默认赋值为undefined；
this——赋值；
函数声明——赋值；
这三种数据的准备情况我们称之为“执行上下文”或者“执行上下文环境”。
函数内部的自由变量的作用域在函数定义的时候就已经定义了他的作用域范围。

函数中的变量的值是在函数执行过程中定义的，而函数中的变量是在定义函数的时候就声明的，具体变量的值要看哪个执行上下文被压栈，
当执行完函数的时候，如果不是js闭包，那么这个执行上下文就会被从栈中销毁，那么如何判断什么样的函数执行完后会是闭包？返回值
是个函数，或者函数作为参数传递。
var funcRet= function(){
	return function(){
		alert('1');
	}
}

闭包：在定义的函数a的内部定义的方法b，b方法引用了a函数的临时变量，那么在函数初始化后，不会销毁a函数引用的变量，就可能出现内存泄漏的情况，
***所谓的“闭包”，就是在构造函数体内定义另外的函数作为目标对象的方法函数，而这个对象的方法函数反过来引用外层外层函数体中
的临时变量。这使得只要目标对象在生存期内始终能保持其方法，就能间接保持原构造函数体当时用到的临时变量值。尽管最开始的构
造函数调用已经结束，临时变量的名称也都消失了，但在目标对象的方法内却始终能引用到该变量的值，而且该值只能通这种方法来访
问。即使再次调用相同的构造函数，但只会生成新对象和方法，新的临时变量只是对应新的值，和上次那次调用的是各自独立的。的确
很巧妙！

**原型模型：



创建对象的流程：用 var anObject = new aFunction() 形式创建对象的过程实际上可以分为三步：第一步是建立一个新对象；第
二步将该对象内置的原型对象设置为构造函数prototype 引用的那个原型对象；第三步就是将该对象作为this 参数调用构造函数，完
成成员设置等初始化工作。

##你也可以在需要的时候，自由选择用对象还是数组来解释和处理问题。只要善于把握JavaScript 的这些奇妙特性，就可以编写出很多简洁而强大的代码来。
？##函数类似于数组，但是它到底是函数还是数组呢？
如果存在量子对象论，就是波粒二相性，=== js中的对象和函数是对象也是数组。这里的数组是指字典，一种任意伸缩的名称值对的集合（List<Map<String,String)>> alist)


js的预编译能力，js是一段一段先编译好定义式的函数，再执行其他代码块。

函数function具有对象化能力，function与object超然结合


三、js创建对象的方法。

1.JSON
var company=
{
	chairman:{name:'zhaohj',age:23}, 
	manager:{name:'zhangj',age:22}
}
js的存储结构是字典，用","隔开每个项目，用":"来连接对象中每个属性与对应的属性值

其实，JSON 就是JavaScript 对象最好的序列化形式，它比XML 更简洁也更省空间。对象可以作为一个JSON 形式的字符串，在网络间自由传递和交换信息。
而当需要将这个JSON 字符串变成一个JavaScript 对象时，只需要使用eval 函数这个强大的数码转换引擎，就立即能得到一个JavaScript 内存对象。（这
里面的eval函数的作用，例如：<script> eval("var a = 1;alert(a);")</script> 执行这段代码的结果是声明一个a变量，并弹窗显示a的值）正是由于
JSON 的这种简单朴素的天生丽质，才使得她在AJAX 舞台上成为璀璨夺目的明星。

2.new
apply 与 call的异同

function Person(name){
	this.name= name;
	this.say = function(){alert('i\'m '+this.name+' of '+typeof(this))};
}

function boy(name,age){
	Person.call(this,name);//boy的this赋给Person函数，boy函数继承了Person函数
	this.age = age;
	this.sayHello = function(){alert('i\'m'+this.name);}
}
var zhoujl  = new Person('zjl');
var zhaohj = new boy('zhaohj','23');
alert(zhoujl.say == zhaohj.say);
alert(Person.constructor == Person);
alert(zhaohj.constructor == boy);

	从上面函数可以包含一个this参数，构造对象。用函数的构造函数参入this参数而创建的对象不但拥有成员数据，而且有方法数据，也就是说boy函数的say方法
是通过调Person函数的构造方法并加上boy的this参数而创建的对象，那么boy和Person的say方法是逻辑一样但方法数据和成员数据不同，zhjl.say==zhaohj.say //false 
一个方法可以达到同一个函数创造的对象共享函数的目的，也就是这些函数的代码副本中保存的都是一个指向同一个构造函数的指针。
	对象可以掩盖原型对象的属性和方法，一个构造函数原型对象也可以掩盖上层构造函数原型对象既有的属性和方法。这种掩盖
其实只是在对象自己身上创建了新的同名de 属性和方法。JavaScript 就是用这简单的掩盖机制实现了对象的“多态”性，与静态对
象语言的虚函数和重载(override)概念不谋而合。

原型的真滴
Person(name){
	this.name= name;
}

Person.Say= function(){
	alert('i\'m'+this.name);
}

function Boy(name,age){
	Person.call(this,name);
	this.age=age;
}
Boy.prototype = new Person();

Boy.prototype.Smile = function(){
	alert('hi! i\'m'+this.name +',and i\'m' + age +' years old.');
}
1.建立一个新对象。
2.将这个对象的内置的prototype设置为创建该对象的构造函数的prototype。
3.将该对象作为this参数，传入原对象，
我们已经知道，用 var anObject = new aFunction() 形式创建对象的过程实际上可以分为三步：
第一步是建立一个新对象；
第二步将该对象内置的原型对象设置为构造函数prototype 引用的那个原型对象；
第三步就是将该对象作为this 参数调用构造函数，完成成员设置等初始化工作。
对象建立之后，对象上的任何访问和操作都只与对象自身及其原型链上的那串对象有关，与构造函数再扯不
上关系了。换句话说，构造函数只是在创建对象时起到介绍原型对象和初始化对象两个作用。