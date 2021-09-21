---
layout: post
title:  "Symfony5 MVC整合VueJS前端開發的方式"
subtitle: "目前在做的Site project雖然是用Symfony5 MVC架構製作，但部份頁面使用Vue JS前後分離開發"
date:   2021-03-06 09:00:00 +0800
categories: Jekyll
tags:
- "Jekyll"
---

目前在做的Site project雖然是用Symfony5 MVC架構製作，但部份頁面使用Vue JS前後分離開發。

注意！這種開發方式不使用 Vue Router，因為 Router 由 Symfony 處理。

整合前端開發的方式如下：

在 webpack.config.js 新增一個入口點

```javascript
.addEntry('app-vue', [  
  './assets/app.js'  
])
```

在入口點 app.js 設定要載入的元件 App

```javascript
// any CSS you import will output into a single css file (app.css in this case)  
import './styles/app.css';

// start the Stimulus application  
import './bootstrap';

import Vue from 'vue';
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css';
import App from './components/App';

new Vue({
    el: '#app',
    render: h => h(Purchase)
});
```

然後在 App.vue 中就是 Vue 元件的開發

```vue
<template>  

</template>  

<script>  
export default {  
  name: "App"  
}  
</script>  

<style scoped>  
  
</style>
```

在 Symfony5 Twig 樣版中，要載入編譯後的 CSS

```html
{% block stylesheets %}  
{{ parent() }}  
{{ encore_entry_link_tags('app-vue') }}  
{% endblock %}
```

和 JS

```html
{% block javascripts %}  
{{ parent() }}  
{{ encore_entry_script_tags('app-vue') }}  
{% endblock %}
```

前端檔案架構如下

![](/images/medium/1__xVCfdobqzqNerXYwCjSeUA.png)

開發時，需要開 2 個 terminal，一個用來啟動前端

`yarn dev-server`

![](/images/medium/1__BSdrpp8mLwIDX5CXLOgcKg.png)

一個用來啟動後端

`symfony serve -d`

![](/images/medium/1__CB9x0Sm5k2dh__I6QB5MeIg.png)

當然，佈署時的 JS 和 CSS 編譯要另外下指令產生