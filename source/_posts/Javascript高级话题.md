---
title: Javascript高级话题
date: 2017/11/23 09:48
tags: [前端,Javascript]
categories: 前端
comments: true
---
面向对象、作用域、闭包、设计模式等。
# 1. 常见的JS类定义方式
- 构造函数原型
- 对象创建

　　原型法是通用老方法，对象创建是ES5推荐使用的方法.目前来看，原型法更普遍。

1) 构造函数方法定义类：
```
function Person() {
    this.name = 'HalZhan';
}
Person.prototype.sayName = function() {
    alert(this.name);
}
var person = new Person();
person.sayName();
```

2) 对象创建方法定义类：
```
var Person = {
    name: 'HalZhan',
    sayName: function() { 
        alert(this.name);
    }
};
var person = Object.create(Person);
person.sayName();
```

# 2. JS类继承方法
- 原型链法
- 属性复制法
- 构造器应用法

　　另外，由于每个对象可以是一个类，这些方法也可以用于对象类的继承。
1) 原型链法：
```
function Animal() {
    this.name = 'animal';
}
Animal.prototype.sayName = function () {
    alert(this.name);
};

function Person() { }
Person.prototype = Animal.prototype; // 人继承自动物
Person.prototype.constructor = Person; // 更新构造函数为人
```

2) 属性复制法：
```
function Animal() {
    this.name = 'animal';
}
Animal.prototype.sayName = function () {
    alert(this.name);
};

function Person() { }

for (prop in Animal.prototype) {
    Person.prototype[prop] = Animal.prototype[prop];
} // 复制动物的所有属性到人量边
Person.prototype.constructor = Person; // 更新构造函数为人
```

3) 构造器应用法：
```
function Animal() {
    this.name = 'animal';
}
Animal.prototype.sayName = function () {
    alert(this.name);
};

function Person() {
    Animal.call(this); // apply, call, bind方法都可以．细微区别，后面会提到．
}
```

# 3. JS类多继承的实现方法
　　就是类继承里边的属性复制法来实现。因为当所有父类的prototype属性被复制后，子类自然拥有类似行为和属性。

# 4. JS作用域描述
　　大多数语言里边都是块作作用域，以{}进行限定，js里边不是。js里边叫函数作用域，就是一个变量在全函数里有效。比如有个变量p1在函数最后一行定义，第一行也有效，但是值是undefined。
```
var globalVar = 'global var';

function test() {
    alert(globalVar); // undefined, 因为globalVar在本函数内被重定义了，导致全局失效，这里使用函数内的变量值，可是此时还没定义
    var globalVar = 'overrided var'; //　globalVar在本函数内被重定义
    alert(globalVar);　// overrided var
}
alert(globalVar); // global var，使用全局变量
```

# 5. js里边的this的指向
　　this指的是对象本身，而不是构造函数。
```
function Person() {
}
Person.prototype.sayName = function () {
    alert(this.name);
}

var person1 = new Person();
person1.name = 'HalZhan';
person1.sayName(); // HalZhan
```

# 6. apply, call和bind的区别
　　三者都可以把一个函数应用到其他对象上，注意不是自身对象。apply,call是直接执行函数调用，bind是绑定，执行需要再次调用。apply和call的区别是apply接受数组作为参数，而call是接受逗号分隔的无限多个参数列表。
```
function Person() {
}
Person.prototype.sayName = function () {
    alert(this.name);
}

var obj = { name: 'HalZhan' }; // 注意这是一个普通对象，它不是Person的实例
var param1, param2, param3;
// 1) apply
Person.prototype.sayName.apply(obj, [param1, param2, param3]);

// 2) call
Person.prototype.sayName.call(obj, param1, param2, param3);

// 3) bind
var sn = Person.prototype.sayName.bind(obj);
sn([param1, param2, param3]); // bind需要先绑定，再执行 
sn(param1, param2, param3); // bind需要先绑定，再执行
```

