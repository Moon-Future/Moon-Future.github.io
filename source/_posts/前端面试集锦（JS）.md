---
title: 前端面试集锦（JS）
date: 2019-04-12 17:00:00
tags: [面试, Javascript]
---

# 一、DOM事件
## 1、基本概念：DOM事件的级别
```js
// DOM0
element.onclick = function() {}
// DOM1 没有涉及跟实践有关的东西
// DOM2 IE：attachEvent
element.addEventListener('click', function() {}, false)
// DOM3 事件类型增加，如鼠标事件，键盘事件等等
element.addEventListener('keyup', function() {}, false)
```

## 2、DOM模型
- 捕获
- 冒泡

## 3、DOM事件流
捕获 ---> 目标阶段 ---> 冒泡

## 4、描述DOM事件捕获的具体流程
捕获：window ---> document ---> html ---> body ---> ... ---> 目标元素  
冒泡：目标元素 ---> ... ---> body ---> html ---> document ---> window

## 5、Event对象的常见应用
```js
event.preventDefault()
event.stopPropagation()
event.stopImmediatePropagation()
event.currentTarget()
event.target()
```

## 6、自定义事件
```js
var eve = new Event('test');
window.addEventListener('test', function() {}, false)
window.dispatchEvent(eve);
```