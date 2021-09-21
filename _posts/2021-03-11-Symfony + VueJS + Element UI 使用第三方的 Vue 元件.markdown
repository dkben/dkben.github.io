---
layout: post
title:  "Symfony + Vue.js + Element UI 使用第三方的 Vue 元件"
subtitle: "Element UI 是第三方的 Vue.js 元件，用來做一些特殊的 HTML 效果"
date:   2021-03-11 09:00:00 +0800
categories: Symfony VueJs
tags:
- "Symfony"
- "VueJs"
---

Element UI 是第三方的 Vue.js 元件，用來做一些特殊的 HTML 效果

> 网站快速成型工具
>
> Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库

[**Element - The world's most popular Vue UI framework**  
_Element，一套为开发者、设计师和产品经理准备的基于 Vue 2.0 的桌面端组件库_element.eleme.io](https://element.eleme.io/#/zh-CN "https://element.eleme.io/#/zh-CN")[](https://element.eleme.io/#/zh-CN)

安裝

`yarn add element-ui`

![](/images/medium/1__x4GRgLe133FzWGzVqyLxNA.png)

未使用前

![](/images/medium/1__ohRq11Jp4La1WrGj9deWZQ.png)

在主設定檔中加入以下

```javascript
import Vue from 'vue';  
import ElementUI from 'element-ui';  
import 'element-ui/lib/theme-chalk/index.css';  
import Delivery from './components/Delivery';
```

![](/images/medium/1__nvGdmM8XjiwYjmz__mynZ9A.png)

引入元件

註冊元件

![](/images/medium/1__leJW6NdkbyRPBEUcWOvlOQ.png)

使用元件

![](/images/medium/1__9NeUHZLuU__apiMtv4GdFrQ.png)

效果

![](/images/medium/1__9P7CP3mrENIQ__SokAxlGNg.png)