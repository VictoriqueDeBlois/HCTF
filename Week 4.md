#CRYPTO(只会这个)
##密码学从0开始之xxx
>题目ID： 69<br>
题目描述： http://ctf.lazysheep.cc:8084/cry4.py<br>

因为不会用Python, 所以就网上看了看基本, 会看了后自己翻译成c++<br>
* 于是自己写了个c++
翻译后
```c++
#include <iostream>
#include <string>
#include <vector>
using namespace std ;

uint8_t A[] = {85, 128, 177, 163, 7, 242, 231, 69, 185, 1, 91, 89, 80, 156, 81, 9, 102, 221, 195, 33, 31, 131, 179, 246, 15, 139, 205, 49, 107, 193, 5, 63, 117, 74, 140, 29, 135, 43, 197, 212, 0, 189, 218, 190, 112, 83, 238, 47, 194, 68, 233, 67, 122, 138, 53, 14, 35, 76, 79, 162, 145, 51, 90, 234, 50, 6, 225, 250, 215, 133, 180, 97, 141, 96, 20, 226, 3, 191, 187, 57, 168, 171, 105, 113, 196, 71, 239, 200, 254, 175, 164, 203, 61, 16, 241, 40, 176, 59, 70, 169, 146, 247, 232, 152, 165, 62, 253, 166, 167, 182, 160, 125, 78, 28, 130, 159, 255, 124, 153, 56, 58, 143, 150, 111, 207, 206, 32, 144,
     75, 39, 10, 201, 204, 77, 104, 65, 219, 98, 210, 173, 249, 13, 12, 103, 101, 21, 115, 48, 157, 147, 11, 99, 227, 45, 202, 158, 213, 100, 244, 54, 17, 161, 123, 92, 181, 243, 184, 188, 84, 95, 27, 72, 106, 192, 52, 44, 55, 129, 208, 109, 26, 24, 223, 64, 114, 19, 198, 23, 82, 120, 142, 178, 214, 186, 116, 94, 222, 86, 251, 36, 4, 248, 132, 25, 211, 199, 30, 87, 60, 127, 155, 41, 224, 151, 237, 136, 245, 37, 170, 252, 8, 42, 209, 46, 108, 88, 183, 149, 110, 66, 235, 229, 134, 73, 38, 118, 236, 119, 154, 216, 217, 240, 22, 121, 174, 93, 126, 230, 228, 18, 148, 220, 172, 2, 137, 34};

uint8_t B[] = {0, 2, 3, 7, 1, 5, 6, 4};

uint8_t C[] = {179, 132, 74, 60, 94, 252, 166, 242, 208, 217, 117, 255, 20, 99, 225, 58, 54, 184, 243, 37, 96, 106, 64, 151, 148, 248, 44, 175, 152, 40, 171, 251, 210, 118, 56, 6, 138, 77, 45, 169, 209, 232, 68, 182, 91, 203, 9, 16, 172, 95, 154, 90, 164, 161, 231, 11, 21, 3, 97, 70, 34, 86, 124, 114, 119, 223, 123, 167, 47, 219, 197, 221, 193, 192, 126, 78, 39, 233, 4, 120, 33, 131, 145, 183, 143, 31, 76, 121, 92, 153, 85, 100, 52, 109, 159, 112, 71, 62, 8, 244, 116, 245, 240, 215, 111, 134, 199, 214, 196, 213, 180, 189, 224, 101, 202, 201, 168, 32, 250, 59, 43, 27, 198, 239, 137, 238, 50,
     149, 107, 247, 7, 220, 246, 204, 127, 83, 146, 147, 48, 17, 67, 23, 93, 115, 41, 191, 2, 227, 87, 173, 108, 82, 205, 49, 1, 66, 105, 176, 22, 236, 29, 170, 110, 18, 28, 185, 235, 61, 88, 13, 165, 188, 177, 230, 130, 253, 150, 211, 42, 129, 125, 141, 19, 190, 133, 53, 84, 140, 135, 10, 241, 222, 73, 12, 155, 57, 237, 181, 36, 72, 174, 207, 98, 5, 229, 254, 156, 178, 128, 55, 14, 69, 30, 194, 122, 46, 136, 160, 206, 26, 102, 218, 103, 139, 195, 0, 144, 186, 249, 79, 81, 75, 212, 234, 158, 163, 80, 226, 65, 200, 38, 187, 113, 63, 24, 25, 142, 51, 228, 35, 157, 216, 104, 162, 15, 89};

uint8_t D[] = {2, 4, 0, 5, 6, 7, 1, 3};

uint8_t LShift (uint8_t t, uint8_t k)
{
    uint8_t tmp ;
    k %= 8 ;
    tmp = ((t << k) | (t >> (8 - k))) & 0xff ;
    return tmp ;
}

string encode(uint8_t p)          //将p 转换成用 “ | O ” 表示的2进制
{
    string ret ;
    for ( unsigned i = 0 ; i < 8 ; i++)
    {
        if ((p >> i) & 1)   
            ret = '|' + ret ;
        else
            ret = 'O' + ret ;
    }
    return ret ;
}

int main()
{
    string plain_str = "abcdefgh123456" ;
    //char key_str[] = {/*lose*/} ;
    char cipher_str[] = "||OO|O||||O|||O||O|O||O||O|||||OO||||O|O|O||OOOO|||OOOOOO|OOOO||O|O|||O||OO|||O||O|OOO||O||||OO|||O|OOOO||O|||O|" ;
    char flag_cipher_str[] = "OOO|O|||OO|OO||OOO|||||OOO|||OOO||O||||OOO||OO|O|||OO|OOO||OOOOOOOOOO||OOO|O|O|O|||O||OO|OOO|O|OOOOO|OO|||||O|O||O|O|OOO|O|OOO||OOOO|O||O|OOOOOO|||OOO|O|||OO|||OO|O||OO|O|OOO|O" ;

    vector<uint8_t> plain ;
    for ( unsigned i = 0 ; i < plain_str.length() ; i++)
    {
        plain.push_back(plain_str[i]) ;
    }

    vector<uint8_t> key ;
    for ( unsigned i = 0 ; i < 8 ; i++)
    {
        key.push_back(key[i]) ;
    }

    vector<uint8_t> t1 ;                              //用A表替换明文
    for ( uint8_t i = 0 ; i < plain.size() ; i++)
    {
        t1.push_back(A[plain[i]]) ;
    }

    vector<uint8_t> t2 ;
    for ( unsigned i = 0 ; i < t1.size() ; i++)     //用一定算法替换t1
    {
        t2.push_back(LShift(t1[i] , B[i % 8])) ;
    }

    for ( unsigned time = 0 ; time < 16 ; time++)
    {
        for ( unsigned i = 0 ; i < t2.size() ; i++) //用c表替换t2
            t2[i] = C[t2[i]] ;

        for ( uint8_t i = 0 ; i < t2.size() ; i++) //用一定算法替换t2
            t2[i] = LShift(t2[i] , i ^ D[i % 8]) ;

        for ( unsigned i = 0 ; i < t2.size() ; i++) //用密钥加密t2
            t2[i] = t2[i] ^ key[i%8] ;
    }

    string out ;
    for ( unsigned i = 0 ; i < t2.size() ; i++)  //将t2各元素转换成2进制
        out += encode(t2[i]) ;
    cout << out << endl ;
    return 0 ;
}
```
这样以后~~对我来说~~就明朗多了<br>

