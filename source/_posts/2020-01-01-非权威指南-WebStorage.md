---
title: 非权威指南-WebStorage
header-img: back.png
header-color: 'white'
tag-color: 'RGB(32,24,40)'
catalog: true
toc-number: false
date: 2020-01-01 09:33:58
tags: ['webStorage', '客户端识别', 'web 存储']
---

## 1. 概念

Web Storage 与 [cookie](/2019/12/31/非权威指南-cookie/) 概念类似，都用于存储会话数据。但是 Web Storage 仅在本地存储数据，存储容量也比 cookie 更大 (一般为 5M, cookie 为 4K)

Web Storage包括两种存储方式：`sessionStorage` 和 `localStorage` ，这两种存储方式的唯一区别是生命周期。前者在会话结束(浏览器/标签关闭)后删除，后者理论上不手动删除将永久存在。

## 2. 机制简述

在支持 Web Storage 的浏览器中 window 对象实现了 WindowSessionStorage 和 WindowLocalStorage 对象并将其挂载在 `sessionStorage` 和 `localStorage` 属性

当使用 `window.sessionStorage` 和 `window.localStorage` 时会创建相应的 Storage 对象，可以设置、获取、移除数据项

## 3. 使用

### 3.1 添加数据项

> sessionStorage.setItem(key, value)	添加数据项
>
> localStorage.setItem(key, value)	添加数据项

**注意添加的数据项的类型只能为 string，如果要添加的数据项为对象则需要先进行序列化**

```js
  sessionStorage.setItem('username', JSON.stringify({name:'tangBii', age:20}))
  localStorage.setItem('username','tangBii')
```

<img src="session-set.png" alt="session-set" style="zoom:67%;" />

<img src="local-set.png" alt="local-set" style="zoom:67%;" />

### 3.2 读取数据

> sessionStorage.getItem(key)	读取数据
>
> localStorage.getItem(key)	读取数据

```js
  console.log(JSON.parse(sessionStorage.getItem('username')))
  console.log(localStorage.getItem('username'))
```

![getitem](getitem.png)

关闭标签或浏览器后重新读取数据，可以发现使用 sessionStorage 存储的数据项被删除，而使用 localStorage 存储的数据项依然存在

### 3.3 删除数据

> sessionStorage.removeItem(key)	删除数据项
>
> sessionStorage.clear()	清空数据
>
> localStorage.removeItem(key)	删除数据项
>
> localStorage.clear()	清空数据

```js
 sessionStorage.clear()
 localStorage.removeItem('username')
```

## 4. 简单示例

一个可以保存用户偏好颜色的界面

```html
<body>
<p>请选择偏好的背景颜色：</p>
<button id="btn01">red</button>
<button id="btn02">green</button>
<button id="btn03">blue</button>
<script type="text/javascript">
  // 设置背景颜色为用户之前选择的偏好颜色，默认为灰色
  const backColor = localStorage.getItem('backColor') || 'gray'
  document.body.style.backgroundColor = backColor
  const btns = document.getElementsByTagName('button')

  for (let i = 0, length = btns.length; i < length; i++) {
    btns[i].onclick = () => {
      const backColor = btns[i].innerHTML
      document.body.style.backgroundColor = backColor

      // 记录用户偏好的颜色
      localStorage.setItem('backColor', backColor)
    }
  }
</script>
```

背景颜色默认为灰色，用户选择偏好后下次再打开页面背景颜色将显示为用户偏好颜色