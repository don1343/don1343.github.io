[TOC]

### 了解 attachEvent 

```html
<script>
    // 三种注册事件的方法
    // 第一种
    btn.onclick = function() {
        // 无法给同一个对象的同一个事件注册多个事件处理函数
    }
	// 第二种
    btn.addEventListener('click', function() {
        // 有浏览器兼容性问题,支持 IE9+
    })
    // 第三种
    btn.attachEvent('onclick', function() {
        // 事件名前面必须加 on
		// IE 老版本特有的方法 IE6 ~ IE10 支持
    })
</script>
```

### 注册事件方法兼容性处理

```html
<script>
    function addEventListener(element, eventName, fn) {
        // 判断当前浏览器是否支持 addEventLinstener
        if (element.addEventListener) {
            // 第三个参数默认是 false
            element.addEventListener(eventName, fn);
        } else if (element.attachEvevt) {
            element.attachEvent('on' + eventName, fn)
        } else {
            // element.onclick = fn；
            emement['on' + eventName] = fn;
        }
    }
    
    addEventListener(btn, 'click', function() {
        alert('Hello');
    })
</script>
```

### 移除事件

- `EventTarget.removeEventListener`删除使用 EventTartget.addEventListener() 方法添加的事件 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/removeEventListener)

```html
<input type="button" vaule="Click" id="btn">
<script>
    // 第一种
    btn.onclick = function() {
        alert('Hello');
        // 移除事件
        btn.onclick = null;
    }
    // 第二种 如果想要移除事件，注册事件的时候就不能用匿名函数
    function btnClick() {
        alert('Hello');
        // 移除事件
        btn.removeEventListener('click', btnClick)
    }
    btn.addEventListener('click', btnclick)
    
    // 第三种 IE 老版本浏览器特有方法，非标准特性，了解即可
    function btnClick() {
        alert('Hello');
        btn.detachEvent('onclick', btnClick)
    }
    btn.attachEvent('onclick', btnclick)
</script>
```

### 移除事件兼容性处理

```html
<input type="button" vaule="Click" id="btn">
<script>
    function removeEvent(el, eventName, fn) {
        if (element.removeEventListener) {
            element.removeEventListener(eventName, fn);
        } else if (element.detachEvent) {
            element.detachEvent('on' + eventName, fn)
        } else {
            emement['on' + eventName] = null;
        }
    }
</script>
```

### 事件冒泡

- 事件冒泡：是一种从下往上的传播方式。事件从最开始由最具体的元素（文档中嵌套最深的那个节点接受，也就是 DOM 最低层的子节点），然逐渐向上传播到最不具体的那个节点
- 事件捕获：与事件冒泡相反。事件最开始由不太具体的节点最早接受事件，而最具体的节点最后接受事件

```html
<div id="box1">
    <div id="box2">
        <div id="box3"></div>
    </div>
</div>

<script>
	// addEventListener 的第三个参数的作用
    var box1 = document.getElementById('box1');
    var box2 = document.getElementById('box2');
    var box3 = document.getElementById('box3');
    
    // 事件的三个阶段
    // 第一阶段：事件捕获阶段
    // 第二阶段：执行当前点击的元素
    // 第三阶段：事件冒泡阶段
    
    var arr = [box1, box2, box3];
    // 第三个参数为 false 的时候，事件冒泡(从内到外)
    for  (var i = 0; i < arr.length; i++) {
        arr[i].addEventLisrener('click', function() {
            console.log(this.id);
        }, false)
    }
    
    // 第三个参数为 true 的时候，事件捕获(从外到内)
    for  (var i = 0; i < arr.length; i++) {
        arr[i].addEventLisrener('click', function() {
            console.log(this.id);
        }, true)
    }
    
    // box.onclick 和 box.attachEvent 都无法实现事件捕获
    for  (var i = 0; i < arr.length; i++) {
        arr[i].onclick = function() {
            console.log(this.id);
        }
    }
    document.body.onclick = function() {
        console.log('body');
    }
    document.onclick = function() {
        console.log('document');
    }
</script>
```

