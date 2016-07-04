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

### click 绑定
click绑定在DOM元素上添加事件句柄以便元素被点击的时候执行定义的JavaScript 函数
注1：传参数给你的click 句柄
```javascript 
<button data-bind="click: function() { viewModel.myFunction(1, 2) }"> 
    Click me  
  </button> 
```
注2：访问事件源对象
<button data-bind="click: myFunction">  myFunction: function (event)
如果你需要的话，可以使用匿名函数的第一个参数传进去，然后在里面调用：
<button data-bind="click: function(event) { viewModel.myFunction(event, 'param1', 'param2') }"> 
注3: 允许执行默认事件
默认情况下，Knockout会阻止冒泡，防止默认的事件继续执行。例如，如果你点击一个a连接，在执行完自定义事件时它不会连接到href地址。这特别有用是因为你的自定义事件主要就是操作你的view model，而不是连接到另外一个页面。

当然，如果你想让默认的事件继续执行，你可以在你click的自定义函数里返回true。
注4：控制this句柄
注5：防止事件冒泡 `clickBubble`
可以通过额外的绑定clickBubble来禁止冒泡

### event 绑定
大部分情况下是用在keypress，mouseover和mouseout上。
 <div data-bind="event: { mouseover: enableDetails, mouseout: disableDetails }"> 
 你可以声明任何JavaScript函数 – 不一定非要是view model里的函数。你可以声明任意对象上的任何函数，例如： event: { mouseover: someObject.someFunction }。

View model上的函数在用的时候有一点点特殊，就是不需要引用对象的，直接引用函数本身就行了，比如直接写event: { mouseover: enableDetails } 就可以了，而无需写成： event: { mouseover: viewModel.enableDetails } (尽管是合法的)。
### submit 绑定
浏览器会执行你定义的绑定函数而不会提交这个form表单到服务器上。可以很好地解释这个，使用submit绑定就是为了处理view model的自定义函数的，而不是再使用普通的HTML form表单。如果你要继续执行默认的HTML form表单操作，你可以在你的submit句柄里返回true。

### enable 绑定
enable绑定使DOM元素只有在参数值为 true的时候才enabled。在form表单元素input，select，和textarea上非常有用。
非布尔值会被解析成布尔值。例如0和null被解析成false，21和非null对象被解析给true。

### disable 绑定
disable绑定使DOM元素只有在参数值为 true的时候才disabled。在form表单元素input，select，和textarea上非常有用。

disable绑定和enable绑定正好相反，详情请参考enable绑定。

### value 绑定
当用户编辑表单控件的时候， view model对应的属性值会自动更新。同样，当你更新view model属性的时候，相对应的元素值在页面上也会自动更新。

其它参数
valueUpdate
如果你使用valueUpdate参数，那就是意味着KO将使用自定义的事件而不是默认的离开焦点事件。下面是一些最常用的选项：
`change`(默认值) - 当失去焦点的时候更新view model的值，或者是<select>
`keyup` – 当用户敲完一个字符以后立即更新view model。
`keypress` – 当用户正在敲一个字符但没有释放键盘的时候就立即更新view model。不像 keyup，这个更新和keydown是一样的。
`afterkeydown` – 当用户开始输入字符的时候就更新view model。主要是捕获浏览器的keydown事件或异步handle事件。

### checked 绑定
checked绑定是关联到checkable的form表单控件到view model上 - 例如checkbox（<input type='checkbox'>）或者radio button（<input type='radio'>） 。当用户check关联的form表单控件的时候，view model对应的值也会自动更新，相反，如果view model的值改变了，那控件元素的check/uncheck状态也会跟着改变。

### options 绑定
options绑定控制什么样的options在drop-down列表里（例如：<select>）或者 multi-select 列表里 （例如：<select size='6'>）显示。此绑定不能用于<select>之外的元素。关联的数据应是数组（或者是observable数组），<select>会遍历显示数组里的所有的项。
`optionsCaption`值是字符串型，作为默认项显示
<select data-bind='options: myOptions, optionsCaption: "Select an item...", value: myChosenValue'></select>
KO会在所有选项上加上这一个项，并且设置value值为undefined。所以，如果myChosenValue被设置为undefined（默认是observable的），那么上述的第一个项就会被选中。
`optionsText`列表的text来显示
`optionsValue`该<option>的value值
`selectedOptions`

### selectedOptions 绑定
selectedOptions绑定用于控制multi-select列表已经被选择的元素，用在使用options绑定的<select>元素上。

### uniqueName 绑定
uniqueName绑定确保所绑定的元素有一个非空的name属性。如果该元素没有name属性，那绑定会给它设置一个unique的字符串值作为name属性。

## tips
### 循环嵌套
### 对象用observableArray监视