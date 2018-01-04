```c
/* 程式功能: 輸入5個實數，計算並輸出其平均值 */
#include <stdio.h>
void main(void) {
    int inv=0;        /*計數用變數*/
    double sum=0;     /*計算總和*/
    float Data;       /*輸入值存在Data變數*/
    do {
        printf("輸入實數:");     /*在螢幕上顯示字串*/
        scanf("%f", &Data); /*由鍵盤輸入數值*/
        sum = sum + Data;  /*將輸入值加到sum */
        inv = inv+1;
    } while(inv < 5);                   /*若inv小於5，繼續執行*/
    printf( "平均值= %f ",sum/5.0); /*印出平均值*/
    printf( "\n");                 /*換行*/
}
```