### 事件委托

事件委托就是利用事件冒泡原理，把事件加到父元素或祖先元素上，触发执行效果。

```html
<ul id="ul">
    <li>纯阳</li>
    <li>天策</li>
    <li>七秀</li>
    <li>少林</li>
    <li>万花</li>
</ul>
<script>
    var ul = document.getEmelentById('ul');
    // e 事件参数或者事件对象，当事件发生的时候，可以获取一些和时间相关的数据
    ul.onclick = function(e) {
        console.log(this);  // 此时的 this 是整个 ul
        // 获取当前点击的 li
        // e.target 真正触发事件的对象
        console.log(e.target);
        // 让其他 li 的高亮显示取消
        ul.children.style.backgroundColor = '';
        // 让当前点击的 li 高亮显示
        e.target.style.backgroundColor = '#999';
    }
</script>
```

### 事件对象

- DOM 事件模型中的时间对象常用属性
  1. type 用于获取事件类型
  2. target 获取事件目标
  3. stopPropagation() 阻止事件冒泡
  4. preventDefault() 阻止事件默认行为
- IE 事件模型中的事件对象常用属性
  1. type 用于获取事件类型
  2. srcElement 获取事件目标
  3. cancelBubble 阻止事件冒泡
  4. returnValue 阻止事件默认行为


```html
<div id="box1">
    <div id="box2">
        <div id="box3"></div>
    </div>
</div>
<input type="button" id="btn" value="Click">
<script>
	// 通过事件对象，可以获取到事件发生的时候和时间相关的一些数据
    var btn = document.getElementById('btn');
    btn.onclick = function(e) {
        // DOM 标准中，是给事件处理函数一个参数
        // e 就是事件对象
        // 在老版本的 IE 中获取事件对象的方式 window.enevt
        e = e || window.event;
        
        // 事件处理的阶段  1,捕获阶段  2，目标阶段  3，冒泡阶段
        console.log(e.eventPhase)；
        // target 获取真正触发事件的对象   浏览器兼容问题，在老版本的 IE 中，使用 srcElement
        var target = e.target || e.srcElement;
        console.log(target);
        // 事件处理函数中所属的对象 和 this 一样
        console.log(e.currentTagget);
    }
    
    var box1 = document.getElementById('box1');
    var box2 = document.getElementById('box2');
    var box3 = document.getElementById('box3');
    
    var arr = [box1, box2, box3];
    for  (var i = 0; i < arr.length; i++) {
        arr[i].onclick = function(e) {
            e = e || window.event;
            console.log(e.eventPhase);
            var target = e.target || e.srcElement;
            console.log(target);
            console.log(e.currentTarget);
        }
    }
</script>
```

### 事件委托/代理

优势：

- 节省内存占用，减少事件注册
- 新增子对象时无需再次对其绑定时间，适合动态添加元素

```html
<div id="box"></div>
<script>
	var box = document.getElementById('box');
    
    box.onclick = fn;
    box.onmouseover = fn;
    box.onmouseout = fn;
    // e.type 获取事件名称
    function fn(e) {
        e = e || window.event;
        switch (e.type) {
            case 'click':
                console.log('点击');
                break;
            case 'mouseover':
                console.log('经过');
                break;
            case 'mouseout':
                console.log('离开');
                break;
        }
    }
</script>
```

### 获取鼠标坐标

```html
<div id="box"></div>
<script>
    var box = document.getElementById('box');
    box.onclick = function(e) {
        e = e || window.event;
        // 相对于浏览器视窗的坐标
        console.log(e.clientX);
        console.log(e.clientY);
        // 相对于页面文档的坐标  有兼容性问题 IE9+
        console.log(e.pageX);
        console.log(e.pageY);
    }
</script>
```

### 案例：图片跟随光标

