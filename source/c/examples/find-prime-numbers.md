```c
/**
 * Program Name: prime2.c
 * Purpose: List all prime numbers less than or equal n
 * Author: Shiuh-Sheng Yu
 * Department of Information Management National ChiNan University
 * Since: 2004/12/08
 * Modified Date:
 */
#include <stdio.h>
#include <stdlib.h>
#include <sys/timeb.h>
int main() {
    int n=10000000, checkNum, total=1, i, j, found;
    struct timeb begin, end;
    int primes[1000000];
    printf("Please input an integer:");
    scanf("%d", &n);
    if (n <= 1) {
        printf("Please input an integer larger than 1.\n");
        return 1;
    }
    ftime(&begin);
    primes[0] = 2;
    j = 0;
    for (checkNum = 3; checkNum <= n; checkNum++) {
        found = 1;
        for (i = 0; i <= j; i++) {
            if (checkNum%primes[i] == 0) {
                found = 0;
                break;
            }
        }
        if (found) {
            primes[total++] = checkNum;
            if (primes[j]*primes[j] <= checkNum) {
                j++;
            }
        }
    }
    ftime(&end);
    printf("There are %d primes less than or equal %d, used %dms\n", total, n, end.time*1000+end.millitm-begin.time*1000-begin.millitm);
}
```

***

```c
/**
 * Program Name: prime.c
 * Purpose: List all prime numbers less than or equal n
 * Author: Shiuh-Sheng Yu
 * Department of Information Management National ChiNan University
 * Since: 2004/11/21
 * Modified Date:
 */
#include <stdio.h>
#include <stdlib.h>
#include <sys/timeb.h>
int main() {
    unsigned int n=10000000, checkNum, multiple, total;
    struct timeb begin, end;
    unsigned char *dividable; // 每一個bit代表該位置是否可被整除
    printf("Please input an integer:");
    scanf("%ud", &n);
    ftime(&begin);
    dividable = (unsigned char *)calloc((n+7)/8,1);
    if (dividable == NULL) {
        printf("No enought memory.\n");
        return;
    }
    total = 0;
    for (checkNum = 2; checkNum <= n; checkNum++) {
        if (!(dividable[checkNum/8] & 1 << checkNum % 8)) { // 如果不被整除
            total++;
            // 消去checkNum的倍數
            for (multiple = checkNum << 1; multiple <= n; multiple += checkNum) {
                dividable[multiple/8] |= 1 << multiple % 8;
            }
        }
    }
    ftime(&end);
    printf("There are %d primes less than or equal %d, used %dms\n", total, n, end.time*1000+end.millitm-begin.time*1000-begin.millitm);
}
```
