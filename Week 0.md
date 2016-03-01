#Week 0
##WEB
###WEB从0开始之0
>题目ID： 26<br>
>题目描述： WEB页面的HTML，CSS，JS客户端是可以查看的哦～你能在平台源码中找到FLAG么？<br>
>Hint: 不知你有没有发现，通过右键看到的源码中没有题目，没有排名信息。<br>
>推荐：chrome -> F12; firefox -> firebug<br>

* chrome -> f12
* Ctrl + f 搜索hctf 得到flag<br>
![](https://raw.githubusercontent.com/VictoriqueDeBlois/HCTF/master/Image/WEB.0.0.jpg)

###WEB从0开始之0.2
>题目ID： 35<br>
>题目描述： 你知道啥是cookie吗？<br>
>那么你会修改它吗？<br>
>http://ctf.lazysheep.cc:8080/web0-2.php<br>

* 使用chrome的EditThisCookie插件<br>
* 将值false改为true<br>
* 刷新网页<br>
* 得到flag<br>
![](https://raw.githubusercontent.com/VictoriqueDeBlois/HCTF/master/Image/WEB.0.0.2.jpg)

##MISC
###MISC从0开始之流量分析0
>题目ID： 33<br>
题目描述： http://ctf.lazysheep.cc:8080/net0.pcap<br>
Hint: PS: FLAG打错了。。格式变成flag{}..懒得改了<br>

* 打开wireshake
* 搜索flag
![](https://raw.githubusercontent.com/VictoriqueDeBlois/HCTF/master/Image/MISC.0.0.jpg)

###MISC从0开始之Steganography0
>题目ID： 32<br>
题目描述： AK菊苣的小姐姐们之0～<br>
http://ctf.lazysheep.cc:8080/steg0.html<br>

* 保存图片<br>
* 用笔记本打开最下面(居然和信安导论讲的一样)<br>
![](https://raw.githubusercontent.com/VictoriqueDeBlois/HCTF/master/Image/misc_Steganography0.jpg)

###MISC从0开始之编码0
>题目ID： 25<br>
题目描述： SENURntUSElTSVNCQVNFNjRFTkNPREV9<br>
Hint: base系列编码<br>

* 站长工具base64解码

##CRYPTO
###密码学从0开始之0
>题目ID： 23<br>
题目描述： ojam{AopzpzJhlzhyWhzzdvyk}<br>
Hint: Caesar's code<br>

* 凯撒密码需要移位，根据flag格式，ojam -> hctf 所以移位为7
* 于是自己写了个C，运行得到flag
~~~c
#include <stdio.h>
#include <ctype.h>
#include <stdlib.h>
#include <string.h>
#define MOVE 7

int main()
{
    char *str = "ojam{AopzpzJhlzhyWhzzdvyk}" ;
    char *prt , *phead ;
    prt = strdup(str) ;
    phead = prt ;
    while (*prt != '\0')
    {
        if (isalpha(*prt))
        {
            if (islower(*prt))
            {
                if (!islower(*prt - MOVE))
                    *prt = 'z' - (MOVE - *prt + 'a') + 1;
                else
                    *prt = *prt - MOVE ;
            }
            else if (isupper(*prt))
            {
                if (!isupper(*prt - MOVE))
                    *prt = 'Z' - (MOVE - *prt + 'A') + 1;
                else
                    *prt = *prt - MOVE ;
            }
        }
        prt ++ ;
    }
    printf("%s" , phead) ;
    return 0;
}
```
![](https://raw.githubusercontent.com/VictoriqueDeBlois/HCTF/master/Image/0.0.jpg)
