##### tags: `is1ab-note`
<h1 style="font-size: 3rem;">is1ab WebSecurity學習影片 筆記 1 (11/24/2024)</h1>



# <span class="red">簡單Web拆解步驟</span>

1. <span class="purple">**觀察網站(像是有甚麼功能、使用者介面...)**</span>

2. <span class="purple">**使用網頁工具DevTools(F12)**</span>

&emsp;

# <span class="red">Broken Access Control</span>

- <span class="purple">**垂直越權：普通用戶 → 管理員**</span>
    - **```/admin_panel```**
    - **```/admin```**
    - **```/admin_deluser```**

- **<span class="red">2024 新生盃 不要跑</span>：https://hackmd.io/@AndyShen/r1AP2biN1x**

- <span class="purple">**水平越權：使用者A → 使用者B**</span>
    - **```/myAccount?user=5```**
    - **```/myAccount?user=6```**

* <span class="purple">**Insecure direct object reference (IDOR)**</span>

- **<span class="red">2025 TSC CTF Be_IDol</span>：https://hackmd.io/@AndyShen/rJnGZYPvye**

&emsp;

# <span class="red">經典漏洞</span>

## <span class="blue">Path Traversal / Local File Inclusion (LFI)</span>

- ![image](https://hackmd.io/_uploads/BJTMQwgX1x.png)
- <span class="blue">**將上圖連結變成下圖**</span>
- ![image](https://hackmd.io/_uploads/Hy8wXwxXye.png)
- ![image](https://hackmd.io/_uploads/rkfN7wg7yx.png)

- **<span class="red">HTB Baby Nginxatsu</span>： https://hackmd.io/G0CanGsTTzC11FRJbw15kA**

- **<span class="red">2025 TSC CTF Ave Mujica</span>：https://hackmd.io/@AndyShen/SJ6kIWGPkg**

## <span class="blue">**XSS**</span>

- ![image](https://hackmd.io/_uploads/rkIHBvgm1l.png)
- ![image](https://hackmd.io/_uploads/BJvvSvg7Jx.png)
- ![image](https://hackmd.io/_uploads/ryUIBwemyg.png)
- <span class="blue">**將上圖的XSS變成下圖**</span>
- ![image](https://hackmd.io/_uploads/SkARVPgQJe.png)

## <span class="blue">CSRF</span>
## <span class="blue">**SQL Injection**</span>
## <span class="blue">**Command Injection**</span>

- ![image](https://hackmd.io/_uploads/rygzIvx7Jx.png)

&emsp;

# <span class="red">Web世界觀</span>

- ![image](https://hackmd.io/_uploads/Hkp3dvemyl.png)

* <span class="light_purple">**Web前端框架、套件**</span>
    - Bootstrap, jQuery, React

* <span class="light_purple">**Web前端語言**</span>
    - HTML, CSS, JavaScript

* <span class="purple">**Web開發框架(後端)**</span>
    - Laravel, Express, Spring, Flask

* <span class="purple">**Web後端語言**</span>
    - PHP, Node.js, Java, python

* **Server**
    - Apache, Nginx, IIS

* **Data Storage**
    - Database, Cache, File Storage

* **Operation Environment**
    - OS(Linux/Windows), Cloud, Container

## <span class="red">HTTP Protocol</span>

* <span class="blue">**HTTP：<span class="red">H</span>yper<span class="red">T</span>ext <span class="red">T</span>ransfer <span class="red">P</span>rotocol**</span>

* ![image](https://hackmd.io/_uploads/HJwpnDlQ1e.png)

&emsp;

### <span class="red">HTTP request</span>
    
- ![image](https://hackmd.io/_uploads/SJb2Ave7kg.png)
- <span class="blue">**\r\n：HTTP使用CR(\r)LF(\n)換行**</span>

* <span class="red">**HTTP request：Method**</span>
    - <span class="blue">**動詞，表想做的事 e.g. GET, POST, PUT, DELETE, PATCH, HEAD...**</span>

* <span class="red">**HTTP request：Path**</span>
    - **e.g. ....com<span class="blue">++/login?redirect=%2f++</span>#login... → Path + Query Parameter**

* <span class="red">**HTTP request：Protocol Version**</span>

* <span class="red">**HTTP request：Header**</span>
    - ![image](https://hackmd.io/_uploads/ryEV7Tb7yg.png)

&emsp;

### <span class="red">HTTP response</span>

- ![image](https://hackmd.io/_uploads/B1UdNpZXkx.png)
- <span class="blue">**\r\n：HTTP使用CR(\r)LF(\n)換行**</span>

* <span class="red">**HTTP status code**</span>
    - **1xx：修但幾勒 e.g. 101 (Switch Protocol)**
    - **2xx：讚 e.g. 200 (OK)**
    - **3xx：走開 e.g. 301 (Moved Permanently)**
    - **4xx：你怪怪的(從Server視角) e.g. 403 (Forbidden)**
    - **5xx：我怪怪的(從Server視角) e.g. 500 (Internal Server Error)**

* <span class="red">**HTTP response：Header**</span>
    - ![image](https://hackmd.io/_uploads/HyenDaWQJx.png)
    - <span class="blue">**提供server要告訴client的附加資訊(像是伺服器環境)**</span>

* <span class="red">**HTTP response：Body**</span>
    - ![image](https://hackmd.io/_uploads/SkR8K6WQye.png)
    - <span class="blue">**CRLF Injection**</span>

&emsp;

## <span class="red">Cookie</span>

- ![image](https://hackmd.io/_uploads/rkIH5a-myx.png)
- <span class="blue">**在Header內**</span>

* <span class="red">**Cookie Attribute**</span>
    - ![image](https://hackmd.io/_uploads/S1Qoq6b7Je.png)

* <span class="red">**Session**</span>
    - ![image](https://hackmd.io/_uploads/B1lG3pZXyl.png)

* <span class="red">**Signed Cookie**</span>
    - ![image](https://hackmd.io/_uploads/Syuan6bmJl.png)

&emsp;

## <span class="red">Web Hacking</span>

- <span class="red">**Fundamental Concept**</span>
- ![image](https://hackmd.io/_uploads/Bkp-Jyfmkl.png)

* <span class="red">**Recon 偵查 (Reconnaissance)**</span>
* ![image](https://hackmd.io/_uploads/ByD5kyzmJx.png)
* **https://www.wappalyzer.com/**

&emsp;

## <span class="red">Information Leak資訊洩漏</span>

- ![image](https://hackmd.io/_uploads/Hkf8ZJf71e.png)

- <span class="red">**常見套路**</span>
    * **```robots.txt```**
        - ![image](https://hackmd.io/_uploads/SyWDfJMmkg.png)
    * **```.git / .svn / .bzr```**
        - ![image](https://hackmd.io/_uploads/BJ8nzkfm1g.png)
    * **```.DS_Store```**
        - ![image](https://hackmd.io/_uploads/rJW4myfQkl.png)
    * **```.index.php.swp```**
        - ![image](https://hackmd.io/_uploads/H1KvmJzQJx.png)
    * **```Backup files```**
        - ![image](https://hackmd.io/_uploads/SJC_7kMmke.png)

&emsp;

## <span class="red">Web的兩大分類</span>

- <span class="red">**File-based**</span>
    * ![image](https://hackmd.io/_uploads/r1LZSyfQJx.png)

- <span class="red">**Route-based**</span>
    * ![image](https://hackmd.io/_uploads/Syuzr1GXyg.png)

### <span class="red">File-based：Webshell</span>

- ![image](https://hackmd.io/_uploads/rJ-TH1zmyg.png)

- **e.g.(範例攻擊步驟)** : 
    - (1) **```echo '<?php eval($_GET["cmd"]); ?>' > cmd.php```**
    - (2) **將 cmd.php 檔放入(沒限制的)上傳點並取得結果(產出)**
    - (3) **將獲得的結果加上 " ```cmd = 想要執行的 shell script``` " (e.g. ```ls -al```)**
    - (4) **就能夠從結果中獲得額外資訊**
    * ![image](https://hackmd.io/_uploads/H1Kx9j_71e.png)

- **<span class="red">2024 新生盃 Babyshell</span>：https://hackmd.io/@AndyShen/B1E-roPHyg**

- **<span class="red">ISS 期末 CTFd exchange：https://hackmd.io/@AndyShen/ryePCTJPJx**

&emsp;
    
- **<span class="red">Prevent & Bypass</span>**
    * **檢查<span class="red">++Post Content Type++</span>**
        - **像是剛剛模擬攻擊的<span class="purple">Content Type</span>可以在<span class="light_purple">DevTools(F12)</span>中Network欄位底下的Payload中查看**
        - ![image](https://hackmd.io/_uploads/S1u0piOQJg.png)

    * **檢查<span class="red">++File Signature++</span> (<span class="red">++Magic Number++</span>)**
        - **https://filesignatures.net**
        - **不同類型的檔案會有各自的file signature**
        - **<span class="purple">Magic Number</span> + <span class="blue">PHP code</span> → <span class="red">Webshell</span>**

    * **檢查<span class="red">++副檔名++</span>**
        - **<span class="purple">黑名單</span>**
            * **大小寫會失效**
            * **<span class="light_purple">pht</span>, <span class="light_purple">phtml</span>, <span class="light_purple">php\[3, 4, 5, 7\] ...</span>**
            * **<span class="light_purple">XSS (html, svg)</span>**
            * **<span class="light_purple">.htaccess</span>**
                - **e.g. 如果檔案名稱有meow就當php執行**
                - ![image](https://hackmd.io/_uploads/B1giyfhu71l.png)
    
        - **<span class="purple">白名單</span>**

&emsp;

### <span class="red">Path Traversal</span>

- **利用../的方式跳過(特定)目錄**

- ![image](https://hackmd.io/_uploads/BybYc3O7Je.png)

&emsp;

- **<span class="red">常見的System Information Path</span>**
    * ![image](https://hackmd.io/_uploads/rkPVLnuXJx.png)
    
&emsp;
    
- **<span class="red">常見的Network Information Path</span>**
    * ![image](https://hackmd.io/_uploads/SkB0UhOXke.png)

&emsp;

### <span class="red">Local File Inclusion (LFI)</span>

- ![image](https://hackmd.io/_uploads/rk4f32uXyx.png)

- **<span class="blue">通常發生在PHP，現在越來越少</span>**

- **LFI → RCE：嘗試找到<span class="light_purple">可以寫(可以控制)</span>的檔案並將其include**
    * ![image](https://hackmd.io/_uploads/BykFy6_QJg.png)

&emsp;

* **<span class="red">PHP 最新技巧</span>**
    - **只要<span class="light_purple">檔名可控</span> → 可<span class="blue">生成任意檔案內容</span>**
        * **e.g. GitHub - synacktiv/php_filter_chain_generator**
    
    - **只要<span class="light_purple">檔名可控</span> → <span class="blue">沒有顯示內容</span>也能夠<span class="blue">讀出檔案內容</span>**
        * **e.g. GitHub - synacktiv/php_filter_chains_oracle_exploit**
    