---
title: C 語言的總覽
---
一個 C 語言程式長得如下的範例:

```c
/*
 * 註解
 */
#include <stdio.h>
int x;
int main() {
    int y; // main裡的y
    x = 1;
    y=foo(x); // 這裡的y是main裡的y
    printf("I have done\n");
    return 0;
}
int foo(int z) {
    int y; // foo裡的y
    return z;
}
```

C 語言是由一個以上的函數所組成的，函數的名稱只能宣告一次，其中有個特別的函數叫做 main，是 C 程式開始執行的地方。以 `/* 註解 */` 括起來的部分，以及 `//` 後面直到行尾的部分是註解，所謂註解是指該部分只是給人看的，並不會影響 Compiler 的編譯結果。原始程式碼由 `#` 號開頭的部分，是交由前置處理器來處理；`#include` 這個前置處理器命令，是把 Source code file 和其後的 Header file 合成為另一個修改過的 Source code file。換言之，`#include <stdio.h>` 其功能就好比我們用文字處理器將系統目錄內的 `stdio.h` 檔案插入到 `#include` 的位置上。

C 語言並不要求一定要引入 `stdio.h`，此範例要寫 `#include <stdio.h>` 的原因在於程式內使用了 `printf` 函數，這個函數是存在於系統程式庫內，為了讓 Compiler 了解 `printf` 是在別的地方定義的，因此要將宣告 `printf` 長相的 `stdio.h` 包含進來。

有時候你會看到

```c
#include "my.h"
```

`#include` 後面的檔名可用 `<>` 或 `"" `括起來，其差別在於 `<>` 表示系統的引入檔，在 UNIX 的 `gcc` 會去目錄 `/usr/include` 下面找,`"" `則表示使用者的引入檔，preprocessor 會在和這個 `.c` 一樣的目錄下尋找。為甚麼要分 `<>` 或 `"" `這兩種呢？硬碟上的空間被分成很多樹狀的目錄，不同的目錄下可以存在同名的目錄，這就像每個系每個年級都有 26 號同學，要區分這些 26 號同學，我們得說 『資管大一 26 號』或『跟我在同一個房間內的 26 號』. `#include <26 號>` 就像說 『資管大一 26 號』，`#include "26 號"` 則像『跟我在同一個房間內的 26 號』。

第二行的

```c
int x;
```

是宣告全域 (global) 變數 `x` 的型態為 int(整數)。合法的變數名稱是由英文字母或底線開頭，後面可加上不定長的文數字和底線，如 `_x123`。所謂變數宣告是要求 compiler 在某個記憶體上配置足夠容納該型態的空間，對 C 語言來說，int 型態比較複雜一點，其大小在不同的平台上可能不同，但以目前 32 bits 的機器而言，int 一般為 4 個 bytes 大小，因此 `int x;`這個指令，是要 compiler 在某塊記憶體上 (對程式設計師而言不用管實際上在哪裡) 分配 4 個 bytes 準備放資料，而這 4 個 bytes 的記憶體我們在程式內就以 `x` 這個符號來代表他。凡是宣告在函數之外的變數，都可稱為全域變數。

函數的宣告包含幾個部分：傳回值、函數名稱、`()` 內的參數列、以及 `{}` 內的程式碼。

```c
int main() {}
```

表示 `main` 這個函數不需要參數，其傳回值的型態為 int

每一個函數寫作的開始和結束點，是由 `{}` 來表達。函數可以有多行的敘述 (statement)。每一個敘述由分號 `;` 或右大括弧 `}` 結尾。敘述分成兩大類，第一類是變數宣告，第二類是運算指令，運算指令可以是運算式或流程控制敘述。敘述只能寫在函數裡面，不像變數可以宣告在函數外面成為全域變數。C 語言要求運算指令內所用到的變數必須在該運算指令前宣告。變數宣告的寫法為

```c
型態名稱 變數名稱 ;
```

C 語言的合法型態名稱及意義請參見型別的詳細說明。

```c
int y;
```

要 compiler 準備 4 個 bytes 的空間，以存放 int，並將該空間以 `y` 這個符號代表。

```c
x = 1;
```

表示將整數 1 複製到 `x` 所代表的記憶體。此處的 `x` 指的是宣告在 `main` 函數之前的 `x`

```c
y=foo(x);
```

表示將整數 `x` 的值複製到堆疊上，CALL foo，然後將傳回值 (可能是放在暫存器或某塊記憶體上面) 複製到整數 `y`。詳情見函數與遞迴

```c
return 0;
```

表示將整數 0 回傳給呼叫 `main` 的函數。是誰呼叫 `main` 呢? 這就由作業系統負責了，此處不做詳述。

```c
int foo(int z){}
```

宣告呼叫 `foo` 這個函數時，需要傳入一個 int 當參數 (事實上此 int 是放在稱為 Stack 的某塊記憶體上)，這個在堆疊上的變數我們稱之為 z。

```c
int y;
```

在堆疊上產生新的變數 `y`，此 `y` 和 `main` 裡面宣告的 `y` 是完全不同的變數。

```c
return z;
```

表示將整數 `z` 記憶體的內容複製到暫存器上，然後以此數值當做 `foo` 函數的傳回值。

如果一個變數定義在函數的外面 (如範例裡的 `x`)，則此變數稱為 global variable(全域變數)。全域變數的可見範圍 (意思是哪些函數) 是程式內的所有函數 (前面有 `static`, `extern` 等修飾字的情況於後面的章節再討論)；其存在期間從該程式執行就存在，程式結束該變數才會消失。

