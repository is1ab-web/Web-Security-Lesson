##### tags: `is1ab-note`
<h1 style="font-size: 3rem;">is1ab WebSecurity學習影片 筆記 2 (12/1/2024)</h1> 

# <span class="red">**Outline**</span>

- <span class="red">**Injection**</span>

    * <span class="blue">**Prompt Injection**</span>

	* <span class="blue">**Code/Command Injection**</span>

	* <span class="blue">**SQL Injection**</span>

	* <span class="blue">**Template Injection**</span>

&emsp;

- <span class="red">**Server Side Request Forgery (SSRF)**</span>

	* <span class="blue">**HTTP Based**</span>

	* <span class="blue">**Gopher Based**</span>

	* <span class="blue">**Tricks**</span>

&emsp;

- <span class="red">**Deserialization**</span>

	* <span class="blue">**PHP**</span>

	* <span class="blue">**Pickle**</span>

&emsp;

# <span class="red">**Injection**</span>

- **使用者輸入成為<span class="light_purple">++指令++</span>、<span class="light_purple">++程式碼++</span>、<span class="light_purple">++查詢++</span>的一部分 → <span class="red">改變原本程式碼的預期行為</span>**

- <span class="red">**核心概念</span>：嘗試解決掉預設指令(例如使用<span class="purple">" ; (分號)"</span> , <span class="purple">" | (pipe line)"</span> , <span class="purple">" && (and)"</span> , <span class="purple">" || (or)"</span>)，並讓自製(通常有害)的指令能夠被執行**

