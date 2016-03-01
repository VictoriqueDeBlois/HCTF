#Week 2
##CRYPTO
###密码学从0开始之2.0
>题目ID： 63<br>
题目描述： http://ctf.lazysheep.cc:8082/cry2.php<br>
Hint: 开始现代密码学了，先热个身，很简单的对称密码<br>
https://zh.wikipedia.org/wiki/%E6%B5%81%E5%8A%A0%E5%AF%86<br>

思考是这样的:<br>
1.这是对称密码<br>
2.有点现代气息<br>
3.只给出了明文和密文<br>
4.明文密文flag密文的长度差不多可能一样<br>
5.能这样推出密钥的话只有像费娜姆密码那样了吧<br>

* 保存了网页
* 用winhex提出了明文 密文 flag密文(果然长度一样)
* 于是自己写了个C
```c
#include<stdio.h>
#include<stdlib.h>

char clear[19] = {
	0x74, 0x68, 0x69, 0x73, 0x69, 0x73, 0x61, 0x65, 0x61, 0x73, 0x79, 0x63, 0x72, 0x79, 0x70, 0x74,
	0x6F, 0x2E, 0x2E
};

char cipher[19] = {
	0x7A, 0x5E, 0x31, 0x33, 0x41, 0xFD, 0x75, 0xB7, 0x34, 0xED, 0x7D, 0xC5, 0xFA, 0x97, 0x0F, 0xF1,
	0x99, 0x8C, 0xB7
};

char flag[19] = {
	0x66, 0x55, 0x2C, 0x26, 0x53, 0xE8, 0x72, 0xB4, 0x33, 0xB2, 0x77, 0xC9, 0xED, 0x8F, 0x0C, 0xFC,
	0x88, 0x83, 0xE4
};

char *xordata(char *data1 , char *data2)
{
    char *done ;
    int i ;
    done = (char *)calloc(19 , sizeof(char)) ;
    for ( i = 0 ; i < 19 ; i++)
    {
        *(done + i) = *(data1 + i) ^ *(data2 + i) ;
    }
    return done ;
}

int main( int argc , char *argv[ ] )
{
    char *flag1 , *key ;
    int i ;
    key = xordata(clear , cipher) ;
    flag1 = xordata(key , flag) ;
    for ( i = 0 ; i < 19 ; i++)
    {
        printf("%c" , *(flag1+i)) ;
    }
    return 0 ;
}
```
运行程序,得到flag<br>
![](https://github.com/VictoriqueDeBlois/HCTF/blob/master/Image/crypto.0.2.jpg)
