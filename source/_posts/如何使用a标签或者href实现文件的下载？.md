title: 如何使用a标签或者href实现文件的下载？
date: 2018/06/23
categories: 十万个为什么

---

1. 利用iframe实现（下载的文件域名不方便直接写）

html：

```
  
    <a href="javascript:Slide.downLoadCooperationAgreement();">年度合作协议</a>
```

js：

```  
    Slide.downLoadCooperationAgreement = function(){
        var url = System.domainUrlTotUedPxy[System.testTotPxyFlag]+'/download/annualCooperationAgreement_v1.docx';
        var size = $("#cooperationAgreement_download").size();
        if (size == 0) {
           $("body").append("<iframe id = 'cooperationAgreement_download' style='display:none;' src='"+url+"'></iframe>");
        } else {
          $("#cooperationAgreement_download").attr("src", url);
        }
      };
```
2. 利用a标签实现，download属性可以用来命名下载的文件。
```
    <a href="/i/w3school_logo_white.gif" download="w3logo">
      <img border="0" src="/i/w3school_logo_white.gif" alt="W3School">
    </a>
```

原文：
 - [王老师的积累](https://github.com/wang-qingqing/accumulate/blob/master/%E6%A1%86%E6%9E%B6%E7%B1%BB/%E5%85%B6%E5%AE%83/%E4%B8%8B%E8%BD%BD%E6%96%87%E4%BB%B6%EF%BC%88a%E6%A0%87%E7%AD%BE%E7%9A%84href%E5%AE%9E%E7%8E%B0%EF%BC%89.md)


