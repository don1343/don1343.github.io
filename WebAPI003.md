### 父子节点

- `Node.parentNode`返回指定的节点在 DOM 树中的父节点 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/parentNode)
- `Node.childNodes`返回包含指定节点的子节点的集合，该集合为即时更新的集合 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/childNodes)
- `ParentNode.children`是一个只读属性，返回一个 Node 的子元素的集合，该属性为一个即时更新的属性 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/ParentNode/children)

```html
<div id="box">
	<span>span</span>    
    <p>p标签</p>
    <!-- 这里是注释 -->
</div>
<script>
    // var box = document.getElementById('box');
    // console.log(box.childNodes);  // 返回所有的元素节点
    // console.log(box.parentNode);
    
    // for (var i = 0; i < box.childNodes.length; i++) {
        // var node = box.childNodes[i];
        // if (node.nodeType === 1) {
        //     console.log(node);
        // }
    // }
    
    /* ========================================= */
    // 直接获取子元素的属性  children
    var box = document.getElementById('box');
    console.log(box.children);  // 获取到所有的子元素
</script>
```

### 案例：隔行变色

`Node.hasChildNodes()`方法返回一个布尔值，表明当前节点是否包含有子节点 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/hasChildNodes)

```html
<ul id="list">
    <li>七秀</li>
    <li>万花</li>
    <li>天策</li>
    <li>纯阳</li>
    <li>少林</li>
</ul>
<script>
	var list = document.getElementById('list');
    // 判断是否有子节点
    if (list.hasChildNodes()) {
        for (var i = 0; i < mv.children.length; i++) {
            var li = mv.children[i];
            if (i % 2 === 0) {
                // 奇数行
                li.style.backgroundColor = '#f00';
            } else {
                // 偶数行
                li.style.backgroundColor = '#f0f'
            }
        }
    }
</script>
```

### 获取第一个子节点和最后一个子节点

- `Node.firstChild`只读属性，返回当前节点中的第一个子节点，如果没有子节点，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/firstChild)
- `Node.lastChild`只读属性，返回当前节点的最后一个子节点。如果没有子节点，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/lastChild)
- `ParentNode.firstElementChild`只读属性，返回对象的第一个子元素，如果没有子元素，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/ParentNode/firstElementChild) （此方法仅支持 IE9+）
- `ParentNode.lastElementChild`只读属性，返回对象的最后一个子元素，如果没有子元素，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/ParentNode/lastElementChild) （此方法仅支持 IE9+）

```html
<div id="box">
    <div>逍遥此身君子意</div>
    <ul>
        <li>一壶温酒向长空</li>
    </ul>
    <span>哈哈哈</span>
</div>
<script>
	var box = document.getElementById('box');
    // 获取 box 的第一个子节点
    console.log(box.firstChild);
    // 获取 box 的最后一个子节点
    console.log(box.lastChild);
    // 获取 box 的第一个子元素
    console.log(box.firstEleentChild);
    // 获取 box 的最后一个子元素
    console.log(box.lastElementChild);
    
    var firstBox = getFirstElementChild(box);
    console.log(firstBox);
    
    var lastBox = getLastElementChild(box);
    console.log(lastBox);
    
    // 处理 firstElementChild 浏览器兼容性问题
    function getFirstElementChild(el) {
        var node, nodes = el.childNodes, i = 0;
        while (node = nodes[i++]) {
            if (node.nodeType === 1) {
                return node;
            }    
        }
        return null;
    }
    
    // 第二种处理 firstElementChild 浏览器兼容性问题
    function getFirstElementChild(el) {
        for (var i= 0; i < el.childNodes.length; i++) {
            if (el.childNodes[i].nodeType == 1) {
                return el.childNodes[i];
            }
        }
    }
    
    // 处理 lastElementChild 浏览器兼容性问题
    function getLastElementChild(el) {
        var node, nodes = el.childNodes, i = el.cihldNodes.length - 1;
        while (node = nodes[i--]) {
            if (node.nodeType === 1) {
                return node;
            }
        }
        return null;
    }
    
    // 第二种处理 lastElementChild 浏览器兼容性问题
    function getLastElementChild(el) {
        for (var i= el.childNodes.length - 1; i > 0; i--) {
            if (el.childNodes[i].nodeType == 1) {
                return el.childNodes[i];
            }
        }
    }
</script>
```

### 案例：菜单栏制作

