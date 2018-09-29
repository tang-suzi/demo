
# 学习笔记

## 三栏布局

    layout.html
    浮动解决方案
        浮动会脱离文档流 需要清除浮动 处理周边关系
        优点 兼容性好 快捷
        缺点 子元素也需要脱离文档流 可用性较差
    绝对定位解决方案
        中间部分高度超出  此部分高度会改变
        当界面的宽度小于left+right的时候 中间超出部分挤出 中间部分消失
    flexbox解决方案
        比较完美
    table解决方案
        优点 兼容性比较好
        缺点 三栏布局当其中一个高度超出 其他的也会超出高度
    网格布局解决方案
        缺点 内容超出高度 其本身高度不会变化  但是内容会超出

## 盒模型

    box.html
    基本概念: 标准模型+IE模型
        标准模型: margin border padding content
        IE模型: margin content(border+padding+content)
    标准模型和IE模型的区别
        标准模型: width=content宽度 height=content高度
        IE模型: width=border-left+padding-left+content+padding-right+border-right
                height=border-top+padding-top+content+padding-bottom+border-bottom
    css如何设置这两种模式
        标准模型:box-sizing: content-box(浏览器默认)
        IE模型:box-sizing: border-box
    js如何设置获取盒模型对应的宽和高
        dom.style.width/height 只能获得内敛样式
        dom.currentStyle.width/height 得到渲染后的宽高(IE支持)
        window.getComputedStyle(dom).width/height 兼容Chrome和Firefox
        dom.getBoundingClientRect().width/height 计算元素的绝对位置 还可以获取top left
---
    边距重叠
    BFC 边距重叠解决方案
        BFC的基本概念 块级格式化上下文
        BFC的原理(渲染规则)
            1.BFC这个元素的垂直方向的边距会发生重叠
                解决方法:给元素加一个父元素并激活BFC
            2.BFC的区域不会与浮动元素的box重叠
                overflow:hidden
            3.BFC在页面中是一个容器 外面的元素不会影响其里面的元素
            4.计算BFC高度的时候 浮动元素也会参与计算
        如何创建BFC
            float值不为none
            position值不是static或者relative
            非块级盒子的块级容器(例如inline-blocks，table-cells，and table-captions)
            overflow不为visible
        BFC的使用场景
            1.清除元素之间的影响
            2.清除内部浮动元素对父级元素的影响
            3.创建自适应布局
    IFC 内敛元素格式化上下文

## DOM事件

    event.html
    基本概念 DOM事件的级别
        DOM0 element.onclick=function(){}

        DOM2 element.addEventListener('click',function(){},false)

        DOM3 element.addEventListener('keyup',function(){},false)
    DOM事件模型
        冒泡
        捕获
    DOM事件流
        事件通过捕获到达目标元素 目标元素再上传到window对象  捕获>目标阶段>冒泡
    描述DOM事件捕获的基本流程
        window>document>html>body>结构一层一层向下传
    Event对象的常见应用
        event.preventDefault() 阻止默认事件
        event.stopPropagation() 阻止冒泡
        event.stopImmediatePropagation() 一个按钮绑定了两个click事件1,2 我想通过优先级的方式 第一个响应函数是A 第二个响应函数是B 按照优先级的方式我想在A被点击的时候不要再执行B了 A的响应函数中加入此方法 会阻止B的执行
        event.currentTarget 指的是绑定了事件监听的元素(可以理解为触发事件元素的父级元素)
        event.target 当前被点击的元素(真正触发事件的那个元素)
    自定义事件
        var eve = new Event('custome');
        ev.addEventListener('custome',function(){
            console.log('custome');
        })
        ev.dispatchEvent(eve)

        CustomEvent
        Event和CustomEvent都是用来做自定义事件的
        区别: customEvent除了可以定义事件名后面还可以跟一个object来做指定参数

