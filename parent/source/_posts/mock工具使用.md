title: mock工具使用
date: 2018/06/14
categories: 效率工具
tags:
  - mock
toc: true
---


## mock的基础解释
### 目的
  可以在不依赖后端环境的情况下，进行前端开发，帮助编写单元测试,减少联调成本，提高工作效率。

### 实现功能
  A.能渲染模板
  B.实现请求路由映射
  C.数据接口代理到生产或者测试环境

<!--more-->
### 常用方式
  在开发过程中，模拟后台接口数据，快速地实现前端开发。

## mock的用法
### 安装
```
    npm install mockjs
```
### 使用mock
```
        var Mock = require('mockjs');
        var mock = Mock.mock({
            ......
        })
```
### 语法

 mock的语法规范包含两层规范：数据模板（DTD）和数据占位符（DPD）。

#### 数据模板DTD

    A.模板规则：'name|rule':value
```
        其中，name:属性名； rule:属性规则；value：属性值。
            属性名和规则之间用|隔开，规则是可以选的。
```
    B.7个rule
    ```
        'name|min-max':value
        'name|count':value
        'name|min-max.dmin-dmax':value
        'name|min-max.dcount':value
        'name|count.dmin-dmax':value
        'name|count.dcount':value
        'name|+step':value
```

**（1）生成规则需要根据属性值的类型才能确定（2）属性值可以含有@占位符（3）属性值可以指定最终值的初始值和类型**

    C.属性值是String
```
        var data = Mock.mock({
            'name1|1-3' : 'a', //重复生成1到3个a
            'name2|2' : 'b' //生成bb
        })
```
    D.属性值是Number
```
        var data = Mock.mock({
            'name1|+1' : 4,  //生成4,如果循环每次加1
            'name2|1-7' : 2, //生成一个数字，1到7之间
            'name3|1-4.5-8' : 1  //生成一个小数，整数部分1到4，小数部分5到8位（！！！表示小数点之后的位数范围）
        })
```
    E.属性值是Boolean
```
        var data = Mock.mock({
            'name|1': true, //生成一个布尔值，各一半
            'name1|1-3': true  // 1/4是true，3/4是false
        })
```
    F.属性值是Object
```
        var obj = {
            a:1,
            b:2,
            c:3,
            d:4
        }
        var data = Mock.mock({
            'name|1-3': obj,
            'name|2': obj
        })
```

    G.属性值是Array

```
        var arr = [1,2,3];
        var data = Mock.mock({
            'name1|1': arr, //从数组里随机取出1个值
            'name2|2': arr, //数组重复count次，这里count为2
            'name3|1-3': arr  //数组重复1到3次
        })
```
    H.属性值是Function

```
        var fun = function(x){
            return x+10;
        }
        var data = Mock.mock({
            'name':fun(10)         //返回函数的返回值20
        })
```

    I.属性值是RegExp
        根据正则表达式反向生成对应的字符串，用于生成自定义格式的字符串
```
            var data = Mock.mock({
                ‘name1':/[a-z][A-Z]/,
                'name2':/\d{1,3}/
            })
```
        会根据各自的正则表达式进行适配，并且随机返回


#### 数据占位符DPD

关于占位符，占位符只是在属性值是字符串的时候，在字符串里占个位置，并不会出现在最终的属性值中。

占位符的格式为：@占位符,关于占位符需要知道以下几点：

- 用@标识符标识后面的字符串是占位符

- 占位符的值是从Mock.Random方法中引用的

- 可以通过Mock.Random.extend()来扩展自定义占位符

- 占位符可以引用数据模板中的属性

- 占位符优先引用数据模板中的属性

- 占位符支持相对路径和决定路径
```
    var data = Mock.mock({
        name:{
            name1:'@FIRST',
            name2:'@LAST'
        }    
    })
```

#### Mock.mock()

- Mock.mock(rurl,rtype,template|function(opt))

```
    rurl: ajax请求的地址
    rtype: ajax请求的类型，如’GET','POST'
    template: 数据模板，就是之前那些个例子
    function: 生成相应数据的函数
```

- 常用的方法

```
    A. Mock.mock(template)

    B. Mock.mock(rurl,template)，模拟ajax,匹配接收到url的ajax请求，把template对应的数据返回返回

    C. Mock.mock(rurl,function(opt)),模拟ajax,会把函数执行的结果作为ajax回调返回

    D. Mock.mock(rurl,rtype,template) 同上，只是对ajax的类型有要求

    E. Mock.mock(rurl,rtype,function) 同上
```

#### Mock.setup(setting)

配置拦截ajax请求的行为，支持的配置项有timeout。

```
    Mock.setup({
        timeout:200
    })
    Mock.setup({
        timeout:'200-500
    })
```

#### Mock.valid(template,data)

这个函数用来判断，数据模板和数据是否一样。

#### Mock.toJSONShema(template)

```
    var template = Mock.mock({
        'name|1-3':5
    })
    var tjs = Mock.toJSONSchema(tempalte);
```

####  Mock.Random

```
    var Random = Mock.Random;
    var em1 = Mock.email();
    var em2 = Mock.mock('@email');
    var em3 = Mock.mock({
        email:'@email'
    })
```

## mock的原理
1. Mock主要是关于测被测体与外部的交互逻辑。一般是指在不同场景下多次的与外部交互。

2. 当外部依赖是硬件，或者外部程序没有准备好的情况下，Mock就能帮助完成单元测试。

3. Mock的作用就是能通过设置输入输出值，让被测代码的各种工作场景都可以被测试覆盖。

4. 为方便测试，程序设计应该采用依赖倒置原则，让被测试类依赖于接口，Mock类实现该接口。

**被测类通过调用接口最终调用到Mock类，完成测试。而被测试类可以不作任何修改就能在实际系统里工作。
这就是使用TDD能很自然的使产品代码于外部依赖松耦合。**

参考：
- [王老师的总结](https://github.com/wang-qingqing/accumulate/blob/master/%E5%B7%A5%E5%85%B7%E7%B1%BB/%E5%85%B3%E4%BA%8Emock.md)
- https://www.zhihu.com/question/35436669/answer/62753889
- https://segmentfault.com/a/1190000010211622
- [深入理解Mock工作原理](https://blog.csdn.net/ibelieve1974/article/details/55220015)