```html
<imgge scr="" id="ts">
<script>
    var ts = document.getElementById('ts');
    document.onmouseover = funtion(e) {
        e = e || window.event;
        // ts.style.left = e.clintX - 10 + 'px';
        // ts.style.top = e.clintY - 10 + 'px';
        
        ts.style.left = e.pageX - 10 + 'px';
        ts.style.top = e.pageY - 10 + 'px';
    }
</script>
```

### 获取页面的滚动距离

```html
<script>
    document.onclick = function() {
        // 输出页面滚动出去的距离
        // Chrome
        console.log(document.body.scrollLeft);
        console.log(document.body.scrollTop);
        
        // documentElement 文档的根元素
        // 部分浏览器获取的方法
        console.log(document.documentElement.scrollLeft);
        console.log(document.documentElement.scrollTop);
    }
    
    // 获取页面滚动距离的浏览器兼容问题
    function getScroll() {
        var scrollLeft = document.body.scrollLeft || document.documentElement.scrollLeft;
        var scrollTop = document.body.scrollTop || document.documentElement.scrollTop;
        return {
            scrollLeft: scrollLeft,
            scrollTop: scrollTop
        }
    }
    
    document.onclick = function() {
        e = e || window.event;
        console.log(getScroll().scrollLeft);
        console.log(getScroll().scrollTop);
    }
</script>
```

### pageX 兼容处理

```html
<script>
	document.onclick = function() {
        e = e || window.event;
        console.log(getPage(e).pageX);
        console.log(getPage(e).pageY);
    }
    function getPage(e) {
        var pageX = e.pageX || e.clintX + getScroll().scrollLeft;
        var pageY = e.pageY || e.clintY + getScroll().scrollTop;
        return {
            pageX: pageX,
            pageY: pageY
        }
    }
</script>
```

### 获取鼠标在页面上的位置

```html
<div id="box">
    
</div>
<script>
	var box = document.getElementById('box');
    box.onclick = function(e) {
        // 获取盒子在页面上的位置
        console.log(this.offsetLeft);
        console.log(this.offsetTop);
        
        e = e || window.event;
        
        // 获取鼠标在盒子中的位置 = 鼠标的坐标 - 盒子的坐标
        var x = getPage(e).pageX - this.offsetLeft;
        var y = getPage(e).pageY - this.offsetTop;
        console.log(x);
        console.log(y);
    }
</script>
```

### 取消默认行为 和 阻止冒泡

```html
<a href="http://baidu.com" id="link">百度</a>
<script>
	var link = ..;
    link.onclick = function() {
        return false;
        // DOM 标准方法
        e.preventDefault;
        // IE 老版本
        e.returnValue = false;
    }
</script>
```

```html
<div id="box1">
    <div id="box2">
        <div id="box3"></div>
    </div>
</div>

<script>
	// addEventListener 的第三个参数的作用
    var box1 = document.getElementById('box1');
    var box2 = document.getElementById('box2');
    var box3 = document.getElementById('box3');
    
    var arr = [box1, box2, box3];
    for  (var i = 0; i < arr.length; i++) {
        arr[i].onclick = function(e) {
            console.log(this.id);
            // DOM 标准方法
            e.stopPropagetion();
    		// 老版本 IE
		    e.cancleBubble();
        }
    } 
```

### 案例：只能输入数字

```html
<input type="text" id="text">
<script>
	var text = document.getElementById('text');
    // 键盘事件  keydown 键盘按下    keyup 键盘弹起
    // keydown 的时候，我们所按的键还没有落入文本框
    // keyup 的时候，我们所按的键已经落入文本框
    text.onkeydown = function(e) {
        // 判断当前用户按下的键是都是数字
        e = e || window.event;
        // 键盘码  如果 e.keyCode 的值在 48 到 57 之间
        console.log(e.keyCode);
        // 按下后退键会删除一个字符
        if (e.keyCode < 48 || e.keyCode > 57 && e.keyCode !== 8) {
            // 非数字
            // 取消默认行为
            // e.preventDefault(); // 此方法会继续执行后面的方法
            return false;  // 此方法取消后面的所有执行
        }
    }
</script>
```

