# 驴妈妈H5开发规范

**本规范的主要目标**：

* 代码风格一致

  通过保持代码风格的一致，使代码具有良好的可读性，降低代码的维护成本，提高团队间的协作效率。

* 最佳实践

  通过遵守最佳实践，可以降低 bug 引入的可能，提高页面性能，编写出可扩展可维护的代码。

**本规范的编写原则**：努力兼顾代码风格与开发效率的平衡，既要努力做到 "使每一行代码都像是同一个人编写的"，又要尽量避免 "大家都觉得这样很麻烦"。

**本规范的更新**：本规范是当前阶段的一个总结，不是最终版，也不会有最终版，随着代码风格的优化、最佳实践的发展，本规范也会变化。

**本规范的代码说明**：不符合规范的代码写法，会有注释 `不好`，符合规范的代码写法，会有注释 `好`，最佳写法，会有注释 `最佳`；在空格相关的规范中，使用 `·` 号来强调表示空格。

### **规范最重要的是执行。**

## 目录

* [JavaScript](#javascript)
    * [命名](#命名)
    * [注释](#注释)
* [HTML](#html)
    * [命名](#html-name)
    * [空格](#html-space)
    * [标签](#标签)
    * [属性](#html-property)
    * [注释](#html-comments)
* [CSS](#css)
    * [分离](#分离)
    * [空格](#css-space)
    * [引号](#css-quotes)
    * [小数](#小数)
    * [零值](#零值)
    * [注释](#css-comments)
* [页面相关](#页面相关)
    * [整体结构](#整体结构) 
    * [loading](#loading)
    * [图片](#图片)
    * [分享](#分享)

## JavaScript

### 命名

* 避免使用单一字母命名，让你的名字具有实际含义。

    ```js
    // 不好
    function q() {
        // ...
    }

    // 好
    function query() {
        // ...
    }
    ```
* 使用 [小驼峰命名法](https://zh.wikipedia.org/wiki/%E9%A7%9D%E5%B3%B0%E5%BC%8F%E5%A4%A7%E5%B0%8F%E5%AF%AB) 来命名基本类型、对象和函数。

    > 即使是常量也不要使用全部大写，大写的名称难以阅读。在 ES5 中由于没有 `const` 关键字，将一个实际不会改变的变量命名为全部大写，有助于在视觉上与普通变量加以区分，并方便语法检测工具进行检查。而在 ES6 中，这是没有必要的。

    ```js
    // 不好
    const TAXFORPRODUCT = 0.9;
    let this_is_my_object = {};
    function c() {}

    // 好
    const taxForProduct = 0.9;
    let thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

* 使用 [Pascal命名法](https://zh.wikipedia.org/wiki/%E5%B8%95%E6%96%AF%E5%8D%A1%E5%91%BD%E5%90%8D%E6%B3%95) 来命名构造函数和类。

    ```js
    // 不好
    function user(options) {
        this.name = options.name;
    }

    const bad = new user({
        name: 'Jack',
    });

    // 好
    class User {
        constructor(options) {
            this.name = options.name;
        }
    }

    const good = new User({
        name: 'Jack',
    });
    ```

* 缩略词应该保持全部大写或者全部小写。

    ```js
    // 不好
    import SmsContainer from './containers/SmsContainer';

    // 不好
    const HttpRequests = [
        // ...
    ];

    // 好
    import SMSContainer from './containers/SMSContainer';

    // 好
    const HTTPRequests = [
        // ...
    ];
    ```

### 注释

* 对于类、方法、复杂的代码逻辑、从名称上难以理解的变量、正则表达式等，尽可能加上注释，以便于后期维护。

    ```js
    // 是否是由 tab 切换引起的页面滚动
    let isScrollingByTab = false;

    /**
     * 获取对象的指定属性
     * 属性可以是一个用点号连接的多层级路径
     * @param {object} object 对象
     * @param {string} path 属性值，可以是路径，如：'a.b.c[0].d'
     * @param {any} [defaultVal=''] 取不到属性时的默认值
     * @returns {any} 获取到的属性值
     */
    function getPathValue(object, path, defaultVal = '') {
        // ...
    }
    ```

* 单行注释使用 `//`，总是在需要注释的内容的上方加入注释，并在注释之前保留一个空行，除非该注释是在当前代码块的第一行。

    ```js
    // 不好
    const active = true;  // 当前 tab 状态

    // 好
    // 当前 tab 状态
    const active = true;

    // 不好
    function getType() {
        console.log('获取type...');
        // 设置默认 type 为 'no type'
        const type = this.type || 'no type';

        return type;
    }

    // 好
    function getType() {
        console.log('获取type...');

        // 设置默认 type 为 'no type'
        const type = this.type || 'no type';

        return type;
    }

    // 好
    function getType() {
        // 设置默认 type 为 'no type'
        const type = this.type || 'no type';

        return type;
    }
    ```

* 多行注释使用 `/** ... */`，并尽可能遵循 [jsdoc](http://www.css88.com/doc/jsdoc/) 注释规范。

    > 遵循 jsdoc 规范的注释，能够很方便的自动生成 api 文档；同时，也能提升编码体验，以 Visual Studio Code 为例，当你调用函数时，会浮动提示该函数的描述、参数类型、返回值等信息。

    ```js
    // 不好
    // 获取对象的指定属性
    // 属性可以是一个用点号连接的多层级路径
    function getPathValue(object, path, defaultVal = '') {
        // ...
    }

    // 好
    /**
    * 获取对象的指定属性
    * 属性可以是一个用点号连接的多层级路径
    */
    function getPathValue(object, path, defaultVal = '') {
        // ...
    }

    // 最佳
    /**
     * 获取对象的指定属性
     * 属性可以是一个用点号连接的多层级路径
     * @param {object} object 对象
     * @param {string} path 属性值，可以是路径，如：'a.b.c[0].d'
     * @param {any} [defaultVal=''] 取不到属性时的默认值
     * @returns {any} 获取到的属性值
     */
    function getPathValue(object, path, defaultVal = '') {
        // ...
    }
    ```

* 在注释内容之前加上 `FIXME: ` 可以提醒自己或其他开发人员这是一个需要修改的问题。

    ```js
    class Calculator extends Abacus {
        constructor() {
            super();

            // FIXME: 不应该使用一个全局变量
            total = 0;
        }
    }
    ```

* 在注释内容之前加上 `TODO: ` 可以提醒自己或其他开发人员这里需要一些额外工作。

    > 在 Visual Studio Code 中，可以安装 [TODO Highlight](https://marketplace.visualstudio.com/items?itemName=wayou.vscode-todo-highlight) 扩展，支持高亮显示 `FIXME:`、`TODO:` 等特殊注释，并自动生成列表展示。

    ```js
    class Calculator extends Abacus {
        constructor() {
            super();

            // TODO: total 要写成可以被传入的参数修改
            this.total = 0;
        }
    }
    ```



## HTML

<a id="html-name" name="html-name"></a>
### 命名

* 标签名全部小写。

    ```html
    <!--不好-->
    <DIV>这是一个块级元素</DIV>

    <!--好-->
    <div>这是一个块级元素</div>
    ```

* 属性名全部小写，使用 `-` 作为分隔符。

    ```html
    <!--不好-->
    <div Class="contain" itemCode="1"></div>

    <!--好-->
    <div class="contain" item-code="1"></div>
    ```

* 样式类全部小写，使用 `-` 作为分隔符。

    ```html
    <!--不好-->
    <button class="btnPrimay btn_outline">选择城市</button>

    <!--好-->
    <button class="btn-primay btn-outline">选择城市</button>
    ```

<a id="html-space" name="html-space"></a>
### 空格

* 使用 4 个空格作为缩进。

    > 主流的编辑器一般都支持设定 Tab 对应的空格数，以 Visual Studio Code 为例，设置方式：点击文件 -> 首选项 -> 设置，搜索 "editor.tabSize"。

    ```html
    <!--不好-->
    <div>
    ··<span>这是一行文本</span>
    </div>

    <!--好-->
    <div>
    ····<span>这是一行文本</span>
    </div>
    ```

* 不要混用 Tab 和空格。

    > 这可能会导致一些格式上的异常，例如：在 Jade 中混用 Tab 和空格就会出错。

* 标签内的文本避免使用空格，应该使用样式来实现。

    ```html
    <!--不好-->
    <p>
        &nbsp;&nbsp;这是一些文本
    </p>

    <!--好-->
    <p style="text-indent: 2em;">
        这是一些文本
    </p>

    <!--不好-->
    <div>￥&nbsp;1000</div>

    <!--好-->
    <div>￥<span style="margin-left: 5px">1000</span></div>
    ```

<a id="html-quotes" name="html-quotes"></a>
### 引号

* 属性值的最外层使用双引号。

    ```html
    <!--不好-->
    <div class=container></div>

    <!--不好-->
    <div class='container'></div>

    <!--好-->
    <div class="container"></div>
    ```

### 标签

* 不要在自闭合标签的结尾使用 `/` 。

    ```html
    <!--不好-->
    <img src="logo.png" alt="company"/>
    <br/>

    <!--好-->
    <img src="logo.png" alt="company">
    <br>
    ```

* 不要省略可选的关闭标签。

    > [HTML5规范](https://www.w3.org/TR/REC-html40/index/elements.html) 指出一些标签可以省略关闭标签，但我们认为这样做降低了可读性。

    ```html
    <!--不好-->
    <body>
        <ul>
            <li>111
            <li>222
            <li>333
        </ul>

    <!--好-->
    <body>
        <ul>
            <li>111</li>
            <li>222</li>
            <li>333</li>
        </ul>
    </body>
    ```

<a id="html-property" name="html-property"></a>
### 属性

* 引入 css 和 js 时不需要指明 `type` 属性。

    > `text/css` 和 `text/javascript` 分别是它们的默认值。

    ```html
    <!--不好-->
    <link type="text/css" rel="stylesheet" href="global.css">
    <script type="text/javascript" src="public.js"></script>

    <!--好-->
    <link rel="stylesheet" href="global.css">
    <script src="public.js"></script>
    ```

* 布尔属性不需要指明属性的值。

    > 布尔属性存在代表取值为 true，属性不存在代表取值为 false。

    ```html
    <!--不好-->
    <input type="text" disabled="disabled">
    <input type="checkbox" value="1" checked="checked">

    <!--好-->
    <input type="text" disabled>
    <input type="checkbox" value="1" checked>
    ```

* 尽量将 `class`、`id` 等使用频率高的属性放在前面。

    ```html
    <!--不好-->
    <input type="text" placeholder="请输入关键字" id="search-input" disabled class="form-control">

    <!--好-->
    <input class="form-control" id="search-input" type="text" placeholder="请输入关键字" disabled>
    ```

* 使用 `data-*` 创建标准化的自定义属性。

    > 这是创建自定义属性的标准方式；加上 `data-` 前缀能够明确表示这是一个自定义属性，而非原生属性；另外，在 `JavaScript` 中还可以通过 `document.querySelector(selector).dataset` 方便的访问它们。

    ```html
    <!--不好-->
    <div class="container" code="freetour"></div>

    <!--好-->
    <div class="container" data-code="freetour"></div>
    ```

<a id="html-comments" name="html-comments"></a>
### 注释

* 对于重要的元素，尽可能加上注释，以便于后期维护。

    ```html
    <!--筛选面板上方的加载中动画，主要用于：1. 点击筛选项，2. 点击清空筛选 时显示-->
    <div id="loading2" style="display:none;">
        <div></div>
    </div>

    <!--筛选面板上方的提示语，主要用于无筛选数据时显示-->
    <div id="filterTip" style="display:none;">
        <span>该筛选项无数据，已恢复至默认选项！</span>
    </div>
    ```

## CSS

<a id="css-space" name="css-space"></a>
### 分离

* 对于较简单的规则，尽量分离成通用样式，面向属性，而非面向业务。

    > 面向业务的css设计，将导致大量的样式重复定义，难以维护；而面向属性的设计能大大提高重用性，也有利于统一维护修改。

    ```css
    /* 不好 */
    /* 用户登录按钮 */
    .btn-user-login {
        background-color: #d30779;
        text-align: center;
        padding-bottom: 10px;
    }

    /* 好 */
    .background-highlight {
        background-color: #d30779;
    }

    .text-center {
        text-align: center;
    }

    .p-b-10 {
        padding-bottom: 10px;
    }
    ```

### 空格

* 使用 4 个空格作为缩进。

    > 主流的编辑器一般都支持设定 Tab 对应的空格数，以 Visual Studio Code 为例，设置方式：点击文件 -> 首选项 -> 设置，搜索 "editor.tabSize"。

    ```css
    /* 不好 */
    p {
    ··color: #555;
    }

    /* 好 */
    p {
    ····color: #555;
    }
    ```

* 在 `{` 前加 1 个空格。

    ```css
    /* 不好 */
    p{
        color: #555;
    }

    /* 不好 */
    p
    {
        color: #555;
    }

    /* 好 */
    p·{
        color: #555;
    }
    ```

* 将 `}` 放置于新行。

    ```css
    /* 不好 */
    p {
        color: #555;}

    /* 好 */
    p {
        color: #555;
    }
    ```

* 在属性值前加 1 个空格。

    ```css
    /* 不好 */
    p {
        color:#555;
    }

    /* 好 */
    p {
        color:·#555;
    }
    ```

* 在属性值中的 `,` 后面加 1 个空格。

    ```css
    /* 不好 */
    .element {
        background-color: rgba(0,0,0,.5);
    }

    /* 好 */
    .element {
        background-color: rgba(0,·0,·0,·.5);
    }
    ```

* 在注释的 `/*` 后和 `*/` 前各加 1 个空格。

    ```css
    /* 不好 */
    /*这是一行注释*/
    p {
        color: #555;
    }

    /* 好 */
    /*·这是一行注释·*/
    p {
        color: #555;
    }
    ```

* 属性值中的 `(` 前不要加空格。

    ```css
    /* 不好 */
    .element {
        background-color: rgba·(0, 0, 0, .5);
    }

    /* 好 */
    .element {
        background-color: rgba(0, 0, 0, .5);
    }
    ```

* 选择器 `>`、`+`、`~`、`:` 等前后都不要加空格。

    ```css
    /* 不好 */
    div·>·p {
        color: #555;
    }

    div·:·after {
        content: "";
    }

    /* 好 */
    div>p {
        color: #555;
    }

    div:after {
        content: "";
    }
    ```

* 各规则之间保留一个空行。

    ```css
    /* 不好 */
    p {
        color: #555;
    }
    .element {
        background-color: rgba(0, 0, 0, .5);
    }

    /* 好 */
    p {
        color: #555;
    }

    .element {
        background-color: rgba(0, 0, 0, .5);
    }

    ```

* 在规则声明中有多个选择器时，每个选择器独占一行。

    ```css
    /* 不好 */
    div, p, span {
        color: #555;
    }

    /* 好 */
    div,
    p,
    span {
        color: #555;
    }
    ```

<a id="css-quotes" name="css-quotes"></a>
### 引号

* 字符串类型的值使用双引号。

    ```css
    /* 不好 */
    .element:after {
        content: '';
        background-image: url(logo.png);
    }

    /* 好 */
    .element:after {
        content: "";
        background-image: url("logo.png");
    }
    ```

### 小数

* 不要设置小数作为像素值。

    > 像素值为小数时会被解析成整数，且各浏览器解析方式存在差异。

    ```css
    /* 不好 */
    div {
        width: 9.5px;
    }

    /* 好 */
    div {
        width: 10px;
    }
    ```

* 去掉小数点前面的 0。

    ```css
    /* 不好 */
    .element {
        color: rgba(0, 0, 0, 0.5);
    }

    /* 好 */
    .element {
        color: rgba(0, 0, 0, .5);
    }
    ```

### 零值

* 属性值为 `0` 时，后面不要加单位。

    ```css
    /* 不好 */
    .element {
        width: 0px;
    }

    /* 好 */
    .element {
        width: 0;
    }
    ```

* 定义无边框的样式时，将属性值设为 `0` 而不是 `none`。

    ```css
    /* 不好 */
    div {
        border: none;
    }

    /* 好 */
    div {
        border: 0;
    }
    ```

<a id="css-comments" name="css-comments"></a>
### 注释

* 对于较生僻的样式、解决特定问题的样式、需要特别注明的样式等，尽可能加上注释，以便于后期维护。

    ```css
    button {
        /* 移除iOS下的原生样式 */
        -webkit-appearance: none;
    }

    .modal {
        /* 蒙版的层级必须大于700，以放在列表之上；同时必须小于900，以放在导航之下 */
        z-index: 701;
    }
    ```

* 总是使用 `/* ... */` 创建标准注释，即便是在 sass 中也不要使用 `//` 。


## 页面相关

### 整体结构

* 页面 html 模板如下：
```html
<!DOCTYPE HTML>
<html>
    <head>
        <meta charset="UTF-8">
        <title>超级自由行</title>
        <meta http-equiv="X-UA-Compatible" content="IE=edge,chrome=1"/>
        <meta name="renderer" content="webkit"/>
        <meta name="Keywords" content="自由行,景点门票,跟团游,自驾游,机票,酒店"/>
        <meta name="Description" content="驴妈妈旅游网-中国新型的B2C旅游电子商务网站，为旅游者提供景区门票、自由行、度假酒店、机票、国内游、出境游等一站式旅游服务，《自在游天下,就找驴妈妈!》"/>
        <link rel="stylesheet" type="text/css" href="//s1.lvjs.com.cn/styles/v6/header_new.css"/>
        <link rel="stylesheet" type="text/css" href="插件或业务css地址"/>
    </head>
    <body>
        <!--正文 code here-->
        <script type="text/javascript" src="//s1.lvjs.com.cn/js/new_v/jquery-1.7.2.min.js"></script>
        <script type="text/javascript" src="插件或业务js地址"></script>
    </body>
</html>
```
* CSS：css 在 head 部分引用，业务 css 前需引用全局 css (v6/header_new.css)，不要合并请求； 
* JS ：js 在 body 闭合标签前引用，插件、业务 js 前需引用全局 js (jquery-1.7.2.min.js)，不要合并请求； 

### loading
> 使用方法：调用接口时，传 ` loadingType: '' ` 参数，即可自定义使用不同加载效果（需引用 public.min.js）。
* 主接口请求中： 
页面主接口请求开始到返回前，显示小驴转转转加载动画；  
页面部分模块数据请求中，对应位置显示 "三个红点" 加载动画；  

* tab 切换接口请求中：  
当点击 tab 后，对应列表数据未返回时，应设置一定的高度，防止页面上滑，并显示 "三个红点" 加载动画，使用方法同上。

### 图片

* 图片样式自适应
    * img：`width` 百分比，`height` 自适应；
    * div：`background` + `padding-bottom` 。

* 有超过一屏的图片展示，需使用 lazyload 插件，实现懒加载；

* 产品图片背景
    * 背景统一为灰色小驴，居中显示；

* 防止轮播图平铺  

  > 在网速慢的情况下，轮播里面的图片会出现平铺显示的情况  

  需在外层盒子限制其高度（`padding-bottom` 或 "固定高度"）。

* 尽量使用icon-font
  阿里图标库地址: [驴妈妈官方icon](http://www.iconfont.cn/collections/detail?spm=a313x.7781069.1998910419.d9df05512.691448eacX8bHq&cid=599)


