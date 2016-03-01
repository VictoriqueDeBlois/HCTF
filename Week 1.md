#Week 1
##CRYPTO
###密码学从0开始之1
>题目ID： 50 <br>
题目描述： http://ctf.lazysheep.cc:8081/cry1.html<br>
flag不是标准格式，提交你解出的明文就行，flag全是大写<br>

* 莫尔斯电码
* 于是自己写了个C
* 顺便写了更多的功能
  * 可以自己定义替换的密码本
  * 加密和解密
  * 程序的根目录下最多需要三个txt: cipher 密文,clear 明文,form 替换密码本
  * 字母间隔用'\036'

```c
#include <stdio.h>
#include <string.h>
#include <stdlib.h>
#include <ctype.h>

typedef struct formNode
{
    char *str ;
    char alpha ;
    struct formNode *next ;
}FORM ;

int flag = 0 ;

char *delspace(char *str)
{
    char *done , *pstr , *pdone ;
    int len ;
    len = strlen(str) ;
    pstr = str ;
    done = (char*)calloc(len + 1 , sizeof(char)) ;
    pdone = done ;
    while(*pstr != '\0')
    {
        if (isspace(*pstr))
            pstr ++ ;
        else
        {
            *pdone = *pstr ;
            pdone ++ ;
            pstr ++ ;
        }
    }
    *pdone = '\0' ;
    len = strlen(done) ;
    strncpy(str , done , len + 1) ;
    free(done) ;
    return str ;
}

FORM *createForm(FILE *formText)
{
    FORM *phead , *pnew , *ptail ;
    char text[100] ;
    char *ptext ;
    rewind(formText) ;
    if (fgets(text , 100 , formText) == NULL)
        return NULL ;
    if (*(text + strlen(text) - 1) == '\n' )
        *(text + strlen(text) - 1) = '\0' ;
    ptext = delspace(text) ;
    phead = (FORM*)calloc(1 , sizeof(FORM)) ;
    pnew = ptail = phead ;
    phead ->alpha = *ptext ;
    ptext ++ ;
    phead ->str = strdup(ptext) ;
    while (fgets(text , 100 , formText) != NULL)
    {
        if (*(text + strlen(text) - 1) == '\n' )
            *(text + strlen(text) - 1) = '\0' ;
        ptext = delspace(text) ;
        pnew = (FORM *)calloc(1 , sizeof(FORM)) ;
        pnew ->alpha = *ptext ;
        ptext ++ ;
        pnew ->str = strdup(ptext) ;
        ptail ->next = pnew ;
        ptail = pnew ;
    }
    return phead ;
}

char *searchFormAlpha (char ch , FORM *form)
{
    if (isalpha(ch))
        ch = toupper(ch) ;
    while (form != NULL)
    {
        if (ch == form ->alpha)
            return form->str ;
        form = form ->next ;
    }
    return NULL ;
}

char searchFormStr (const char *str , FORM *form)
{
    while(form != NULL)
    {
        if (strcmp(str , form ->str) == 0)
            return form ->alpha ;
        form = form ->next ;
    }
    return 0 ;
}

int check (FORM *form)
{
    int count = 0 ;
    while(form != NULL)
    {
        printf("%c %s\n" , form ->alpha , form->str) ;
        form = form ->next ;
        count ++ ;
    }
    return count ;
}

char *fgetsrt(FILE *fp)
{
    char *done , *prt ;
    char ch ;
    done = (char*)calloc(20 , sizeof(char)) ;
    prt = done ;
    while((ch = fgetc(fp)) != '\036')
    {
        if (ch == EOF)
        {
            flag = 1 ;
            return done ;
        }
        *prt = ch ;
        prt ++ ;
    }
    return done ;
}

int decrypt (FILE *cipher , FILE *clear , FORM *form)
{
    char *str ;
    char ch ;
    int count  = 0 ;
    rewind(cipher) ;
    rewind(clear) ;
    while(flag != 1)
    {
        str = fgetsrt(cipher) ;
        if (*str != '\0')
        {
            ch = searchFormStr(str , form) ;
            if (isalpha(ch))
                ch = toupper(ch) ;
            if (ch == 0)
                fprintf(clear , "/%s/" , str) ;
            else
                fprintf(clear , "%c" , ch) ;
            free(str) ;
            count ++ ;
        }
        else
        {
            if (flag != 1)
                fprintf(clear , " ") ;
            free(str) ;
            count ++ ;
        }
    }
    return count - 1 ;
}

int encrypt (FILE *cipher , FILE *clear , FORM *form)
{
    char tmp ;
    char *str ;
    int count = 0 ;
    rewind(clear) ;
    rewind(cipher) ;
    while ((tmp = fgetc(clear)) != EOF)
    {
        if (tmp == ' ')
        {
            fprintf(cipher,"\036") ;
            count ++ ;
            continue ;
        }
        str = searchFormAlpha(tmp , form) ;
        if (str == NULL)
            fprintf(cipher ,"%c\036" , tmp) ;
        else
            fprintf(cipher ,"%s\036" , str) ;
        count ++ ;
    }
    return count ;
}

int main( int argc , char *argv[ ] )
{
    FILE *cipherText , *clearText , *formtext ;
    FORM *form ;
    char select ;
    if ((formtext = fopen("form.txt" , "r")) == NULL )
        exit(1) ;
    form = createForm(formtext) ;
    begin :
    printf("1.encrypt\n2.decrypt\n[ ]\b\b") ;
    select = getchar() ;
    if (select == '1')
    {
        if ((clearText = fopen("clear.txt" , "r")) == NULL )
            exit(1) ;
        cipherText = fopen("cipher.txt" , "w") ;
        encrypt(cipherText , clearText , form) ;
        fclose(cipherText) ;
        fclose(clearText) ;
        printf("done\n") ;
        system("pause") ;
    }
    else if (select == '2')
    {
        if ((cipherText = fopen("cipher.txt" , "r")) == NULL )
            exit(1) ;
        clearText = fopen("clear.txt" , "w") ;
        decrypt(cipherText , clearText , form) ;
        fclose(cipherText) ;
        fclose(clearText) ;
        printf("done\n") ;
        system("pause") ;
    }
    else
    {
        system("CLS") ;
        goto begin ;
    }
    return 0 ;
}
```
因为我的替换表是这样的<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/crypto.0.1.1.jpg)<br>
所以我先把原来的电码转换了一下<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/crypto.0.1.0.jpg)<br>
接着运行程序<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/crypto.0.1.2.jpg)<br>
得到flag<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/crypto.0.1.3.jpg)<br>
时针不停在转动?<br>

