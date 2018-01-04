```c
/* 程式功能：計算計程車費率 */
#include <stdio.h>
int main(void) {
    int i, diff, cost;
    
    printf("請輸入里程(單位為公尺):");
    scanf("%d", &i);
    if (i <= 1500)  /*里程數小於等於1500公尺*/
        cost = 70;
    else {
        diff = i - 1500; /*計算里程數超過1500公尺的里程費率*/
        if (diff%500 == 0) /*里程數不足500公尺仍以500公尺費率計算*/
            cost = 70 + (diff / 500) * 5;
        else
            cost= 70 + ((diff / 500) + 1) * 5;
    }
    printf("總共車資為: %d 元", cost);
    printf("\n");
    return 0;
}
```
