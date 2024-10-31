---
title: 【网页】给浏览器里没有图标的书签加上ico
date: 2024-10-31
category: 网络
tags: [网页]
sticky: 1
---

书签栏中总有几个网站没有默认的favicon图标，强迫症当然是不能忍的。

《如何修改 Chrome 书签栏中的网站 Favicon 图标》介绍了修改任意图标的办法

有点麻烦，而且我的需求只是消除默认图标->

方法如下：

寻找一个心仪的ico格式的图标网络链接
Chrome打开待修改网站;
打开 更多工具 -> 开发者工具 （F12）
找到打开控制台(Console)页
粘贴进以下代码，并回车以执行（可能会等上几秒）

```javascript
var url = 'https://ico.58pic.com/ajax/download?id=151130&format=ico',
  tag = document.createElement('link')
tag.setAttribute('href', url)
tag.setAttribute('rel', 'shortcut icon')
$('head').append(tag)
```

其中，将url替换为你图标的链接

注：利用此方法所加载的图标，对于没有默认图标的网站，可以一直存在，但是对于已有图标的网站，重新刷新网站就会失效
