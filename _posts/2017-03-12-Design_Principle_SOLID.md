---
layout: post
title: "Design Principle - SOLID"
author: "Iverson Hong"
modified: 2018-05-20
tags: [Design Pattern]
---

程式設計上遵循 SOLID 這五項基本原則，可寫出好維護易擴充的程式架構
 
## Single Responsibility Principle (SRP)##

**單一職責**

> A class should have only one reason to change

一個class只專心做一件事。

如一個class要負責過多職責，等於把這些職責耦合在一起，當其中一個職責變化或修改時，其他的職責可能發生意想不到的錯誤。

## Open Closed Principle (OCP)##

**開放封閉原則**

> software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification

軟體設計應該對擴充是開放的，但對修改是封閉的。我們希望在不修改現有程式碼的前提之下，增加新的程式來擴充新的功能。

理想上是這樣，但如何做到呢？首先要先清楚地建立程式模組間的上下階層關係，如A模組使用到B模組，B模組使用到C模組，A->B->C

我們希望A改變時，B不受到影響，B改變時，C不受到影響。也就是C為此架構中的最底層，也是最需要被保護的部分。因此，我們可以透過隱藏C的內部資訊來保護C不被B所影響，
實際做法可使用抽象介面來隔離上下階層的關係，使得B不直接依賴C，而是透過介面的方式來操作C(高階模組不應該依賴於低階模組，應該依賴抽象)。可藉此達到不修改原有的C但達到擴充功能的需求。

## Liskov Substitution Principle (LSP)##

**Liskov 替換原則**

> Inheritance should ensure that any property proved about supertype objects also holds for subtype objects

## Interface Segregation Principle (ISP)##

**介面隔離原則**

## Dependency inversion principle (DIP)##

**依賴反轉原則**

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)