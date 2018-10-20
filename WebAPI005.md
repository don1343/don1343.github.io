### location

location 对象是 window 对象下的一个属性，该对象可以获取或设置浏览器地址栏的 URL

### URL 的组成

URL （Uniform Resource Locator）统一资源定位符

URL 的组成 `scheme:// host:port/path?query#fragment`

- scheme:通信协议
  - 常用的http、ftp、mailto等
- host:主机
  - 服务器(计算机)域名系统(DNS)主机名或 IP 地址
- port:端口号
  - 整数，可选。省略时将使用方案的默认端口，如 http 的默认端口是 80
- path:路径
  - 由零或多个 '/' 符号隔开的字符串，一般用来表示主机上的一个目录或文件地址
- query:查询
  - 可选，用于给动态网页传递参数，可有多个参数，用 '&' 符号隔开，每个参数的名和值用'='符号隔开。例如 : name=don
- fragment:信息片段
  - 字符串、锚点

```html
<input type="button" id="btn" value="Click">
<script>
    var btn = document.getElementById('btn');
    btn.onclick = function() {
        console.log(location.href);
        location.href = 'http://baidu.com';
        
        // 此方法和 href 作用一样，可以让页面跳转到指定的地方
        location.assign();
        
        // 替换地址栏中的地址，但是不记录历史(不能后退)
        location.replace('https://google.com');
        
        // 重新加载   
        // 参数 true 强制从服务器获取页面
        // false 如果浏览器有缓存的话，直接从缓存获取页面
        location.reload()
        
        // 关于使用 F5 刷新
        // F5 刷新页面，可能从缓存中加载
        // ctrl + F5 强制刷新，从服务器获取页面
    }
</script>
```


### history 对象

```html
<script>
    // 前进
	history.forward();
    // 后退
    history.back();
    // 前进后退
    history.go(1);
    history.go(-1);
</script>
```

### navigator 对象

- `navigator.userAgent`判断用户浏览器的类型
- `navigator.platform`判断浏览器所在的系统平台类型

### offset 偏移

```html
<div id="box">
    <div id='son'></div>
</div>
<script>
    var box = document.getElementById('box');
    // 获取位置
    // 偏移的距离    左边（最近的设置了定位的父级盒子）的距离
    console.log(box.offsetLeft);
    console.log(box.offsetTop);
    // 获取大小 width + padding + border
    console.log(box.offsetWidth);
    console.log(box.offsetHeight);
    
    var son = document.getElementById('son');
    // 获取当前距离当前元素最近的定位父元素，如果没有定位父元素，获取距离 body 的偏移
    // 如果有定位的父级盒子，那么获取的就是距离已定位的父级盒子的偏移
    console.log(son.offsetLeft);
    console.log(son.offsetTop);
    // 获取 son 的大小 width + padding + border
    console.log(son.offsetWidth);
    console.log(son.offsetHeight);
</script>
```

### client 偏移

```html
<div id="box"></div>
<script>
	var box = document.getElementById('box');
    // clientLeft 获取到的是 border-left 的宽度
   	console.log(box.clientLeft);
    // clientTop 获取到的是 border-top 的宽度
    console.log(box.clientTop);
    
    // 获取大小，包括 padding，但是不包括 border
    // width + padding
    console.log(box.clientWidth);
    // height + padding
    console.log(box.clientHeight);
</script>
```

### scroll 滚动偏移

```html
<div id="box"></div>
<script>
    var box = document.getElementById('box');
    // box 滚动出去的距离
	console.log(box.scrollLeft);
    console.log(box.scrollTop);
    // 真正内容的大小，包括 padding 和未显示的内容，不包括滚动条
    console.log(box.scrollWidth);
    console.log(box.scrollHeight);
    
    // 元素的大小 + padding 不包括滚动条
    // console.log(box.clientWidth);
    // console.log(box.clientHeight);
</script>
```

### 案例：拖拽框

```html
<script>
	var box = document.getElementById('d_box');
    var drop = document.getElementById('drop');
    var box_close = document.getElementById('box_close');
    // 点击关闭，隐藏注册框
    box_close.onclick = function() {
        box.style.display = 'none';
    }
    
    // 注册鼠标拿下事件
    drop.onmousedown = function(e) {
        // 当鼠标按下的时候，获取鼠标在盒子中的位置
        // 鼠标在盒子中的位置 = 鼠标在页面上的位置 - 盒子的位置
        var x = e.pageX - box.offsetLeft;
        var y = e.pageY - box.offsetTop;
        
        // 注册鼠标移动事件
        document.onmousemove = function(e) {
            // 当鼠标在页面上移动的时候
            // 盒子的坐标 = 鼠标当前位置 - 鼠标在盒子中的位置
            var boxX = e.pageX - x;
            var boxY = e.pageY - y;
            box.style.lfet = `${boxX}px`;
            box.style.top = `${boxY}px`;
        }
    }
    
    // 注册鼠标松开事件
    drop.onmouseup = function() {
        document.onmousemove = null;
    }
</script>
```

### 案例：模态框

```html
<script>
	var link = document.getElementById('link');
    var login = document.getElementById('login');
    var bg = document.getElementById('bg');
    var closeBtn = document.getElementById('closeBtn');
    var title = document.getElementById('title');
    // 1.点击按钮，弹出登录框和遮盖层
    link.onclick = function() {
        login.style.display = 'block';
        bg.style.display = 'block';
        return false;
    }
    
    // 2.点击关闭按钮，隐藏登录框和遮盖层
    closeBtn.onclick = function() {
        login.style.diplay = 'none';
        bg.style.display = 'block';
    }
    
    // 3.鼠标按下可以进行拖拽
    title.onmousedown = function(e) {
        var x = e.pageX - login.offsetLeft;
        var y = e.pageY - login.offsetTop;
        document.onmousemove = function(e) {
            login.style.left = e.pageX - x + 256 + 'px';
            login.style.top = e.pageY - y - 140 + 'px';
        }
    }
    
    // 4.鼠标送开取消拖拽效果
    title.onmouseup = function() {
        document.onmousemove = null;
    }
</script>
```