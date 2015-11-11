---
layout: post
title: Javascript Context Menu - 自定义右键菜单
cover: cover.jpg
category: web
tags: [web, javascript]
---

最近要给web topology的node加一个自定义的右键菜单(context menu)，但是难点在于并不是给一些固定的html element
加右键菜单这么简单，如果只是给一些element加context menu,那么有很多library比如jquery context menu
就可以做到。这个topology是用`jit.js`实现的。

### 问题分析：

需要和现有的`jit.js`整合，而`jit.js`产生的Node element并不是有固定id的，而且是动态生成的，所以不能在
`onload`或者其他function中直接`bind`events.而`jit.js`自己其实已经有了`onRightClick`的event listener,
而且`jit.js`library内部有其他部分来check是否是点击在node上，在动态新加入node这个event listener也可以work.
所以我更希望利用这个原有的api来实现context menu。

### 解决思路：

在`onRightClick`这个event被trigger时

- check是否点击在node上
- display context menu on the node

### 解决方案：

我不想要自己写html和css来实现，那样有点麻烦，更希望看别人的源码来修改。

所以找到了一个非常`light-weighted`的library [contextjs](http://lab.jakiestfu.com/contextjs/#), 简单且符合要求.

这个library本来也只有一个加`attach`的method来将context menu bind到一个element的right click
event上，我利用里面的display部分的code, 将原有的`jit.js`的event当作parameter传入，就达到了在
其他event trigger时显示context menu的目的。

### 修改地址:

[github](https://github.com/lefttree/Context.js)