## HTTP协议

    HTTP协议的主要特点
        简单快速
        灵活
        无连接
        无状态
    HTTP报文的组成部分
        请求报文-> 请求行(http方法、页面地址、http协议、版本)、 请求头(key和value值来告诉客户端我要哪些内容要注意什么类型)、 空行(告诉服务端往下发送的下一个不是请求头部分了下一个是请求体了)、 请求体
        相应报文-> 状态行 响应头 空行 响应体
    HTTP方法
        GET 获取资源
        POST 传输资源
        PUT 更新资源
        DELETE 删除资源
        HEAD 获得报文首部
    POST和GET的区别
        * GET在浏览器回退时是无害的 而POST会再次提交请求
          GET产生的URL地址可以被收藏 而POST不可以
        * GET请求会被浏览器主动缓存 而POST不会 除非手动设置
          GET请求只能进行URL编码 而POST支持多种编码方式
        * GET请求参数会被完整保留再浏览器历史记录里 而POST中的参数不会被保留
        * GET请求再URL中传送的参数是有长度限制的(2kb) 而POST没有限制
          对参数的数据类型 GET只接受ASCII字符 而POST没有限制
          GET比POST更不安全 因为参数暴露在URL上 所以不能用来传递敏感信息
        * GET参数通过URL传递 POST放在Request body中
    HTTP状态码
        1xx: 提示信息 - 表示请求已接收 继续处理
        2xx: 成功 - 表示请求已被成功接收
            200 OK: 客户端请求成功
            206 Partial Content: 客户发送了一个带有Range头的GET请求 服务器完成了它
        3xx: 重定向 - 要完成请求必须进行更进一步的操作
            301 Moved Permanently: 所有请求的页面已经转移至新的url
            302 Found: 所请求的页面已经临时转移至新的url
            304 Not Modified: 客户端有缓冲的文档并发出了一个条件性的请求 服务器告诉客户 原来缓冲的文档还可以继续使用
        4xx: 客户端错误 - 请求有语法错误或请求无法实现
            400 Bad Request: 客户端请求有语法错误 不能被服务器所理解
            401 Unauthorized: 请求未经授权 这个状态代码必须和WWW-Authenticate报头域一起使用
            403 Forbidden: 对被请求页面的访问被禁止
            404 Not Found: 请求资源不存在
        5xx: 服务器错误 - 服务器未能实现合法的请求
            500 Internal Server Error: 服务器发生不可预期的错误 原来缓冲的文档还可以继续使用
            503 Server Unavailable: 请求未完成 服务器临时过载或当机 一段时间后可能恢复正常
    什么是持久连接
        HTTP协议采用‘请求-应答’模式 当使用普通模式 即非Keep-Alive模式时 每个请求/应答客户和服务器都要新建一个连接 完成之后立即断开连接(HTTP协议为无连接的协议)

        当使用Keep-Alive模式(又称持久连接、连接重用)时 Keep-Alive功能使客户端到服务器端的连接持续有效 当出现对服务器的后继请求时 Keep-Alive功能避免了建立或者重新建立连接
    什么是管线化
        在使用持久连接的情况下 某个连接上消息的传递类似于
            请求1 -> 响应1 -> 请求2 -> 响应2 -> 请求3 -> 响应3
        某个连接上的消息变成了类似这样
            请求1 -> 请求2 -> 请求3 -> 响应1 -> 响应2 -> 响应3
        *管线化机制通过持久连接完成 仅HTTP/1.1支持此技术
        *只有GET和HEAD请求可以进行管线化 而POST则有所限制
        初次创建连接时不应启动管线机制 因为对方(服务器)不一定支持HTTP/1.1 版本的协议
        管线话不会影响响应到来的顺序 如上面的例子所示 响应返回的顺序并未改变
        HTTP/1.1 要求服务器端支持管线化 但并不要求服务器端也对响应进行管线化处理 只是要求对于管线化的请求不失败即可
        由于上面提到的服务器端问题 开启管线化很可能并不会带来大幅度的性能提升 而且很多服务器端和代理程序对管线化的支持并不好 因此现代浏览器如Chrome和Firefox默认并未开启管线化支持

