# c语言文件读写操作
## 文件指针File
### 1.定义
1.FILE类型是系统预先设定的一种结构体指针struct _iobuf  
2.FILE* 指针作为文件句柄，是文件访问的唯一标识，它由fopen函数创建，fopen打开文件成功，则返回一个有效的FILE*指针，否则返回空指针NULL //[nʌl]  
3.定义方式  
`FILE *fp=fopen(char *filename,char *mode);`  
或
```
FILE *fp;
fp=fopen(char *filename,char *mode);
```
## 一般读写函数

> 字符的读写函数：fgetc()函数和fputc()函数  
字符串的读写函数：fgets()函数和fputs()函数  
数据块的读写函数，主要针对二进制文件：fread()函数和fwrite()函数  
格式化的读写函数，主要针对文本文件：fscanf()函数和fprintf()函数  

## 字符串读写
### 1、fputs()函数
格式：fputs(s,fp);
功能：用来将一个字符串写入指定的文本文件。
其中，fp为文件指针，s可以是字符数组名，字符型指针变量或字符串常量。该函数的功能是将字符串s写入由fp指向的文件中，字符串末尾的‘\0’字符不允写入。
返回值：执行成功，返回所写的最后一个字符；否则，返回EOF。
```
//fputs

#include <stdio.h>
#include <stdlib.h>
void main(){
    FILE *fp;
    char s[][15]={"BeiJing","ShangHai","GuangZhou","NanJing","HangZhou"};
    if((fp=fopen("c:\\1\\wenjiu3.txt","w"))==NULL)
    /*只能相应文件，创建不了文件夹，fopen()函数会寻找相应位置文件夹然后在其中创建所需文件。
    当没有所需文件夹的时候，系统会自动跳转到NULL!*/
    {
        printf("Cannot open the file,strike any key to exit!\n");
        getchar();
        exit(0);
    }
    for(int i=0;i<6;i++){
        fputs(s[i],fp);
        fputs("\n",fp);
    }
    fclose(fp);
}//输出在文件中的第六行，会出现“'jb”这个字符串
```
### 2、fgets()函数
格式：fgets(s,n,fp);

功能：用于从指定的文件中读一个字符串到字符数组中。

其中，s可以是字符型数组名或字符串指针,n是指定读入的字符个数；fp为文件指针。n是一个正整数，表示从文件中最多读取n-1个字符，并将字符串指针s定位在读入的字符串首地址。fgets()函数从文件中读取字符直到遇到回车符或EOF为止，函数会在最后一个字符后加上字符串结束标志’\0’；若有EOF，则不予保留。

返回值：该函数如果执行成功，返回读取字符串；如果失败，则返回空指针NULL，这时，s中的内容不确定。
```
//fgets()函数

#include <stdio.h>
#include <stdlib.h>
void main(){
    FILE *fp;
    char s[6][15];
    if((fp=fopen("c:\\1\\wenjiu3.txt","r"))==NULL){
        printf("Cannot open the file,strike any key to exit!\n");
        getchar();
        exit(0);
    }
    for(int i=0;i<6;i++){
        fgets(s[i],15,fp);
        if(i%2==0)
            printf("%s",s[i]);
    }
    fclose(fp);
}
```
## 格式化读写
### 1.fprintf()函数
格式：fprintf(fp, format, arg1, arg2,….,argn);

功能：fprintf()用来将输出项按指定的格式写入指定的文本文件中，其中格式化规定与printf()函数功能相似，所不同的只是fprintf()函数是将输出的内容写入文件中，而printf()函数则是在屏幕输出。

其中，fp为文件指针，format为指定的格式控制字符串；arg1~argn为输出项，可以是字符、 字符串或各种类型的数值。该函数的功能是按格式控制字符串format给定的格式，将输出项arg1,arg2,…..,argn的值写入fp所指向的文件中。

返回值：如果函数执行成功，返回实际写入文件的字符个数；若出现错误，返回负数。
> fprintf()中格式控制的使用与printf()相同。
例如：fprintf(fp,”r=%f,area=%f\n”,r,area);
```
//fprintf()函数

#include <stdio.h>
#include <stdlib.h>
void main(){
    FILE *fp;
    int data[1000];
    for(int i=0;i<1000;i++)
        data[i]=(rand()%1000);//产生0~999的随机整数
    if((fp=fopen("c:\\1\\wenjiu5.txt","w"))==NULL){
        printf("Cannot open the file,strike any key to exit!\n");
        getchar();
        exit(0);
    }
    for(i=0;i<1000;i++){
        if(i%20==0)
            fprintf(fp,"\n%5d",data[i]);
        else
            fprintf(fp,",%5d",data[i]);
        if((i+1)%20==0)
            fprintf(fp,"\n");
    }
    fclose(fp);

}
```

### 2、fscanf()函数
格式：fscanf(fp,format,arg1,arg2,…..,argn);

功能：fscanf()用来按规定的格式从指定的文本文件中读取数据。它与scanf()函数的功能相似，都是按规定的格式读数据的函数，只是fscanf()函数读的对象不是键盘区，而是文件。

其中，fp为文件指针，format为指定的格式控制字符串；arg1~argn为输入项的地址。该函数的功能是从文件指针fp所指的文本文件中读取数据，按格式控制字符串format给定的格式赋予输入项arg1,arg2,…..,argn中。

返回值：如果该函数执行成功，返回读取项目个数；如果遇到文件末尾，返回EOF;如果赋值失败，返回0.

> fscanf()中格式控制的使用与scanf()相同。
例如：fprintf(fp,”%d,%d\n”,&a,&b);
```
//fscanf()函数

#include <stdio.h>
#include <stdlib.h>
void main(){
    FILE *fp;
    int data[1000];
    if((fp=fopen("c:\\1\\wenjiu5.txt","r+"))==NULL){
        printf("Cannot open the file,strike any key to exit!\n");
        getchar();
        exit(0);
    }
    for(int i=0;i<1000;i++){
        if(i%20==0)
            fscanf(fp,"\n%5d",&data[i]);
        else
            fscanf(fp,",%5d",&data[i]);
    }
    fseek(fp,0,SEEK_END);
    fprintf(fp,"\n\n");
    int j=0;
    for(i=0;i<1000;i++){
        {
            if(data[i]%7==0)
            {
                if(j%20==0)
                    fprintf(fp,"%5d",data[i]);
                if((j+1)%20==0) fprintf(fp,"\n");
                j++;
            }
        }
        fclose(fp);
    }
}
```
