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

一個class應有一個，且只有一個理由可使其改變。

一個class只專心做一件事。如一個class要負責過多職責，等於把這些職責耦合在一起，當其中一個職責變化或修改時，其他的職責可能發生意想不到的錯誤。

## Open Closed Principle (OCP)##

**開放封閉原則**

> Software entities (classes, modules, functions, etc.) should be open for extension, but closed for modification

軟體設計應該對擴充是開放的，但對修改是封閉的。我們希望在不修改現有程式碼的前提之下，增加新的程式來擴充新的功能。

理想上是這樣，但如何做到呢？首先要先清楚地建立程式模組間的上下階層關係，如A模組使用到B模組，B模組使用到C模組，A->B->C。

我們希望A改變時，B不受到影響，B改變時，C不受到影響。也就是C為此架構中的最底層，也是最需要被保護的部分。因此，我們可以透過隱藏C的內部資訊來保護C不被B所影響，
實際做法可使用抽象介面來隔離上下階層的關係，使得B不直接依賴C，而是透過介面的方式來操作C(高階模組不應該依賴於低階模組，應該依賴抽象)。可藉此達到不修改原有的C但達到擴充功能的需求。

## Liskov Substitution Principle (LSP)##

**Liskov 替換原則**

> Subtypes must be substitutable for their base types.

子類別必須要能取代它的父類別。只有當子類別替換掉父類別且軟體行為不受到影響時，子類別才能在父類別的基礎上增加新的行為。

這常常是使用繼承時的準則，反例如下：企鵝繼承鳥類、正方形繼承矩形。這些會有什麼問題呢？
企鵝不會飛，但鳥會飛，若是企鵝繼承鳥的話，在飛這個method必須另外處理，這些另外處理常常會造成程式不可預期的錯誤，同理正方形繼承矩形時，必須在設定長跟寬時特殊處理，才會在呼叫GetArea()時得到正確結果。

解決方法應為兩者(鳥/企鵝、矩形/正方形)都繼承一共同介面，也就是上下階層不互相依賴，轉而依賴介面，其道理與開放封閉原則一致。

## Interface Segregation Principle (ISP)##

**介面隔離原則**

> Clients should not be forced to depend on methods that they do not use.

類別不應被迫實作一個它用不到的method。我們常常在使用介面時，把一堆method放入同一介面，再把此介面提供給A, B class繼承使用，
但其中有些介面中的的method在B class中並不會實作，如foo()，這時可能會在B class foo()內寫丟出exception，但這種寫法明顯違反了"**Liskov 替換原則**"。

解決方法應為再定義另一介面，其內有foo() method，B class只繼承原有介面，而A class繼承原有介面以及含有foo()的介面。

## Dependency inversion principle (DIP)##

**依賴反轉原則**

> High-level modules should not depend on low-level modules. Both should depend on abstractions.

> Abstractions should not depend on details. Details should depend on abstractions.

----------

[[C#系列文章]](http://yu-qiao-hong.github.io/tags/#C#)

[[Design Pattern系列文章]](http://yu-qiao-hong.github.io/tags/#Design Pattern)