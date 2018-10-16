### 表单元素的属性

- `value`用于大部分表单元素内容的回去(option除外)
- `type`可以获取 input 标签的类型
- `disabled`禁用属性
- `checked`复选框选中属性
- `selected`下拉菜单选中属性

### 案例：点击按钮禁用文本框

```html
<input type="text" id="text" value="点击禁用文本框">
<input type="button" id="btn">
<script>
	var btn = document.getElementById('btn');
    var text = document.getElementById('text');
    var flag = false;  // 默认为不禁用状态
    btn.onclick = function() {
        if (flag) {
            text.disabled = false;
            this.value = '点击禁用文本框';
            flag = !flag;
        } else {
            text.disabled = true;
            this.value = '点击启用文本框';
            flag = !flag;
        }
    }
</script>
```

### 案例：给文本框赋值并将获取到的文本框的值输出到页面

```html
<body>
    <input type="text"><br>
    <input type="text"><br>
    <input type="text"><br>
    <input type="text"><br>
    <input type="text"><br>
    <input type="text"><br>
    <input type="buttton" id="btn" value="获取">
</body>
<script>
    // 获取按钮元素
    var btn = document.getElementById('btn');
    // 获取所有的 input 元素
    var list = document.getElementsByTagName('input');
    // 定义一个空数组用来存放文本框的 value
    var textList = [];
    // 给 btn 注册点击事件
    btn.onclick = function() {
        // 对所有的 input 元素进行遍历
        for (var i = 0; i < list.length; i++) {
            // 找到符合条件的 input 文本框
            if (list[i].type === 'text') {
                // 把 i 赋值给 每一项的 value 属性
                list[i].value = i;
                // 将每一项添加到新的数组
                textList.push(list[i].value);
            }
        }
        // 调用数组的 join 方法将数组用 | 符号分割为符合条件的字符串
        var str = textList.join('|');
    }
    console.log(str);
</script>
```
### 案例：检测用户名是否是 3-6 位，密码是否是 6-8 位，如果不满足要求高亮显示文本框

```html
请输入用户名：<input type="text" id="username"><br>
请输入密码：<input type="password" id="password"><br>
<input type="button" id="btn">
<script>
	var username = document.getElementById('username');
    var password = document.getElementById('password');
    var btn = document.getElementById('btn');
    btn.onclick = function() {
        if (username.value.length < 3 || username.value.length > 6) {
            username.style.backgroundColor = '#ff0';
        }
        if (password.value.length < 6 || password.value.length > 8) {
            password.style.backgroundColor = '#ff0';
        }
    }
</script>
```

### 案例：设置下拉框的选中项

```html
<input type="button" id="btn" value="点击查看吃什么">
<select id="selList">
    <option value="1">川菜</option><br>
    <option value="2">面食</option><br>
    <option value="3">米饭</option><br>
    <option value="4">米线</option><br>
    <option value="5">麻辣烫</option><br>
</select>
<script>
	// 1.给按钮注册事件
    var btn = document.getElementById('btn');
    btn.onclick = function() {
        // 2.获取下拉框中的所有 option
        var options = document.getElementById('selList').getElementsByTagName('option');
        // 3.随机生成索引
        var randomIndex = parseInt(Math.random() * option.length);
        // 4.根据索引获取 option，并让 option 选中
        options[randomIndex].selected = true;
    }
</script>
```

### 案例：搜索文本框

```html
<style>
    .gray {
        color: gray;
    }
    
    .black {
        color: black;
    }
</style>
<input type="search" class="gray" id="txtSearch" value="请输入搜索关键字">
<input type="button" id="btn" value="搜索">
<script>
    var txtSearch = document.getElementById('txtSearch');
    // 1.当文本框获得焦点，默认文字清空，并让输入的文字颜色为空
    txtSearch.onfocus = function() {
        if (this.value === '请输入搜索关键字') {
            this.value = '';
            this.className = 'black';
        }
    }
    // 2.当文本框失去焦点，还原默认文字
    txtSearch.onblur = function() {
        // if (this.value.length === 0) {
        if (this.value === '' || this.value = '请输入搜索关键字') {
            this.value = '请输入搜索关键字';
            this.className = 'gray';
        }
    }
</script>
```

### 案例：全选反选

```html
<script>
    // 获取全选按钮
	var j_cbAll = document.getElementById('j_cbAll');
    // 获取 input 列表
    var list = document.getElementById().getElementsByTagName('input');
    // 获取反选按钮
    var btn = document.getElementById('btn');
    // 给全选按钮注册点击事件
    j_cbAll.onclick = function() {
        for (var i = 0; i < list.length; i++) {
            if (list[i].type === 'checkbox') {
                // 将每一项状态同步为全选按钮的状态
                list[i].checked = this.checked;
            }
        }
    }
    // 复选框的点击事件
    for (var i = 0; i < list.length; i++) {
        list[i].onclick = function() {
            checkAllCheckBox();
        }
    }
    // 对复选框的状态进行判断，如果任意一项复选框没被选中，则改变全选按钮的 checked 状态为 false
    function checkAllCheckBox() {
        var flag = true;
        for (var i = 0; i < list.length; i++) {
            if (list[i].type === 'checkbox') {
                // 如果有复选框没被选中
                if (!list[i].checked) {
                    // flag 取反
                    flag = !flag;
                    // 跳出循环
                    break;
                }
            }
        }
        // 将全选按钮的 checked 状态和 flag 同步
        j_cbAll.checked = flag;
    }
    // 反选
    btn.onclick = function() {
        for (var i = 0; i < list.length; i++) {
            list[i].checked = !list[i].checked;
        }
        checkAllCheckBox();
    }
</script>
```

