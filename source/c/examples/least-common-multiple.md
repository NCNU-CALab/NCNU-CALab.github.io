```c
int gcd(int x, int y) {
    int tmp;
    // 如果x < y 則下面的迴圈執行第一次時就會交換x,y了
    while (x % y != 0) {
        tmp = y;
        y = x % y;
        x = tmp;
    }
    return y;
}
int lcm(int x, int y) {
    return x * y / gcd(x,y);
}
```
