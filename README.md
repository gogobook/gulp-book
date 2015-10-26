這是forked from [nimojs/gulp-book](https://github.com/nimojs/gulp-book)。<br>
由於不習慣看簡體字，因此順手轉成正體。在此感謝原版的貢獻。
gulp 入門指南
===========

gulp 是基於 node 實現 Web 前端自動化開發的工具，利用它能夠極大的提高開發效率。

在 Web 前端開發工作中有很多「重複工作」，比如壓縮CSS/JS文件。而這些工作都是有規律的。找到這些規律，並編寫 gulp 配置代碼,讓 gulp 自動執行這些「重複工作」。

點擊右上角的 **[Watch](https://github.com/nimojs/gulp-book/subscription)** 訂閱本書，點擊 Star 收藏本書。

- [訂閱本書](https://github.com/nimojs/gulp-book/issues/7)
- [論壇](https://github.com/nimojs/gulp-book/issues)

**因為 Node 的全局包安裝都是在C盤，所有請在C盤使用 gulp 以方便熟悉 gulp **

## 目錄

- [安裝 Node 和 gulp](chapter1.md)
- [使用 gulp 壓縮 JS](chapter2.md)
- [使用 gulp 壓縮 CSS](chapter3.md)
- [使用 gulp 壓縮圖片](chapter4.md)
- [使用 gulp 編譯 LESS](chapter5.md)
- [使用 gulp 編譯 Sass](chapter6.md)
- [使用 gulp 構建一個項目](chapter7.md)




將規律轉換為 gulp 代碼
-------------------

現有目錄結構如下：

```
└── js/
    └── a.js
```

### 規律

1. 找到 js/目錄下的所有 .js 文件
2. 壓縮這些 js 文件
3. 將壓縮後的代碼另存在 dist/js/ 目錄下

### 編寫 gulp 代碼

```js
// 壓縮 JavaScript 文件
gulp.task('script', function() {
    // 1. 找到
    gulp.src('js/*.js')
    // 2. 壓縮
        .pipe(uglify())
    // 3. 另存
        .pipe(gulp.dest('dist/js'));
});
```

### 代碼執行結果

代碼執行後文件結構

```
└── js/
│   └── a.js
└── dist/
    └── js/
        └── a.js
```

a.js 壓縮前
```
function demo (msg) {
    alert('--------\r\n' + msg + '\r\n--------')
}

demo('Hi')
```
a.js 壓縮後
```
function demo(n){alert("--------\r\n"+n+"\r\n--------")}demo("Hi");
```

此時 `dist/js` 目錄下的 `.js` 文件都是壓縮後的版本。

你還可以監控 `js/` 目錄下的 js 文件，當某個文件被修改時，自動壓縮修改文件。啟動 gulp 後就可以讓它幫助你自動構建 Web 項目。

-----------------

gulp 還可以做很多事，例如：

1. 壓縮CSS
2. 壓縮圖片
3. 編譯Sass/LESS
4. 編譯CoffeeScript
5. markdown 轉換為 html

[開始閱讀：安裝 Node 和 gulp](chapter1.md)
