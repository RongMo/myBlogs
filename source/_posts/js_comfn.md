---
title: JavaScript常用函数汇总
date: 2017-03-15 11:00:20
tags: JavaScript
---

> 在很多传统语言（C/C++/Java/C#等）中，函数都是作为一个二等公民存在，你只能用语言的关键字声明一个函数然后调用它，如果需要把函数作为参数传给另一个函数，或是赋值给一个本地变量，又或是作为返回值，就需要通过函数指针(function pointer)、代理(delegate)等特殊的方式周折一番。而在JavaScript世界中函数却是一等公民，它不仅拥有一切传统函数的使用方式（声明和调用），而且可以做到像简单值一样赋值、传参、返回，这样的函数也称之为第一级函数（First-class Function）。不仅如此，JavaScript中的函数还充当了类的构造函数的作用，同时又是一个Function类的实例(instance)。这样的多重身份让JavaScript的函数变得非常重要。

<!-- more -->

## 1.原生JavaScript实现字符串长度截取


```JavaScript
  function cutstr(str, len) {
 		var temp;
        var icount = 0;
        var patrn = /[^\x00-\xff]/;
        var strre = "";
        for (var i = 0; i < str.length; i++) {
            if (icount < len - 1) {
                temp = str.substr(i, 1);
                if (patrn.exec(temp) == null) {
                    icount = icount + 1
                } else {
                    icount = icount + 2
                }
                strre += temp
            } else {
                break
            }
        }
        return strre + "..."
    }
```


## 2.原生JavaScript获取域名主机

```javascript
function getHost(url) {
        var host = "null";
        if(typeof url == "undefined"|| null == url) {
            url = window.location.href;
        }
        var regex = /^\w+\:\/\/([^\/]*).*/;
        var match = url.match(regex);
        if(typeof match != "undefined" && null != match) {
            host = match[1];
        }
        return host;
}
```


## 3.原生JavaScript元素显示的通用方法

```javascript
function $(id) {
        return !id ? null : document.getElementById(id);
    }
    function display(id) {
        var obj = $(id);
        if(obj.style.visibility) {
            obj.style.visibility = obj.style.visibility == 'visible' ? 'hidden' : 'visible';
        } else {
            obj.style.display = obj.style.display == '' ? 'none' : '';
        }
    }
```


## 4.原生JavaScript实现checkbox全选与全不选

    function checkAll() {
        var selectall = document.getElementById("selectall");
        var allbox = document.getElementsByName("allbox");
        if (selectall.checked) {
            for (var i = 0; i < allbox.length; i++) {
                allbox[i].checked = true;
            }
        } else {
            for (var i = 0; i < allbox.length; i++) {
                allbox[i].checked = false;
            }
        }
    }


## 5.原生JavaScript完美判断是否为网址

```javascript
function IsURL(strUrl) {
    var regular = /^\b(((https?|ftp):\/\/)?[-a-z0-9]+(\.[-a-z0-9]+)*\.(?:com|edu|gov|int|mil|net|org|biz|info|name|museum|asia|coop|aero|[a-z][a-z]|((25[0-5])|(2[0-4]\d)|(1\d\d)|([1-9]\d)|\d))\b(\/[-a-z0-9_:\@&?=+,.!\/~%\$]*)?)$/i
    if (regular.test(strUrl)) {
        return true;
    }
    else {
        return false;
    }
}
```


## 6.原生JavaScript获得URL中GET参数值

```javascript
// 用法：如果地址是 test.htm?t1=1&t2=2&t3=3, 那么能取得：GET["t1"], GET["t2"], GET["t3"]
function get_get(){ 
  querystr = window.location.href.split("?")
  if(querystr[1]){
    GETs = querystr[1].split("&")
    GET =new Array()
    for(i=0;i<GETs.length;i++){
      tmp_arr = GETs[i].split("=")
      key=tmp_arr[0]
      GET[key] = tmp_arr[1]
    }
  }
  return querystr[1];
}
```


## 7.原生JavaScript跨浏览器添加事件

```javascript
function addEvt(oTarget,sEvtType,fnHandle){
    if(!oTarget){return;}
    if(oTarget.addEventListener){
        oTarget.addEventListener(sEvtType,fnHandle,false);
    }else if(oTarget.attachEvent){
        oTarget.attachEvent("on" + sEvtType,fnHandle);
    }else{
        oTarget["on" + sEvtType] = fnHandle;
    }
}
```


## 8.原生JavaScript实现返回顶部的通用方法

```javascript
function backTop(btnId) {
    var btn = document.getElementById(btnId);
    var d = document.documentElement;
    var b = document.body;
    window.onscroll = set;
    btn.style.display = "none";
    btn.onclick = function() {
        btn.style.display = "none";
        window.onscroll = null;
        this.timer = setInterval(function() {
            d.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            b.scrollTop -= Math.ceil((d.scrollTop + b.scrollTop) * 0.1);
            if ((d.scrollTop + b.scrollTop) == 0) clearInterval(btn.timer, window.onscroll = set);
        },
        10);
    };
    function set() {
        btn.style.display = (d.scrollTop + b.scrollTop > 100) ? 'block': "none"
    }
};
backTop('goTop');
```


## 9.原生JavaScript实现全选通用方法

```javascript
function checkall(form, prefix, checkall) {
    var checkall = checkall ? checkall : 'chkall';
    for(var i = 0; i < form.elements.length; i++) {
        var e = form.elements[i];
        if(e.type=="checkbox"){
            e.checked = form.elements[checkall].checked;
        }
    }
}
```


## 10.原生JavaScript实现全部取消选择通用方法

```javascript
function uncheckAll(form) {
    for (var i=0;i<form.elements.length;i++){
        var e = form.elements[i];
        if (e.name != 'chkall')
        e.checked=!e.checked;
    }
}
```


## 11.原生JavaScript获取单选按钮的值

```javascript
function get_radio_value(field){
    if(field&&field.length){    
        for(var i=0;i<field.length;i++){        
            if(field[i].checked){            
                return field[i].value;                                
            }            
        }        
    }else {        
        return ;                
    }    
}
```


## 12.原生JavaScript获取复选框的值

```javascript
function get_checkbox_value(field){    
    if(field&&field.length){    
        for(var i=0;i<field.length;i++){            
            if(field[i].checked && !field[i].disabled){
                return field[i].value;
            }
        }        
    }else {
        return;
    }            
}
```


## 13.原生JavaScript判断变量是否空值

```javascript
/**
 * 判断变量是否空值
 * undefined, null, '', false, 0, [], {} 均返回true，否则返回false
 */
function empty(v){
    switch (typeof v){
        case 'undefined' : return true;
        case 'string'    : if(trim(v).length == 0) return true; break;
        case 'boolean'   : if(!v) return true; break;
        case 'number'    : if(0 === v) return true; break;
        case 'object'    : 
            if(null === v) return true;
            if(undefined !== v.length && v.length==0) return true;
            for(var k in v){return false;} return true;
            break;
    }
    return false;
}
```

## 14.后记 

本文转自[MR_LP___李鹏](http://www.jianshu.com/u/5a2fd0b8fb30)参考于 [小萧ovo](https://zhuanlan.zhihu.com/p/24205965) 的码农在线，感谢作者。