- **包括：** ![image](https://hackmd.io/_uploads/BJoy6jFXyg.png)

&emsp;

## <span class="red">**Prompt Injection**</span>
    
- **引導式注入，通常是<span class="blue">針對自然語言</span>的注入**

&emsp;

## <span class="red">**Command Injection**</span>

- **Dangerous Function：**

- ![image](https://hackmd.io/_uploads/HkyY1nK7kg.png)

- **e.g. 如果有個Tool這樣設計(如下圖)，那我們可以對name進行注入**
    * ![image](https://hackmd.io/_uploads/rkH4lht7yx.png)
    
    * **上面這裡如果在<span class="light_purple">user input</span>的部分沒有做過濾，那就可以使用像是 ```';id;'```這種惡意的payload獲取<span class="blue">ID</span>相關資訊**

- **e.g. 又或是像下圖(Pwned：Binary Exploitation)**
    * ![image](https://hackmd.io/_uploads/rJusPlcmJl.png)
    
    * **類似於上一題的情況，我們甚至可以更改payload的語句進而<span class="blue">顯示該目錄下的所有檔案</span>**
    
### <span class="red">**Basic Tricks:**</span>

- ![image](https://hackmd.io/_uploads/SyZH3xcmkg.png)
    
- **Command Substitution：**
    * ![image](https://hackmd.io/_uploads/HkrR3ecXke.png)

    * **這樣的payload造成的風險目前我想像不太到，看起來更像是hacker<span class="blue">偵察時</span>會製作的payload**

- **其他代替Space的方案**
    * ![image](https://hackmd.io/_uploads/SyKlgW97kg.png)
    
    * **{cat,/flag}：效果和```cat /flag```是相同的**
    
- **Bypass(繞過) Blacklist**
    * ![image](https://hackmd.io/_uploads/B1qWbb5QJl.png)

&emsp;

## <span class="red">**SQL Injection**</span>

- **Introduction to SQL**
    * ![image](https://hackmd.io/_uploads/SJynVb5XJe.png)

- **SQL Scenario**
    * ![image](https://hackmd.io/_uploads/HJ1RHZc71l.png)

- **SQL <span class="red">Injection</span> Scenario**
    * ![image](https://hackmd.io/_uploads/HyS_B-9Xkl.png)

    * <span class="blue">**Another Example(If input = <span class="purple">admin' or 1=1 - -</span>)：**</span>
        - ![image](https://hackmd.io/_uploads/rJbEbxoQJl.png)

- **在輸入<span class="red">' (單引號)</span>或是<span class="red">" (雙引號)</span>並提交輸入後，如果整個web server出錯了(產生非正常登錄錯誤後的訊息)，那可能就有<span class="light_purple">SQL Injection</span>相關的漏洞**

### <span class="red">**Type of SQL Injection**</span>

- **<span class="light_purple">Union Based</span>**

- **<span class="light_purple">Blind</span>**
    * **<span class="purple">Boolean Based</span>**
    
    * **<span class="purple">Time Based</span>**

- **<span class="light_purple">Error Based</span>**

- **<span class="light_purple">Out-of-band</span>**

&emsp;

### <span class="red">**Union Based SQL Injection**</span>

- **用來合併多個查詢結果，同時要做<span class="light_purple">Union</span>的多筆查詢結果欄位數需相同**
- ![image](https://hackmd.io/_uploads/SkF88gjXJe.png)


- **Example of <span class="light_purple">Union SQL Injection</span>：**
    * **Union Select可以調用常數或是內建函數**
    * ![image](https://hackmd.io/_uploads/ryNQwxom1l.png)
    * **按照上面的方式，如果在MySQL中就能夠調用user()獲得使用者相關的資料**
    * ![image](https://hackmd.io/_uploads/rkcFveiQke.png)
    * ![image](https://hackmd.io/_uploads/r117uesXyx.png)

- **可以先從<span class="blue">information_schema(MySQL)</span>或<span class="blue">sql_master(SQLite)</span> ...等等資料庫中找到一些資訊(可配合<span class="light_purple">limit</span>, <span class="light_purple">group_concat</span> ...等語法使用)**
    * ![image](https://hackmd.io/_uploads/Bkt1KxiQke.png)

&emsp;

### <span class="red">**Blind SQL Injection - Boolean Based**</span>

- **資料<span class="red">不會</span>被顯示出來，只能得知Yes or No，通常<span class="blue">用於登入</span>或<span class="blue">檢查ID是否被使用過</span>**

- **Example：**
    * ![image](https://hackmd.io/_uploads/ry8Xsxjm1e.png)

&emsp;

### <span class="red">**Blind SQL Injection - Time Based**</span>

- **頁面上甚麼資訊都沒有，不會顯示東西，利用query時產生的<span class="red">時間差</span>判斷**

- **可以利用<span class="light_purple">sleep, query, repeat</span>這些指令造成**

&emsp;

### <span class="red">**Error Based SQL Injection**</span>

- **從錯誤訊息中獲取資訊**

&emsp;

### <span class="red">**Out-of-Band SQL Injection**</span>

- **Example：**
    * **如果windows讀到兩個\\\的檔案，會把它當成網路芳鄰底下的檔案，因此可以<span class="blue">將參數部分的資訊</span>傳至外網**
    * ![image](https://hackmd.io/_uploads/rkCihgj7Jg.png)

- **Read/Write File in MySQL**
    * **Read：**
    * ![image](https://hackmd.io/_uploads/rkwLTeimJl.png)
    * **Write：**
    * ![image](https://hackmd.io/_uploads/B1ov6xo7Je.png)

### <span class="red">**Prevent SQL Injection**</span>

- ![image](https://hackmd.io/_uploads/B1AyL8N4ke.png)

&emsp;

## <span class="red">**Template Injection**</span>

- **Introduction to Template Engine(模板引擎)**
    * **現在大多web framework都會實作**
    * **將<span class="light_purple">使用者介面</span>與<span class="light_purple">資料</span>分離**
    * ![image](https://hackmd.io/_uploads/SymxlWi7kg.png)

### <span class="red">**Server-Side Template Injection**</span>

- **Jinja2 SSTI(Server-Side Template Injection)**
    * ![image](https://hackmd.io/_uploads/Hyn-3E2m1l.png)
    * **<span class="light_purple">/?name</span>可以是 <span class="light_purple">config.SECRET_KEY</span>(SECRET_KEY是用來對cookie進行簽章)**
        - ![image](https://hackmd.io/_uploads/SJUNpN2m1l.png)
    * **但是無法直接RCE，因為import/print這些函數無法直接使用
(code在類似sandbox的環境中執行)**
        - ![image](https://hackmd.io/_uploads/rkuqRV3QJx.png)
    * **所以我們必須使用像是python中的<span class="light_purple">global</span>(全域變數)、<span class="light_purple">class</span>才能RCE**
        - ![image](https://hackmd.io/_uploads/Hk9SkBh7kx.png)
        - ![image](https://hackmd.io/_uploads/B119eS27ke.png)
        - ![image](https://hackmd.io/_uploads/HJPilH37ke.png)

### <span class="red">**Other Template Engines**</span>

- ![image](https://hackmd.io/_uploads/S1frsUVV1x.png)

&emsp;

# <span class="red">**SSRF**</span>

- **SSRF：<span class="red">S</span>erver <span class="red">S</span>ide <span class="red">R</span>equest <span class="red">F</span>orgery(伺服器端請求偽造)**

- **主要攻擊思路：外部使用者向Server發起請求 → <span class="red">存取內網資源</span>**

- **因為HTTP內都是用 <span class="light_purple">\r\n</span> 進行分割，也就是說，如果<span class="red">能夠在Header塞入 ++CRLF Injection++ </span>，就能夠<span class="red">做到任意的Injection</span>**

- ![image](https://hackmd.io/_uploads/SJ_yLB3Q1x.png)

## <span class="red">**SSRF Identify**</span>

- **如何識別?**

    - **回傳內容**

    - <span class="blue">**HTTP Request Log**</span>
        * **cons. 對外 http 被 blocked**

    - <span class="blue">**DNS Query Log**</span>
        * **伺服器端是否有進行DNS查詢**

&emsp;

## <span class="red">**SSRF對URL的分解**</span>

- ![image](https://hackmd.io/_uploads/H19tvHhQyx.png)

&emsp;

## <span class="red">SSRF攻擊面</span>

- **File Protocol**
    * **file:// ：屬於file scheme的部份**

    * **無法使用```$ curl file///```直接列出目錄**

    * ![image](https://hackmd.io/_uploads/B1zZ_S2Xke.png)

## <span class="red">http(s)://</span>

* **通常目的是<span class="blue">存取/攻擊內網的 web service</span>，主要是透過<span class="light_purple">Get, Request</span>這兩種method**

### <span class="red">http(s):// -- Docker API</span>

* **預設關閉，但開發者可能會將其打開，方便開發自定義工具、進行前端應用和一些管理。但同樣的，也可能會因此而受到攻擊**

* ![image](https://hackmd.io/_uploads/BkuGsrn71l.png)

&emsp;

### <span class="red">http(s):// -- Cloud Metadata</span>

- **<span class="blue">Cloud Metadata</span>：儲存cloud service的一些資訊，大多數雲端服務(AWS, GCP ...)都有**

- ![image](https://hackmd.io/_uploads/SJS-4NwNyx.png)

- **如何獲取資料：**
    * ![image](https://hackmd.io/_uploads/S1My3rhXkx.png)

&emsp;

## <span class="red">CRLF Injection</span>

- **上述的介紹都需要透過 <span class="red">Metadata-Flavor Google</span> 這個 <span class="blue">Request Header</span> 進行攻擊，如果沒了這個Header呢?**

- **<span class="red">Ans</span>：我們可以透過 <span class="light_purple">CRLF Injection</span> 注入 <span class="blue">Response Body</span>**

### <span class="red">那如果我們想在 <span class="blue">Request Header</span> 做 <span class="light_purple">CRLF Injection</span> 呢?</span>

- ![image](https://hackmd.io/_uploads/rJs0d4vEJx.png)

- **<span class="red">Ans</span>：答案是可以!!!可以這樣注入Header**

- ![image](https://hackmd.io/_uploads/rk4LtVvNkg.png)

&emsp;

## <span class="red">gopher://</span>

- **神奇的萬用協議(原本的設計目標與全球資訊網類似)，<span class="red">可以建出任意TCP的封包</span>但是<span class="light_purple">無法進行交互</span>**

- ![image](https://hackmd.io/_uploads/rkVgeU3m1x.png)

- **使用gopher建造HTTP GET**
    * ![image](https://hackmd.io/_uploads/SySQgLnmkl.png)

- **Gopher x Redis Scenario**
    * **Redis：<span class="blue">key-value DB</span> with default port 6379**
    
    * ![image](https://hackmd.io/_uploads/SyKhjaPVkg.png)

    * **Thus, we can do it with <span class="light_purple">CRLF Injection</span>, too**

    * ![image](https://hackmd.io/_uploads/Syj_2TDEJg.png)

- **Gopher x PHP-FPM Scenario**
    * **FPM：<span class="red">F</span>astCGI <span class="red">P</span>rocess <span class="red">M</span>anager**

    * ![image](https://hackmd.io/_uploads/HyztWU37Jx.png)
    
    * **Example：塞入事先準備好的內容(file)**
    
    * ![image](https://hackmd.io/_uploads/HyqoaTw41l.png)

&emsp;

## <span class="red">對於SSRF防禦機制的討論</span>

- **Bypass Rule：IP**
    * **如果擋下的IP Address只有127.0.0.1，那你可以：**

    * ![image](https://hackmd.io/_uploads/S1C8zLh7ye.png)

- **Bypass Rule：Domain Name**
    * **合格的Browser在處理Domain時應該要先<span class="red">Normalize</span>**

    * ![image](https://hackmd.io/_uploads/BkSqzUhXyg.png)

- **在檢查URL時，URL parser必須把hostname抓出來檢查，但如果：**

- ![image](https://hackmd.io/_uploads/SJQFUAw41g.png)

- **上面這種情況下，urllib看到的是```hostname = 3.3.3.3```，然而實際request的卻是```hostname = 2.2.2.2```**

&emsp;

## <span class="red">DNS Rebinding</span>

- **作為一個攻擊者，我們能夠將同一個Domain綁<span class="red">兩個不同的IP</span>**

- ![image](https://hackmd.io/_uploads/H1oyVInXJx.png)

- **這意謂著：無法確定<span class="blue">檢查時</span>與<span class="blue">蒐集時</span>的IP相同(以下圖為例)**
    * ![image](https://hackmd.io/_uploads/BkYL4L2myl.png)

&emsp;

# <span class="red">Serialization/序列化</span>

- **將記憶體中的<span class="blue">資料結構、物件</span>轉換成<span class="blue">可傳輸、儲存的格式</span>，像是常見的<span class="purple">JSON檔</span>**

- ![image](https://hackmd.io/_uploads/r1J8UUnmJx.png)

- **反序列化(<span class="red">De</span>serialization)：**
    * ![image](https://hackmd.io/_uploads/r1kn8L2mJg.png)
    
    * **問題在於如果<span class="light_purple">用eval實作</span>反序列化就很危險**

# <span class="red">Deserialization/反序列化</span>

- **反序列化可能會有那些問題?**

    * **如果要被反序列化的資料<span class="red">可控</span>?**
    
    * **過程與結果階段：**
        
        - **會自動呼叫Magic Method**
        
        - **控制程式流程**

## <span class="red">(Python) Pickle</span>

- **序列化使用<span class="light_purple">pickle.dump()</span>，反序列化使用<span class="light_purple">pickle.loads()</span>**

- **Magic Method：\_\_reduce\_\_**
    * ![image](https://hackmd.io/_uploads/SJESKL2Xke.png)

- **反序列化後就能夠<span class="red">進行RCE</span> → 不要使用網路上不信任的pretrain model**
