使用 gulp 壓縮圖片
================

請務必理解如下章節後閱讀此章節：

1. [安裝 Node 和 gulp](chapter1.md)
2. [使用 gulp 壓縮 JS](chapter2.md)


[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/13)
----------

壓縮 圖片文件可降低文件大小，提高圖片載入速度。

找到規律轉換為 gulp 代碼

規律
---
找到 `images/` 目錄下的所有文件，壓縮它們，將壓縮後的文件存放在 `dist/images/` 目錄下。

gulp 代碼
---------

你可以 [下載所有示例代碼](https://github.com/nimojs/gulp-book/archive/master.zip) 或 [在線查看代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter4)



**一、安裝 gulp-imagemin** 模組

提示：你需要使用命令行的 `cd` 切換至對應目錄再進行安裝操作和 gulp 啟動操作。

[學習使用命令行](chapter1.md)

在命令行輸入

```
npm instal gulp-imagemin
```

安裝成功後你會看到如下信息：(安裝時間可能會比較長)

```
gulp-imagemin@2.2.1 node_modules/gulp-imagemin
├── object-assign@2.0.0
├── pretty-bytes@1.0.3 (get-stdin@4.0.1)
├── chalk@1.0.0 (escape-string-regexp@1.0.3, ansi-styles@2.0.1, supports-color@1.3.1, has-ansi@1.0.3, strip-ansi@2.0.1)
├── through2-concurrent@0.3.1 (through2@0.6.3)
├── gulp-util@3.0.4 (array-differ@1.0.0, beeper@1.0.0, array-uniq@1.0.2, lodash._reevaluate@3.0.0, lodash._reescape@3.0.0, lodash._reinterpolate@3.0.0, replace-ext@0.0.1, minimist@1.1.1, vinyl@0.4.6, through2@0.6.3, multipipe@0.1.2, lodash.template@3.3.2, dateformat@1.0.11)
└── imagemin@3.1.0 (get-stdin@3.0.2, optional@0.1.3, vinyl@0.4.6, through2@0.6.3, stream-combiner@0.2.1, concat-stream@1.4.7, meow@2.1.0, vinyl-fs@0.3.13, imagemin-svgo@4.1.2, imagemin-optipng@4.2.0, imagemin-jpegtran@4.1.0, imagemin-pngquant@4.0.0, imagemin-gifsicle@4.1.0)
```

**二、創建 `gulpfile.js` 文件編寫代碼**

在對應目錄創建 `gulpfile.js` 文件並寫入如下內容：

```js
// 獲取 gulp
var gulp = require('gulp');

// 獲取 gulp-imagemin 模組
var imagemin = require('gulp-imagemin')

// 壓縮圖片任務
// 在命令行輸入 gulp images 啟動此任務
gulp.task('images', function () {
    // 1. 找到圖片
    gulp.src('images/*.*')
    // 2. 壓縮圖片
        .pipe(imagemin({
            progressive: true
        }))
    // 3. 另存圖片
        .pipe(gulp.dest('dist/images'))
});

// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 images 任務
    gulp.watch('images/*.*)', ['images'])
});

// 使用 gulp.task('default') 定義預設任務
// 在命令行使用 gulp 啟動 images 任務和 auto 任務
gulp.task('default', ['images', 'auto'])
```

你可以訪問 [gulp-imagemin](https://github.com/sindresorhus/gulp-imagemin) 以查看更多用法。

------

**三、在 `images/` 目錄下存放圖片**

在 `gulpfile.js` 對應目錄創建 `images` 文件夾，並在 `images/` 目錄下存放圖片。

你可以訪問 [https://github.com/nimojs/gulp-book/tree/master/demo/chapter4/images/](https://github.com/nimojs/gulp-book/tree/master/demo/chapter4/images/) 下載示例圖片


--------

**四、運行 gulp 查看效果**

在命令行輸入 `gulp` +回車

你將看到命令行出現如下提示

```
gulp
[18:10:42] Using gulpfile ~/Documents/code/gulp-book/demo/chapter4/gulpfile.js
[18:10:42] Starting 'images'...
[18:10:42] Finished 'images' after 5.72 ms
[18:10:42] Starting 'auto'...
[18:10:42] Finished 'auto' after 6.39 ms
[18:10:42] Starting 'default'...
[18:10:42] Finished 'default' after 5.91 μs
[18:10:42] gulp-imagemin: Minified 3 images (saved 25.83 kB - 5.2%)
```

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/13)

[接著閱讀：使用 gulp 編譯 LESS](chapter5.md)
