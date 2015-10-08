使用 gulp 編譯 LESS
================

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/14)

請務必理解如下章節後閱讀此章節：

1. [安裝 Node 和 gulp](chapter1.md)
2. [使用 gulp 壓縮 JS](chapter2.md)

----------

> Less 是一門 CSS 預處理語言，它擴充了 CSS 語言，增加了諸如變數、混合（mixin）、函數等功能，讓 CSS 更易維護。


安裝
---

```
npm install gulp-less
```

基本用法
-------

你可以 [下載所有示例代碼](https://github.com/nimojs/gulp-book/archive/master.zip) - [或在線查看代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter5)

```js
// 獲取 gulp
var gulp = require('gulp')
// 獲取 gulp-less 模組
var less = require('gulp-less')

// 編譯less
// 在命令行輸入 gulp less 啟動此任務
gulp.task('less', function () {
    // 1. 找到 less 文件
    gulp.src('less/**.less')
    // 2. 編譯為css
        .pipe(less())
    // 3. 另存文件
        .pipe(gulp.dest('dist/css'))
});

// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 less 任務
    gulp.watch('less/**.less', ['less'])
})

// 使用 gulp.task('default') 定義預設任務
// 在命令行使用 gulp 啟動 less 任務和 auto 任務
gulp.task('default', ['less', 'auto'])
```

你可以訪問 [gulp-less](https://github.com/plus3network/gulp-less) 以查看更多用法。

LESS 代碼和編譯後的CSS代碼
----------

[less/a.less](https://github.com/nimojs/gulp-book/tree/master/demo/chapter5/less/a.less)

```css
.less{
	a{
        color:pink;
    }
}
```
[less/import.less](https://github.com/nimojs/gulp-book/tree/master/demo/chapter5/less/import.less)


```css
@import "a.less";
.import{
	a{
		color:red;
    }
}
```
[less/a.css](https://github.com/nimojs/gulp-book/tree/master/demo/chapter5/dist/css/a.css)

```css
.less a {
  color: pink;
}
```
[less/import.css](https://github.com/nimojs/gulp-book/tree/master/demo/chapter5/dist/css/import.css)

```css
.less a {
  color: pink;
}
.import a{
  color: red;
}
```

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/14)

[接著閱讀：使用 gulp 編譯 Sass](chapter6.md)
