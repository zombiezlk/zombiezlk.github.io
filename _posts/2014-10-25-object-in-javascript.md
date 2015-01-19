---
title: JavaScript中的Object详解
author: Nick
layout: post
category: tech
---
本文的英文原文地址是[JavaScript Objects in Detail][1]，因为觉得写的很好，便翻译成中文以方便中文开发者。

JavaScript的核心,同时也是最常用和最基础的数据类型便是Object（对象）。除了比较比较复杂的Object数据类型外，JavaScript还有五种简单的数据类型：Number，String，Boolean，Undefined，Null。注意，这些简单的（原始的）数据类型是不可变的，它们不能被改变，但Object却可以。

**什么是对象**

一个对象是一个以名值对的方式存储一些原始数据类型（有时候也有引用数据类型）的列表。列表中的每一项就是被称为属性（函数则被称为方法），每个属性的名字都必须是唯一的，类型可以是字符串或者数字。

这里是一个简单的对象：

`var myFirstObject = {firstName: "Richard", favoriteAuthor: "Conrad"};`

重申一下：思考一下一个对象是以名值对的方式存储每一个项目（属性或方法）的。在这个例子中，对象中的属性名分别为firstName和favoriteAuthor，它们的值分别为"Richard"和"Conard"。

属性名可以是一个字符串或者一个数字，当属性名是一个数字的时候，它的值必须用方括号来访问。稍后会提到方括号访问。这里是一个对象中含有数字作为属性名的例子：

<pre lang="javascript">var ageGroup = {30: "Children", 100:"Very Old"};
console.log(ageGroup.30) // 这里会抛出异常​
​// 这里展示了你应该如何拿到属性30的值"Children"
console.log(ageGroup["30"]); // Children​
​
​//最好避免使用数字作为属性名</pre>

作为一个JavaScript开发者你将会经常用到对象，大多数时候是为了存储数据还有你自己的方法和函数。

**引用数据类型和原始数据类型**

这两种数据类型的一个主要的差别就是引用数据类型的数据是作为一个引用存储起来的，它并不直接作为一个值存储在变量之中，而原始数据类型则是如此。

例如：

		// 原始数据类型字符串是作为一个值存储起来的
		var person = "Kobe"; 
		var anotherPerson = person; // anotherPerson = the value of person
		​person = "Bryant"; // person的值改变了​
		
		console.log(anotherPerson); // Kobe​
		console.log(person); // Bryant

即使我们将变量person的值改成"Bryant"，变量anotherPerson的值仍然会是"Kobe"。

对比上面原始数据类型的数据存储方式，我们来看一下引用数据类型Object是怎样存储数据的

<pre lang="javascript">var person = {name: "Kobe"};
​var anotherPerson = person;
person.name = "Bryant";
​
console.log(anotherPerson.name); // Bryant​
console.log(person.name); // Bryant</pre>

在这个例子中，我们将person对象赋值给anotherPerson对象，但是由于person中的数据是作为引用而不是作为一个确切的值存储的，当我们将person.name属性的值改为&#8221;Bryant&#8221;的时候，anotherPerson.name的属性值也改变了，因为它从来就不是真正地存储一个确切的值，它存储的只是person的属性的引用。

**对象中Data Properties（即存储数据的属性）的特性**

每个存储数据的属性不止是一个名值对，它还拥有3个特性（这3个特性默认被设置为true）：

— **Configurable Attribute**: 标志该属性是否能被删除或者更改。

— **Enumerable: Specifies** 标志该属性是否能够在一个 for/in 循环中被返回。

— **Writable**: 标志该属性是否能够被更改。

ECMAScript 5 中将属性分为accessor properties和Data Properties，accessor properties就是指由getter和setter方法定义的属性。这个会在以后的文章中提到。

**创建对象**

创建对象有两种方式：

1.	对象直接量

	最简单直接的创建对象的方式：

		// 用对象直接量创建的空对象
		var myBooks = {};// 用对象直接量创建的带有4个属性或方法的对象
		var mango = {
			color: "yellow",
			shape: "round",
			sweetness: 8,
		​	howSweetAmI: function () {console.log("Hmm Hmm Good");}
		}

