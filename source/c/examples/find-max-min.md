```c
/* 程式功能: 輸入10個數，並從中找出最大與最小的數 */
#include <stdio.h>
void main(void) {
    int i=1;
    float L, S, X;      /* L=最大數，S=最小數，X=輸入值*/
    printf("輸入一數值: ");
    scanf("%f", &X);     /*由鍵盤輸入數值*/
    L = S = X;
    do {
        printf("輸入一數值: ");
        scanf("%f",&X);
        if (X > L) {
            L = X; /*若輸入的值大於L，將L設成輸入值X */
        }
        if (X < S) {
            S = X; /*若輸入的值小於S，將S設成輸入值X */
        }
        i = i + 1;
    } while (i < 10);
    /*印出總和*/
    printf("最大值為 %f, 最小值為 %f ", L, S) ;
    printf("\n") ;
}
```
