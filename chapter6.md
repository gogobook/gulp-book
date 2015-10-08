使用 gulp 編譯 Sass
==================

請務必理解如下章節後閱讀此章節：

1. [安裝 Node 和 gulp](chapter1.md)
2. [使用 gulp 壓縮 JS](chapter2.md)

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/15)

----------

> Sass 是一種 CSS 的開發工具，提供了許多便利的寫法，大大節省了開發者的時間，使得 CSS 的開發，變得簡單和可維護。

本章使用 `ruby-sass` 編譯 css,若你沒有安裝 ruby 和 sass 請移步 [使用ruby.taobao安裝 Sass](https://github.com/nimojs/blog/issues/14)

安裝
---

```
npm install gulp-ruby-sass
```

基本用法
-------

你可以 [下載所有示例代碼](https://github.com/nimojs/gulp-book/archive/master.zip) 或 [在線查看代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter6)

```js
// 獲取 gulp
var gulp = require('gulp')
// 獲取 gulp-ruby-sass 模組
var sass = require('gulp-ruby-sass')

// 編譯sass
// 在命令行輸入 gulp sass 啟動此任務
gulp.task('sass', function() {
    return sass('sass/') 
    .on('error', function (err) {
      console.error('Error!', err.message);
   })
    .pipe(gulp.dest('dist/css'))
});


// 在命令行使用 gulp auto 啟動此任務
gulp.task('auto', function () {
    // 監聽文件修改，當文件被修改則執行 images 任務
    gulp.watch('sass/**/*.scss', ['sass'])
});

// 使用 gulp.task('default') 定義預設任務
// 在命令行使用 gulp 啟動 sass 任務和 auto 任務
gulp.task('default', ['sass', 'auto'])
```


Sass 代碼和編譯後的 CSS 代碼
----------

[sass/a.scss](https://github.com/nimojs/gulp-book/tree/master/demo/chapter6/sass/a.scss)

```css
.sass{
	a{
        color:pink;
    }
}
```

[sass/import.scss](https://github.com/nimojs/gulp-book/tree/master/demo/chapter6/sass/import.scss)


```css
@import "a.scss";
.import{
	a{
		color:red;
    }
}
```

[sass/a.css](https://github.com/nimojs/gulp-book/tree/master/demo/chapter6/dist/css/a.css)

```css
.sass a {
  color: pink;
}
```

[sass/import.css](https://github.com/nimojs/gulp-book/tree/master/demo/chapter6/dist/css/import.css)

```css
.sass a {
  color: pink;
}
.import a{
  color: red;
}
```
[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/15)

[接著閱讀：使用 gulp 開發一個項目](chapter7.md)