```html
<div id="menu">
    <ul>
        <!-- href 的作用把浏览器的地址栏切换成指定的地址，如果是 http 则自动省略 -->
        <li><a href="javascript:;">七秀</a></li>
        <li><a href="javascript:;">万花</a></li>
        <li><a href="javascript:;">天策</a></li>
        <li><a href="javascript:;">少林</a></li>
        <li><a href="javascript:;">纯阳</a></li>
        <li><a href="javascript:;">藏剑</a></li>
        <li><a href="javascript:;">五毒</a></li>
        <li><a href="javascript:;">唐门</a></li>
        <li><a href="javascript:;">明教</a></li>
        <li><a href="javascript:;">丐帮</a></li>
        <li><a href="javascript:;">苍云</a></li>
        <li><a href="javascript:;">长歌</a></li>
        <li><a href="javascript:;">霸刀</a></li>
    </ul>
</div>
<script>
    var menu = my$('menu');
    var ul = getFirstElementChild(menu);
    for (var i = 0; i < ul.children.length; i++) {
        // 找到 a 链接并赋值给 link
		var link = getFirstElementChild(ul.children[i]);
        // 给 a 链接注册点击事件
        link.onclick = linkClick;
    }
    function linkClick() {
        // 让其他的 li 取消高亮
        for (var i = 0; i < ul.children.length; i++) {
            ul.children[i].className = '';
        }
        // 当前 a 标签所在的 li 高亮显示
        this.parentNode.className = 'current';
        // return false;    根据 a 链接的 href 属性来选择设置否
    }
</script>
```

### 扩展：a 链接的 href 属性

```html
<a href="http://baidu.com">百度</a><br>
<a href="#">不跳转</a>
<a href="javascript:void(0)">什么都不做</a>
<a href="javascript:;">什么都不做</a>
<a href="javascript:undefined;">什么都不做</a>
<a href="javascript:0">什么都不做</a>
```

### 获取兄弟节点

- `Node.nextSibling`是一个只读属性，返回当前节点的下一个兄弟节点，如果没有其他节点，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/nextSibling)
- `previousSibling`是一个只读属性，返回当前节点的前一个兄弟节点，如果没有，则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/previousSibling)
- `nextElementSibling`是一个只读属性，返回当前节点的下一个元素节点，如果没有则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/NonDocumentTypeChildNode/nextElementSibling)
- `previousElementSibling`是一个只读属性，返回当前节点的前一个元素节点，如果没有则返回 null [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/NonDocumentTypeChildNode/previousElementSibling)

```html
<div id="box">
    <div>老大</div>
    <div id="second">老二</div>
    <div>老三</div>
    <div>老四</div>
    <div>老五</div>
</div>
<script>
	var second = document.getElementById('second');
    // 获取下一个兄弟节点
    console.log(second.nextSibling);
    // 获取下一个元素节点
    console.log(second.nextElementSibling);
    // 获取上一个兄弟节点
    console.log(second.previousSibling);
    // 获取上一个元素节点
    console.log(second.previousElementSibling);
    
    // nextElementSibling 兼容性处理
    function getNextElementSibling(el) {
        while (el = el.nextSibling) {
            if (el.nodeType === 1) {
                return el;
            }
        }
        return null;
    }
    var next = getNextElementSibling(second);
    console.log(next);
    
    // previousElementSibling 兼容性处理
    function getPreviousElementSibling(el) {
        while (el = el.previousSibilng) {
            if (el.nodeType === 1) {
                return el;
            }
        }
        return null;
    }
    var previous = getPreviousElementSibling(second);
    console.log(previous);
</script>
```

### 动态创建元素的方法

- `document.write`将一个文本字符串写入由 [document.open](https://developer.mozilla.org/zh-CN/docs/Web/API/document.open) 打开的文档流。因为此方法需要向文档流中写入内容，因此在关闭（已加载）的文档上调用此方法会自动调用 document.open ，此操作将清空该文档的内容。[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/write)
- `innerHTML`
- `Document.createElement()`方法创建由标签名指定的 HTML 元素。[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/createElement)
- `Node.appendChild()`方法将一个节点添加到指定父节点的子节点列表末尾 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/appendChild)

