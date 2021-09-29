---
layout: post
title:  "Closure，即匿名函式（Anonymous functions），也叫閉包"
subtitle: "PHP 基本"
date:   2019-10-11 09:00:00 +0800
categories: PHP
tags:
- "PHP"
---

允許臨時建立一個沒有指定名稱的函式，經常用作回撥函式引數的值

```php
$greet = function() {
    echo 'Hello there.';
};

$greet();
```

![Untitled](/images/2019-10-11/2019-10-11-01.png)

傳值進去

```php
$name = 'friend';
$greet = function() use ($name) {
    echo 'Hello there, ' . $name . '.';
};

$greet();
```

![Untitled](/images/2019-10-11/2019-10-11-02.png)

注意，如果這樣寫，會導致語法錯誤，盡管看起來很合理...

```php
$greet = function(name) {
    echo 'Hello there, ' . $name . '.';
};
```

傳多個值進去

```php
$name = 'friend';
$ben = 'Ben';

$greet = function() use ($name, $ben) {
    echo 'Hello there, ' . $name . ', ' . $ben . '.';
};

$greet();
```

![Untitled](/images/2019-10-11/2019-10-11-03.png)