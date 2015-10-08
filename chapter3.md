使用 gulp 壓縮 CSS
================

請務必理解如下章節後閱讀此章節：

1. [安裝 Node 和 gulp](chapter1.md)
2. [使用 gulp 壓縮 JS](chapter2.md)

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/12)

----------


壓縮 css 代碼可降低 css 文件大小，提高頁面打開速度。

我們接著將規律轉換為 gulp 代碼

規律
---
找到 `css/` 目錄下的所有 css 文件，壓縮它們，將壓縮後的文件存放在 `dist/css/` 目錄下。

gulp 代碼
---------

你可以 [下載所有示例代碼](https://github.com/nimojs/gulp-book/archive/master.zip) 或 [在線查看代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter3)

當熟悉 [使用 gulp 壓縮 JS](chapter2.md) 的方法後，配置壓縮 CSS 的 gulp 代碼就變得很輕鬆。


**一、安裝 gulp-minify-css** 模組

提示：你需要使用命令行的 `cd` 切換到對應目錄後進行安裝操作。

[學習使用命令行](chapter1.md)

在命令行輸入

```
npm install gulp-minify-css
```

安裝成功後你會看到如下信息：(安裝時間可能會比較長)

```
gulp-minify-css@1.0.0 node_modules/gulp-minify-css
├── object-assign@2.0.0
├── vinyl-sourcemaps-apply@0.1.4 (source-map@0.1.43)
├── clean-css@3.1.8 (commander@2.6.0, source-map@0.1.43)
├── through2@0.6.3 (xtend@4.0.0, readable-stream@1.0.33)
├── vinyl-bufferstream@1.0.1 (bufferstreams@1.0.1)
└── gulp-util@3.0.4 (array-differ@1.0.0, beeper@1.0.0, array-uniq@1.0.2, lodash._reescape@3.0.0, lodash._reinterpolate@3.0.0, lodash._reevaluate@3.0.0, replace-ext@0.0.1, minimist@1.1.1, multipipe@0.1.2, vinyl@0.4.6, chalk@1.0.0, lodash.template@3.3.2, dateformat@1.0.11)
```

**二、參照 [使用 gulp 壓縮 JS](chapter2.md) 創建 `gulpfile.js` 文件編寫代碼**

在對應目錄創建 `gulpfile.js` 文件並寫入如下內容：

```js
// 獲取 gulp
var gulp = require('gulp')

// 獲取 minify-css 模組（用於壓縮 CSS）
var minifyCSS = require('gulp-minify-css')

// 壓縮 css 文件
// 在命令行使用 gulp css 啟動此任務
gulp.task('css', function () {
    // 1. 找到文件
    gulp.src('css/*.css')
    // 2. 壓縮文件
        .pipe(minifyCSS())
    // 3. 另存為壓縮文件
        .pipe(gulp.dest('dist/css'))
})

// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 css 任務
    gulp.watch('css/*.css', ['css'])
});

// 使用 gulp.task('default') 定義預設任務
// 在命令行使用 gulp 啟動 css 任務和 auto 任務
gulp.task('default', ['css', 'auto'])
```

你可以訪問 [gulp-minify-css](https://github.com/jonathanepollack/gulp-minify-css) 以查看更多用法。

------

**三、創建 css 文件**

在 `gulpfile.js` 對應目錄創建 `css` 文件夾，並在 `css/` 目錄下創建 `a.css` 文件。

```css
/* a.css */
body a{
    color:pink;
}
```

--------

**四、運行 gulp 查看效果**

在命令行輸入 `gulp` +回車

你將看到命令行出現如下提示

```
gulp
[17:01:19] Using gulpfile ~/Documents/code/gulp-book/demo/chapter3/gulpfile.js
[17:01:19] Starting 'css'...
[17:01:19] Finished 'css' after 6.21 ms
[17:01:19] Starting 'auto'...
[17:01:19] Finished 'auto' after 5.42 ms
[17:01:19] Starting 'default'...
[17:01:19] Finished 'default' after 5.71 μs
```

gulp 會創建 `dist/css` 目錄，並創建 `a.css` 文件，此文件存放壓縮後的 css 代碼。
[dist/css/a.css](https://github.com/nimojs/gulp-book/blob/master/demo/chapter3/dist/css/a.css)

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/12)

[接著閱讀：使用 gulp 壓縮圖片](chapter4.md)
