```c
/* 程式功能: 輸入一個正整數N，並判別此數是否為質數 */
#include <math.h>
#include <stdio.h>
main() {
    long N;
    int i;
    int key =0;
    
    printf("輸入一正整數N:");
    scanf("%ld", &N);
    if (N == 1) {       /*小於2的數非質數*/
        printf("%ld非質數\n", N);
    } else {
        if (N <= 3) {       /* 2 和 3 為質數*/
            printf("%ld是質數\n", N);
        } else {
            for (i=3; i <= N/2; i++) {
                if (N % i == 0) {
                    printf("%ld非質數\n", N);
                    return;
                }
            }
            printf("%ld是質數\n", N);
        }
    }
}
```