```html
<div id="box">
    <p>这是一个段落</p>
</div>
<script>
	var box = document.getElementById('box');
    // 创建 p 标签
    var p = document.createElement('p');
    // 给 p 设置一些内容和样式
    p.innerText = '这是一个动态创建的段落';
    p.style.color = '#f00';
    p.style.fontSize = '20px';
    p.onclick = function() {
        alert('Hello')
    };
    // 把 p 添加到 box 的子节点列表末尾
    box.appendChild(p);
</script>
```

### 案例：点击按钮动态创建列表，并且实现鼠标放上去高亮显示

用 innerHTML 、字符串拼接和数组方法创建动态列表，**推荐使用数组方法**

```html
<input id="btn" type="button" value="ClickMe">
<div id="box"></div>
<script>
	var datas = ['七秀', '万花', '少林', '天策', '纯阳'];
    var btn = document.getElementById('btn');
    btn.onclick = function() {
        var box = document.getElementById('box');
        // 第一种方法，此方法效率低
        // box.innnerHTML = '<ul>';
        
        // 第二种方法
        // var html = '<ul>';
        
        // 使用数组替代字符串拼接
        var arr = [];
        arr.push('<ul>');
        
        for (var i = 0; i < datas.length; i++) {
            var data = datas[i];
            // 第一种
            // box.innerHTML += `<li>${data}</li>`;
            
            // 第二种
            // html += `<li>${data}</li>`;
            
            arr.push(`<li>${data}</li>`)
        }
        // 第一种
        // box.innerHTML += '</ul>';
        
        // 第二种
        // html += '</ul>';
        
        arr.push('</ul>');
        box.innerHTML = arr.join(''); 
    }
</script>
```

使用 createElement 方法动态创建列表

```html
<input id="btn" type="button" value="ClickMe">
<div id="box"></div>
<script>
	var btn = document.getElementById('btn'),
      box = document.getElementById('box'),
      datas = ['七秀', '万花', '少林', '纯阳', '天策'];
    
    btn.onclick = function() {
      // 动态创建 ul
      var ul = document.createElement('ul');
      // 把 ul 添加到 box 里面，此操作会将 DOM 树重新绘制
      box.appendChild(ul);

      for (var i = 0; i < datas.length; i++) {
        // 动态创建 li
        var li = document.createElement('li');
        // 把 li 添加到 ul 里面
        ul.appendChild(li);
        // 设置 li 的内容
        li.innerText = datas[i];

        // 给 li 注册事件
        li.onmousemove = liMouseOver;
        li.onmouseout = liMouseOut;
      }
    }

    function liMouseOver() {
      // 鼠标经过高亮显示
      this.style.backgroundColor = '#999';
    }

    function liMouseOut() {
      // 鼠标离开恢复默认状态
      this.style.backgroundColor = '';
    }
</script>
```

### 案例：动态创建表格

`Node.removeChild`方法从 DOM 中删除一个子节点，返回删除的节点。[点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/removeChild)

```html
<div id="box"></div>
<script>
	var box = document.getElementById('box'),
      // 创建表格元素
      table = document.createElement('table'),
      thead = document.createElement('thead'),
      tbody = document.createElement('tbody'),
      tr = document.createElement('tr'),
      // 表头数据
      headList = ['姓名', '门派', '心法', '操作'],
      // 表格主体内容的数据
      bodyList = [
        { name: '神基喵算', school: '明教', method: '焚影'},
        { name: '夜夕', school: '七秀', method: '冰心'},
        { name: '麻小婶', school: '天策', method: '铁牢律'},
        { name: '希声无形', school: '纯阳', method: '紫霞功'},
        { name: '安慕希希', school: '万花', method: '花间'}
      ];
    // 按顺序将表头添加到 box
    box.appendChild(table);
    table.appendChild(thead);
    thead.appendChild(tr);
    
    for (var i = 0; i < headList.length; i++) {
      var th = document.createElement('th');

      th.innerText = headList[i];
      th.style.border = '1px solid #000';
      th.style.height = '35px';
      th.style.width = '80px';
      th.style.backgroundColor = '#ccc';

      tr.appendChild(th);
    }

    table.appendChild(tbody);

    for (var i = 0; i < bodyList.length; i++) {
      tr = document.createElement('tr');
      a =document.createElement('a');

      for (var k in bodyList[i]) {
        td = document.createElement('td');
        td.style.border = '1px solid #000';
        td.style.textAlign = 'center';
        td.innerText = bodyList[i][k];

        tr.appendChild(td);
      }

      td = document.createElement('td');
      td.style.textAlign = 'center';
      td.style.border = '1px solid #000';

      a.innerText = '删除';
      a.href = 'javascript:;';
      a.onclick = delThis;

      td.appendChild(a);
      tr.appendChild(td);
      tbody.appendChild(tr);
    }

    function delThis() {
      var tr = this.parentNode.parentNode;
      tbody.removeChild(tr);
    }
</script>
```

