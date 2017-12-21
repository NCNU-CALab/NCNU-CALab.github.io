---
title: The 3n + 1 problem
date: 2017-12-22 03:53:25
category: ACM
tags:
- 'brute force'
---

## 題目說明
考慮以下的演算法：

1. 輸入 n
1. 印出 n
1. 如果 n = 1 結束
1. 如果 n 是奇數 那麼 n=3*n+1
1. 否則 n=n/2
1. GOTO 2
1. 例如輸入 22，得到的數列： 22 11 34 17 52 26 13 40 20 10 5 16 8 4 2 1

據推測此演算法對任何整數而言會終止（當列印出 1 的時候）。雖然此演算法很簡單，但以上的推測是否真實卻無法知道。然而對所有的 n ( 0 < n < 1,000,000 ) 來說，以上的推測已經被驗證是正確的。

給一個輸入 n，透過以上的演算法我們可以得到一個數列（1 作為結尾）。此數列的長度稱為 n 的 cycle-length。上面提到的例子，22 的 cycle length 為 16。

問題來了：對任 2 個整數 i，j 我們想要知道介於 i，j（包含 i，j）之間的數所產生的數列中最大的 cycle length 是多少。

**數學式範例**
{% math %}
x = {-b \pm \sqrt{b^2-4ac} \over 2a}
{% endmath %}

## 解題思路

## Pseudo Code

## 完整程式碼

**Hello.class**
```java
public class Hello {
    public static void main(String[] argv) {
        System.out.println("Hello World");
    }
}
```