之后再对这个加密算法进行分析
* 大部分我用注释写在了代码中
* 最后是转换2进制，可以暂时不用管它
* 注意在使用各种函数，方式替换的时候，其实明文的各个元素的顺序，数量实质上没有改变
* 所以可以对于单个元素逐一破解，一个元素可能情况就最多256种（**这里有一个问题**）
* 先逐一试出8位密码，在对flag解密
* 于是自己继续写c++
```c++
uint8_t convert8bit(char *str)
{
    uint8_t num = 0 ;
    if (*str == '|')
        num += 128 ;
    if (*(str+1) == '|')
        num += 64 ;
    if (*(str+2) == '|')
        num += 32 ;
    if (*(str+3) == '|')
        num += 16 ;
    if (*(str+4) == '|')
        num += 8 ;
    if (*(str+5) == '|')
        num += 4 ;
    if (*(str+6) == '|')
        num += 2 ;
    if (*(str+7) == '|')
        num += 1 ;
    return num ;
}

void decode (vector<uint8_t> & cipher , char *str)
{
    char tmp[9] = {'\0'} ;
    char *pstr = str ;
    uint8_t i ;
    while (*pstr != '\0')
    {
        for ( i = 0 ; i < 8 ; i++)
        {
            *(tmp + i) = *pstr ;
            pstr ++ ;
        }
        cipher.push_back(convert8bit(tmp)) ;
    }
}

uint8_t getKey (uint8_t t2AlphaOrigin , uint8_t cipherAlpha , uint8_t index)
{
    uint8_t key = 0 ;
    uint8_t t2Alpha ;
    for ( key = 32 ; key < 127 ; key++)           //在可打印范围内
    {
        t2Alpha = t2AlphaOrigin ;
        for ( unsigned time = 0 ; time < 16 ; time++)
        {
            t2Alpha = C[t2Alpha] ;
            t2Alpha = LShift(t2Alpha , index ^ D[index%8]) ;
            t2Alpha ^= key ;
        }
        if (t2Alpha == cipherAlpha)
            return key ;
    }
    return 0 ;
}

uint8_t getFlag (uint8_t cipherAlpha , vector<uint8_t> key , uint8_t index)
{
    uint8_t flagAlpha ;
    uint8_t flagAlphaOrigin , t1 , t2 ;
    for (flagAlpha = 32 ; flagAlpha < 127 ; flagAlpha++)       //在可打印范围内
    {
        flagAlphaOrigin = flagAlpha ;
        t1 = A[flagAlpha] ;
        t2 = LShift(t1 , B[index % 8]) ;
        for ( unsigned time = 0 ; time < 16 ; time++)
        {
            t2 = C[t2] ;
            t2 = LShift(t2 , index ^ D[index%8]) ;
            t2 ^= key[index % 8] ;
        }
        if (cipherAlpha == t2)
            return flagAlphaOrigin ;
    }
    return 0 ;
}

int main()
{
    vector<uint8_t> plain ;
    for ( unsigned i = 0 ; i < plain_str.length() ; i++)
    {
        plain.push_back(plain_str[i]) ;
    }

    vector<uint8_t> t1 ;                              
    for ( uint8_t i = 0 ; i < plain.size() ; i++)
    {
        t1.push_back(A[plain[i]]) ;
    }

    vector<uint8_t> t2 ;
    for ( unsigned i = 0 ; i < t1.size() ; i++)     
    {
        t2.push_back(LShift(t1[i] , B[i % 8])) ;
    }
          /* main里面上面的代码还要保留 */
    vector<uint8_t> cipher ;          //将密文先转换成数组
    decode(cipher , cipher_str) ;

    vector<uint8_t> key ;               //得到密钥
    for ( unsigned i = 0 ; i < 8 ; i++)
    {
        key.push_back(getKey(t2[i] , cipher[i] , i)) ;
    }

    cout << "KEY: " << endl ;
    for (unsigned i = 0 ; i < 8 ; i++)
    {
        cout << key[i] << " " ;
    }
    cout << endl ;

    vector<uint8_t> flag_cipher ;
    decode(flag_cipher , flag_cipher_str) ;

    vector<uint8_t> flag ;               //得到flag
    for (unsigned i = 0 ; i < flag_cipher.size() ; i++)
    {
        flag.push_back(getFlag(flag_cipher[i] , key , i)) ;
    }

    cout << "FLAG: " << endl ;
    for (unsigned i = 0 ; i < flag.size() ; i++)
    {
        cout << flag[i] << " " ;
    }
    cout << endl ;
    return 0 ;
}
```
`这里存在一个问题`<br>
如果在注释*在可打印范围内*的代码处将key和flagAlpha的范围改成0~255<br>
密钥就会得到：{ '\x61', '\x40', '\x31', '\x44', '\x39', '\x2A', '\x0E', '\x16' }<br>
然后计算出来的flag就会有非打印字符：68 63 74 66 7b 68 `09` `e3` 79 6f 75 41 72 33 `fa` `d7` 64 69 41 30 21 7d<br>
范围改为在32~127之后，运行程序得到flag