###密码学从0开始之1.1
>题目ID： 54<br>
题目描述： http://ctf.lazysheep.cc:8081/cry2.html<br>
你知道01的奥秘么？<br>
Hint: 这可不是啥古典密码了<br>

一开始先以为是2进制密码（培根密码或费娜姆密码）<br>
用前面的程序进行替换，结果什么都不是<br>
于是想是不是2进制的文件什么的<br>
* 于是自己写了个C
```c
#include <stdio.h>
#include <stdlib.h>

int fcount (FILE *fp)
{
    int count = 0 ;
    while(fgetc(fp) != EOF)
        count ++ ;
    return count ;
}

char convert8bit(char *str)
{
    char num = 0 ;
    if (*str == '1')
        num += 128 ;
    if (*(str+1) == '1')
        num += 64 ;
    if (*(str+2) == '1')
        num += 32 ;
    if (*(str+3) == '1')
        num += 16 ;
    if (*(str+4) == '1')
        num += 8 ;
    if (*(str+5) == '1')
        num += 4 ;
    if (*(str+6) == '1')
        num += 2 ;
    if (*(str+7) == '1')
        num += 1 ;
    return num ;
}

int *convertFile(FILE *fp , FILE *fp2)
{
    char tmp[10] ;
    rewind(fp2) ;
    rewind(fp) ;
    while(fgets(tmp , 9 , fp) != NULL)
    {
        fputc( convert8bit(tmp) , fp2 ) ;
    }
    return 0 ;
}

int main( int argc , char *argv[ ] )
{
    FILE *origin , *result ;
    origin = fopen("key.txt" , "r") ;
    result = fopen("flag" , "wb") ;
    convertFile(origin , result) ;
    return 0 ;
}
```
运行程序,得到flag文件<br>
用记事本打开什么都不是<br>
用文件识别工具识别一下<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/crypto.1.1.0.jpg)<br>
于是得到了flag<br>
![](https://github.com/VictoriqueDeBlois/HCTF/raw/master/Image/flag.png)<br>

##WEB
###WEB从0开始之PHP代码审计0
```php
<?php
require "flag.php";
if (isset($_GET['name']) and isset($_GET['password'])) {
    if ($_GET['name'] == $_GET['password'])
        print 'No,No,No!';
    else if (sha1($_GET['name']) === sha1($_GET['password']))
        die('Flag: '.$flag1);
    else
        print 'Invalid password';
}
else{
    show_source(__FILE__); 
}
?>
```
这道题，我们首先要确保name和password的值不能相同，<br>
其次，sha1加密之后的name和password的值又必须完全相同 我们知道，<br>
这时的a[0] = 1;所以name[] = 1和password[]= 2相比较，可以跳过第一个判断，<br>
而如果使用sha1对一个数组进行加密，返回的将是NULL，NULL===NULL，这是成立的，<br>
所以构造两个数组<br>
http://ctf.lazysheep.cc:8081/web1.php*?name[]=1&password[]=2*<br>
(其实我不会这个  ~~百度的~~  )