### 自定义属性的操作

- `element.getAttribute()`返回元素上一个指定的属性值，如果指定的属性不存在，则返回 null 或 ""(空字符串) [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/getAttribute)
- `element.setAttribute()`设置指定元素上的一个属性值，如果属性已经存在，则更新该属性；否则将添加一个新的属性用指定的名称和值 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/setAttribute)
- `element.removeAttribute()`从指定的元素中删除一个元素 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Element/removeAttribute)

```html
<div id="box" personId="1" age="18">
    张三
</div>
<script>
	var box = document.getElementById('box');
    // 打印自有属性
    console.log(box.id);
    // 获取自定义属性，不能通过 console.log() 方法打印
    console.log(box.getAttribute('age'));
    console.log(box.geiAttribute('personId'));
    // 设置自定义属性
    box.seAttribute('sex', 'male');
    // 设置已有的属性(用的比较少)
    box.setAttribute('class', 'test');
    // 删除属性
    box.removeAttribute('age');
</script>
```

### 样式操作

```html
<style>
    .cls {
        background-color: #f00;
    }
</style>
<input type="button" id="btn" value="click"> <br>
<input type="text" id="txt">
<script>
    // 根据 id 获取元素，封装成函数
    function my$(id) {
        return document.getElementById(id)
    }
	// 样式操作
    // 设置多个样式属性的时候使用类样式比较方便
    // 设置少量样式属性的时候使用 style 比较方便
    
    // 1.使用类样式
    my$('btn').onclick = function() {
        my$('txt').className = 'cls';
    }
    // 2.使用 style
    // 使用此方法设置高度和宽度的时候必须带单位
    my$('btn').onclick = function() {
        my$('txt').style.fontSize = '20px';
        }
    }
</script>
```

### 案例：开关灯

```html
<input type="button" value="点击关灯" id="btn">
<script>
    var flag = true;
    my$('btn').onclick = function() {
        if (flag) {
            doucment.body.style.backgroundColor = 'black';
            this.value = '点击关灯';
            flag = !flag;
        } else {
            document.body.style.backgroundColor = 'white';
            this.value = '点击开灯';
            flag = !flag;
        }
    }
</script>
```

### 案例：显示隐藏二维码