2通过constructor创建对象

这是第二种常见的创建对象的方式，constructor就是一个用来初始化对象的构造函数，你要用new关键词来使用它。

<pre lang="javascript">
var mango = new Object ();

mango.color = "yellow";

mango.shape= "round";

mango.sweetness = 8;

​

mango.howSweetAmI = function () {

	console.log("Hmm Hmm Good");

}

</pre>

你可以在你的对象中使用一些保留词作为属性名字，但聪明的做法是尽量避免这样做。

对象可以放任何数据类型，包括数字，数组甚至是其他对象。


**创建对象的实用范式**

对一次性的用来存储数据的简单对象来说，上面两种方式已经足够用了。

但是想象一下如果你有一个用来展示水果还有它们的详细信息的应用，每个水果都有颜色，形状，甜度，价钱等属性，还有一个showName方法。那每一次你想要创建一种新的水果就必须不厌其烦地创建这些属性和方法。

<pre lang="javascript">
var mangoFruit = {

	color: "yellow",

	sweetness: 8,

	fruitName: "Mango",

	nativeToLand: ["South America", "Central America"],

​

​	showName: function () {

		console.log("This is " + this.fruitName);

	},

​	nativeTo: function () {

		this.nativeToLand.forEach(function (eachCountry) {

							console.log("Grown in:" + eachCountry);

			});

	}

}

</pre>

如果你有10种水果，你将输入10遍同样的代码，还有，如果你想改一下nativeTo这个函数呢？你就必须在10个不同的地方更改这个函数。同样的，如果增加的是网站的用户呢？明显的之前的方法已经不适用了，特别是在开发大型应用的时候。

为了解决这些重复性问题，软件工程师们发明了范式（重复性问题和常见任务的解决方案）来使应用开发更加高效和流程化。

这里有两种创建对象的一般范式。

1.**Constructor 范式**

<pre lang="javascript">function Fruit (theColor, theSweetness, theFruitName, theNativeToLand) {

​

this.color = theColor;

this.sweetness = theSweetness;

this.fruitName = theFruitName;

this.nativeToLand = theNativeToLand;

​

this.showName = function () {

	console.log("This is a " + this.fruitName);

}

​

this.nativeTo = function () {

	this.nativeToLand.forEach(function (eachCountry) {

						console.log("Grown in:" + eachCountry);

								});

	}
}

</pre>

用这种范式可以轻易地创建出所有的水果：

<pre lang="javascript">var mangoFruit = new Fruit ("Yellow", 8, "Mango", ["South America", "Central America", "West Africa"]);

mangoFruit.showName(); // This is a Mango.​

mangoFruit.nativeTo();

​//Grown in:South America​

​// Grown in:Central America​

​// Grown in:West Africa​

​

​var pineappleFruit = new Fruit ("Brown", 5, "Pineapple", ["United States"]);

pineappleFruit.showName(); // This is a Pineapple.

</pre>

如果你想要更改fruitName函数，你只需要在一个地方改就可以了。这个范式通过可以继承的Fruit函数封装了所有水果共有的特性和方法。

**注意：**

继承的属性是定义在对象的prototype属性中的。

例如：someObject.prototype.firstName = “rich”;

自有的属性是直接定义在对象中的。

例如：

<pre lang="javascript">// 让我们先创建一个对象

var aMango = new Fruit ();

//现在我们直接在对象中定义mangoSpice属性，这是一个自有属性

aMango.mangoSpice = “some value”;

</pre>

引用一个对象里的属性我们使用object.property，例如：

<pre lang="javascript">console.log(aMango.mangoSpice); // “some value”

</pre>

调用一个对象里的方法我们使用object.method（），例如：

<pre lang="javascript">// 首先添加一个方法

aMango.printStuff = function () {return “Printing”;}

// 现在调用它

aMango.printStuff ();

</pre>

2.**原型范式**

