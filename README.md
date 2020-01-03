#实际开发过程或者面试中碰到的css问题汇总
* 1.负margin
    
        margin-left:-100px
        不设置宽度的时候,增加宽度
        设置宽度的时候,向左移动对应距离
        负margin会改变浮动元素的显示位置,
        之后元素不影响
        即使我的元素写在DOM的后面， 
        我也能让它显示在最前面。
        圣杯布局、双飞翼布局啊什么的，都是利用这个原理实现的。
        
        margin-top:-100px
        不会增加高度，只会产生向上位移,后面元素受影响
        
        margin-right:-100px
        不设置宽度的时候增加宽度
        设置宽度的时候 改变后面元素位置
        
        margin-bottom:-100px
        自身无偏移值,，其后元素受影响(上移了)
    
* 2.position:fixed 在ios下无效
        
        ios下唤起软键盘之后,页面position:fixed失效,
        变成了absolote定位,所以当页面超过一屏并且滚动的时候,
        失效的fixed会随元素滚动
        解决思路:
            <body class="layout-scroll-fixed">
                <!-- fixed定位的头部 -->
                <header>
                </header>
                <!-- 可以滚动的区域 -->
                <main>
                    <div class="content">
                    <!-- 内容在这里... -->
                    </div>
                </main>
                <!-- fixed定位的底部 -->
                <footer>
                    <input type="text" placeholder="Footer..."/>
                    <button class="submit">提交</button>
                </footer>
            </body>
>参考插件 `isScroll.js`
* 怎样修改chrome记住密码后自动填充表单的黄色背景

        webkit-text-fill-color 设置文本颜色，-webkit-box-shadow inset设置填充
        input:-webkit-autofill,
        input:-webkit-autofill:hover, 
        input:-webkit-autofill:focus {
           -webkit-text-fill-color: yellow;
           -webkit-box-shadow: 0 0 0px 1000px #000 inset;
        }

* 文本溢出省略号

        1.单行文本
        overflow: hidden；（文字长度超出限定宽度，则隐藏超出的内容）
        white-space: nowrap；（设置文字在一行显示，不能换行）
        text-overflow: ellipsis；（规定当文本溢出时，显示省略符号来代表被修剪的文本）
        2.多行文本
        display: -webkit-box；（和 1 结合使用，将对象作为弹性伸缩盒子模型显示 ）
        -webkit-line-clamp: 2；（用来限制在一个块元素显示的文本的行数）
        -webkit-box-orient: vertical；（和 1 结合使用 ，设置或检索伸缩盒对象的子元素的排列方式 ）
        overflow: hidden；（文本溢出限定的宽度就隐藏内容）
        text-overflow: ellipsis；（多行文本的情况下，用省略号“…”隐藏溢出范围的文本)
        
        兼容性一般： -webkit-line-clamp 属性只有 WebKit 内核的浏览器才支持
        多适用于移动端页面，因为移动设备浏览器更多是基于 WebKit 内核

* 让图文不可复制
        
        -webkit-user-select: none;
        -ms-user-select: none;
        -moz-user-select: none;
        -khtml-user-select: none;
        user-select: none;
        
* span与span之间有看不见的空白

        设置父元素 font-size: 0; 或者父元素设置 display:flex/inline-flex;，

* 如何改变选中文本的文字颜色和背景色

        ::selection {
            background-color: #000;
            color: white;
        }

* less mixin 省略号封装

        /**
        * 溢出省略号
        * @param {Number} 行数
        */
        .mixinEllipsis(@rowCount) {
            & when (@rowCount = 1) {
                overflow: hidden;
                text-overflow: ellipsis;
                white-space: nowrap;
            }
            & when (@rowCount > 1) {
                min-width: 0;
                overflow: hidden;
                text-overflow: ellipsis;
                display: -webkit-box;
                -webkit-line-clamp: @rowCount;
                -webkit-box-orient: vertical;
            }
        }

* 说说z-index有什么需要注意的地方

        background/border ==> z-index<0 ==> block ==> float ==> inline/inline-block ==> z-index:auto ==> z-index:0 ==> z-index > 0

* position
    
        static，默认值。位置设置为static的元素，它始终会处于文档流给予的位置。
        inherit，规定应该从父元素继承 position 属性的值。
            但是任何的版本的 Internet Explorer （包括 IE8）都不支持属性值 “inherit”。
        fixed，生成绝对定位的元素。
            默认情况下，可定位于相对于浏览器窗口的指定坐标。元素的位置通过 “left”, “top”, “right” 以及 “bottom” 属性进行规定。
            不论窗口滚动与否，元素都会留在那个位置。但当祖先元素具有transform属性且不为none时，就会相对于祖先元素指定坐标，
            而不是浏览器窗口。
        absolute，生成绝对定位的元素，相对于距该元素最近的已定位的祖先元素进行定位。
            此元素的位置可通过 “left”、”top”、”right” 以及 “bottom” 属性来规定。
        relative，生成相对定位的元素，相对于该元素在文档中的初始位置进行定位。
            通过 “left”、”top”、”right” 以及 “bottom” 属性来设置此元素相对于自身位置的偏移。
