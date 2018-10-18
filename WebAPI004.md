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

### 事件对象 2

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

### 事件对象3

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

### 取消默认行为的执行 和 取消冒泡

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

精选文章[JS 中的事件绑定、事件监听、事件委托是什么](https://juejin.im/entry/57ea329e67f3560057ad41a6)

