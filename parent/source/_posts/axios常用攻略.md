title: axios常用攻略
date: 2018/06/14
categories: 库/框架
tags:
  - axios
toc: true
---



### 安装
npm install axios

**axios 依赖本机要支持ES6 Promise实现。 如果您的环境不支持ES6 Promises，您可以使用polyfill。**

<!--more-->

### 常见用法

1. 执行get请求

```
        // 向具有指定ID的用户发出请求
        axios.get('/user?ID=12345')
        .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });



        // 也可以通过 params 对象传递参数
        axios.get('/user', {
            params: {
            ID: 12345
            }
        })
        .then(function (response) {
            console.log(response);
        })
        .catch(function (error) {
            console.log(error);
        });
```

2. 执行post请求

```
    axios.post('/user', {
        firstName: 'Fred',
        lastName: 'Flintstone'
    })
    .then(function (response) {
        console.log(response);
    })
    .catch(function (error) {
        console.log(error);
    });
```

3. 执行多个并发请求

```
function getUserAccount() {
  return axios.get('/user/12345');
}
function getUserPermissions() {
  return axios.get('/user/12345/permissions');
}
axios.all([getUserAccount(), getUserPermissions()])
  .then(axios.spread(function (acct, perms) {
    //两个请求现已完成
  }));
```


更多可参考：
https://ykloveyxk.github.io/2017/02/25/axios%E5%85%A8%E6%94%BB%E7%95%A5/
