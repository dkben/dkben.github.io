---
layout: post
title:  "Symfony 的 Serializer 序列化器，顯示A circular reference has been detected when
serializing the object of…"
subtitle: "A circular reference has been detected when serializing the object of class"
date:   2021-03-01 09:00:00 +0800
categories: Symfony
tags:
- "Symfony"
---

Symfony 的 Serializer 序列化器報錯顯示A circular reference has been detected when
serializing the object of…

![](/images/medium/1__e8CEOEmVTBb73eI83hsvNw.png)

A circular reference has been detected when serializing the object of class

在序列化類的對象時檢測到循環引用

這是因為在 ORM 中有 relation 關係，每一個 relation 關係看似屬性，但其實會關連其它 Entity 的取值方法

![](/images/medium/1__ujDTX__UuUTShf__dyqa__p3Q.png)

因為 Serializer 的套件版本更新滿快的，所以解法會因為 Symfony 版本而不同

[https://github.com/symfony/serializer](https://github.com/symfony/serializer)

目前得知有 4 種做法可以解決

1.  直接在 repository 中寫客製化的查詢返回陣列，不經過 ORM
2.  設定 Serializer 的深度 Handling Serialization Depth，沒測試過，而且感覺 Entity 要多設定很多東西不是很方便
3.  在 Entity 中使用 Groups 分組，測試過但沒有成功
4.  使用 ignored attributes 參數，忽略 relation 關聯查詢，最後是使用這個方法 (目前我用 Symfony 5.2)

![](/images/medium/1__blEDMfIH79XOO7rRO2SR2Q.png)

有取到值了

![](/images/medium/1__qkG5d2GvDh77Oupl2__iueg.png)

參考資料

> The Serializer Component  
> [https://symfony.com/doc/current/components/serializer.html](https://symfony.com/doc/current/components/serializer.html)
>
> Symfony 3.2 A circular reference has been detected (configured limit: 1)  
> [https://stackoverflow.com/questions/44286530/symfony-3-2-a-circular-reference-has-been-detected-configured-limit-1/44286659](https://stackoverflow.com/questions/44286530/symfony-3-2-a-circular-reference-has-been-detected-configured-limit-1/44286659)
>
> Serializer using Normalizer returns nothing when using setCircularReferenceHandler #24121  
> [https://github.com/symfony/symfony/issues/24121](https://github.com/symfony/symfony/issues/24121)
>
> A circular reference has been detected?  
> [https://helperbyte.com/questions/1977/a-circular-reference-has-been-detected](https://helperbyte.com/questions/1977/a-circular-reference-has-been-detected)
>
> How to get Array Results in findAll() — Doctrine?  
> [https://stackoverflow.com/questions/42910599/how-to-get-array-results-in-findall-doctrine](https://stackoverflow.com/questions/42910599/how-to-get-array-results-in-findall-doctrine)
>
> Databases and the Doctrine ORM  
> [https://symfony.com/doc/current/doctrine.html](https://symfony.com/doc/current/doctrine.html)