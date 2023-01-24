# Apache HTTP Server 雜記

[官方文件](https://httpd.apache.org/docs/2.4/configuring.html)  
個人使用 Apache 的紀錄，方便自己日後查詢，若有錯誤或缺漏還請發 issues 。

## Apache  HTTP Server 是什麼

一個歷史悠久的網頁伺服器，內部有許多相關的軟體，目前主要版本為 2 ，相關說明請看 [Wikipedia](https://zh.wikipedia.org/zh-tw/Apache_HTTP_Server)。

其他相關的軟體有： [IIS](https://zh.wikipedia.org/zh-tw/%E7%B6%B2%E9%9A%9B%E7%B6%B2%E8%B7%AF%E8%B3%87%E8%A8%8A%E6%9C%8D%E5%8B%99) 、 [Nginx](https://zh.wikipedia.org/zh-tw/Nginx)

## 下載
### Linux
執行套件相關安裝 apache 或 apache2
### Windows
去官網...只有原始碼  
要去其他編譯好的網站裝，個人習慣是 [Apache Lounge](https://www.apachelounge.com/download/)

## 設定檔
### Linux
主要設定檔原則上是在 /etc/apache2/apache2.conf

### Windows
主要設定檔原則上是在 {安裝路徑}/conf/httpd.conf

## 設定檔語法概說
會說概說的原因是因為我還真不知道還有多少語法...
* \# 只要是 \# 開頭的都是註解
* 標籤(<>)除少數例外，是可以巢狀設定的
* 後面的設定會覆寫前面的設定

## 常見指令
* Define  
定義變數  
只要上面有 `Define {變數名稱} {變數值}` 就可以把 `${{變數名稱}}` 視為 `{變數值}` 使用  
同時也可以使用 IfDefine 標籤判斷該變數是否已被定義  
> 官方警告，變數名稱不可有 ":" 避免跟 RewriteMap 的語法衝突
* ServerRoot  
最根本的路徑，所有設定的路徑都從這裡(通常是主資料夾的路徑)
* Listen  
要監聽的 Port  
常見寫法為 `Listen {Port}`  
如果要限制來源也可以使用 `Listen {IP}:{Port}`  
只是如果要限制來源通常會使用其他方式來進行限制(如：VirtualHost 標籤、Require 指令)
* LoadModule  
載入模組
> 注意，模組不要亂加，因為有些設定是用 IfModule 標籤包起來，有載入對應的模組才會觸發，所以任意載入模組可能會造成設定大亂
* LoadFile  
載入特定檔案(例如外掛的 .dll .so 檔案)
* ServerAdmin  
這伺服器的管理者信箱
* ServerName  
主機名稱  
當使用者輸入此網址時，提供服務(主要在 VirtualHost 標籤中會特別需要)
* ServerAlias  
主機名稱別名  
> 通常使用萬用字元時使用
* AllowOverride (只在 Directory 標籤內有效)  
是否啟用 .htaccess 以及允許執行的項目類型(新版本 Apache 預設為 None 實務上常設定為 All)
* ErrorLog  
錯誤日誌  
設定錯誤日誌的路徑，如果是相對路徑，則會與 ServerRoot 組合  
如果使用 pipe (|) 開頭，則會將錯誤訊息寫入該命令中
* Include  
載入其他設定檔  
如果該設定檔可能不存在，則建議使用 IncludeOptional 指令
* Options  
目錄選項  
通常建議設定為 +FollowSymLinks -Indexes +ExecCGI
* ServerTokens  
回應相關設定的訊息  
一般建議設定為 Prod 避免透漏過多版本訊息，但其他人仍可透過其他方式知道相關訊息


## 常見標籤
* Directory  
目錄設定  
設定目錄相關設定，可使用 regex
> 特例： <Directory "/"> 表示所有透過 Apache 讀取的資料，強烈建議設定  
> AllowOverride none  
> Require all denied  
* DocumentRoot  
網站根目錄  
所有 URI 皆會從此處讀取  
如果是相對路徑，則會與 ServerRoot 組合
* Files  
檔案設定  
設定檔案相關設定，可使用 regex
* IfModule
判斷是否有載入對應的模組
可以使用 identifier (ex. rewrite_module) 或 file (ex. mod_rewrite.c)
* VirtualHost  
虛擬主機設定
