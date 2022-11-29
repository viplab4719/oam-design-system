# OAM Design System

以下僅說明如何在新項目中include `oam-design-system`。有關使用信息，請查看[documentation website](http://hotosm.github.io/oam-docs/)。
有關如何開發 `oam-design-system` 的信息，請查看 [DEVELOPMENT.md](DEVELOPMENT.md) 
---

樣式指南和UI組件庫，旨在標準化所有 OAM 相關應用程序的外觀，同時定義編碼最佳慣例和常規。
安裝 `npm`模組：（模組尚不可用。使用直接鏈接）
```
npm install https://github.com/hotosm/oam-design-system#v1.0.0
```
對於最新版本，請省略標籤。

**注意：**
此設計系統對每個元素進行了一些假設，如下所述。

檢查 [OAM docs](https://github.com/hotosm/oam-docs/blob/master/gulpfile.js) 的構建系統，這是一個使用 `oam-design-system`的項目。

## Overview

共享資產都在 `assets` 目錄中。整理如下：
### assets/scripts
Utility libraries and shared components.
實用程序庫和共享組件。

**USAGE**  
Use as any node module:
用作任何*網站*模組：
```js
import { Dropdown, user } from 'oam-design-system';
```  
如果您想最小化打包後的大小，還可以直接包含組件。
Bindings exported from `oam-design-system` are also available in `oam-design-system/assets/scripts`
從 `oam-design-system` 輸出的綑綁物也可在  `oam-design-system/assets/scripts` 中使用
### assets/styles
需要 [Bourbon](https://github.com/lacroixdesign/node-bourbon) and [Jeet](https://github.com/mojotech/jeet).

**INSTALLATION**  
將模組路徑添加到 gulp-sass 的  `includePaths` 中。應該看起來像：
```js
.pipe($.sass({
  outputStyle: 'expanded',
  precision: 10,
  includePaths: require('node-bourbon').with('node_modules/jeet/scss', require('oam-design-system/gulp-addons').scssPath)
}))
```
  
`oam-design-system` 使用  [Google Fonts](https://goo.gl/FZ0Ave) 上提供的 **Open Sans**。
它需要包含在應用程序中：
```
<link rel="stylesheet" type="text/css" href="//fonts.googleapis.com/css?family=Open+Sans:300italic,400italic,700italic,400,300,700" />
```

**USAGE**  
現在您可以將它放入主要的 scss 文件中：
```scss
// Bourbon is a dependency
@import "bourbon";

@import "jeet/index";

@import "oam-design-system";
```

### assets/icons 
`oam-design-system` 包括 svg 圖標，這些圖標被編譯成網頁字體並包含在樣式中。
要使用它們，請檢查 `_oam-ds-icons.scss` 中的class names。

### assets/graphics
在項目之間共享的圖形。

**INSTALLATION**  
將 `graphicsMiddleware` 添加到 browserSync。這只是為了幫助開發。
應該看起來像：
```js
browserSync({
  port: 3000,
  server: {
    baseDir: ['.tmp', 'app'],
    routes: {
      '/node_modules': './node_modules'
    },
    middleware: require('oam-design-system/gulp-addons').graphicsMiddleware(fs) // <<< This line
  }
});
```
*基本上，每次對`/assets/graphics/**` 之類的路徑發出請求時，browserSync 都會首先檢查`oam-design-system`文件夾。如果它沒有找到任何東西，它將在正常項目的asset folder中查找。*

您還需要確保在構建時復製圖像。

這可確保在構建項目時復製圖形。
```js
gulp.task('images', function () {
  return gulp.src(['app/assets/graphics/**/*', require('oam-design-system/gulp-addons').graphicsPath + '/**/*'])
    .pipe($.cache($.imagemin({
```

**USAGE**  
只需使用路徑 `assets/graphics/[graphic-type]/[graphic-name]` 放入圖像。
所有可用的圖像都可以在[here](assets/graphics/)找到。

## License
Oam Design System 在 **BSD 3-Clause License** 下獲得許可，有關更多詳細信息，請參閱[LICENSE](LICENSE) 文件。