### 常用的元素操作方法

- `Node.insertBefore()`方法在参考节点之前插入一个节点作为一个指定父节点的子节点 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/insertBefore)
- `Node.replaceChild()`方法用指定的节点替换当前节点的一个子节点，并返回被替换掉的节点 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Node/replaceChild)

```html
<input type="button" value="添加" id="btn">
  <ul id="box">
    <li>七秀</li>
    <li>万花</li>
    <li>天策</li>
    <li>纯阳</li>
    <li>少林</li>
  </ul>
  <ul id="test"></ul>
<script>
    var btn = document.getElementById('btn');
    var box = document.getElementById('box');
    btn.onclick = function() {
      var li = document.createElement('li');
      li.innerText = '藏剑';
      // 添加到指定位置
      // box.insertBefore(li, box.children[2]);
      // 替换指定位置的元素
      // box.replaceChild(li, box.children[3]);
      var test = document.getElementById('test');
      // 剪切元素到新的父元素
      test.appendChild(box.children[0]);
    }
</script>
```

### appendChild 的特性（剪切效果）

```html
<input type="button" id="btn" value="Click">
<ul id="box">
    <li>1</li>
    <li>2</li>
    <li>3</li>
    <li>4</li>
</ul>
<ul id="test"></ul>
<script>
	var btn = document.getElementById('btn'),
        box = document.getElementById('box'),
        test = document.getElementById('ul');
        btn.onclick = function() {
            // 剪切效果
            test.appendChild(box.children[0])
        }
</script>
```

### 案例：选择水果

```html
<select multiple id="all">
    <option>七秀</option>
    <option>纯阳</option>
    <option>少林</option>
    <option>天策</option>
    <option>万花</option>
</select>
<input type="button" id="btn1" vallue=">>">
<input type="button" id="btn2" vallue="<<">
<input type="button" id="btn3" vallue=">">
<input type="button" id="btn4" vallue="<">
<select multiple id="select">
    <option></option>
</select>
<script>
	var all = document.getElementById('all'),
      btn1 = document.getElementById('btn1'),
      btn2 = document.getElementById('btn2'),
      btn3 = document.getElementById('btn3'),
      select = document.getElementById('select');

    btn1.onclick = function () {
      var num = all.children.length;
      for (var i = 0; i < num; i++) {
        select.appendChild(all.children[0]);
      }
    }
	
    // 使用这种方式移动子元素的话，如果子元素有时间，移动之后元素的时间丢失
    // select.innerHTML = all.innerHTML;
    // 使用此方法清空元素的时候，如果子元素有时间，此时会发生内存泄漏
    // all.innerHTML = '';
    
    btn2.onclick = function () {
      var arr = [];
      for (var i = 0; i < select.children.length; i++) {
        arr.push(select.children[i]);
      }

      for (var i = 0; i < arr.length; i++) {
        all.appendChild(arr[i]);
      }
    }

    btn3.onclick = function () {
      var arr = [];
      for (var i = 0; i < all.children.length; i++) {
        if (all.children[i].selected) {
          arr.push(all.children[i]);
          all.children[i].selected = false;
        }
      }

      for (var i = 0; i < arr.length; i++) {
        select.appendChild(arr[i]);
      }
    }

    btn4.onclick = function () {
      var arr = [];
      for (var i = 0; i < select.children.length; i++) {
        if (select.children[i].selected) {
          arr.push(select.children[i]);
          select.children[i].selected = false;
        }
      }

      for (var i = 0; i < arr.length; i++) {
        all.appendChild(arr[i]);
      }
    }
</script>
```

### addEventListener 事件监听

`EventTarget.addEventListener()`方法将指定的监听器注册到事件源上，当该对象触发指定的时间时，指定的回调函数就会被执行。 [点击查看文档](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener)

```html
<input type="button" id="btn" value="click">
<script>
	var btn = document.getElementById('btn');
    btn.addEventListener('click', function() {
        alert('注册事件');
    })
</script>
```





































