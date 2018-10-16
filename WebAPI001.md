### Web API 介绍

API（Application Programming Interface，应用程序编程接口）是一些预先定义的函数，目的是提供应用程序与开发人员基于某软件或硬件得以访问一组例程的能力，而又无需访问源码，或理解内部工作机制的细节。

- 任何开发语言都有自己的 API
- API 的特征输入和输出（I/O）
- API 的使用方法（例如console.log()）

Web API 是 浏览器提供的一套操作浏览器功能和页面元素的接口（BOM 和 DOM）

[MDN API文档](https://developer.mozilla.org/zh-CN/docs/Web/API)

### JavaScript 的组成

JavaScript 由 ECMAScript 、DOM 和 BOM 组成

- ECMAScript 语法规范
- BOM （Browser Object Model）浏览器对象模型——操作浏览器功能的 API
- DOM （Document Object Model）文档对象模型——操作页面功能的 API

### DOM 相关概念

DOM 是 W3C 组织推荐的处理可扩展标记语言的标准编程接口。DOM 是一个基于树的 API 文档

- 文档：一个网页可以称为文档
- 节点：网页中的所有内容都是节点（标签、属性、文本、注释等）
- 元素：网页中的标签
- 属性：标签的属性

DOM 经常进行的操作

- 获取元素
- 对元素进行操作（设置其属性或调用其方法）
- 动态创建元素
- 事件（什么时机做什么事情）

### 获取页面元素

根据 id 获取元素 `doucment.getElementById()` [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementById)

```js
var main = document.getElementById('id')
console.dir(mian);  // 打印该对象
```

根据 tag 获取元素 `document.getElementsByTagName()` [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByTagName)

```js
// 该方法获取到的属性是动态的，根据页面元素的变化而发生变化
document.getElementsByTagName('p')
```

其他获取页面元素的方法

`document.getElementsByName()` [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByName)

`doucment.getElementsByClassName()` [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/getElementsByClassName)

**如果不考虑兼容问题或者做移动端开发需重点掌握！！！**

- `doucument.querySelector()`返回文档中与指定选择器或选择器匹配的第一个 html 元素，如果找不到匹配项，则返回null。 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelector)
- `document.querySelectorAll()`返回指定的选择器匹配的文档中的元素列表，返回的对象是一个节点的集合，即 NodeList。 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/querySelectorAll)

```js
// 根据标签的 name 属性获取元素,浏览器兼容不好，不推荐使用
doucument.getElementsByName()
// 根据标签的 class 属性来获取元素，兼容 IE9 以后，不推荐使用
document.getElementsByClassName()
// 返回指定选择器的第一个元素,兼容 IE8 以上
document.querySelector()
document.querySelectorAll()
```

### 事件

事件三要素

- 事件源：触发(被)事件的元素
- 事件名称：click(例)
- 事件处理程序：事件触发后要执行的代码(要执行的函数)

`HTMLElement.click`鼠标点击事件 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/HTMLElement/click)

```html
<button id="btn">点击</button>
<script>
    // 1.获取按钮
	var btn = document.getElementById('btn');
    // 2.给按钮注册事件
    // 事件名称： click
    // 事件源：谁触发的时间
    // 事件处理函数
    btn.onclick = function() {
        this.value = '别点了';
    }
</script>
```

### 案例：点击切换图片

```html
<body>
    <button id="btn">点击</button>
    <img src="./img/a.jpb" id="img">
</body>
<script>
    // 1.获取元素
	var btn = document.getElementById('btn');
    var img = document.getElementsByTagName('img');
    var flag = true;
    // 2.给按钮注册事件
    btn.onclick = function() {
        if (flag) {
            img.src = './img/b.jpg';
            flag = !flag;
        } else {
            img.src = './img/a.jpg';
            flag = !flag;
        }
    }
</script>
```

### 非表单元素的属性

非表单元素有 `href` `title` `id` `src` `className` 等

```html
<a hrft="http://baidu.com" id="link" title="百度一下" ask="ask">百度</a>
<img src='./img/a.jpg' alt="star" id="img">
<script>
	var link = document.getElementById('link');
    var img = document.getElementById('img');
    console.log(link.href);  // http://baidu.com
    console.log(link.title);  // 百度一下
    // 此方法不能直接获取自定义属性，只能获取原生的属性
    console.log(link.ask);  // undefined
</script>
```

### 案例：点击按钮隐藏示

```html
<style>
    #box {
        width: 200px;
        height: 200px;
        background-color: #f00;
    }
    
    .show {
        display: block;
    }
    
    .hidden {
        display: none;
    }
</style>
<body>
    <input id="btn" value="隐藏" type="button">
    <div id="box"></div>
</body>
<script>
	var btn = document.getElementById('btn');
    var box = document.getElementById('box');
    var isShow = true;
    
    btn.onclick = function() {
        if (isShow) {
            box.className = 'hidden';
            this.value = '点击显示';
            isShow = !isShow;
        } else {
            box.className = 'show';
            this.value = '点击隐藏';
            isShow = !isShow;
        }
    }
</script>
```

### innerHTML 和 innerText

`Node.innerHTML`设置或获取 HTML 语法表示的元素的后代

[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/innerHTML)

- 获取内容的时候，如果内容中有标签，会把标签也获取到
- 设置内容的时候，如果内容中带有标签，会按 HTML 进行解析

`Node.innerText`是一个表示一个节点及其后代的"渲染"文本内容的属性

[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/innerText)

- 获取内容的时候会自动过滤掉标签，并且会把前后的换行和空白都删掉
- 设置内容的时候，如果内容中带有标签，会把标签以文本的形式显示出来，如果想正常解析标签，需要使用转义符来输入标签

HTML 常用转义符

```
"		&quot;
'		&apos;
&		&amp;
<		&lt;   // less than  小于
>		&gt;   // greater than  大于
空格	   &nbsp;
©		&copy;
```

### 兼容性处理

```html
<div id="box">
    hello
</div>
<script>
    var box = document.getElementById("box");
    // 如果属性不存在，返回 undefined
    console.log(box.a);
    // 如果属性存在，返回 string
    console.log(box.id);
    function getInnerText(element) {
        // 判断当前浏览器是否支持元素的 innerText 属性，如果支持，就使用 element.innerText 获取内容
        // 如果不支持 innerText 属性，就使用 element.textContent 获取内容
        if (typeof element.innderText === 'string') {
            return element.innerText;
        } else {
            return element.textContent;
        }
    }
</script>
```







