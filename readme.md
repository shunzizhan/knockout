# knockout技术分享

-------------------


## 入门

### 简介
Knockout有如下4大重要概念：
- 声明式绑定 (Declarative Bindings)：使用简明易读的语法很容易地将模型(model) 数据关联到DOM元素上。
- UI界面自动刷新 (Automatic UI Refresh)：当您的模型状态(model state)改变时，您的UI界面将自动更新。
- 依赖跟踪 (Dependency Tracking)：为转变和联合数据，在你的模型数据之间隐式建立关系。
- 模板 (Templating)：为您的模型数据快速编写复杂的可嵌套的UI。

### 好处

重要特性:
- 优雅的依赖追踪- 不管任何时候你的数据模型更新，都会自动更新相应的内容。
- 声明式绑定- 浅显易懂的方式将你的用户界面指定部分关联到你的数据模型上。
- 灵活全面的模板- 使用嵌套模板可以构建复杂的动态界面。
- 轻易可扩展- 几行代码就可以实现自定义行为作为新的声明式绑定。
额外的好处：
- 纯JavaScript类库 – 兼容任何服务器端和客户端技术
- 可添加到Web程序最上部 – 不需要大的架构改变
- 简洁的 – Gzip之前大约25kb
- 兼容任何主流浏览器 (IE 6+、Firefox 2+、Chrome、Safari、其它)
- Comprehensive suite of specifications （采用行为驱动开发） 意味着在新的浏览器和平台上可以很容易通过验证。


## Knockout绑定语法
### visible 绑定
当参数设置为一个假值时(例如：布尔值false， 数字值0， 或者null， 或者undefined) ,该绑定将设置该元素的style.display值为none，让元素隐藏。它的`优先级高于你在CSS里定义的任何display样式`。

当参数设置为一个真值时(例如：布尔值true，或者非空non-null的对象或者数组) ,该绑定会删除该元素的style.display值，让元素可见。然后你在`CSS里自定义的display样式将会自动生效`。

如果参数是监控属性observable的，那元素的visible状态将根据参数值的变化而变化，如果不是，那元素的visible状态将只设置一次并且以后不在更新。
注：使用函数或者表达式来控制元素的可见性

### text 绑定
KO将参数值会设置在元素的innerText (IE)或textContent(Firefox和其它相似浏览器)属性上。原来的文本将会被覆盖。

如果参数是监控属性observable的，那元素的text文本将根据参数值的变化而更新，如果不是，那元素的text文本将只设置一次并且以后不在更新。

如果你传的不是数字或者字符串(例如一个对象或者数组)，那显示的文本将是yourParameter.toString()的等价内容。(object)
>注1：使用函数或者表达式来决定text值
>如果你想让你的text更可控，那选择是创建一个依赖监控属性(dependent observable),
>然后在它的执行函数里编码，决定应该显示什么样的text文本。
>注2：关于HTML encoding
>设置元素的innerText或textContent (而不是innerHTML)
>关于IE 6的白空格whitespace
>`Welcome, <span data-bind="text: userName"></span> to our web site.`->`Welcome, <span data-bind="text: userName">&nbsp;</span> to our web site.`

### html 绑定
html绑定到DOM元素上，使得该元素显示的HTML值为你绑定的参数。如果在你的view model里声明HTML标记并且render的话，那非常有用。
KO设置该参数值到元素的innerHTML属性上，`元素之前的内容将被覆盖。`

### css 绑定
该参数是一个JavaScript对象，属性是你的CSS class名称，值是比较用的true或false，用来决定是否应该使用这个CSS class。
非布尔值会被解析成布尔值。例如， 0和null被解析成false，21和非null对象被解析成true。

应用的CSS class的名字不是合法的JavaScript变量命名 如果你想使用my-class 在my-class两边加引号作为一个字符串使用

### style 绑定
style绑定是添加或删除一个或多个DOM元素上的style值
该参数是一个JavaScript对象，属性是你的style的名称，值是该style需要应用的值。
你可以一次设置多个style值

应用的style的名字不是合法的JavaScript变量命名
错误： { font-weight: someValue }; 正确： { fontWeight: someValue }

错误： { text-decoration: someValue }; 正确： { textDecoration: someValue }

### attr 绑定
attr 绑定提供了一种方式可以设置DOM元素的任何属性值
该参数是一个JavaScript对象，属性是你的attribute名称，值是该attribute需要 应用的值。
应用的属性名字不是合法的JavaScript变量命名 如果你要用的属性名称是data-something的话 解决方案是：在data-something两边加引号作为一个字符串使用