# web security
### 一個安全的web服務都是建立在不相信使用者任何操作所建立的<br/>一個駭客就是在非正常的行為下取得開發者非預期的資料或操作

### CTF思考路徑 : 
1) 觀察:路徑、網站的功能、 開發者工具
    - 路徑怎麼帶著資料的?![image](https://hackmd.io/_uploads/HkWyKLbQyg.png)
	- 噴了什麼錯
    - 網站可以用什麼操作，如:有什麼輸入框?有什麼按鈕?
    - 開發者工具![image](https://hackmd.io/_uploads/HJ4Lt8bQyx.png)
#### 觀察伴隨著思考，除了要看到網頁發生了什麼事，更要嘗試理解網頁怎麼做到的
2) 嘗試:
    - 路徑改改看、看看發生什麼事![image](https://hackmd.io/_uploads/Hy2j9U-7kx.png)
![image](https://hackmd.io/_uploads/ByoA98-m1l.png)
不同aid會導向不同的頁面
    - 有些資訊會寫在特定路徑下，如:robot.txt、/.htaccess
    - 開發者工具
    	- element看看有什麼元件可以玩的
        - source看看有什麼程式是被載入到網頁上的，再用console玩玩看
        - network看看網頁上的操作會發送request到後端，發送的方式、內容又是甚麼樣的形式
        - application看看cookies、storage存了啥東東
#### 依照你認為有可能的漏洞攻擊看看，可以一個一個試試看，但想要快速的確認方向，經驗的要求相對較高(hack sense)
![test](https://hackmd.io/_uploads/B1b3uv-Xkg.png)

## 有哪些常見的漏洞呢?
### 1. 權限控制失效(Broken Access Control) 
- 原因: 過度相信前端回傳的request或資料
- 包含: 直接撈取前端的資料做操作(insecure direct object reference)、沒有對路徑的權限控管(path traversal)、資訊瀏覽被錯誤的顯示或沒有控管(information leak)
- 特徵: 修改前端內容並傳送到server、dot dot slash attack(../../)
- CTF: 
    - [LAB: Cat Shop](#lab:Cat_Shop)
	- [HTB: baby nginxatsu](#baby_nginxatsu)
- 案例: 
    - 對網址內的學生資料過於信任，導致只需修改url的資料 可以隨意查詢非學生本人的個人資料
    連結: [國立中山大學 存取權限失控、歷年或全校學生個資外洩](https://zeroday.hitcon.org/vulnerability/ZD-2024-00683)
### 2. 注入式攻擊 (INJECTION)
- XSS注入(XSS injection)
	- 原因: 對輸入內容的html語句渲染
	- 特徵: 可以輸入到前端的內容會渲染html語法
	- 包含: 反射型XSS(Reflected)、DOM型XSS、儲存型XSS(Stored)
	- CTF: 
	- 案例: 
		- 沒有驗證parameter傳遞的內容，導致js語法可以被錯誤的執行
		連結: [國立臺東大學圖書資訊館 Reflected XSS](https://zeroday.hitcon.org/vulnerability/ZD-2024-01516)
	
- 命令注入弱點(Command injection)
	- 原因: 對於輸入的指令語句沒有控管導致錯誤的指令語句直接執行
	- 特徵: 輸入到terminal上的語法可以透過;來達到執行非預期的command語句
	- CTF: 
	- 案例: 
- CRLF injection
	- 原因: 在header沒有過濾掉\r\n，導致header設定執行到非預期的內容
	- 特徵: header中的\r\n沒有正確地被過濾
	- CTF: 
	- 案例: 
- code injection
	- 原因: 程式碼中直接調用使用者的輸入，以至於未知的程式語法被執行
	- 特徵: 依撰寫的程式語言不同
	- CTF: 
	- 案例: 
- SQL injection
	- 原因: sql語法被輸入內容錯誤的結束或閉合導致語法邏輯的錯誤判斷
	- 特徵: 「'」的輸入會使得使用端錯誤
	- CTF: 
	- 案例:
- Template injection
### 3. 伺服器端請求偽造(Server Side Request Forgery,SSRF)
- 原因: 沒有在對外server存取站點的控管，以致於內網的資訊被不當存取
- 特徵: 有對外發起request的功能
- CTF:
- 案例: 
### 4. 不安全的反序列化(insecure deserialization)
- 原因: 因錯誤的序列化方式或沒有對序列化的內容做驗證，讓非預期的內部基礎函式被調用
- 特徵: 
- CTF: 
- 案例: 

## lab:Cat_Shop
來源: 交大教學影片
### 第1輪觀察:
- 是一個購買商品的網站 (flag的購買頁面按鈕不能點按)
- 可以點選按鈕到商品的購買頁面並購買商品(發現網頁抓取前端的花費值並傳到後端)
- #### 網頁如何知道該顯示哪個商品頁面? :arrow_right: 可能透過url的有序id顯示畫面 
### 第1輪嘗試:
- 修改id後成功地進入flag的購買頁面 :arrow_right: 成功
### 第2輪觀察:
- 購買金額過高無法購買
- #### 可不可以調整金額? :arrow_right: 調整element的value值
### 第2輪嘗試:
- 調整element並點按購買 :arrow_right: 成功取得flag

### 總結:
- 透過更改url中parameter的id值達到進入開發者沒預期的頁面(水平越權)
     :arrow_right_hook: **insecure direct object reference - Horizontal privilege escalation**
- 透過更改element中cost的value值達到以開發者預期之外的價格做操作(垂直越權)
    :arrow_right_hook: **insecure direct object reference - Vertical privilege escalation**


## baby_nginxatsu
## ![94.237.51.182_41871_auth_login](https://hackmd.io/_uploads/ByRMAI441l.png)

### 第1次觀察:
- 是一個登陸頁面
- 可以註冊帳號
![94.237.51.182_41871_auth_login](https://hackmd.io/_uploads/HJSGC8EN1e.png)

### 第1次嘗試
- 用sql injection打打看
![94.237.51.182_41871_auth_login (1)](https://hackmd.io/_uploads/S1JYCUE4kl.png)
失敗了
![94.237.51.182_41871_auth_login (2)](https://hackmd.io/_uploads/S1eDnCI4Nyg.png)
### 第2次觀察:
- 註冊登陸後有一個自訂nginx設定的設定檔
![94.237.51.182_41871_](https://hackmd.io/_uploads/HJV_kv4EJx.png)
- 點了generate config，並進入config檢視畫面看了一段有關/storage的註解
![image](https://hackmd.io/_uploads/H12BeDE4Je.png)
![image](https://hackmd.io/_uploads/S1Uhew4NJg.png)
### 第2次嘗試:
- 嘗試直接進入 /storage的路徑下
![image](https://hackmd.io/_uploads/HyDm2y5Xyl.png)
進去了，還找到了db的backup

- 解壓縮後看裡面有一個sqlite的檔案
![image](https://hackmd.io/_uploads/rJ-aJlcXJg.png)
- 打開後看到裡面有這些table，只有users的table有值
![image](https://hackmd.io/_uploads/rywkGgqmyg.png)
- 看起來在users存了帳號跟加密的密碼，並且資料量並不多，我決定一筆筆嘗試
![image](https://hackmd.io/_uploads/BJsoflq7kx.png)
- 嘗試用常見的密碼加密方式解碼，最後找到用MD5解碼
![image](https://hackmd.io/_uploads/Hkii5g97Jx.png)
成功
![image](https://hackmd.io/_uploads/Hyu3GwNEye.png)
### 總結:
- 把私密資訊或註解顯示在前端
	:arrow_right_hook: **information leak**
- 透過更改url來存取檔案儲存地址
    :arrow_right_hook: **path traversal**