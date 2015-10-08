安裝 Node 和 gulp
================

gulp 是基於 node 實現的，那麼我們就需要先安裝 node。

> Node 是一個基於Chrome JavaScript V8引擎建立的一個平台，可以利用它實現 Web服務，做類似PHP的事。

打開 https://nodejs.org/ 點擊綠色的 **INSTALL** 按鈕下載安裝 node。

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/10)

<a href="#hash_cli" name="hash_cli"></a>

使用終端/命令行
-------------

### 命令行
在 Windows 中可按 <kbd>徽標鍵</kbd>（alt鍵左邊）+ <kbd>R</kbd> 打開輸入 `cmd` + <kbd>Enter</kbd> 打開命令行。

### 終端(Mac)
打開 Launchpad（像火箭一樣的圖標），在屏幕上方搜索框中輸入 `終端` + <kbd>Enter</kbd> 打開終端。

### 查看 node 版本號
在終端/命令行中輸入 `node -v` 檢測node是否安裝成功，安裝成功會顯示出 node 的版本號。

### 跳轉目錄
終端/命令行 中可使用 `cd 目錄名` 跳轉至指定目錄，Mac 中還可以使用 `ls` 查看當前目錄下的文件列表。

#### Windows
Windows 下可使用如下命令跳轉至指定目錄：

```
// 跳轉至 C 盤根目錄
cd c:\
// 跳轉至當前目錄的 demo 文件夾
cd demo
// 跳轉至上一級
cd ..
```

#### Mac
Mac 中建議只在 Documents 目錄下進行文件操作。

```
// 跳轉至文檔目錄
cd /Users/你的用戶名/Documents/
// 或第一次打開終端時直接輸入
cd Documents
// 查看目錄下文件列表
ls
// 創建文件夾
mkdir demo
// 跳轉至當前目錄下的 demo 文件夾
cd demo
// 跳轉至上級目錄
cd ..
```

### 退出運行狀態
如果你在命令行中啟動了一些一直運行的命令，你的命令行會進入「運行」狀態，此時你不可以在命令行進行其他操作。可透過 `Ctrl + C` 停止 gulp。（Mac 中使用 `control + C`）

後面的章節中如果代碼中存在 `gulp.watch` 並在命令行運行了 `gulp` 則需要使用 `Ctrl + C` 退出任務。

npm 模組管理器
-------------
如果你瞭解 npm 則跳過此章節

若你不瞭解npm 請閱讀 [npm模組管理器](http://javascript.ruanyifeng.com/nodejs/npm.html)

安裝 gulp
----

npm 是 node 的包管理工具，可以利用它安裝 gulp 所需的包。（在安裝 node 時已經自動安裝了 npm）

在命令行輸入

```
npm install -g gulp 
```

若一直沒安裝成功，請[使用 cnpm 安裝](https://github.com/nimojs/blog/issues/20)(npm的國內加速鏡像)

意思是：使用 npm 安裝全局性的(`-g`) gulp 包。

> 如果你安裝失敗，請輸入`sudo npm install -g gulp `使用管理員權限安裝。（可能會要求輸入密碼）

安裝時請注意命令行的提示信息，安裝完成後可在命令行輸入 `gulp -v` 以確認安裝成功。

至此，我們完成了準備工作。接著讓 gulp 開始幫我們幹活吧！

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/10)

[閱讀下一章節：使用 gulp 壓縮JS](chapter2.md)