<pre lang="javascript">function Fruit () {}

​
Fruit.prototype.color = "Yellow";

Fruit.prototype.sweetness = 7;

Fruit.prototype.fruitName = "Generic Fruit";

Fruit.prototype.nativeToLand = "USA";

​

Fruit.prototype.showName = function () {

console.log("This is a " + this.fruitName);

}

​

Fruit.prototype.nativeTo = function () {

console.log("Grown in:" + this.nativeToLand);

}

</pre>

用原型范式创建对象：

<pre lang="javascript">var mangoFruit = new Fruit ();

mangoFruit.showName(); //​

mangoFruit.nativeTo();

​// This is a Generic Fruit​

​// Grown in:USA

</pre>

**扩展阅读**

要充分了解两种范式和它们的优劣，可以阅读*Professional JavaScript for Web Developers *这本书的第六章。你将会知道Zakas最推荐那一种（提示：不是上面的两种）。

**如何引用对象中的属性**

主要方式有点运算符和中括号两种

1.**点运算符**

<pre lang="javascript">// 到目前为止我们的例子都是用点运算符，这里也是

​var book = {title: "Ways to Go", pages: 280, bookMark1:"Page 20"};

​

​// 取得 book 对象的属性值

console.log ( book.title); // Ways to Go​

console.log ( book.pages); // 280

</pre>

2.**中括号**

<pre lang="javascript">// 用中括号取得 book 对象的属性值

console.log ( book["title"]); //Ways to Go​

console.log ( book["pages"]); // 280​

​

​//又或者你把属性名存在变量里

​var bookTitle = "title";

console.log ( book[bookTitle]); // Ways to Go​

console.log (book["bookMark" + 1]); // Page 20

</pre>

访问一个不存在的属性会返回undefined。



**自有属性和继承属性**

对象拥有自有属性和继承属性。自有属性是定义在对象中的，继承属性在对象中的prototype对象中，可以被继承。

要知道一个属性是自有的还是继承的，你可以这样做：

<pre lang="javascript">// 创建一个新的带有schoolName​属性的school对象

​var school = {schoolName:"MIT"};

​

​// 返回trues因为schoolName是school对象的自有属性

console.log("schoolName" in school); // true​

​

​// 返回false因为我们并没有在school object里面定义一个schoolType属性，这个属性也不是school对象从原型对象Object.prototype中继承下来的。

console.log("schoolType" in school); // false​

​// 返回true因为school从Object.prototype​继承了方法toString。

console.log("toString" in school); // true

</pre>

**hasOwnProperty**

想知道一个对象是不是有自有属性，你可以用hasOwnProperty方法。当你需要遍历一个对象而又只想要它的自有属性时这个方法十分有用。

<pre lang="javascript">// 创建一个新的带有schoolName​属性的school对象

​var school = {schoolName:"MIT"};

​

​// 返回trues因为schoolName是school对象的自有属性

console.log(school.hasOwnProperty ("schoolName")); // true​

​// 返回false因为school从Object.prototype​继承了方法toString，所以toString不是school对象的自有属性。

console.log(school.hasOwnProperty ("toString")); // false

</pre>

**引用和遍历对象中的属性**

要引用对象中可枚举的属性（自有的和继承的），你需要用到for/in循环或者一个普通的for循环。

<pre lang="javascript">// 创建一个新的带有3个​属性的school对象: schoolName, schoolAccredited, and schoolLocation。

​var school = {schoolName:"MIT", schoolAccredited: true, schoolLocation:"Massachusetts"};

​

​//用for/in循环取到school对象的属性值。​

​for (var eachItem in school) {

	console.log(eachItem); // Prints schoolName, schoolAccredited, schoolLocation​

}

</pre>

**引用继承属性**

从Object.prototype继承的属性是不可枚举的，所以for/in 循环对它们无效。

**对象的原型属性（Prototype Property）和原型特性（Prototype Attribute）**

这两个概念非常重要，有兴趣的可以看看这篇文章 [JavaScript Prototype in Plain, Detailed Language][2]。