精选文章[JS 中的事件绑定、事件监听、事件委托是什么](https://juejin.im/post/5a94bbea5188257a6049bcc5)

### BOM 

BOM（Browser Object Model） 是指浏览器对象模型，浏览器对象模型提供了独立于内容的、可以与浏览器窗口进行互动的对象结构。BOM 由多个对象组成，其中代表浏览器窗口的 window 对象是 BOM 的顶层对象，其他对象都是该对象的子对象。

```javascript
// window 是浏览器的顶级对象，当我们使用 window 的成员时，window 可以省略不写
// window.alert(); ========> alert();
// window.document.write();  ========>  document.write();
var age = 18;  // 定义的全局变量都是 window 对象的属性
console.log(window.age);  // 18
console.dir(window);  // 打印出所有的 window 对象属性
// window 的默认属性只能获取，不能赋值,尽量不要起这样的变量名，例如 name、top 等
```

### 对话框

了解即可，兼容性不好，且长的丑，一般不用

```html
<input type="button" id="btn1" value="alert">
<input type="button" id="btn2" value="prompt">
<input type="button" id="btn3" value="confirm">
<script>
	var btn1 = document.getElementById('btn1');
    var btn2 = document.getElementById('btn2');
    var btn3 = document.getElementById('btn3');
    // 打开浏览器看效果
    btn1.onclick = function() {
        alert('Hello World!');
    }
    
    btn2.onclick = function() {
        var tips = prompt('请输入用户名');
        console.log(tips);
    }
    
    btn3.onclick = function() {
        var isOk = confirm('是否删除数据');
        console.log(isOk);
    }
</script>
```

### onload 和 onunload

```html
<script>
	// var box = document.getElementById('box');
    // console.dir(box);   // 这里获取不到 box，因为页面从上到下执行，box 还没有加载
    
    // onload 页面加载完成后执行
    onload = function() {
        var box = document.getElementById('box');
    	console.dir(box);
    }
    
    // onunload 页面卸载的时候执行
    onunload = function() {
        // 在 onunload 中，所有的对话框都无法使用
        // 页面释放的时候，window 对象被冻结，这将会阻止所有的对话框执行
        // F5 刷新的时候发生了两件事情
        // 1.卸载页面    2.重新加载页面
        console.log('bye');
    }
</script>
<body>
    <div id="box">
    
    </div>
</body>
<script>
	// 当页面上的元素创建完毕后就会执行
</script>
```

### setTimeout() 

setTimeout() 定时器，在指定的好秒速到达之后执行指定的函数，只执行一次 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setTimeout)

```html
<input type="button" id="btn1" value="开启定时器">
<input type="button" id="btn2" value="取消定时器">
<script>
	// setTimeout() 隔一段时间执行，并且只会执行一次
    var btn1 = document.getElementById('btn1');
    var btn2 = document.getElementById('btn2');
    // 定时器的标识
    var timerId;
    btn1.onclick = function() {
        // 定时器有两个参数
        // 1.要执行的函数，第一个参数可以放命名函数，也可以放匿名函数
        // 2.间隔的时间(单位是毫秒)
        // 定时器的返回值是一个整数，这个整数是定时器的标识
        
        // timerId = setTimeout(function() {
        //     alert('Boommmmmmmmm');
        // }, 3000);
        
        timerId = setTimeout(fn, 3000);
        
        function fn() {
            alert('BOOMMMMMMMMM！！！！！！');
        }
        
        btn2.onclick = function() {
            // 取消定时器的执行
            clearTimeout(timerId);
        } 
    }
</script>
```

### 案例：删除提示

`clearTimeout()`取消通过 `setTimeout()`设置的定时器 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/WindowTimers/clearTimeout)

