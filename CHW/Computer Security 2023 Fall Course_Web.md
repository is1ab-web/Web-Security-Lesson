---
title: 'Computer Security 2023 Fall Course'
disqus: hackmd
---

[Web] Computer Security 2023 Fall Course
===

## Table of Contents

[TOC]

#  Web (Ref:Splitline)
```
aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1MSXdnR1Bpd1NUNCZhYl9jaGFubmVsPWVkdS1jdGY=

aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj1rcDM5NkZPT2RySSZhYl9jaGFubmVsPWVkdS1jdGY=

aHR0cHM6Ly93d3cueW91dHViZS5jb20vd2F0Y2g/dj0ydTNpSXdKUjhhWSZhYl9jaGFubmVsPWVkdS1jdGY=
```
**● [[Web-lab] Computer Security 2023 Fall Course Writeup](https://hackmd.io/@CHW/SkYei2sPp)**

![image](https://hackmd.io/_uploads/ryBTOWCB6.png)

![image](https://hackmd.io/_uploads/SkSz2Z0HT.png)
![image](https://hackmd.io/_uploads/B1zPnbRS6.png)

![image](https://hackmd.io/_uploads/ByZ96-Ara.png)
## Google Hacking (by.NISRA)
![image](https://hackmd.io/_uploads/rkgG6fAra.png)
//site:.cn filetype:xlsx 帳號 密碼 (google) \
//site:.system8.(org).edu.tw intext:"管理" (google)\
![image](https://hackmd.io/_uploads/ry_j0zAHa.png)
![image](https://hackmd.io/_uploads/Hy3n0G0Ha.png)
![image](https://hackmd.io/_uploads/r1D0AMASp.png)
## 隱寫術 (by.NISRA)
![image](https://hackmd.io/_uploads/H1OZONRHT.png)
### Tools: Winhex & 010 Editor 
### ● JPG: FF D8 ~ FF D9
![image](https://hackmd.io/_uploads/SkdCdERBa.png)
**BTW. JPG圖形大小編碼:FF CO**
### ● PNG: IHDR Image header
![image](https://hackmd.io/_uploads/H1irY4RB6.png)
> png File signature開頭 89 50 4E 47 0D 0A 1A 0A \
> 010 Editor: Edit >> Insert/Overwrite >> Insert Bytes 可寫入

![image](https://hackmd.io/_uploads/B1sbsNCr6.png)



## HTTP Protocol
![image](https://hackmd.io/_uploads/rJ-NZzAST.png)
(\r\n: HTTP 使用CR(\r) LF(\n) 換行)
```console
──(frankchang㉿DESKTOP-CHW)-[~]
└─$ nc example.com 80
GET / HTTP/1.1

HTTP/1.1 400 Bad Request
Content-Type: text/html
Content-Length: 349
Connection: close
Date: Wed, 06 Dec 2023 15:10:32 GMT
Server: ECSF (laa/7B10)

<?xml version="1.0" encoding="iso-8859-1"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
         "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
        <head>
                <title>400 - Bad Request</title>
        </head>
        <body>
                <h1>400 - Bad Request</h1>
        </body>
</html>
┌──(frankchang㉿DESKTOP-CHW)-[~]
└─$ echo 'GET / HTTP/1.1\r\nHost: example.com\r\n\r\n' | nc example.com 80
HTTP/1.1 505 HTTP Version Not Supported
Content-Type: text/html
Content-Length: 379
Connection: close
Date: Wed, 06 Dec 2023 15:14:14 GMT
Server: ECSF (laa/7B93)

<?xml version="1.0" encoding="iso-8859-1"?>
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN"
         "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml" xml:lang="en" lang="en">
        <head>
                <title>505 - HTTP Version Not Supported</title>
        </head>
        <body>
                <h1>505 - HTTP Version Not Supported</h1>
        </body>
</html>
```
![image](https://hackmd.io/_uploads/rkYAFSRHp.png)
### curl https://www.bbc.com -H "Host:pypi.org"
![image](https://hackmd.io/_uploads/SkZ50BRHa.png)
![image](https://hackmd.io/_uploads/rJENkIABT.png)
▲curl 的目的地與Host Header不同
![image](https://hackmd.io/_uploads/SJzdaBAHp.png)
▼Reverse 功用
![image](https://hackmd.io/_uploads/BJ2GxIRSp.png)
▼ HTTP Response Code
![image](https://hackmd.io/_uploads/Syv_G8ArT.png)

## HTTP Cookie & Session
![image](https://hackmd.io/_uploads/SkMuD8CBT.png)
![image](https://hackmd.io/_uploads/HkwGs80Sa.png)
### ● Signed Cookie (明文存在Cookie，但要做簽章)
![image](https://hackmd.io/_uploads/BJB_oL0S6.png)
### ● ey 開頭Base64 幾乎就是 Cookie
> ' **{"** ' >(base64 encode)> ' eyI= '

## .htaccess
![image](https://hackmd.io/_uploads/r1rMQuAHp.png)

# Path Traversal
![image](https://hackmd.io/_uploads/H1xbLWCH6.png)
![image](https://hackmd.io/_uploads/HyWG8WRHp.png)
![image](https://hackmd.io/_uploads/H19V8-AB6.png)
▼ /static alias 到 /home/app/static
![image](https://hackmd.io/_uploads/BkloQ_RST.png)
▼ 收集資訊
![image](https://hackmd.io/_uploads/rJmXNOABT.png)
![image](https://hackmd.io/_uploads/SJBrE_0Sa.png)
![image](https://hackmd.io/_uploads/r1h3EOAB6.png)
# LFI: Local File Inclusion
任意讀檔+執行
![image](https://hackmd.io/_uploads/ryVHSuRSa.png)
## phpinfo decode
![image](https://hackmd.io/_uploads/BkqcSdCHp.png)
![image](https://hackmd.io/_uploads/r1BaHu0Sa.png)
> php:// -Manual
> read/write
> ![image](https://hackmd.io/_uploads/HkCVUuRBp.png)

# PHP 最新技巧
![image](https://hackmd.io/_uploads/BJLzPdASa.png)

# [CRLF Injection](https://owasp.org/www-community/vulnerabilities/CRLF_Injection)
不只控制Header，還控制瀏覽器 & Response Body
![image](https://hackmd.io/_uploads/BJoIHLAHa.png)

# Command Injection
![image](https://hackmd.io/_uploads/Hy6kYPxIp.png)
### 在linux shell command裡面，" " 裡面是可以執行Command
![image](https://hackmd.io/_uploads/ryeSFvlIp.png)
> ping "$(id)" 會被展開
> ping 'uid=0(root) gid=0(root) group=0(root)'

![image](https://hackmd.io/_uploads/B1bncvxUa.png)
![image](https://hackmd.io/_uploads/SynY3wlLa.png)

# SQL Injection
Structured Query Language 結構式查詢語言
e.q MySQL, MSSQL, Oracle, PostgreSQL
![image](https://hackmd.io/_uploads/Hk6XbctIp.png)
### 大部分Injection出現在字串拼接
▼所有使用者
![image](https://hackmd.io/_uploads/SJN8_aYIa.png)
### " \ " 不一定能Escape ex.SQLite
![image](https://hackmd.io/_uploads/ryf3lCtL6.png)
## Data Exfiltration
![image](https://hackmd.io/_uploads/Byv_90YU6.png)

### UNION
聯集column
![image](https://hackmd.io/_uploads/By1NGCFU6.png)
▼ UNION SELECT 新增資料
![image](https://hackmd.io/_uploads/rkIifAt8a.png)
id 改成不存在的東西，前端顯示SELECT進去的資料
![image](https://hackmd.io/_uploads/Hy2Jm0FU6.png)
▼ 注入撈的資料 ex. MySQL function
![image](https://hackmd.io/_uploads/ry54X0KL6.png)
**MySQL function**
![image](https://hackmd.io/_uploads/HJy57RF86.png)
▼ 從Users取出Password
![image](https://hackmd.io/_uploads/HyQpQCFI6.png)
![image](https://hackmd.io/_uploads/BJ7bNRYIp.png)
### Information_schema

![image](https://hackmd.io/_uploads/HyuB4RKIa.png)
▼ 利用Information_schema.table/columns取得已存在的table/columns name
![image](https://hackmd.io/_uploads/rJuxrRYIa.png)
![image](https://hackmd.io/_uploads/BkbiHAtUp.png)

▼ group_concat 把所有資料輸出列成一個字串
![image](https://hackmd.io/_uploads/r1RrrAKLp.png)
![image](https://hackmd.io/_uploads/SJ00rRK8p.png)

### Blind
#### Boolean Based
檢查帳號密碼、Username有沒有被註冊過
![image](https://hackmd.io/_uploads/Bk-Qw0YIa.png)
▼ 用and強迫吃我們給的條件
![image](https://hackmd.io/_uploads/B1kDv0KIT.png)
▼ 假設user()值是mysql
* 確定id=1是有資料的
* 控制T/F的資料: 確定function return值長度
* return值長度二分搜: >0 >16 >8 >4 >6 =5

![image](https://hackmd.io/_uploads/SkDft0KLp.png)
![image](https://hackmd.io/_uploads/SkDSFRY8p.png)
▲ ASCII 二分搜

#### Time Based
不知道是不是有資料
![image](https://hackmd.io/_uploads/Sy1TFAYIp.png)
▼ 伺服器response花很久時間，代表符合條件
![image](https://hackmd.io/_uploads/HJD15CYUp.png)

#### Out-of-Band
▼ windows網路芳鄰: windows讀\\開頭的檔案，會當網路芳鄰底下的檔案存取
![image](https://hackmd.io/_uploads/ryFacAYU6.png)

## 用MySQL讀寫檔
![image](https://hackmd.io/_uploads/Sy-2oAYL6.png)\
前提: 配置要開\
![image](https://hackmd.io/_uploads/HyvToRK8p.png)

## Sqlmap
![image](https://hackmd.io/_uploads/BkQFh0FLp.png)

# Template Injection
 ![image](https://hackmd.io/_uploads/HyjOa0tLa.png)
### Jinja2 (Flask模板)
![image](https://hackmd.io/_uploads/rk6Q0RKI6.png)
![image](https://hackmd.io/_uploads/rk-vR0YUa.png)
▼ 測試模板引擎
![image](https://hackmd.io/_uploads/BJU5R0t8T.png)
▼ 單config可dump出設定值。SECRET_KEY管理簽章Cookie，可偽造成別的使用者，自己簽Cookie。
![image](https://hackmd.io/_uploads/SJhA0RKI6.png)
▼ RCE
環境類似Sandbox，普通Python做不了
![image](https://hackmd.io/_uploads/SkKzgJqLT.png)
![image](https://hackmd.io/_uploads/By2pxy58a.png)
▲ Python中所有function都會有__globals__來儲存全域變數。import os 拿到system。
 ![image](https://hackmd.io/_uploads/SkukM1qL6.png)
**全部的python物件都是一個Object。** 
![image](https://hackmd.io/_uploads/SJIFmJcLa.png)
> Object底下的subclasses[132]:os._wrap_close
> class底下的method:__ init __ (建構值constructor)

▼ 可知道12345是在class 'list' (物件的類別)，源頭是object
![image](https://hackmd.io/_uploads/r1-aEy5Ua.png)
![image](https://hackmd.io/_uploads/SkhWH1qUa.png)

### Other Template Engines
![image](https://hackmd.io/_uploads/S1GdLkcUT.png)

# SSRF
提供網址存的到內網:
原功能
![image](https://hackmd.io/_uploads/ByVwZeq8p.png)
預覽localhost
![image](https://hackmd.io/_uploads/rJgcblqUp.png)

![image](https://hackmd.io/_uploads/SJEyMec8T.png)
![image](https://hackmd.io/_uploads/H13uGeqUT.png)
![image](https://hackmd.io/_uploads/SJf9Mg98p.png)

### SSRF攻擊面: Protocol
![image](https://hackmd.io/_uploads/ByKXmlqLT.png)
▼不可curl directory ex.file:///
![image](https://hackmd.io/_uploads/rJOAQe58T.png)

![image](https://hackmd.io/_uploads/r1OIVgq8a.png)
![image](https://hackmd.io/_uploads/HkhP4gcUa.png)
● [SSRF Bible Cheatsheet](https://cheatsheetseries.owasp.org/assets/Server_Side_Request_Forgery_Prevention_Cheat_Sheet_SSRF_Bible.pdf)
#### https
![image](https://hackmd.io/_uploads/rktHBe9Ip.png)
![image](https://hackmd.io/_uploads/Bk7FSx9I6.png)
▼ 以下都需要Request Header: Metadata-Flavor (Google)
![image](https://hackmd.io/_uploads/BkARrlqU6.png)
▼ 可使用CRLF 注入Request Header: Metadata-Flavor (Google)
![image](https://hackmd.io/_uploads/r1bdUe58p.png)
![image](https://hackmd.io/_uploads/BJtVPecLp.png)
![image](https://hackmd.io/_uploads/ByULvg9UT.png)
![image](https://hackmd.io/_uploads/H1nE_e9Lp.png)

#### gopher
![image](https://hackmd.io/_uploads/ryM9Zyo8a.png)
![image](https://hackmd.io/_uploads/H1l1E1iIp.png)
![image](https://hackmd.io/_uploads/ryvA4JjIp.png)
▼ Gopher x MySQL
![image](https://hackmd.io/_uploads/BkBnQ_2Ua.png)
[Gopher 相關payload](https://github.com/tarunkant/Gopherus)
▼ Gopher x Redis
![image](https://hackmd.io/_uploads/SyBrV_2La.png)
▼ CRLF Injection x Redis
![image](https://hackmd.io/_uploads/Hkch4O2Lp.png)
▼ Gopher x PHP-FPM
![image](https://hackmd.io/_uploads/HyhhH_nLa.png)
![image](https://hackmd.io/_uploads/rJCkUd3Ip.png)

![image](https://hackmd.io/_uploads/B1dBSun8p.png)
## Bypass Rule_ IP
![image](https://hackmd.io/_uploads/rylSL_3Lp.png)
## Bypass Rule_ Domain Name
![image](https://hackmd.io/_uploads/H1z9IOnUp.png)
▼ Ref: https://www.unicode.org/reports/tr46/
![image](https://hackmd.io/_uploads/Hk7cvO3U6.png)
Domain Obfuscator: https://splitline.github.io/domain-obfuscator/
## URL Parser
![image](https://hackmd.io/_uploads/SyKHKd28T.png)
urllib

## DNS Rebinding
檢查網址SSRF風險: Domain > Localhost
![image](https://hackmd.io/_uploads/SysEc_2Up.png)
![image](https://hackmd.io/_uploads/By9l2_286.png)
![image](https://hackmd.io/_uploads/HynX2d28p.png)
 
# Insecure Deserialization
![image](https://hackmd.io/_uploads/Bk300_38p.png)
> Javascript
> 一個object 存在array, boolean,string (存在記憶體的物件)
> 透過stringify轉成jason字串結果 (序列化)

![image](https://hackmd.io/_uploads/ryHkxYhUp.png)
不安全: eval 做反序列化，Code Injection
![image](https://hackmd.io/_uploads/SJEQeK28T.png)

![image](https://hackmd.io/_uploads/ry_teK2La.png)
## Python Pickle
![image](https://hackmd.io/_uploads/rkwWWYn8a.png)
> key:cat 值:meow dump成Binary字串

![image](https://hackmd.io/_uploads/r1g5NMfDa.png)
> 把Arguments 丟給Function吃

![image](https://hackmd.io/_uploads/HkQxSGGvp.png)

![image](https://hackmd.io/_uploads/HJoGUGzvT.png)

使用pickle tools 看 Binary 在做什麼 
### ▼ **pickletools: disassembly**
![image](https://hackmd.io/_uploads/B16tuGMwT.png)
dis=disassembly
### ▼ **pickletools: optimize**
![image](https://hackmd.io/_uploads/SkUauMGva.png)

### ▼ **pickletools: Memo & Stack**
反序列化Pickler時，稱幫忙反序列化的東西: Unpickler
```Plain Text
> python gen-pkl.py
  0: \x80 PROTO    4    #定義一個protocol:4
  2: \x95 FRAME    27    #不會影響執行
 11: \x8c SHORT_BINUNICODE 'posix'   #PUSH BinaryUnicode 
 18: \x8c SHORT_BINUNICODE 'system'
 26: \x93 STACK_GLOBAL    #from a import b "__import__(stack.pop())[stack.pop()]"
 27: \x8c SHORT_BINUNICODE 'whoami' #PUSH BinaryUnicode
 35: \x85 TUPLE1    #將Stack頂端變成一個TACO
 36: R    REDUCE    #__reduce__ function
 37: .    STOP    #指令頂端回傳回去，得反序列化結果
 highest protocol among opcodes = 4
```
> **Stack:**\
> **['posix']**   #SHORT_BINUNICODE\
> **['posix','system']**  #SHORT_BINUNICODE\
> **(POP) 'system','posix'**   #STACK_GLOBAL\
> **[]**  #from 'posix' import 'system'   #STACK_GLOBAL\
> **[<fn system>]**   #STACK_GLOBAL\
> **[<fn system>, 'whoami']**  #SHORT_BINUNICODE   
> **[<fn system>, ('whoami', )]**   #TUPLE1 \
> 
> #REDUCE
> 
> **arg=stack.pop()**\
> **fn=stack.pop()**\
> **stack.push(fn(arg))**
> 
> **['CHW']**\
>**return stack.pop()**   #STOP

功能:
1. 創造字串TACO ex.'str', 123 , (1,2,3)
2. 可以import
3. call function
4. set attribute 設定屬性值  #obj.attr=value
5. set index  #obj[index]=value

![image](https://hackmd.io/_uploads/ByBFJ4GDa.png)
> 先call function __ import__('os')\
> 拿到回傳值後，get attribute拿到 'system'這個function\
> 直接呼叫拿到的system


[pickle — Python object serialization 官方](https://docs.python.org/3/library/pickle.html)

### __ reduce__ Magic Method
![image](https://hackmd.io/_uploads/ryYOWtn86.png)

![image](https://hackmd.io/_uploads/HkVIGtnLp.png)
![image](https://hackmd.io/_uploads/BJk9fKhIT.png)
> 達到RCE

▼ pickletools
![image](https://hackmd.io/_uploads/SJ7UmtnI6.png)

## PHP (De)Serialization
![image](https://hackmd.io/_uploads/HkQWG4Gva.png)

![image](https://hackmd.io/_uploads/ryF3S8Ew6.png)

物件序列化格式表
![image](https://hackmd.io/_uploads/Hk1rUL4wT.png)
## 在PHP裡面，所有變數都是以$開頭
### PHP Magic method
![image](https://hackmd.io/_uploads/rk-TULEv6.png)
#### ● __destruct()
![image](https://hackmd.io/_uploads/HJ8nOINDa.png)
> 物件被銷毀，自動做destruct的行為

#### ● __toString()
![image](https://hackmd.io/_uploads/HyX9K8NwT.png)
![image](https://hackmd.io/_uploads/r1UnKLEDp.png)

#### ● __wakeup()
▼ 序列化 (正常印出序列化物件)
![image](https://hackmd.io/_uploads/BJJEjI4PT.png)
> $ser = 序列化後的字串

▼ 在反序列化時，會自動call __wakeup()
![image](https://hackmd.io/_uploads/B1wOoLVva.png)

#### ● __get()
![image](https://hackmd.io/_uploads/rkIxCINv6.png)
> 物件序列化之後，做反序列化(取名為meow的屬性值)。
> 自動call了 __get()
![image](https://hackmd.io/_uploads/B1omALNDa.png)

#### ● __call()
呼叫一個不存在的method自動乎叫 __call()
![image](https://hackmd.io/_uploads/Syc0CUEvT.png)
> 呼叫一個不存在的method meow()，

#### 例子
![image](https://hackmd.io/_uploads/ryeFiyPNDT.png)
> Command Injextion 注入 __wake():sound
![image](https://hackmd.io/_uploads/SkKXbvVva.png)

**現實世界可能會需要經過好幾層的串聯，才可以串到一個真正想要利用的method 攻擊點**
## POP Chain
堆疊Web世界觀ROP的應用鏈。\
把很多很小的Code片段串起來達到想做的結果。
![image](https://hackmd.io/_uploads/SkfxNwVDa.png)
[PHPGGC: PHP Generic Gadget Chains:](https://github.com/ambionics/phpggc) 收集現實世界裡各種反序列化的利用鏈
![image](https://hackmd.io/_uploads/HJtpqvVDp.png)

![image](https://hackmd.io/_uploads/Hy1NsDNDp.png)
> 55:00\
> 假設有三個Class:Cat, Magic, Caster
> > Cat:\
> > 有兩個屬性值(Magic & Spell)\
> > Magic物件，在__wakeup()會對magic使用cast這個method，echo東西出來
> 
> 可亂改屬性質(ex. magic)，可將magic指向別人(ex. Caster)
> > Caster:\
> > Cast method裡面有個public的屬性值: Cast_func，所以可改\
> > 他把cast_func當作Function Name接上我可以控制的 $val\
> > (回推)cast 來自magic，Magic我們可控，所以可把Magic改成Caster 代表Function 可控。
> > 參數可控嗎? $val 來自Cat的屬性值: spell，所以也可控。
> > ![image](https://hackmd.io/_uploads/HJ0QavNwp.png)
> > ![image](https://hackmd.io/_uploads/ryTmx_Nwp.png)

## Java (De)Serialization
ysoserial: https://github.com/frohoff/ysoserial
> 類似 PHPGGC，產出 Java gadget chains

![image](https://hackmd.io/_uploads/B1HGVnzO6.png)

## .Net (De)Serialization
![image](https://hackmd.io/_uploads/Hy15r3Mua.png)


### PHP小知識
1. **PHP 可直接呼叫一個String，這個String 可以是Function Name**
![image](https://hackmd.io/_uploads/ryzvbdNw6.png)
> 值是system 的string
2. call_user_func & call_user_func_array
![image](https://hackmd.io/_uploads/Bkxlfu4vp.png)
![image](https://hackmd.io/_uploads/ryOZfuNvp.png)
3. String 也可以是一個Array
![image](https://hackmd.io/_uploads/SkRg7ONwp.png)
> 陣列需要包含[class,method] //new Cat()→ meow\
> 形式: Object & String 被Array包住，Object可序列化、String也可序列化\
> 所以 **[new Cat(), 'meow'] 可被序列化**

# Frontend Security
## **同源政策: 同protocol、同host、同port**
![image](https://hackmd.io/_uploads/r1XsQCzO6.png)
> 訪問google.com，可以幫我管理google上的文件(ex. gmail)\
> 但不會在FB發一篇文 (不能在A網站傳送B網站的資料)

![image](https://hackmd.io/_uploads/rym9E0GdT.png)
![image](https://hackmd.io/_uploads/HkkKrCM_6.png)
> Cross-origin writes 
>> - Link
>> - Redirect
>> - Submit form
>> 
>Cross-origin embedding
>> ![image](https://hackmd.io/_uploads/rJUoUAz_6.png)

# CSRF: Cross-site Request Forgery
濫用同源政策
![image](https://hackmd.io/_uploads/HkuVD0Gdp.png)
![image](https://hackmd.io/_uploads/SkIssCz_T.png)
> 偽造刪文連結

![image](https://hackmd.io/_uploads/HJo2oRG_p.png)

POST method
![image](https://hackmd.io/_uploads/rJmt2AGd6.png)

### Superlogout
![image](https://hackmd.io/_uploads/HyLW0AGu6.png)

## CSRF Token
![image](https://hackmd.io/_uploads/SymQARMd6.png)
![image](https://hackmd.io/_uploads/ryB1yJmu6.png)
(比對前面POST Request案例)
![image](https://hackmd.io/_uploads/r16IJkXup.png)
![image](https://hackmd.io/_uploads/SJY5yymdp.png)

![image](https://hackmd.io/_uploads/HJm1e1Qua.png)

### SameSite Cookie
![image](https://hackmd.io/_uploads/ByjYey7_p.png)

![image](https://hackmd.io/_uploads/rkbg-k7_a.png)
Browser compatibility: https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie

# XSS
![image](https://hackmd.io/_uploads/Sk8d7y7Op.png)
![image](https://hackmd.io/_uploads/B12EwZRHa.png)
XSS GAME: http://www.xssgame.com/
## HTML Entity Encode
![image](https://hackmd.io/_uploads/HJGafJmd6.png)

## Self-XSS
![image](https://hackmd.io/_uploads/Byvz4kXdp.png)
![image](https://hackmd.io/_uploads/ryioSymu6.png)

![image](https://hackmd.io/_uploads/SkbxDyXdp.png)
## Reflected XSS
![image](https://hackmd.io/_uploads/B14Mv17dp.png)
> 利用性弱，需要發連結給受害者→可轉成短網址

## Stored XSS
![image](https://hackmd.io/_uploads/SJtpDyQ_6.png)
> Payload 儲存到Server/Database裡面 (ex. 留言板)

## DOM-based XSS
![image](https://hackmd.io/_uploads/Hki8ukQ_p.png)
> Reflected/Stored DOM-based XSS

## Event Handler
Handle 每個元素，發生某件事的event
![image](https://hackmd.io/_uploads/HJ2MKy7_6.png)

## javascript: Scheme
![image](https://hackmd.io/_uploads/HJEpjy7Oa.png)
> 瀏覽器中，javascript開頭後面接要跑的js (也是一個超連結)

## 防止XSS
### 1. BlackList
![image](https://hackmd.io/_uploads/B1Yl1xQup.png)
 ```
[space]on ...=
    <svg <TAB> onload=alert(1)>  #<TAB>
    <svg \n onload=alert(1)>  #\n
    <svg / onload=alert(1)>  #/

javascript:
    <a herf="\x01javascript:alert(1)">X</a> #塞亂七八糟字元
    <a herf="java\tscript:alert(1)">X</a>  #\t
    <a herf="java&TAB;script:alert(1)">X</a>  #&TAB; HTML encoded的模式

<script
    ex. JSFuck
```

**XSS Payload cheatsheet**:  https://portswigger.net/web-security/cross-site-scripting/cheat-sheet

### JSFuck (合法JS)
![image](https://hackmd.io/_uploads/BkFX-xmua.png)
> 丟console是可以執行的

![image](https://hackmd.io/_uploads/B1sEDkEda.png)

### 2. Escape HTML/JavaScript syntax
![image](https://hackmd.io/_uploads/Sypuuy4u6.png)
![image](https://hackmd.io/_uploads/B19CdJE_p.png)
> javascript:alert 經過entity encode 不會改變 → 必須過濾連結

### 3. Filter HTML syntax
![image](https://hackmd.io/_uploads/ryam9JEda.png)
![image](https://hackmd.io/_uploads/HJiNqkN_a.png)

[DOMPurify](https://github.com/cure53/DOMPurify): 過濾sytax工具 → https://cure53.de/purify

## CSP: Content Security Policy
![image](https://hackmd.io/_uploads/Bk7Hs14dT.png)
![image](https://hackmd.io/_uploads/Sk38o1N_p.png)
> 有效防禦XSS的Policy

![image](https://hackmd.io/_uploads/HyfxJlN_T.png)
CSP Evaluator: https://csp-evaluator.withgoogle.com/
![image](https://hackmd.io/_uploads/BJEvylVup.png)
> object-src: 載入非影音標籤物件套用的規則。如：<object>、<embed>及<applet>\
> base-uri: 可以強制更改所有網站的base 
> ![image](https://hackmd.io/_uploads/rJb5xxVda.png)
> script-src:\
>> (特例允許) unsafe-eval:  不可出現eval(...)，把字串當成JS執行的東西。\
>  (特例允許) unsafe-inline: <script>alert()</script>, < svg onload=alert()>
>
><script src="/app.js"></script> 就不是inline (沒有把HTML和JS混在一起)
>> 特例允許eval & inline ?! 危險?!\
>> nonce: ![image](https://hackmd.io/_uploads/HJ3OXlV_p.png)
>> 'nonce-l9GaccLDqZYYryQvKJoS5Q'\
>> 確保response有script nonce\
>> ![image](https://hackmd.io/_uploads/SJo84lEOa.png)
>> <script nonce="l9GaccLDqZYYryQvKJoS5Q"> 與Header nonce相同則可以載入。導致攻擊者無法預測每次的Header nonce value

## XS-Leaks
XS-Leaks Wiki: https://xsleaks.dev/
### Time-based
![image](https://hackmd.io/_uploads/rJ6IOgEda.png)
![image](https://hackmd.io/_uploads/rJvOul4u6.png)
> 找到 有資料會有的行為 & 沒資料會有的行為 之間差異性。\
> 就可以做到side channel
### Frame Count
![image](https://hackmd.io/_uploads/BkM2tlNdT.png)
> 可讀到頁面上有多少iframe

## DOM Clobbering
![image](https://hackmd.io/_uploads/Sy_wje4u6.png)
![image](https://hackmd.io/_uploads/ryEYilE_T.png)
> ![image](https://hackmd.io/_uploads/By-ijeNuT.png)\
> WHY? 在HTML中所有id都會變成一個變數
> ![image](https://hackmd.io/_uploads/rJzIne4O6.png)

![image](https://hackmd.io/_uploads/ryX5nlVuT.png)
> id會被綁到windows上\
> name屬性會被擋到document上，但只限embed, form, img & object。


![image](https://hackmd.io/_uploads/r1hIpe4Op.png)
meow=123 不會被蓋過去
![image](https://hackmd.io/_uploads/Hk_kalN_6.png)
![image](https://hackmd.io/_uploads/Bkn9RlN_T.png)
> ![image](https://hackmd.io/_uploads/rJZT0gVOT.png)\
> `<a href="" id="meow">owo</p>  #加入超連結`
> ![image](https://hackmd.io/_uploads/HJWQJZVuT.png)\
> `<a href="a:alert(1)" id="meow">owo</p> `
> ![image](https://hackmd.io/_uploads/r1DoyWN_a.png)\
>若沒有加a: → 變成相對路徑
> `<a href="alert(1)" id="meow">owo</p> `
> ![image](https://hackmd.io/_uploads/HyMxx-V_p.png)

# Prototype Pollution
![image](https://hackmd.io/_uploads/SJKibbE_a.png)
![image](https://hackmd.io/_uploads/H1iv-WNup.png)
> a 設定巢狀屬性，新物件b也會出現
> ![image](https://hackmd.io/_uploads/rkoMGWNd6.png)
> 所有物件的proto都會指向 Object.prototype
> // 竄改到原形鏈的始祖
> ![image](https://hackmd.io/_uploads/BJm7mbEO6.png)


# 基礎思路
![image](https://hackmd.io/_uploads/ryx0l7wAHp.png)
## Recon
![image](https://hackmd.io/_uploads/By87Qv0S6.png)

![image](https://hackmd.io/_uploads/S14TwwABp.png)
## Webshell
![image](https://hackmd.io/_uploads/HJHHdvCSa.png)

![image](https://hackmd.io/_uploads/SJVv9SxIa.png)
# Dangerous function
![image](https://hackmd.io/_uploads/B1ay1Ug8a.png)
