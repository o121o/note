# OpenSSL 雜記

個人使用 OpenSSL 的紀錄，因為網路資源很少有 Windows 相關資料，所以這資料以 Windows 為主，若有錯誤或缺漏還請發 issues 。

## OpenSSL 是什麼
都在看 github 了，這個跳過(請參考 [Wikipedia](https://zh.wikipedia.org/zh-tw/OpenSSL) )。

## OpenSSL 安裝
### Linux
直接下 `openssl version` 有成功執行就表示已經安裝，
還沒安裝請上 [官網](https://www.openssl.org/) 執行安裝相關步驟

### Windows
#### [官網](https://www.openssl.org/) 安裝
最簡單……才怪，官網只有 Linux 的安裝包，其他請自己從原始碼打包，或是繼續往下看。

#### 從已經安裝的軟體找
1. Git

	沒錯， Git 裡面就有
	* 執行檔 [Git 資料夾]/usr/bin/openssl.exe
	* 設定檔 [Git 資料夾]/usr/ssl/openssl.cnf

1. Apache

	身為一個網頁伺服器，當然有
	* 執行檔 [Apache 資料夾]/bin/openssl.exe
	* 設定檔 [Apache 資料夾]/conf/openssl.cnf

## OpenSSL 執行
* 直接 `cd` 到執行檔所在資料夾就可以執行了，如果有需要引用外部檔案，相對路徑的基準點就是當前的資料夾(**config 除外**)
* 因為沒有設定環境變數，所以執行時若遇到 `Can't open C:\..../openssl.cnf for reading, No such file or directory` 請在最後加上 ` -config=[設定檔"完整路徑"]`

## OpenSSL 相關執行範例
預設皆使用 PEM 格式

### 產生公私鑰
這基本上算是使用 OpenSSL 最基礎的東西，舉凡要申請 SSL 憑證，或是要與其它系統交換資料，都會需要
* 先產生私鑰  
`openssl genrsa 4096` 就好了，記得把產生出來的字串另存  
當然也可以在後面加上 `> {想存的檔案名稱.key}`  
正常情況下檔案開頭會是 `-----BEGIN PRIVATE KEY-----`
正常情況下檔案結尾會是 `-----END PRIVATE KEY-----`

* 再產生公鑰  
`openssl rsa -in {私鑰檔案.key} -pubout`  
一樣也可以在後面加上 `> {想存的檔案名稱.public}`
正常情況下檔案開頭會是 `-----BEGIN PUBLIC KEY-----`
正常情況下檔案結尾會是 `-----END PUBLIC KEY-----`

### 產生 CSR
CSR 是產生 SSL 憑證必要的檔案之一，需要先準備一些必要資料(視申請的 SSL 等級而定，至少要知道是哪個網址要申請 SSL，其他可以直接 Enter 帶預設值)。
> Windows 使用一般指令方式只能申請一個網域，如果要申請多個請自行使用設定檔方式或直接改用 Linux 產生
```
openssl req -nodes -newkey rsa:2048 -keyout {私鑰檔案名稱.key} -out {檔案名稱.csr}
```
注意這邊的公私鑰是專屬於這個 CSR 使用的，不要搞混了
通常重新建立 CSR 時會同時重新建立對應的私鑰，如果想繼續使用也可以
```
openssl req -nodes -new -key {私鑰檔案名稱.key} -out {檔案名稱.csr}
```
`-nodes` 表示不需要加密  
`-newkey rsa:2048` 表示同時用 RSA-2048 建立私鑰  
`-new -key {私鑰檔案名稱.key}` 表示使用該私鑰建立 CSR  
`-out {檔案名稱.csr}` 要會出的 CSR 檔案名稱

* 輸入指令後會出現下列問題提供填寫  
Country Name (國家/地區二字簡稱) TW  
State or Province (地區全名) Taiwan  
Locality Name (地區名稱，選) Taipei  
Organization Name (組織全名) My Company  
Organizational Unit (部門名稱，選) RD  
Common Name (要申請 SSL 的網址) www.example.com  
Email Address (管理人 Email，選) admin@example.com

之後產產生的 CSR 就可以去向憑證機構申請 SSL 相關資料了