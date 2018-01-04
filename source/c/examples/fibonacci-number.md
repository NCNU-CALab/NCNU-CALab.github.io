```c
/* 程式功能: 輸入一個正整數N，計算第N項之費氏級數 (使用for的流程控制) */
#include <stdio.h>
void main(void) {
    int N;
    int i, Y=0, X=1, Fn, Z=0;
    printf("計算正整數N的費氏級數:\n");
    printf("N = ");
    scanf("%d", &N);
    if (N == 1) {
        Fn = 1;
    } else {
        for(i = 2; i <= N; i++) {
            Z = X + Y;
            Fn = Z;
            Y = X;
            X = Z;
        }
    }
    printf("第 %d 項費氏級數= %d ", N, Fn);
    printf("\n");
}
```