# 7. caller, callee和arguments分别是什么
　　caller,callee之间的关系就像是employer和employee之间的关系，就是调用与被调用的关系，二者返回的都是函数对象引用。arguments是函数的所有参数列表，它是一个类数组的变量。
```
function parent(param1, param2, param3) {
    child(param1, param2, param3);
}

function child() {
    console.log(arguments); // { '0': 'halzhan1', '1': 'halzhan2', '2': 'halzhan3' }
    console.log(arguments.callee); // [Function: child]
    console.log(child.caller); // [Function: parent]
}

parent('halzhan1', 'halzhan2', 'halzhan3');
```

# 8. 什么是闭包，闭包有哪些用处
　　闭包简而言之，两个字：函数，更多字：嵌套函数的父子自我引用关系。所有函数都是闭包。通俗的说，闭包就是作用域范围，因为js是函数作用域，所以函数就是闭包。全局函数的作用域范围就是全局，所以无须讨论。更多的应用其实是在内嵌函数，这就会涉及到内嵌作用域，或者叫作用域链。说到内嵌，其实就是父子引用关系(父函数包含子函数，子函数因为函数作用域又引用父函数，所以叫闭包），这就会带来另外一个问题，什么时候引用结束？如果不结束，就会一直占用内存，引起内存泄漏。好吧，不用的时候就引用设为空，死结就解开了。

# 9. defineProperty, hasOwnProperty, isEnumerable的用途
　　Object.defineProperty(obj, prop, descriptor)用来给对象定义属性,有value,writable,configurable,enumerable,set/get等。

　　hasOwnProerty用于检查某一属性是不是存在于对象本身，继承来的父亲的属性不算。

　　isEnumerable用来检测某一属性是否可遍历，也就是能不能用for..in循环来取到。

# 10. js常用设计模式的实现思路，单例，工厂，代理，装饰，观察者模式等
```
 // 1) 单例: 任意对象都是单例，无须特别处理
var obj = { name: 'halzhan', age: 24 };

// 2) 工厂: 就是同样形式参数返回不同的实例
function Person() { this.name = 'Person1'; }
function Animal() { this.name = 'Animal1'; }

function Factory() { }
Factory.prototype.getInstance = function (className) {
    return eval('new ' + className + '()');
}

var factory = new Factory();
var obj1 = factory.getInstance('Person');
var obj2 = factory.getInstance('Animal');
console.log(obj1.name); // Person1
console.log(obj2.name); // Animal1

// 3) 代理: 就是新建个类调用老类的接口,包一下
function Person() { }
Person.prototype.sayName = function () { console.log('halzhan'); }
Person.prototype.sayAge = function () { console.log(24); }

function PersonProxy() {
    this.person = new Person();
    var that = this;
    this.callMethod = function (functionName) {
        console.log('before proxy:', functionName);
        that.person[functionName](); // 代理
        console.log('after proxy:', functionName);
    }
}

var pp = new PersonProxy();
pp.callMethod('sayName'); // 代理调用Person的方法sayName()
pp.callMethod('sayAge'); // 代理调用Person的方法sayAge() 

// 4) 观察者: 就是事件模式，比如按钮的onclick这样的应用.
function Publisher() {
    this.listeners = [];
}
Publisher.prototype = {
    'addListener': function (listener) {
        this.listeners.push(listener);
    },

    'removeListener': function (listener) {
        delete this.listeners[listener];
    },

    'notify': function (obj) {
        for (var i = 0; i < this.listeners.length; i++) {
            var listener = this.listeners[i];
            if (typeof listener !== 'undefined') {
                listener.process(obj);
            }
        }
    }
}; // 发布者

function Subscriber() {

}
Subscriber.prototype = {
    'process': function (obj) {
        console.log(obj);
    }
};　// 订阅者

var publisher = new Publisher();
publisher.addListener(new Subscriber());
publisher.addListener(new Subscriber());
publisher.notify({ name: 'halzhan', ageo: 24 }); // 发布一个对象到所有订阅者
publisher.notify('2 subscribers will both perform process'); // 发布一个字符串到所有订阅者

```

# 11. 列举数组相关的常用方法
push/pop, 

shift/unshift, 

split/join, 

slice/splice/concat, 

sort/reverse, 

map/reduce, 

forEach, 

filter

# 12. 列举字符串相关的常用方法
indexOf/lastIndexOf/charAt, 

split/match/test,

slice/substring/substr,

toLowerCase/toUpperCase