## 原型链

    proto.html
    创建对象有几种方法
        1. var o1 = {name: 'o1'}
           var 011 = new Object({name:'o11'})
        2. var M = function(){this.name='o2'}
           var o2 = new M()
        3. var P = {name:'o3'}
           var o3=Object.create(p)
    原型 构造函数 实例 原型链
        只要是对象就是一个实例
        任何一个函数只要被new使用了 就是一个构造函数
        函数都有一个portotype指向原型对象->构造函数的原型对象通过construct(构造器)与构造函数保持了关联
        构造函数通过new和实例进行关联
        实例通过__proto__与原型对象进行关联

        原型链: 在使用New方法初始化函数的时候得到的新对象的__proto__属性会指向函数对象的原型对象，而函数对象的原型对象又继承至原始对象
        把这个有__proto__串起来的直到Object.prototype.__proto__为null的链叫做原型链。原型链实际上就是js中数据继承的继承链。
---
    instanceof的原理
        instanceof用来判断实例是否属于某个对象
        实例对象内部---->构造函数原型链---->实例对象父对象的原型链
    new运算符
        1.一个新对象被创建 它继承自foo.prototype
        2.构造函数foo被执行 执行的时候 相应的传参会被传入 同时上下文(this)会被指定为这个新实例 new foo等同于 new foo() 只能用在不传递任何参数的情况
        3.如果构造函数返回了一个‘对象’ 那么这个对象会取代整个new出来的结果 如果构造函数没有返回对象 那么new出来的结果为步骤1创建的对象

## 面向对象

    oop.html
    类与实例
        类的声明
        生成实例
    类与继承
        如何实现继承
        继承的几种方式

## 通信

    什么是同源策略及限制
        同源策略限制从一个源加载的文档或脚本如何与来自另一个源的资源急性交互
        这事一个用于隔离潜在恶意文件的关键的安全机制
        源: 协议、域名、端口
        限制:
            Cookie、LocalStorage和IndexDB无法读取
            DOM无法获得
            AJAX请求不能发送
    前后端如何通信
        Ajax 同源下面的通信方式
        WebSpcket 不限制同源策略 不受同源策略的限制
        CORS 支持跨域通信 也支持同源通信
    如何创建Ajax
        XMLHttpRequest对象的工作流程
        兼容性处理
        事件的触发条件
        事件的触发顺序
---
    跨域通信的几种方式
        JSONP
            原理: 利用script标签的可以不同源加载实现的
            如何实现
                1.在window全局注册一个函数
                2.给服务器传递这个函数的名字 同时可以有参数
                3.服务器传递这个函数 内部填充有数据 就可以拿到数据
                4.删除全局注册的那个函数
        Hash
            原理: 页面A通过iframe或frame嵌入了B窗口 目标是A给B发消息
            实现:
                1.拿到B的url地址
                2.改变其hash值
                3.在B中接收 onhashchange
        postMessage
        WebSocket
            1.var ws = new WebSocket('wss://echo.websocket.org')
            2.onopen,onmessage,onclose
        CORS
            浏览器会拦截ajax请求 如果他觉得这个ajax请求是跨域的 他会在http头中加一个origin 如果只是一个普通的ajax 则会被浏览器拦截

```javascript
fetch('/some/url',{
    method: 'get',
    //加一些配置就可以实现跨域的通信
    //参考(http://www.ruanyifeng.com/blog/2016/04/cors.html)
}).then(function(response){

}).catch(function(err){

})
```

## 安全

    CSRF (cross-site request forgery)跨站请求伪造
        攻击原理:
![CSRF攻击原理](https://pic002.cnblogs.com/img/hyddd/200904/2009040916453171.jpg)

        防御措施
            Token验证: 访问网站时 会主动上传cookie 但是不会主动上传token 如果访问接口时 没有带token 则不予通过验证
            Referer验证: 页面来源验证 如果来自于自己站点 则通过 否则不予通过
            隐藏令牌: 隐藏在httphead头中 类似于token 只是使用方式的区别
---
    XSS (cross-site scripting)跨域脚本攻击
        攻击原理
            攻击者对含有漏洞的服务器发起XSS攻击(注入JS代码)
            诱使受害者打开受到攻击的服务器URL
            受害者在Web浏览器中打开URL，恶意脚本执行
        攻击方式
            1.反射型: 发出请求时, XSS代码出现在URL中 作为提交到服务器端 服务器解析后响应 XSS随相应内容一起返回给浏览器 最后浏览器解析执行XSS代码 这个过程就像一次发射 所以交反射型XSS
            2.存储型: 存储型XSS和反射型的XSS区别在于 存储型的XSS提交的代码会存储在服务器端(数据库、内存、文件系统等) 下次请求目标页面时不用再提交XSS代码
        防御措施
            1.编码: 对用户输入的数据进行HTML Entity编码
            2.过滤: 移除用户上传的DOM属性 如onerror等 移除用户上传的style节点 script节点 iframe节点等
            3.校正: 避免直接对HTML Entity编码 使用DOM Prase转换 校正不匹配的DOM标签

## 算法

    排序
![排序算法导图](https://images2015.cnblogs.com/blog/1093977/201707/1093977-20170718164528771-1976850903.jpg  (n:数据规模 k:‘桶’的个数 In-place:占用常数内存,不占用额外内存 Out-place:占用额外内存))

        冒泡排序
        选择排序
            selectionSort.html
            选择排序是一种简单直观的排序算法，无论什么数据进去都是O(n2) 的时间复杂度。所以用到它的时候，数据规模越小越好。唯一的好处可能就是不占用额外的内存空间了吧。通俗来说就是你们中间谁最小谁就出列，站到队列的最后边，然后继续对着剩余的无序数组说你们中间谁最小谁就出列，站到队列的最后边，一直到最后一个，继续站到最后边，这样数组就有了顺序，从小到大。
            实现步骤
                在未排序序列中找到最小（大）元素，存放到排序序列的起始位置
                从剩余未排序元素中继续寻找最小（大）元素，然后放到已排序序列的末尾。
                重复第二步，直到所有元素均排序完毕。
        插入排序
        希尔排序(递减增量排序)
        shellSort.html
            先将整个待排序的记录序列分割成为若干子序列分别进行直接插入排序，待整个序列中的记录基本有序时,再对全体记录进行依次直接插入排序
            实现步骤
                选择一个增量序列 t1，t2，……，tk，其中 ti > tj, tk = 1；
                按增量序列个数 k，对序列进行 k 趟排序；
                每趟排序，根据对应的增量 ti，将待排序列分割成若干长度为 m 的子序列，分别对各子表进行直接插入排序。仅增量因子为 1 时，整个序列作为一个表来处理，表长度即为整个序列的长度
        归并排序
        快速排序(二分法)
            quickSort.html
            快速排序由于排序效率在同为O(N*logN)的几种排序方法中效率较高，因此经常被采用，再加上快速排序思想----分治法也确实实用。快速排序是一种既不浪费空间又可以快一点的排序算法。
            实现步骤
                先从数列中取出一个数作为“基准”。
                分区过程：将比这个“基准”大的数全放到“基准”的右边，小于或等于“基准”的数全放到“基准”的左边。
                再对左右区间重复第二步，直到各区间只有一个数。
        堆排序
        计数排序
        桶排序
        基数排序
[堆栈、队列、链表](https://juejin.im/entry/58759e79128fe1006b48cdfd)
[递归](https://segmentfault.com/a/1190000009857470)