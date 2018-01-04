```c
#include <stdio.h>
#include <stdlib.h>
#include <fcntl.h>
#define size 512
int main(){
    char filename[20],copyfile[20];
    char buf [size];
    int fd1,fd2,fd3;
    //see if correct number of command line arguments
    printf("please input filename:\n");
    scanf("%s",filename);
    if ((fd1= open(filename,O_RDONLY)) == -1) {
        printf("cannot open file.");
        exit(1);
    }
    printf("please input copyfile:\n");
    scanf("%s",copyfile);
    fd3=open(copyfile,O_CREAT|O_WRONLY,0644);
    while ((fd2=read(fd1,buf,size))>0){
        write(fd3,buf,fd2);
    }
    close(fd1);
    close(fd3);
}
```