如果變數是定義在函數裡面 (如範例裡的 `y`)，則此變數稱為 local variable(區域變數) 或 auto variable(自動變數)。區域變數的可見範圍僅侷限於定義該變數的函數；其存在期間則是開始於該函數被呼叫時，結束於該函數 `return` 時。換言之，如果 `foo()` 被呼叫了兩次，則 `foo` 裡宣告的 `y` 就會產生兩次。在 C 語言的執行環境下，區域變數會存放在堆疊上。

寫作程式時，需注意以下風格：

- 程式開頭應加註解，說明用途，建立時間，修改時間與內容，作者，使用工具
- 函數的開頭也應對此函數註解說明
- 見到 {應內縮四個空格，以讓程式容易閱讀
- 每一敘述應獨立一行，也就是說看到;{應該換行

以下是寫作的範例

```c
/* 
 * Program Name: h2.c
 * Subject: "Introduction to Programming" Homework for practice of loop and I/O.
 * Since 1997/10/27
 * 2004/03/03, add more comment
 * 2008/09/28, correct comments typing error
 * ToolKit: gcc
 * Author: Shiuh-Sheng Yu
 *         Dept. of Information Management
 *         National ChiNan University
 */
#include <stdio.h>
 
/**
 * print out rectangle
 * @param size length of the rectangle
 */
void print_rectangle(int size) {
    int i, j;
    for (i = 1; i <= size; i++) {
        for (j = 1;  j <=  size; j++) {
            printf("*");
        }
        printf("\n");
    }
}

/**
 * Read size from keyboard as long as the size is between 1 and 40,
 * then call print_rectangel to print on screen
 */
int main() {
    int size = 0;
    while (scanf("%d",&size) > 0 && size > 0 && size < =  40) {
        print_rectangle(size);
    }
}
```

## 常用的 IO 函數

C 語言沒有定義標準函數庫 (Library), 但因為 IO 是撰寫程式幾乎一定要用到的功能, 因此我們在此說明不同平台上都會提供的 `printf` 和 `scanf` 函數, 以方便讀者寫作程式。這兩個函數或許在不同平台上的某些特殊用法會有點小出入, 但大致上是相同的。

最簡單的 `printf` 用法如下：

```c
printf("Hello World.\n");
```

這會在標準輸出裝置 (內定是螢幕) 上印出 Hello World。然後換行 `'\n'` 表示換行字元。如果我們要印出變數的內容，可以使用如下的形式

```c
#include <stdio.h>
int main() {
    int varInt = 0;
    long varLong = 0;
    float varFloat = 2;
    double varDouble = 0;
    char varChar = 'A';
    char *varString = "Hello";
    printf("varInt=%d, varLong=%ld, varFloat=%f, varDouble=%lf, varChar=%c, varString=%s\n",varInt, varLong, varFloat, varDouble, varChar, varString);
}
```

`printf` 的第一個參數是一個字串，字串內以 `%` 開頭的符號有特別意義，表示此部份要用後面的參數取代。`%d` 或 `%i` 以十進位數字表示 int， `%x` 以十六進位表示 int，`%f` 表示浮點數 (float or double)，`%c` 表示字元，`%s` 表示字串，`%%` 表示 `%`，`%3d` 表示最少 3 個數字靠右不足 3 個填空白，`%-3d` 表示最少 3 個數字靠左不足 3 個填空白，`%03d` 表示最少 3 個數字靠右不足 3 個填 0。如果要印出 `%` 號的話怎麼辦，那就用 `%%` 來代表了。`printf` 相關參數如下

- `%d` `%i` 十進位整數
- `%u` unsigned 十進位整數
- `%x` unsigned 16 進位整數，小寫表示 (a-f)
- `%X` unsigned 16 進位整數，大寫表示 (A-F)
- `%o` unsigned 8 進位整數
- 以上加 `-` 和數字表示左對齊最少幾位，如 `%-9d` 表示左切齊最少印 9 位
- 以上加 `+` 和數字表示右對齊最少幾位，如 `%+9d` 表示右切齊最少印 9 位，不足處補空白
- 以上加 `+` 和 0 開頭的數字表示右對齊最少幾位，不足補 0，如 `%+09d` 表示右切齊最少印 9 位，不足處補 0
- `%ld` 以十進位印出 long
- `%lld` 以十進位印出 long long
- `%f` 浮點數 float
- `%e` `%E` 科學表示法浮點數 double, `%e` 小寫 `%E` 大寫
- `%g` `%G` 依照 double 的數值自動選擇以 %f 或 `%e` 格式印出
- 以上加 `-` 和具有小數點的數字表示左對齊最少幾位，如 `%-9.2f` 表示左切齊最少印 9 位，其中小數點以下 2 位
- 以上加 `+` 和具有小數點的數字表示右對齊最少幾位，如 `%=9.2f` 表示右切齊最少印 9 位，其中小數點以下 2 位
- `%c` unsigned char
- `%s` string(array of char terminated by 0)
- `%p` pointer to void
- `%%` 印出 %

`printf()` 傳回整數，代表輸出的字元數

`scanf` 用來做輸入，其參數和 `printf` 很類似，但要記得

- int 用 `%i` 或 `%d`
- unsigned 用 `%u`
- float 用 `%f`
- double 用 `%lf`
- long 用 `%ld`
- long long 用 `%lld`
- short 用 `%hd`

`scanf()` 傳回整數，代表有幾個成功讀入。

```c
#include <stdio.h>
int main() {
    int varInt;
    long varLong;
    float varFloat;
    double varDouble;
    scanf("%d%l%f%lf", &varInt, &varLong, &varFloat, &varDouble);
}
```