**删除对象的属性**

删除对象的属性要用到delete操作符。你不能删除继承的属性，或者 特性为non-configurable的属性。你必须要在原型对象（该属性被定义的地方）中才能删除继承的属性。同时，你也不能删除全局对象（且要求用关键词var定义）的属性。

当删除成功时delete操作符会返回true。但是，要是属性不存在或者不可删除（比如non-configurable的属性或者继承的属性）它也会返回true（真是蛋疼）。

下面是一些例子：

<pre lang="javascript">var christmasList = {mike:"Book", jason:"sweater" }

​delete christmasList.mike; // 删除mike属性

​

​for (var people in christmasList) {

console.log(people);

}

​// 只打印出jason​

​// mike属性被删除了。

​

​delete christmasList.toString; // 返回true， 但是toString没被删除因为它是一个继承的方法。

​

​// 调用toString还是能正常工作。 ​

christmasList.toString(); //"[object Object]"​

​

​// 你可以删除一个实例的属性，如果这个属性是这个实例的自有属性的话。例如，我们可以删除在上面定义的school对象的educationLevel属性因为这个属性是定义在实例中的:我们用"this"关键词去定义这个属性，当我们声明HigherLearning函数的时候，我们并没有在这个函数的原型中定义educationLevel属性。

​

console.log(school.hasOwnProperty("educationLevel")); true​

​// educationLevel是school对象的自有属性,所以可以删除。

​delete school.educationLevel; true

​

​//educationLevel property从school实例中删除了。​

console.log(school.educationLevel); undefined

​

​// 但是educationLevel属性仍然存在于HigherLearning函数中。

​var newSchool = new HigherLearning ();

console.log(newSchool.educationLevel); // University​

​

​// 如果我们在HigherLearning函数的原型中定义一个educationLevel2属性:​

HigherLearning.prototype.educationLevel2 = "University 2";

​

​// 这个educationLevel2属性在HigherLearning的实例中就不会是自有属性了。

​

​// educationLevel2属性不是school实例的自有属性。

console.log(school.hasOwnProperty("educationLevel2")); false​

console.log(school.educationLevel2); // University 2​

​

​// 让我们试着删除继承的educationLevel2属性。

​delete school.educationLevel2; true (总是会返回true的，上面说过了)

​

​//educationLevel2属性还在。

console.log(school.educationLevel2); University 2​

</pre>

**序列化和反序列化对象**

要通过http传输你的对象或者将它转化为字符串类型，你需要将它序列化（也就是把它变成字符串）；你可以用 JSON.stringify 函数进行序列化。在ECMAScript 5中你要使用json2这个库（Douglas Crockford大神写的）才能使用 JSON.stringify 函数，这在ECMAScript 5里面已经标准化了。

例子：

<pre lang="javascript">var christmasList = {mike:"Book", jason:"sweater", chelsea:"iPad" }

JSON.stringify (christmasList);

​// 打印以下字符串:​

​// "{"mike":"Book","jason":"sweater","chels":"iPad"}"

​

​// 要将转化为字符串的对象格式化，加上"null"和 "4"作为参数。

JSON.stringify (christmasList, null, 4);

"{

"mike": "Book",

"jason": "sweater",

"chels": "iPad"​

}"

​

​// JSON.parse Examples

// 下面是一个JSON字符串，所以我们不能用类似christmasListStr.mike这种方式取到其中的属性值。

​var christmasListStr = '{"mike":"Book","jason":"sweater","chels":"iPad"}';

​

​// 把它转化为一个对象

​var christmasListObj = JSON.parse (christmasListStr);

​

​// 转化为对象后我们就可以用点运算符取得属性值了。

console.log(christmasListObj.mike); // Book

</pre>

关于JavaScript对象的更多细节，包括ECMAScript 5中对对象的改动，可以阅读*The Definitive* *Guide* 第六版的第六章。

 [1]: http://javascriptissexy.com/javascript-objects-in-detail/
 [2]: http://javascriptissexy.com/javascript-prototype-in-plain-detailed-language/