---
layout: post
title:  "JS 函數、具名函式表示式 NFE (Named Function Expression)"
subtitle: "Named Function Expression"
date:   2020-01-05 09:00:00 +0800
categories: JavaScript
tags:
- "JavaScript"
---

在 JavaScript 中函數就是值，而每個值都有一個類型，簡言之函數就是物件

可以通過 name 屬性來取得一個函數的名稱 ( 非執行 )

第 1 種

```javascript
function sayHi() {
    return 'hi';
}

console.log(sayHi.name);
```

第 2 種

```javascript
let sayHi = function() {
    return 'hi';
};

console.log(sayHi.name);
```

第 3 種

```javascript
function f(sayHi = function() {}){
    console.log(sayHi.name);
}

f();
```

3 種的結果相同

![Untitled](/images/2020-01-05/2020-01-05-01.png){:width="800px"}

但是注意⚠️，它只取得函數名稱，並不是執行函數

---

## 具名函式表示式 NFE (Named Function Expression)

給函式表達式一個名稱，用途是在內部執行自己，外部是無法直接使用的

```javascript
let sayHi = function justSayHi(who) {
    if (who) {
        console.log(`Hello, ${who}`);
    } else {
        justSayHi('Guest');
    }
};

sayHi('Ben');
sayHi();
```

![Untitled](/images/2020-01-05/2020-01-05-02.png){:width="800px"}

經測試，如果改為 return 將會取得 undefined，目前仍未理解....

```javascript
let sayHi = function justSayHi(who) {
    if (who) {
        return `Hello, ${who}`;
    } else {
        justSayHi('Guest');
    }
};

console.log(sayHi('Ben'));
console.log(sayHi());
```

![Untitled](/images/2020-01-05/2020-01-05-03.png){:width="800px"}