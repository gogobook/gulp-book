使用 gulp 壓縮 JS
================

請務必理解如下章節後閱讀此章節：

1. [安裝 Node 和 gulp](chapter1.md)

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/11)

----------

壓縮 js 代碼可降低 js 文件大小，提高頁面打開速度。在不利用 gulp 時我們需要透過各種工具手動完成壓縮工作。

所有的 gulp 代碼編寫都可以看做是將規律轉化為代碼的過程。

規律
---

找到 `js/` 目錄下的所有 js 文件，壓縮它們，將壓縮後的文件存放在 `dist/js/` 目錄下。

gulp 代碼
----

你可以 [下載所有示例代碼](https://github.com/nimojs/gulp-book/archive/master.zip) 或 [在線查看代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter2)

**建議**：你可以只閱讀下面的代碼與註釋或同時閱讀代碼解釋

gulp 的所有配置代碼都寫在 `gulpfile.js` 文件。

**一、新建一個 `gulpfile.js` 文件**
```
chapter2
└── gulpfile.js
```

---------

**二、在 `gulpfile.js` 中編寫代碼**

```js
// 獲取 gulp
var gulp = require('gulp')
```

> `require()` 是 node （CommonJS）中獲取模組的語法。
> 
> 在 gulp 中你只需要理解 `require()` 可以獲取模組。

---------

**三、獲取 `gulp-uglify` 組件**

```js
// 獲取 uglify 模組（用於壓縮 JS）
var uglify = require('gulp-uglify')
```

---------

**四、創建壓縮任務**

```js
// 壓縮 js 文件
// 在命令行使用 gulp script 啟動此任務
gulp.task('script', function() {
    // 1. 找到文件
    gulp.src('js/*.js')
    // 2. 壓縮文件
        .pipe(uglify())
    // 3. 另存壓縮後的文件
        .pipe(gulp.dest('dist/js'))
})
```

- `gulp.task(name, fn)` - 定義任務，第一個參數是任務名，第二個參數是任務內容。
- `gulp.src(path)` - 選擇文件，傳入參數是文件路徑。
- `gulp.dest(path)` - 輸出文件
- `gulp.pipe()` - 管道，你可以暫時將 pipe 理解為將操作加入執行隊列

參考：[gulp API文檔](http://www.gulpjs.com.cn/docs/api/)

---------

**五、跳轉至 `gulpfile.js` 所在目錄**

打開命令行使用 `cd` 命令跳轉至 `gulpfile.js` 文件所在目錄。

例如我的 `gulpfile.js` 文件保存在 `C:\gulp-book\demo\chapter2\gulpfile.js`。

那麼就需要在命令行輸入
```
cd C:\gulp-book\demo\chapter2
```

> Mac 用戶可使用 `cd Documents/gulp-book/demo/chapter2/` 跳轉

---------

**六、使用命令行運行 script 任務**

在控制台輸入 `gulp 任務名` 可運行任務，此處我們輸入 `gulp script` 回車。

注意：輸入 `gulp script` 後命令行將會提示錯誤信息
```
// 在命令行輸入
gulp script

Error: Cannot find module 'gulp-uglify'
    at Function.Module._resolveFilename (module.js:338:15)
    at Function.Module._load (module.js:280:25)
```

`Cannot find module 'gulp-uglify'` 沒有找到 `gulp-uglify` 模組。

----------

**七、安裝 `gulp-uglify` 模組**

因為我們並沒有安裝 `gulp-uglify` 模組到本地，所以找不到此模組。

使用 npm 安裝 `gulp-uglify` 到本地

```
npm install gulp-uglify
```

安裝成功後你會看到如下信息：
```
gulp-uglify@1.1.0 node_modules/gulp-uglify
├── deepmerge@0.2.7
├── uglify-js@2.4.16 (uglify-to-browserify@1.0.2, async@0.2.10, source-map@0.1.34, optimist@0.3.7)
├── vinyl-sourcemaps-apply@0.1.4 (source-map@0.1.43)
├── through2@0.6.3 (xtend@4.0.0, readable-stream@1.0.33)
└── gulp-util@3.0.4 (array-differ@1.0.0, beeper@1.0.0, array-uniq@1.0.2, object-assign@2.0.0, lodash._reinterpolate@3.0.0, lodash._reescape@3.0.0, lodash._reevaluate@3.0.0, replace-ext@0.0.1, minimist@1.1.1, chalk@1.0.0, lodash.template@3.3.2, vinyl@0.4.6, multipipe@0.1.2, dateformat@1.0.11)
chapter2 $
```

在你的文件夾中會新增一個 `node_modules` 文件夾，這裡面存放著 npm 安裝的模組。

目錄結構：
```
├── gulpfile.js
└── node_modules
	└── gulp-uglify
```

接著輸入 `gulp script` 執行任務

```
gulp script
[13:34:57] Using gulpfile ~/Documents/code/gulp-book/demo/chapter2/gulpfile.js
[13:34:57] Starting 'script'...
[13:34:57] Finished 'script' after 6.13 ms
```

------------

**八、編寫 js 文件**

我們發現 gulp 沒有進行任何壓縮操作。因為沒有js這個目錄，也沒有 js 目錄下的 `.js` 後綴文件。

創建 `a.js` 文件，並編寫如下內容

```
// a.js
function demo (msg) {
    alert('--------\r\n' + msg + '\r\n--------')
}

demo('Hi')
```

目錄結構：
```
├── gulpfile.js
├──  js
│	└── a.js
└── node_modules
	└── gulp-uglify
```

接著在命令行輸入 `gulp script` 執行任務

gulp 會在命令行當前目錄下創建 `dist/js/` 文件夾，並創建壓縮後的 `a.js` 文件。

目錄結構：
```
├── gulpfile.js
├──  js
│	└── a.js
├──  dist
│	└── js
│		└── a.js
└── node_modules
	└── gulp-uglify
```

[dist/js/a.js](https://github.com/nimojs/gulp-book/blob/master/demo/chapter2/dist/js/a.js)
```js
function demo(n){alert("--------\r\n"+n+"\r\n--------")}demo("Hi");
```

---------

**九、檢測代碼修改自動執行任務**

`js/a.js`一旦有修改 就必須重新在命令行輸入 `gulp script` ，這很麻煩。

可以使用 `gulp.watch(src, fn)` 檢測指定目錄下文件的修改後執行任務。

在 `gulpfile.js` 中編寫如下代碼：
```
// 監聽文件修改，當文件被修改則執行 script 任務
gulp.watch('js/*.js', ['script']);
```

但是沒有命令可以運行 `gulp.watch()`，需要將 `gulp.watch()` 包含在一個任務中。

```
// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 script 任務
    gulp.watch('js/*.js', ['script'])
})
```

接在在命令行輸入 `gulp auto`，自動監聽 `js/*.js` 文件的修改後壓縮js。

```
$gulp auto
[21:09:45] Using gulpfile ~/Documents/code/gulp-book/demo/chapter2/gulpfile.js
[21:09:45] Starting 'auto'...
[21:09:45] Finished 'auto' after 9.19 ms
```

此時修改 `js/a.js` 中的代碼並保存。命令行將會出現提示，表示檢測到文件修改並壓縮文件。

```
[21:11:01] Starting 'script'...
[21:11:01] Finished 'script' after 2.85 ms
```
至此，我們完成了 gulp 壓縮 js 文件的自動化代碼編寫。

**注意：**使用 `gulp.watch` 後你的命令行會進入「運行」狀態，此時你不可以在命令行進行其他操作。可透過 `Ctrl + C` 停止 gulp。

> Mac 下使用 `control + C` 停止 gulp

**十、使用 gulp.task('default', fn) 定義預設任務**

增加如下代碼

```js
gulp.task('default', ['script', 'auto']);
```

此時你可以在命令行直接輸入 `gulp` +回車，運行 `script` 和 `auto` 任務。

最終代碼如下：

```js
// 獲取 gulp
var gulp = require('gulp')

// 獲取 uglify 模組（用於壓縮 JS）
var uglify = require('gulp-uglify')

// 壓縮 js 文件
// 在命令行使用 gulp script 啟動此任務
gulp.task('script', function() {
    // 1. 找到文件
    gulp.src('js/*.js')
    // 2. 壓縮文件
        .pipe(uglify())
    // 3. 另存壓縮後的文件
        .pipe(gulp.dest('dist/js'))
})

// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 script 任務
    gulp.watch('js/*.js', ['script'])
})


// 使用 gulp.task('default') 定義預設任務
// 在命令行使用 gulp 啟動 script 任務和 auto 任務
gulp.task('default', ['script', 'auto'])
```

去除註釋後，你會發現只需要 11 行代碼就可以讓 gulp 自動監聽 js 文件的修改後壓縮代碼。但是還有還有一些性能問題和缺少容錯性，將在後面的章節詳細說明。


你可以訪問 [gulp-uglify](https://github.com/terinjokes/gulp-uglify) 以查看更多用法。

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/11)

[接著閱讀：使用 gulp 壓縮 CSS](chapter3.md)
