---
layout: post
title:  "JS 的 setTimeout 與 setInterval"
subtitle: "setTimeout & setInterval"
date:   2020-01-07 09:00:00 +0800
categories: JavaScript
tags:
- "JavaScript"
---

訂時器 setTimeout

2 秒後才會執行 sayHi function

```javascript
function sayHi() {
    console.log('hi');
}
setTimeout(sayHi, 2000);
```

![Untitled](/images/2020-01-07/2020-01-07-01.png){:width="800px"}

不可以用單引號、雙引號

因為 setTimeout 需要的是函數的引用

這樣不會執行

```javascript
function sayHi() {
    console.log('hi');
}
setTimeout('sayHi', 2000);
```

如果帶刮號也不行，這樣代表直接執行

```javascript
function sayHi() {
    console.log('hi');
}
setTimeout(sayHi(), 2000);
```

帶參數的狀況

```javascript
function sayHi(name, job) {
    console.log('hi, ' + name + ', job is ' + job);
}
setTimeout(sayHi, 2000, 'Ben', 'Programer');
```

![Untitled](/images/2020-01-07/2020-01-07-02.png){:width="800px"}

設定 setTimeout 時會回傳一個 id

```javascript
function sayHi() {
    console.log('hi');
}
let timerId = setTimeout(sayHi, 10000);
console.log(timerId);
```

![Untitled](/images/2020-01-07/2020-01-07-03.png){:width="800px"}

依據不同定時器，id 數值會產生變化，用來做唯一判別

```javascript
function sayHi() {
    console.log('hi');
}
let timerId = setTimeout(sayHi, 10000);
let timerId2 = setTimeout(sayHi, 20000);
console.log(timerId);
console.log(timerId2);
```

![Untitled](/images/2020-01-07/2020-01-07-04.png){:width="800px"}

透過定時器 id 和 clearTimeout 方法，可以取消定時器

```javascript
function sayHi() {
    console.log('hi');
}
let timerId = setTimeout(sayHi, 10000);
let timerId2 = setTimeout(sayHi, 20000);
console.log(timerId);
console.log(timerId2);
clearTimeout(timerId2);
```

---

setInterval 與 setTimeout 的用法是相同的，參數用法也一樣

只是 setTimeout 只執行一次

但是 setInterval 是定時執行

範例：

每一秒鐘執行一次

第五秒時取消結束

```javascript
let timerId = setInterval(() => console.log('tick'), 1000);
setTimeout(() => { clearInterval(timerId); console.log('stop'); }, 5000);
```

![Untitled](/images/2020-01-07/2020-01-07-05.png){:width="800px"}

注意一個小地方：如果使用 alert 彈跳視窗搭配訂時器的話，Chrome/Opera/Safari 的處理方式並不一致，會得到不同結果

---

遞迴版本的 setTimeout，這種方式其實比 setInterval 更靈活

```javascript
let timerId = setTimeout(function tick() {
    console.log('tick');
    timerId = setTimeout(tick, 1000);
}, 1000);
```

![Untitled](/images/2020-01-07/2020-01-07-06.png){:width="800px"}

![Untitled](/images/2020-01-07/2020-01-07-07.png){:width="800px"}

可以依據不同狀態，動態的增加、減少秒數

對於 setInterval 而言，內部的訂時器會定時執行 function，但並不管每一個 function 中所花費的時間

而遞迴版本的 setTimeout 是在每一次 function 執行完後，再執行下一個 function，所以間隔時間是一致的

沒有那一種比較好的說法，要看使用時機及對象才能決定