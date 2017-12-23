---
title: 程式寫作概論
---
## 文章的組成

### 書的結構

- 書名
- 作者
- 目錄
- 章
    - 標題
    - 節
        - 段落
            - 句子
- 附錄
- 索引

### 學術論文的結構

- 標題
- 作者
- 摘要
- 節
    - 段落
        - 句子
- 引用文獻

## 句子的組成

- 單字
- 文法: 主詞 + 動詞 + 受詞

## 程式語言

### C語言的結構

- 註解 (comments)(給人看的)
- 函數 (functions)
    - 函數名稱 (function name)
    - 參數 (parameters)
    - 區域變數宣告 (local variable declarations)
    - 敘述 (statements)

### 敘述的組成

- 常數、變數、運算符號、分號
- `if () {} else {}`
- `while () {}`
- `do {} while {}`
- `switch () { case 1: case 2: case 3}`
- `break`
- `continue`
- `goto`

### Java 語言的結構

- 註解 (comments)(給人看的)
- 類別 (class)
	- 變數宣告 (variable declarations)
	- 函數宣告 (method declarations)
		- 函數名稱 (function name)
		- 參數 (parameters)
		- 區域變數宣告 (local variable declarations)
		- 敘述 (statements)

## 範例

文字描述如何判斷閏年 透過西元幾年來判斷閏年：4 的倍數的年份是閏年，100 的倍數的年份是平年，400 的倍數的年份是閏年，也就是說，400 年中，只有 97 個閏年 (4 的倍數有 100 個，100 的倍數有 4 個，400 的倍數有 1 個)。

以流程圖 (Flow Chart) 來表示

```flow
mod4=>condition: year % 4 == 0
mod100=>condition: year % 100 == 0
mod400=>condition: year % 400 == 0
no4=>operation: 不是閏年
yes100=>operation: 閏年
no400=>operation: 不是閏年
yes400=>operation: 閏年

mod4(no, right)->no4
mod4(yes)->mod100
mod100(no, right)->yes100
mod100(yes)->mod400
mod400(no, right)->no400
mod400(yes)->yes400
```
  
  
以程式來表示

```c
#include <stdio.h>
int main() {
    int year;
    printf("Please input an the year:"); // 提示使用者輸入資料
    if (scanf("%d", &year) != 1) { // 輸入年份year
        printf("輸入錯誤\n");
        return;
    }
    if (year%4 == 0) {
        if (year%100 == 0) {
            if (year%400 == 0) {
                printf("%d是閏年\n", year);
            } else {
                printf("%d不是閏年\n", year);
            }
        } else {
            printf("%d是閏年\n", year);
        }
    } else {
        printf("%d不是閏年\n", year);
    }
}
```

如果我們把所有可得到閏年的路徑，集合在一起，則更簡潔的寫法是

```c
#include <stdio.h>
int main() {
    int year;
    printf("Please input the year:"); // 提示使用者輸入資料
    if (scanf("%d", &year) != 1) { // 輸入年份year
        printf("輸入錯誤\n");
        return;
    } 
    if (year%400 == 0 || year%4 == 0 && year%100 != 0) {
        printf("%d是閏年\n", year);
    } else {
        printf("%d不是閏年\n", year);
    }
}
```

Java的範例

```java
import java.util.*;
public class Leap {
    public static void main(String[] argv) throws Exception {
        int year = 1984;
        System.out.println("Please input the year:");
        Scanner in = new Scanner(System.in);
        if (!in.hasNextInt()) {
            System.out.println("輸入錯誤");
            return;
        }
        year = in.nextInt();
        if (year%400 == 0 || year%4 == 0 && year%100 != 0) {
            System.out.println(year+"是閏年");
        } else {
            System.out.println(year+"不是閏年");
        }
    }
}
```
