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
