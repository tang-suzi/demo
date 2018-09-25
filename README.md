### 浮动解决方案
    浮动会脱离文档流 需要清除浮动 处理周边关系 
    优点 兼容性好 快捷
    缺点 子元素也需要脱离文档流 可用性较差
### 绝对定位解决方案
    中间部分高度超出  此部分高度会改变
    当界面的宽度小于left+right的时候 中间超出部分挤出 中间部分消失
### flexbox解决方案
    比较完美
### table解决方案
    优点 兼容性比较好
    缺点 三栏布局当其中一个高度超出 其他的也会超出高度
### 网格布局解决方案
    缺点 内容超出高度 其本身高度不会变化  但是内容会超出

### 盒模型
    基本概念: 标准模型+IE模型
        标准模型: margin border padding content

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

### DOM事件
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