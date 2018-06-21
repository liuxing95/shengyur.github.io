title: YAML语言入门
date: 2018/06/20
categories: 效率工具
toc: true
front-matter:
  comments:true
tags:
  - YAML
---

使用场景：

编程免不了要写配置文件，写配置文件的时候，除了json格式，yaml格式的配置文件也很常见。所以决定系统学习一下。
### 简介
YAML 是专门用来写配置文件的语言，非常简洁和强大，远比 JSON 格式方便。
### YAML的基本语法规范
YAML 语言（发音 /ˈjæməl/ ）的设计目标，就是方便人类读写。
<!--more-->
它的基本语法规则如下：
```
1. 大小写敏感

2. 使用缩进表示层级关系

3. 缩进时不允许使用Tab键，只允许使用空格。

4. 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可

5. #表示注释，从这个字符一直到行尾，都会被解析器忽略。
```

### YAML支持的数据格式

YAML支持的数据格式有三种。
```
1. 对象：键值对的集合，又称为映射（mapping）/ 哈希（hashes） / 字典（dictionary）

2. 数组：一组按次序排列的值，又称为序列（sequence） / 列表（list）

3. 纯量（scalars）：单个的、不可再分的值
```
以下分别介绍这三种数据结构。

### 对象
对象的一组键值对，使用冒号结构表示。

```
animal: pets

转为javascript表示为：

 { animal: 'pets' }
```

Yaml 也允许另一种写法，将所有键值对写成一个行内对象。
```
hash: { name: Steve, foo: bar }

转为js表示为：

{ hash: { name: 'Steve', foo: 'bar' } }

```

### 数组
一组连词线开头的行，构成一个数组。

```
- Cat
- Dog
- Goldfish
```
转为js表示为
```
[ 'Cat', 'Dog', 'Goldfish' ]
```
数据结构的子成员是一个数组，则可以在该项下面缩进一个空格。
```
-
 - Cat
 - Dog
 - Goldfish
```
转为 JavaScript 如下。
```
[ [ 'Cat', 'Dog', 'Goldfish' ] ]
```
数组也可以采用行内表示法。
```
animal: [Cat, Dog]
```
转为js表示为：
```
{ animal: [ 'Cat', 'Dog' ] }
```

### 复合结构
对象和数组可以结合使用，形成复合结构。
```
languages:
 - Ruby
 - Perl
 - Python
websites:
 YAML: yaml.org
 Ruby: ruby-lang.org
 Python: python.org
 Perl: use.perl.org
 ```
转为 JavaScript 如下
```
{ languages: [ 'Ruby', 'Perl', 'Python' ],
  websites:
   { YAML: 'yaml.org',
     Ruby: 'ruby-lang.org',
     Python: 'python.org',
     Perl: 'use.perl.org' } }
```

### 纯量
纯量是最基本的、不可再分的值。以下数据类型都属于 JavaScript 的纯量。
```
字符串
布尔值
整数
浮点数
Null
时间
日期
```
注意：
- null 用 ~ 表示。
- 时间采用 ISO8601 格式。
```
  iso8601: 2001-12-14t21:59:43.10-05:00  
  (js表示：{ iso8601: new Date('2001-12-14t21:59:43.10-05:00') })
```
- 日期采用复合 iso8601 格式的年、月、日表示。
```
  date: 1976-07-31

  转为 JavaScript 如下。
  { date: new Date('1976-07-31') }

```
- YAML 允许使用两个感叹号，强制转换数据类型。
```
e: !!str 123
f: !!str true

转为 JavaScript 如下。
{ e: '123', f: 'true' }
```

未完待续。。。










参考：
- http://www.ruanyifeng.com/blog/2016/07/yaml.html