```html
<style>
    #tip {
        width: 300px;
        height: 50px;
        line-height: 50px;
        background-color: #ff0;
        color: #f00;
        opacity: .4;
        text-align: center;
        margin: 150px auto;
        display: none;
    }
</style>
<input type="button" id="btn" value="删除">
<div id="tip">删除成功</div>
<script>
	var btn = document.getElementById('btn');
    var tip = document.getElementById('tip');
    btn.onclick = function() {
        tip.style.display = 'block';
        setTimeout(function() {
            tip.style.display = 'none';
        }, 2000)
    }
</script>
```

### setInterval()

setInterval() 定时器，用法和 setTimeout() 相同，但是在执行的时候，此方法会重复调用一个函数或执行一段代码，在每次调用之间具有固定的时间延迟 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/setInterval)

```html
<input type="button" id="btn1" value="开启定时器">
<input type="button" id="btn2" value="取消定时器">
<script>
    // setInterval() 隔一段时间执行，并且会重复执行
	var btn1 = document.getElementById('btn1');
    var btn2 = document.getElementById('btn2');
    // 定时器的标识
    var timerId;
    btn1.onclick = function() {
        // 第一次执行也要等 3000 毫秒
        var timerId = setInterval(function() {
            alert('早上好，该起床啦');
        }, 3000);
    }
    
    btn2.onclick = function() {
        clearInterval(timerId);
    }
</script>
```

### 案例：倒计时

`clearInterval()`取消通过 `setInterval()`方法设置的定时器 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Window/clearInterval)

```html
<script>
	var day = document.getElementById('day');
    var hour = document.getElementById('hour');
    var minute = document.getElementById('minute');
    var second = document.getElementById('second');
    // 定义一个变量，用来存储结束的时间
    var end = new Date('2018-11-11 00:00:00');
    function getTime(end) {
        // 定义一个变量，存储当前的时间
        var start = new Date();
        // 计算出结束时间和当前时间的差（毫秒）
        var time = parseInt((+end - +start) / 1000);
        var day, hour, minute, second;
        day = parseInt(time / 60 / 60 / 24);
        second = parseInt(time % 60);
        hour = parseInt(time / 60 / 60 % 24);
        minute = parseInt(time / 60 % 60);
        return {
            day: day,
            second: second,
            hour: hour,
            minute: minute
        }
    }
    
    function fn() {
        var result = getTime(end);
        day.innerText = result.day;
        second.innerText = result.second;
        hour.innerText = result.hour;
        minute.innerText = result.minute;
    }
    // 代码优化：在定时器启动前，先执行一次，解决刚打开页面倒计时为 0 的问题
    fn();
    
    setInterval(fn, 1000)
</script>
```

### 案例：简单动画

```html
<style>
    #box {
        width: 200px;
        height: 200px;
        background-color: #f00;
        position: absolute;
    }
</style>
<input type="button" id="btn" value="click">
<div id="box"></div>
<script>
    var btn = document.getElementById('btn');
    var box = document.getElementById('box');
	// 1.点击按钮，让盒子向右移动
    btn.onclick = function() {
        // 通过 style.left 获取的是标签中的 style 属性设置的样式属性值，如果 style 的属性设置中没有该样式属性，获取到的就是空字符串。
        // 该方法无法获取通过 css 设置的样式
        // console.log(box.style.left);
        
        // 获取盒子当前位置  offsetLeft  offsetTop (只读)
        // box.style.left = box.offsetLeft + 10 + 'px';
        
        
        // 2.让盒子不停的向右移动
        var timerId = setInterval(function() {
            // 让盒子停在 500px 的位置
            // 判断盒子当前位置是否到达 500px
            
            // 盒子最终停止的位置
            var target = 500;
            // 步进
            var step = 10;
            if (box.offsetLeft >= target) {
                // 清除定时器
                clearInterval(timerId);
                // 设置横坐标为 500
                box.style.left = target + 'px';
                // 跳出循环
                return;
            }
            box.style.left = box.offsetLeft + step + 'px';
        }, 30)      
    }
</script>
```


