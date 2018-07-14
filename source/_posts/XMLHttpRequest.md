title: XMLHttpRequest
date: 2018/06/10
categories: 前端基础
toc: true
tags:
  - XMLHttpRequest
---

### XMLHttpRequest 是什么？
XMLHttpRequest 是一个 API，它为客户端提供了在客户端和服务器之间传输数据的功能。它提供了一个通过 URL 来获取数据的简单方式，并且不会使整个页面刷新。这使得网页只更新一部分页面而不会打扰到用户。XMLHttpRequest 在 AJAX 中被大量使用。

### 创建 XMLHttpRequest 对象
所有现代浏览器 (IE7+、Firefox、Chrome、Safari 以及 Opera) 都内建了 XMLHttpRequest 对象。

通过一行简单的 JavaScript 代码，我们就可以创建 XMLHttpRequest 对象。

### 创建 XMLHttpRequest 对象的语法
```
xmlhttp=new XMLHttpRequest();
```
老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象
```
xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
```
### 使用 XMLHttpRequest 对象
```
var xmlhttp;
function loadXMLDoc(url)
    {
    xmlhttp=null;
    if (window.XMLHttpRequest){// code for all new browsers
      xmlhttp=new XMLHttpRequest(); //创建xml对象
      }
    else if (window.ActiveXObject){// code for IE5 and IE6
      xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
      }
    if (xmlhttp!=null){
          xmlhttp.onreadystatechange=function(){
              if (xmlhttp.readyState==4){// 4 = "loaded" 	整个请求过程已经完毕

                if (xmlhttp.status==200){请求的响应状态码 (例如, 状态码200 表示一个成功的请求).
                  // ...our code here...TODO
                  }
                else{
                  alert("Problem retrieving XML data");
                  }
                }
          }
          xmlhttp.open("GET",url,true);
          xmlhttp.send(null);
      }
    else{
      alert("Your browser does not support XMLHTTP.");
      }
    }

```

参考：
- https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest
- http://www.w3school.com.cn/xml/xml_http.asp
