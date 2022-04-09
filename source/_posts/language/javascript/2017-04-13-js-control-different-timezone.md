title: 纯前端控制不同时区服务器时间的逻辑
date: 2017/04/13 16:49:25
categories:
- timezone
tags: [javascript,timezone]
---
对于不同地区的人，对一个时间的理解存在时区的差异。

比如中国+8时区的8点，与+0时区的0点是一致的

所以存在：2017-04-11 08:00:00+0800  ==  2017-04-11 00:00:00+0000
<!--more-->
解决方法：

1.服务器全部采用+8时区，无论部署在全球的任何地方

2.浏览器使用js获取到客户端的时区之后，将时间选择框中的时间加上与+8时区的差值传到后端，后端认为传递过来的总是+8时区的时间。

3.浏览器显示时，同样要处理时区的显示，服务器传到前端的时间总是+8时区的。

例如：

-->提交数据

价格的生效时间是在+0时区的电脑上新建的，生效时间选择2017-04-11 00:00:00，那么传递到服务端的时间应该加（(当前时区+00时区offset=0)-(服务器+08时区offset=-8)=8）是2017-04-11 08:00:00

-->获取数据

服务端传递到前端的时间仍然是2017-04-11 08:00:00，但是是+8时区的，所以

a.+0时区电脑的浏览器应该对这个时间做减((服务器+08时区offset=-8)-(当前时区+00时区offset=0))=8小时操作，即为2017-04-11 00:00:00，显示正确
b.+8时区电脑的浏览器应该对这个时间做减((服务器+08时区offset=-8)-(当前时区+08时区offset=-8))=0小时操作，即为2017-04-11 08:00:00，显示正确
c.+9时区电脑的浏览器应该对这个时间做减((服务器+08时区offset=-8)-(当前时区+09时区offset=-9))=1小时操作，即为2017-04-11 07:00:00，显示正确
c.-1时区电脑的浏览器应该对这个时间做减((服务器+08时区offset=-8)-(当前时区-01时区offset=1))=-9小时操作，即为2017-04-10 23:00:00，显示正确


对应的js代码
```
var d = new Date();
//本地时间与GMT时间的时间偏移差
var offset = d.getTimezoneOffset() / 60;
例：
-12时区：offset = 12
-08时区：offset = 8
-01时区：offset = 1
 00时区：offset = 8
+01时区：offset = -1
+08时区：offset = -8
+12时区：offset = -12
+14时区：offset = -14
```