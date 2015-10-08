使用 gulp 構建一個項目
==================


請務必理解前面的章節後閱讀此章節：

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/16)

----------

本章將介紹
- [gulp-watch-path](https://github.com/nimojs/gulp-watch-path)
- [stream-combiner2](https://github.com/gulpjs/gulp/blob/master/docs/recipes/combining-streams-to-handle-errors.md)
- [gulp-sourcemaps](https://github.com/floridoo/gulp-sourcemaps)
- [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer/blob/master/package.json)

並將之前所有章節的內容組合起來編寫一個前端項目所需的 gulp 代碼。

你可以在 [nimojs/gulp-demo](https://github.com/nimojs/gulp-demo) 查看完整代碼。

若你不瞭解npm 請務必閱讀 [npm模組管理器](http://javascript.ruanyifeng.com/nodejs/npm.html)

package.json
------------

如果你熟悉 npm 則可以利用 `package.json` 保存所有 `npm install --save-dev gulp-xxx` 模組依賴和模組版本。

在命令行輸入

```
npm init
```

會依次要求補全項目信息，不清楚的可以直接回車跳過
```
name: (gulp-demo) 
version: (1.0.0) 
description: 
entry point: (index.js) 
test command: 
...
...
Is this ok? (yes) 
```

最終會在當前目錄中創建 `package.json` 文件並生成類似如下代碼：
```js
{
  "name": "gulp-demo",
  "version": "0.0.0",
  "description": "",
  "scripts": {
    "test": "echo \"Error: no test specified\" && exit 1"
  },
  "repository": {
    "type": "git",
    "url": "https://github.com/nimojs/gulp-demo.git"
  },
  "keywords": [
    "gulp",
  ],
  "author": "nimojs <nimo.jser@gmail.com>",
  "license": "MIT",
  "bugs": {
    "url": "https://github.com/nimojs/gulp-demo/issues"
  },
  "homepage": "https://github.com/nimojs/gulp-demo"
}
```

### 安裝依賴

安裝 gulp 到項目（防止全局 gulp 升級後與此項目 `gulpfile.js` 代碼不兼容）
```
npm install gulp --save-dev
```

此時打開 `package.json` 會發現多了如下代碼

```js
"devDependencies": {
	"gulp": "^3.8.11"
}
```

聲明此項目的開發依賴 gulp

接著安裝其他依賴：

> 安裝模組較多，請耐心等待，若一直安裝失敗可使用[npm.taobao.org](http://npm.taobao.org/)


```
npm install gulp-uglify gulp-watch-path stream-combiner2 gulp-sourcemaps gulp-minify-css gulp-autoprefixer gulp-less gulp-ruby-sass gulp-imagemin gulp-util --save-dev
```
此時，[package.json](https://github.com/nimojs/gulp-demo/blob/master/package.json) 將會更新
```js
"devDependencies": {
    "colors": "^1.0.3",
    "gulp": "^3.8.11",
    "gulp-autoprefixer": "^2.1.0",
    "gulp-imagemin": "^2.2.1",
    "gulp-less": "^3.0.2",
    "gulp-minify-css": "^1.0.0",
    "gulp-ruby-sass": "^1.0.1",
    "gulp-sourcemaps": "^1.5.1",
    "gulp-uglify": "^1.1.0",
    "gulp-watch-path": "^0.0.7",
    "stream-combiner2": "^1.0.2"
}
```

當你將這份 gulpfile.js 配置分享給你的朋友時，就不需要將 `node_modules/` 發送給他，他只需在命令行輸入
```
npm install
```
就可以檢測 `package.json` 中的 `devDependencies` 並安裝所有依賴。

設計目錄結構
----------
我們將文件分為2類，一類是源碼，一類是編譯壓縮後的版本。文件夾分別為 `src` 和 `dist`。(注意區分 `dist` 和 ·`dest` 的區別)

```
└── src/
│
└── dist/
```

`dist/` 目錄下的文件都是根據 `src/` 下所有源碼文件構建而成。

在 `src/` 下創建前端資源對應的的文件夾

```
└── src/
	├── less/    *.less 文件
	├── sass/    *.scss *.sass 文件
	├── css/     *.css  文件
	├── js/      *.js 文件
	├── fonts/   字體文件
    └── images/   圖片
└── dist/
```

你可以點擊 [nimojs/gulp-demo](https://github.com/nimojs/gulp-demo/archive/master.zip) 下載本章代碼。

讓命令行輸出的文字帶顏色
-------------------
gulp 自帶的輸出都帶時間和顏色，這樣很容易識別。我們利用 [gulp-util](https://github.com/gulpjs/gulp-util) 實現同樣的效果。

```js
var gulp = require('gulp')
var gutil = require('gulp-util')

gulp.task('default', function () {
    gutil.log('message')
    gutil.log(gutil.colors.red('error'))
    gutil.log(gutil.colors.green('message:') + "some")
})
```
使用 `gulp` 啟動預設任務以測試
![gulp-util](https://cloud.githubusercontent.com/assets/3949015/7137629/a1def1b8-e2ed-11e4-83e0-5a6adb22de6f.png)

配置 JS 任務
--------
### gulp-uglify
檢測`src/js/`目錄下的 js 文件修改後，壓縮 `js/` 中所有 js 文件並輸出到 `dist/js/` 中

```js
var uglify = require('gulp-uglify')

gulp.task('uglifyjs', function () {
    gulp.src('src/js/**/*.js')
        .pipe(uglify())
        .pipe(gulp.dest('dist/js'))
})

gulp.task('default', function () {
    gulp.watch('src/js/**/*.js', ['uglifyjs'])
})
```

`src/js/**/*.js` 是 glob 語法。[百度百科：glob模式](http://baike.baidu.com/view/4019153.htm) 、[node-glob](https://github.com/isaacs/node-glob)

在命令行輸入 `gulp` 後會出現如下消息，表示已經啟動。
```ruby
[20:39:50] Using gulpfile ~/Documents/code/gulp-book/demo/chapter7/gulpfile.js
[20:39:50] Starting 'default'...
[20:39:50] Finished 'default' after 13 ms
```


此時編輯 [src/js/log.js](https://github.com/nimojs/gulp-demo/blob/master/src/js/log.js) 文件並保存，命令行會出現如下消息，表示檢測到 `src/js/**/*.js` 文件修改後重新編譯所有 js。

```ruby
[20:39:52] Starting 'js'...
[20:39:52] Finished 'js' after 14 ms
```

### gulp-watch-path
此配置有個性能問題，當 `gulp.watch` 檢測到  `src/js/` 目錄下的js文件有修改時會將所有文件全部編譯。實際上我們只需要重新編譯被修改的文件。

簡單介紹 `gulp.watch` 第二個參數為 `function` 時的用法。

```js
gulp.watch('src/js/**/*.js', function (event) {
    console.log(event);
    /*
	當修改 src/js/log.js 文件時
    event {
		// 發生改變的類型，不管是添加，改變或是刪除
        type: 'changed', 
		// 觸發事件的文件路徑
        path: '/Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js'
    }
    */
})
```

我們可以利用 `event` 給到的信息，檢測到某個 js 文件被修改時，只編寫當前修改的 js 文件。

可以利用 `gulp-watch-path` 配合 `event` 獲取編譯路徑和輸出路徑。

```js
var watchPath = require('gulp-watch-path')

gulp.task('watchjs', function () {
    gulp.watch('src/js/**/*.js', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')
        /*
        paths
            { srcPath: 'src/js/log.js',
              srcDir: 'src/js/',
              distPath: 'dist/js/log.js',
              distDir: 'dist/js/',
              srcFilename: 'log.js',
              distFilename: 'log.js' }
        */
		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(uglify())
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('default', ['watchjs'])
```

[use-gulp-watch-path 完整代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/use-gulp-watch-path.js)

**`watchPath(event, search, replace, distExt)`**

| 參數 | 說明 |
|--------|--------|
|    event    |`gulp.watch` 回調函數的 `event`|
|    search   |需要被替換的起始字符串|
|    replace  |第三個參數是新的的字符串|
|   distExt   |擴展名(非必填)|


此時編輯 [src/js/log.js](https://github.com/nimojs/gulp-demo/blob/master/src/js/log.js) 文件並保存，命令行會出現消息，表示檢測到 `src/js/log.js` 文件修改後只重新編譯 `log.js`。

```ruby
[21:47:25] changed src/js/log.js
[21:47:25] Dist dist/js/log.js
```

你可以訪問 [gulp-watch-path](https://github.com/nimojs/gulp-watch-path) 瞭解更多。

### stream-combiner2

編輯 `log.js` 文件時，如果文件中有 js 語法錯誤時，gulp 會終止運行並報錯。

當 log.js 缺少 `)`
```js
log('gulp-book'
```

並保存文件時出現如下錯誤，但是錯誤信息不全面。而且還會讓 gulp 停止運行。

```
events.js:85
      throw er; // Unhandled 'error' event
            ^
Error
    at new JS_Parse_Error (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:189:18)
...
...
js_error (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-
-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:1165:20)
    at maybe_unary (/Users/nimojs/Documents/code/gulp-book/demo/chapter7/node_modules/gulp-uglify/node_modules/uglify-js/lib/parse.js:1328:19)

```

應對這種情況，我們可以使用 [Combining streams to handle errors](https://github.com/gulpjs/gulp/blob/master/docs/recipes/combining-streams-to-handle-errors.md) 文檔中推薦的 [stream-combiner2](https://github.com/substack/stream-combiner2)  捕獲錯誤信息。

```js
var handleError = function (err) {
    var colors = gutil.colors;
    console.log('\n')
    gutil.log(colors.red('Error!'))
    gutil.log('fileName: ' + colors.red(err.fileName))
    gutil.log('lineNumber: ' + colors.red(err.lineNumber))
    gutil.log('message: ' + err.message)
    gutil.log('plugin: ' + colors.yellow(err.plugin))
}
var combiner = require('stream-combiner2')

gulp.task('watchjs', function () {
    gulp.watch('src/js/**/*.js', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')
        /*
        paths
            { srcPath: 'src/js/log.js',
              srcDir: 'src/js/',
              distPath: 'dist/js/log.js',
              distDir: 'dist/js/',
              srcFilename: 'log.js',
              distFilename: 'log.js' }
        */
        gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        var combined = combiner.obj([
            gulp.src(paths.srcPath),
            uglify(),
            gulp.dest(paths.distDir)
        ])

        combined.on('error', handleError)
    })
})
```

[watchjs-1 完整代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/watchjs-1.js)

此時當編譯錯誤的語法時，命令行會出現錯誤提示。而且不會讓 gulp 停止運行。

```
changed:src/js/log.js
dist:dist/js/log.js
--------------
Error!
fileName: /Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js
lineNumber: 7
message: /Users/nimojs/Documents/code/gulp-book/demo/chapter7/src/js/log.js: Unexpected token eof «undefined», expected punc «,»
plugin: gulp-uglify
```

### gulp-sourcemaps

JS 壓縮前和壓縮後比較
```js
// 壓縮前
var log = function (msg) {
    console.log('--------');
    console.log(msg)
    console.log('--------');
}
log({a:1})
log('gulp-book')

// 壓縮後
var log=function(o){console.log("--------"),console.log(o),console.log("--------")};log({a:1}),log("gulp-book");
```

壓縮後的代碼不存在換行符和空白符，導致出錯後很難調試，好在我們可以使用 [sourcemap](http://www.ruanyifeng.com/blog/2013/01/javascript_source_map.html) 幫助調試

```js
var sourcemaps = require('gulp-sourcemaps')
// ...
var combined = combiner.obj([
    gulp.src(paths.srcPath),
    sourcemaps.init(),
    uglify(),
    sourcemaps.write('./'),
    gulp.dest(paths.distDir)
])
// ...
```

[watchjs-2 完整代碼](https://github.com/nimojs/gulp-book/tree/master/demo/chapter7/watchjs-1.js)

此時 `dist/js/` 中也會生成對應的 `.map` 文件，以便使用 Chrome 控制台調試代碼 [在線文件示例：src/js/](https://github.com/nimojs/gulp-demo/blob/master/src/js/)

-----

至此，我們完成了檢測文件修改後壓縮 JS 的 gulp 任務配置。

有時我們也需要一次編譯所有 js 文件。可以配置 `uglifyjs` 任務。

```js
gulp.task('uglifyjs', function () {
    var combined = combiner.obj([
        gulp.src('src/js/**/*.js'),
        sourcemaps.init(),
        uglify(),
        sourcemaps.write('./'),
        gulp.dest('dist/js/')
    ])
    combined.on('error', handleError)
})
```

在命令行輸入 `gulp uglifyjs` 以壓縮 `src/js/` 下的所有 js 文件。

配置 CSS 任務
-------

有時我們不想使用 LESS 或 SASS而是直接編寫 CSS，但我們需要壓縮 CSS 以提高頁面載入速度。

### gulp-minify-css

按照本章中壓縮 JS 的方式，先編寫 `watchcss` 任務

```js
var minifycss = require('gulp-minify-css')

gulp.task('watchcss', function () {
    gulp.watch('src/css/**/*.css', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(sourcemaps.init())
            .pipe(minifycss())
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('default', ['watchjs','watchcss'])
```

### gulp-autoprefixer

autoprefixer 解析 CSS 文件並且添加瀏覽器前綴到CSS規則裡。
透過示例幫助理解 

autoprefixer 處理前：
```css
.demo {
    display:flex;
}
```

autoprefixer 處理後：
```css
.demo {
    display:-webkit-flex;
    display:-ms-flexbox;
    display:flex;
}
```
你只需要關心編寫標準語法的 css，autoprefixer 會自動補全。

在 watchcss 任務中加入 autoprefixer:

```js
gulp.task('watchcss', function () {
    gulp.watch('src/css/**/*.css', function (event) {
        var paths = watchPath(event, 'src/', 'dist/')

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(sourcemaps.init())
            .pipe(autoprefixer({
              browsers: 'last 2 versions'
            }))
            .pipe(minifycss())
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})
```

更多 autoprefixer 參數請查看 [gulp-autoprefixer](https://github.com/sindresorhus/gulp-autoprefixer)

有時我們也需要一次編譯所有 css 文件。可以配置 `minifyss` 任務。

```js
gulp.task('minifycss', function () {
    gulp.src('src/css/**/*.css')
        .pipe(sourcemaps.init())
        .pipe(autoprefixer({
          browsers: 'last 2 versions'
        }))
        .pipe(minifycss())
        .pipe(sourcemaps.write('./'))
        .pipe(gulp.dest('dist/css/'))
})
```

在命令行輸入 `gulp minifyss` 以壓縮 `src/css/` 下的所有 .css 文件並複製到 `dist/css` 目錄下

配置 Less 任務
------------
參考配置 JavaScript 任務的方式配置 less 任務

```js
var less = require('gulp-less')

gulp.task('watchless', function () {
    gulp.watch('src/less/**/*.less', function (event) {
        var paths = watchPath(event, 'src/less/', 'dist/css/')

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)
        var combined = combiner.obj([
            gulp.src(paths.srcPath),
            sourcemaps.init(),
            autoprefixer({
              browsers: 'last 2 versions'
            }),
            less(),
            minifycss(),
            sourcemaps.write('./'),
            gulp.dest(paths.distDir)
        ])
        combined.on('error', handleError)
    })
})

gulp.task('lesscss', function () {
    var combined = combiner.obj([
            gulp.src('src/less/**/*.less'),
            sourcemaps.init(),
            autoprefixer({
              browsers: 'last 2 versions'
            }),
            less(),
            minifycss(),
            sourcemaps.write('./'),
            gulp.dest('dist/css/')
        ])
    combined.on('error', handleError)
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless'])
```

配置 Sass 任務
-------------

參考配置 JavaScript 任務的方式配置 Sass 任務

```js
gulp.task('watchsass',function () {
    gulp.watch('src/sass/**/*', function (event) {
        var paths = watchPath(event, 'src/sass/', 'dist/css/')

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)
        sass(paths.srcPath)
            .on('error', function (err) {
                console.error('Error!', err.message);
            })
            .pipe(sourcemaps.init())
            .pipe(minifycss())
            .pipe(autoprefixer({
              browsers: 'last 2 versions'
            }))
            .pipe(sourcemaps.write('./'))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('sasscss', function () {
        sass('src/sass/')
        .on('error', function (err) {
            console.error('Error!', err.message);
        })
        .pipe(sourcemaps.init())
        .pipe(minifycss())
        .pipe(autoprefixer({
          browsers: 'last 2 versions'
        }))
        .pipe(sourcemaps.write('./'))
        .pipe(gulp.dest('dist/css'))
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless', 'watchsass', 'watchsass'])
```

配置 image 任務
----------

```js
var imagemin = require('gulp-imagemin')

gulp.task('watchimage', function () {
    gulp.watch('src/images/**/*', function (event) {
        var paths = watchPath(event,'src/','dist/')

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(imagemin({
                progressive: true
            }))
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('image', function () {
    gulp.src('src/images/**/*')
        .pipe(imagemin({
            progressive: true
        }))
        .pipe(gulp.dest('dist/images'))
})
```

配置文件複製任務
-----------
複製 `src/fonts/` 文件到 `dist/` 中

```js
gulp.task('watchcopy', function () {
    gulp.watch('src/fonts/**/*', function (event) {
        var paths = watchPath(event)

		gutil.log(gutil.colors.green(event.type) + ' ' + paths.srcPath)
        gutil.log('Dist ' + paths.distPath)

        gulp.src(paths.srcPath)
            .pipe(gulp.dest(paths.distDir))
    })
})

gulp.task('copy', function () {
    gulp.src('src/fonts/**/*')
        .pipe(gulp.dest('dist/fonts/'))
})

gulp.task('default', ['watchjs', 'watchcss', 'watchless', 'watchsass', 'watchimage', 'watchcopy'])
```

結語
--------

[完整代碼](https://github.com/nimojs/gulp-demo/tree/master/gulpfile.js)

[訪問論壇獲取幫助](https://github.com/nimojs/gulp-book/issues/16)

你還想瞭解什麼關於 gulp 的什麼知識？ [告訴我們](https://github.com/nimojs/gulp-book/issues/8)

後續還會又新章節更新。你可以[訂閱本書](https://github.com/nimojs/gulp-book/issues/7) 當有新章節發佈時，我們會透過郵件告訴你
