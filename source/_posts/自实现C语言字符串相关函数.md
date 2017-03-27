---
title: 自实现C语言字符串相关函数
date: 2016-03-18 09:14:45
categories: [编程学习,计算机基础,Linux_C]
tags: [Linux_C]
---
### 1. 编写函数实现`strlen()`：

```c
#include <stdio.h>
int strlen(const char string[])
{
    int i=0;
    while(string[i])
        i++;
    return i;
}

int main()
{   
	char str[100];

	printf("亲输入字符串：");
    scanf("%s",str);

    printf("字符串长度：%d\n",strlen(str));
}
```
<!--more-->
![strlen](http://7xrvo9.com1.z0.glb.clouddn.com/0318c%2Fresult-strlen.png)

### 2. 编写函数实现`strcmp()`：

```c
#include <stdio.h>
#define N 30

int strcmp(char *p1, char *p2)
{
    int result = 0;
    while (*p1 && *p2){
          if (*p1 == *p2){
             p1++;
             p2++;
             continue;
          			}
          else{
              result = *p1 - *p2;
              break;
          			}
    		}
    return result;
}

int main(int argc,char *argv[])
{

    char ch1[N],ch2[N];
    char *p_ch1,*p_ch2;
    int result;
    p_ch1 = ch1;
    p_ch2 = ch2;

    printf("亲输入字符串1:\n");
    gets(ch1);

    printf("亲输入字符串2:\n");
    gets(ch2);

    result = strcmp(p_ch1,p_ch2);

    printf("strcmp('%s','%s') = %d\n",ch1,ch2,result);

    return 0;
}
```
![strcmp](http://7xrvo9.com1.z0.glb.clouddn.com/0318c%2Fresult-strcmp.png)

### 3. 编写函数实现`strcpy()`：

```c
#include <stdio.h>
void strcopy(char *p,char *q)
{
	int i;
	for(i=0;p[i]&&q[i];i++)
	p[i]=q[i];
	p[i]='\0';
}
int main()
{
	char a[10],b[10];

	printf("请输入源字符串:");
	scanf("%s",a);

	strcopy(b,a);//b是目的字符串，a复制给b

	printf("您输入的字符串：%s\n",a);
	printf("复制给另一个字串结果：b=%s\n",b);
	return 0;
}
```
![strcpy](http://7xrvo9.com1.z0.glb.clouddn.com/0318c%2Fresult-strcpy.png)

### 4. 编写函数实现`strcat()`：

```c
#include <stdio.h>
#define N 100

char* strcat(char s1[],char s2[])
{
	int i,j;
	for(i=0;s1[i]!=0;i++)
	;
	for(j=0;s2[j]!=0;i++,j++)
	s1[i]=s2[j];

	s1[i]=0;
	return s1;
}
int main()
{
	char s1[N],s2[N],*s;

	printf("亲输入字符串1：");
	scanf("%s",s1);
	printf("亲输入字符串2：");
	scanf("%s",s2);

	s=strcat(s1,s2);
	printf("连接后的字符串：%s\n",s);

	return 0;
}
```
![strcat](http://7xrvo9.com1.z0.glb.clouddn.com/0318c%2Fresult-strcat.png)

---
