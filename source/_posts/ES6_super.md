title: ES6 Class的继承之super关键字
date: 2018/05/22
categories: 前端基础
tags:
  - ES6
---

super关键字有两种用法，既可以当成函数使用，也可以当成对象使用。在这两种情况下，它的用法完全不同。

在使用 JavaScript classes 时，你必须调用 super(); 方法才能在继承父类的子类中正确获取到类型的 this 。

1. 当super作为函数调用时
super作为函数调用时，代表父类的构造函数。ES6 要求，子类的构造函数必须执行一次super函数。

```
class A {}

class B extends A {
  constructor() {
    super();//调用父类的构造函数，否则会报错
  }
}
```

注意：
super虽然代表了父类A的构造函数，但是返回的是子类B的实例，即super内部的this指的是B，因此super()在这里相当于A.prototype.constructor.call(this)。
<!--more-->


```
class A {
  constructor() {
    console.log(new.target.name);
  }
}
class B extends A {
  constructor() {
    super();
  }
}
```

上面代码中，new.target指向当前正在执行的函数。可以看到，在super()执行时，它指向的是子类B的构造函数，而不是父类A的构造函数。也就是说，super()内部的this指向的是B。
**作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错**

提示：
new.target属性，一般用在构造函数之中，返回new命令作用于的那个构造函数。

2. super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类。

```
class Human {
  constructor() {}
  static ping() {
    return 'ping';
  }
}
class Computer extends Human {
  constructor() {}
  static pingpong() {
    return super.ping() + ' pong';//在 静态方法 中，super指向父类
  }
}
Computer.pingpong(); // 'ping pong'
```

提示：
类相当于实例的原型，所有在类中定义的方法，都会被实例继承。如果在一个方法前，加上static关键字，就表示该方法 **不会被实例继承，而是直接通过类来调用**，这就称为“静态方法”。

```
class A {
  p() {
    return 2;
  }
}

class B extends A {
  constructor() {
    super();
    console.log(super.p()); // 2 在普通方法中，super指向父类的原型对象
  }
}

let b = new B();
```

上面代码中，子类B当中的super.p()，就是将super当作一个对象使用。这时，super在普通方法之中，指向A.prototype，所以super.p()就相当于A.prototype.p()。
这里需要注意，由于super指向父类的原型对象，所以定义在父类实例上的方法或属性，是无法通过super调用的。

```
class A {
  constructor() {
    this.p = 2;
  }
}

class B extends A {
  get m() {
    return super.p;
  }
}

let b = new B();
b.m // undefined
```

上面代码中，p是父类A实例的属性，super.p就引用不到它。

如果属性定义在父类的原型对象上，super就可以取到。

ES6 规定,**在子类普通方法中通过super调用父类的方法时，方法内部的this指向当前的子类实例**。

```
class A {
  constructor() {
    this.x = 1;
  }
  print() {
    console.log(this.x);
  }
}

class B extends A {
  constructor() {
    super(); //this指向当前的子类实例--b
    this.x = 2;
  }
  m() {
    super.print();
  }
}

let b = new B();
b.m() // 2
```

由于this指向子类实例，所以 **如果通过super对某个属性赋值，这时super就是this，赋值的属性会变成子类实例的属性**。

```
class A {
  constructor() {
    this.x = 1;
  }
}

class B extends A {
  constructor() {
    super();
    this.x = 2;  //b.x=2
    super.x = 3;  //this.x=3
    console.log(super.x); // undefined
    console.log(this.x); // 3
  }
}

let b = new B();
```



参考：
https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/super
http://es6.ruanyifeng.com/?search=prop&x=0&y=0#docs/class-extends