- `mouseover事件` [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/Events/mouseover%E4%BA%8B%E4%BB%B6)
- `onmouseout事件` [点击查看文档](https://developer.mozilla.org/en-US/docs/Web/Events/mouseout)

```html
<script>
	// 当鼠标移入的时候 onmouseover
    my$('node_small').onmouseover = function() {
        // 调用字符串的 replace 的方法，将 hide 类替换成 show 类
        my$('er').className = my$('er').className.repalce('hide', 'show');
    }
    // 当鼠标移出的时候 onmouseout
    my$('node_small').onmouseout = function() {
        my$('er').className = my$('er').className.repalce('show', 'hide');
    }
</script>
```

### 案例：高亮显示当前输入的文本框

- `onfocus`获得输入框焦点
- `onblur`失去输入框焦点

```html
<input type="text"><br>
<input type="text"><br>
<input type="text"><br>
<input type="text"><br>
<input type="text"><br>
<input type="text"><br>
<script>
    var list = document.getElementsByTagName('input');
    for (var i = 0; i < list.length; i++) {
        if (list[i].type !== 'text') {
            continue;
        }
        list[i].onfocus = function() {
            this.style.backgroundColor = '#999';
        }
        list[i].onblur = function() {
            this.style.backgroundColor = '';
        }
    }
</script>
```

### 案例：设置 div 的大小和位置

```html
<style>
    #box {
        width: 100px;
        height: 100px;
        background-color: pink;
    }
    
    /* .cls {
        width: 200px;
        height: 200px;
        background-color: #f00;
        position: absolute;
        top: 100px;
        left: 100px;
    } */
</style>
<input type="button" id="btn" value="ClickMe"><br>
<div id="box"></div>
<script>
    var flag = true;
    my$('btn').onclick = function() {
        if (flag) {
            // 用类名设置样式权重不够
            // box.className = 'cls';
            my$('box').style.width = '200px';
            my$('box').style.height = '200px';
            my$('box').style.backgroundColor = 'pink';
            flag = !flag;
        } else {
            my$('box').style = '';
            flag = !flag;
        }
        
    }
</script>
```

### 案例：隔行变色（随机色）和鼠标放上去高亮显示当前行

```html
<ul id="list">
    <li>纯阳</li>
    <li>七秀</li>
    <li>少林</li>
    <li>万花</li>
    <li>天策</li>
</ul>
<script>
    // 获取所有的列表项
    var listItem = document.getElementById('list').getElementsByTagName('li');
    for (var i = 0; i < listItem.length; i++) 
        // 定义 rgb 的随机参数变量
        var r = parseInt(Math.random() * 256);
        var g = parseInt(Math.random() * 256);
        var b = parseInt(Math.random() * 256);
    	// 进行条件判断，由于列表项的索引是从 0 开始，所以当 i % 2 = 0 的时候，选中的是奇数行
        if (i % 2 === 0) {
            listItem[i].style.backgroundColor = rbg(' + r + ', ' + g + ', ' + b + ');
        } else {
            listItem[i].style.backgroundColor = rbg(' + r + ', ' + g + ', ' + b + ');
        }
        // 定义一个临时变量用来存放当前背景色
        var bg;
        listItem[i].onmouseover = function() {
            // 当鼠标经过的时候，将当前背景色存储给临时变量 bg
            bg = this.style.backgroundColor;
            // 给当前列表项设置高亮
            this.style.backgroundColor = '#999';
        }
        listItem[i].onmouseout = function() {
            // 鼠标离开的时候，将临时变量中的背景色重新赋值给当前列表项
            this.style.backgroundColor = bg;
        }
    }
</script>
```

### 案例：Tab 栏切换

```html
<script>
    // 获取 Tab 栏头部
    var spans = my$('hd').getElementsByTagName('span');
    // 获取 Tab 栏内容
    var divs = my$('bd').getElementsByTagName('div');
    for (var i = 0; i < spans.length; i++) {
        // 给 Tab 栏的每一项设置自定义属性 index，并将 i 赋值给他
        spans[i].setAttribute('index', i);
        // 注册鼠标经过事件
        spans[i].onmouseover = function() {
            for (var i = 0; i < spans.length; i++) {
                // 将所有 Tab 栏的 class 类清空
                spans[i].className = '';
                // 将当前 Tab 栏的 class 类设置为 current
                this.className = 'current';
                // 把所有 Tab 栏内容区域的 class 类清空
                divs[i].className = '';
            }
            // 获取 Tab 栏头部的 index 属性，并赋值给一个变量
            var index = parseInt(this.getAttribute('index'));
            // 设置 Tab 栏头部对应的内容区域的 class 类为 current
            divs[index].className = 'curent';
        }
    }
</script>
```

### 总结

DOM 对象操作

- 获取元素 `document.getElementById()`、`document.getElementsByTagName()`
- 给元素注册事件 `onmouseover` `onmouseout` `onfocus` `onblur`等
- 操作元素的属性
  - 非表单元素 `href` `title` `src` `alt`等
  - 表单元素 `type` `value` `checked` `disabled` `selected`
  - 公共属性 `id` `className` `style`
  - 自定义属性 `setAtribute` `getAttribute` `removeAttribute`



### 模拟 DOM 树结构

目的：深刻理解 DOM，了解节点相关的属性，了解节点的层次结构

```html
<div id="box">hello</div>
<p id="p">world</p>
<script>
	// var box = document.getElementById('box');
    // console.dir(box)
    // var p = document.getElementById('p');
    // 创建一些具有相同属性的对象  构造函数
    function Node(options) {
        // if (options.className) {
        //     this.className = options.className;
        // }
        // this.className = '';
        
        // this.className = options.className ? options.className : '';
        
        // 设置属性的默认值
        this.className = options.className || '';
        this.id = options.id || '';
        
        // 和节点相关的属性
        // 节点的名称，如果是元素节点，是标签的名称
        this.nodeName = this.nodeName || '';
        // 节点的类型，如果是元素节点 1 属性节点 2 文本节点 3
        this.nodeType = this.nodeType || 1;
        // 节点的值，如果是元素节点，始终是 null
        this.nodeValue = this.nodeValue || null;
        
        // 记录子节点
        this.children = options.children || [];
    }
    
    // || 布尔类型的运算符
    // 如果有一边是 true，则返回 true；两边都是 false ，返回 false
    // 两边也可以是其他类型，会先转换成布尔类型
    // 如果第一个运算数转换成布尔类型是 true ，直接返回这个运算数
    // 如果第一个运算数转换成布尔类型是 false，则返回第二个运算数
    
    // 创建 html 节点
    var html = new Node({
        nodeName: 'html'
    });
    
    // 创建 head 节点
    var head = new Node({
        nodeName: 'head'
    });
    
    // 创建 body 节点
    var body = new Node({
        nodeName: 'body'
    });
    
    // 创建 div 节点
    var div = new Node({
        nodeName: 'div'
    });
    
    // 创建 p 节点
    var p = new Node({
        nodeName: 'p'
    });
    
    // 设置 节点
    html.children.push(head);
    html.children.push(body);
    body.children.push(div);
    body.children.push(p);
</script>
```







