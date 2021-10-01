---
layout: post
title:  "JS 的自執行函式 IIFE (Immediately Invoked Function Expression)"
subtitle: "Immediately Invoked Function Expression"
date:   2020-01-04 09:00:00 +0800
categories: JavaScript
tags:
- "JavaScript"
---

自執行函式 Immediately Invoked Functions Expressions ( IIFE )

IIFE (Immediately Invoked Function Expression) 是一個定義完馬上就執行的 JavaScript function。

他又稱為 Self-Executing Anonymous Function，也是一種常見的設計模式，包含兩個主要部分：

第一個部分是使用Grouping Operator () 包起來的 anonymous function。

這樣的寫法可以避免裡面的變數污染到 global scope。

第二個部分是馬上執行 function 的 expression ()，JavaScript 引擎看到它就會立刻轉譯該 function。

以下都是 IIFE 的宣告方式，較常見的是第 1, 2 種

```javascript
(function() {
    console.log("Brackets around the function");
})();


(function() {
    console.log("Brackets around the whole thing");
}());


!function() {
    console.log("Bitwise NOT operator starts the expression");
}();


+function() {
    console.log("Unary plus starts the expression");
}();
```

![Untitled](/images/2020-01-04/2020-01-04-01.png)


