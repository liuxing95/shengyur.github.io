title: 前端自动化测试探索
date: 2018/06/04
categories: 前端自动化测试
toc: true
tags:
  - Jest
---


### 常用的前端测试工具一览
前端测试工具也和前端的框架一样纷繁复杂，其中常见的测试工具，大致可分为 **测试框架**、**断言库**、**测试覆盖率工具** 等几类。在正式开始本文之前，我们先来大致了解下它们：
### 测试框架
测试框架的作用是提供一些方便的语法来描述测试用例，以及对用例进行分组。
测试一般分两种：BDD和TDD
<!--more-->
1. 先介绍Test-Driven Development(TDD)即测试驱动开发，TDD的原理是在开发功能代码之前，先编写单元测试用例代码，测试代码确定需要编写什么产品代码。国外使用这种开发模式较多，github上大型的开源项目，如果没跑过测试用例，大概是没人敢在生产环境中使用的。
测试驱动对开发过程的要求：
- 单元尽量解耦，否则单元不可测
- 开发前，先设计接口，再实现细节
- 便于回归和内部代码重构 (把所有单元测试，集成测试等都做好之后，实现重构就更加从容，可以一块一块进行)
2. BDD指的是Behavior Drive Development，也就是行为驱动开发。传统的开发模式中，客户很难从技术层面理解问题，开发人员很难从业务需求考虑问题，BDD描述的行为就像一个个的故事(Story)，系统业务专家、开发者、测试人员一起合作，分析需求，然后将这些需求写成一个个的故事(应用场景)。开发者负责填充这些故事的内容，测试者负责检验这些故事的结果。通常，通过设定各种业务场景中该展示的状态、适用的事件，以及场景的执行结果，基本就完成了一个完整测试的定义。

常见的测试框架有 [Jasmine](https://jasmine.github.io/), [Mocha](https://mochajs.org/),以及本文要介绍的 [Jest](https://facebook.github.io/jest/zh-Hans/) 。

### 断言库
断言库主要提供语义化方法，用于对参与测试的值做各种各样的判断。这些语义化方法会返回测试的结果，要么成功、要么失败。常见的断言库有 [Should.js](https://shouldjs.github.io/), [Chai.js](http://www.chaijs.com/) 等。

### 测试覆盖率工具
用于统计测试用例对代码的测试情况，生成相应的报表，比如 [istanbul](https://github.com/gotwarlost/istanbul)。

### Jest

1. 为什么选择Jest
Jest 是 Facebook 出品的一个测试框架，相对其他测试框架，其一大特点就是就是内置了常用的测试工具，比如自带断言、测试覆盖率工具，实现了开箱即用。

而作为一个面向前端的测试框架， Jest 可以利用其特有的[快照测试](https://facebook.github.io/jest/docs/zh-Hans/snapshot-testing.html#content)功能，通过比对 UI 代码生成的快照文件，实现对 React 等常见框架的自动测试。

此外， Jest 的测试用例是并行执行的，而且只执行发生改变的文件所对应的测试，提升了测试速度。目前在 Github 上其 star 数已经破万；而除了 Facebook 外，业内其他公司也开始从其它测试框架转向 Jest ，相信未来 Jest 的发展趋势仍会比较迅猛。

2. 安装
Jest 可以通过 npm 或 yarn 进行安装。以 npm 为例，既可用npm install -g jest进行全局安装；也可以只局部安装、并在 package.json 中指定 test 脚本：
```
{
  "scripts": {
    "test": "jest"
  }
}
```
Jest 的测试脚本名形如*.test.js，不论 Jest 是全局运行还是通过npm test运行，它都会执行当前目录下所有的*.test.js 或.spec.js 文件、完成测试。
3. 基本使用
4. 用例的表示
表示测试用例是一个测试框架提供的最基本的 API ， Jest 内部使用了 Jasmine 2 来进行测试，故其用例语法与 Jasmine 相同。test()函数来描述一个测试用例，举个简单的例子：
sum.js
```
function sum(a, b) {
    return a + b;
  }
  module.exports = sum;
```
sum.test.js
```
const sum = require('./sum');

test('adds 1 + 2 to equal 3', () => {
  //expect(sum(1, 2)).toBe(3);//成功
  expect(sum(1, 2)).toBe(5);//失败  
});
```
其中toBe('Hello world')便是一句断言（ Jest 管它叫 “matcher” ，想了解更多 matcher 请参考文档https://facebook.github.io/jest/docs/zh-Hans/using-matchers.html#content）。
写完了用例，运行在项目目录下执行npm test，即可看到测试结果：
![](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/1%2B2.jpg)
修改测试用例为expect(sum(1, 2)).toBe(3)，执行npm test,可看到测试通过：
![](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/1_2.jpg)


5. 用例的预处理或后处理：
有时我们想在测试开始之前进行下环境的检查、或者在测试结束之后作一些清理操作，这就需要对用例进行预处理或后处理。
- 对测试文件中所有的用例进行统一的预处理，可以使用 beforeAll() 函数；
- 如果想在每个用例开始前进行都预处理，则可使用 beforeEach() 函数；
- 后处理，可以使用对应的 afterAll() 和 afterEach() 函数。
- 如果只是想对某几个用例进行同样的预处理或后处理，可以将先将这几个用例归为一组。使用 describe() 函数即可表示一组用例，再将上面提到的四个处理函数置于 describe() 的处理回调内，就实现了对一组用例的预处理或后处理：

checkAll.js
```
function obj(){
  const o={
    foo:true,
    bar:false
  };
  return o
}
module.exports=obj();
```
checkAll.test.js
```
var testObject = require('./checkAll');


describe('test testObject', () => {
    beforeAll(() => {
        // 预处理操作
    })

    test('is foo', () => {
       expect(testObject.foo).toBeTruthy();
    })

    test('is not bar', () => {
        expect(testObject.bar).toBeFalsy();
    })

    afterAll(() => {
        // 后处理操作
    })
})
```
执行npm test
![](https://raw.githubusercontent.com/shengyur/shengyur.github.io/master/img/describe.jpg)



### Mocha
Mocha允许你使用任意你喜欢的断言库，在上面的例子中，我们使用了Node.js内置的assert模块作为断言
让我们一起看看assert有哪些常见用法



原文：
- https://segmentfault.com/a/1190000004558796
- https://zhuanlan.zhihu.com/p/28162082
