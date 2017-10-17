###什么是JSON？
**JSON的优点：**
1. 基于纯文本，跨平台传递极其简单；
2. `Javascript`原生支持，后台语言几乎全部支持；
3. 轻量级数据格式，占用字符数量极少，特别适合互联网传递；
4. 可读性较强，虽然比不上`XML`那么一目了然，但在合理的依次缩进之后还是很容易识别的；
5. 容易编写和解析，当然前提是你要知道数据结构；
JSON的缺点当然也有，但在作者看来实在是无关紧要的东西，所以不再单独说明。

**JSON的格式或者叫规则：**
JSON能够以非常简单的方式来描述数据结构，`XML`能做的它都能做，因此在跨平台方面两者完全不分伯仲。
1. JSON只有两种数据类型描述符，大括号`{}`和方括号`[]`，其余英文冒号:是映射符，英文逗号,是分隔符，英文双引号""是定义符。
2. 大括号`{}`用来描述一组`不同类型的无序键值对集合`（每个键值对可以理解为`OOP`的属性描述），方括号`[]`用来描述一组`相同类型的有序数据集合`（可对应`OOP`的数组）。
3. 上述两种集合中若有多个子项，则通过英文逗号,进行分隔。
4. 键值对以英文冒号:进行分隔，并且建议键名都加上英文双引号""，以便于不同语言的解析。
5. JSON内部常用数据类型无非就是字符串、数字、布尔、日期、null 这么几个，字符串必须用双引号引起来，其余的都不用，日期类型比较特殊，这里就不展开讲述了，只是建议如果客户端没有按日期排序功能需求的话，那么把日期时间直接作为字符串传递就好，可以省去很多麻烦。

JSON实例：

```js
// 描述一个人
var person = {
    "Name": "Bob",
    "Age": 32,
    "Company": "IBM",
    "Engineer": true
}

// 获取这个人的信息
var personAge = person.Age;

// 描述几个人
var members = [
    {
        "Name": "Bob",
        "Age": 32,
        "Company": "IBM",
        "Engineer": true
    },
    {
        "Name": "John",
        "Age": 20,
        "Company": "Oracle",
        "Engineer": false
    },
    {
        "Name": "Henry",
        "Age": 45,
        "Company": "Microsoft",
        "Engineer": false
    }
]

// 读取其中John的公司名称
var johnsCompany = members[1].Company;

// 描述一次会议
var conference = {
    "Conference": "Future Marketing",
    "Date": "2012-6-1",
    "Address": "Beijing",
    "Members":
    [
        {
            "Name": "Bob",
            "Age": 32,
            "Company": "IBM",
            "Engineer": true
        },
        {
            "Name": "John",
            "Age": 20,
            "Company": "Oracle",
            "Engineer": false
        },
        {
            "Name": "Henry",
            "Age": 45,
            "Company": "Microsoft",
            "Engineer": false
        }
    ]
}
// 读取参会者Henry是否工程师
var henryIsAnEngineer = conference.Members[2].Engineer;
```
###什么是JSONP？
**先说说JSONP是怎么产生的：**
其实网上关于`JSONP`的讲解有很多，但却千篇一律，而且云里雾里，对于很多刚接触的人来讲理解起来有些困难，小可不才，试着用自己的方式来阐释一下这个问题，看看是否有帮助。
1. 一个众所周知的问题，Ajax直接请求普通文件存在跨域无权限访问的问题，甭管你是静态页面、动态网页、web服务、WCF，只要是跨域请求，一律不准；
2. 不过我们又发现，Web页面上调用js文件时则不受是否跨域的影响（不仅如此，我们还发现凡是拥有"src"这个属性的标签都拥有跨域的能力，比如<script>、<img>、<iframe>）；
3. 于是可以判断，当前阶段如果想通过纯web端（`ActiveX`控件、服务端代理、属于未来的HTML5之Websocket等方式不算）跨域访问数据就只有一种可能，那就是在远程服务器上设法把数据装进js格式的文件里，供客户端调用和进一步处理；
4. 恰巧我们已经知道有一种叫做JSON的纯字符数据格式可以简洁的描述复杂数据，更妙的是`JSON`还被`js`原生支持，所以在客户端几乎可以随心所欲的处理这种格式的数据；
5. 这样子解决方案就呼之欲出了，web客户端通过与调用脚本一模一样的方式，来调用跨域服务器上动态生成的`js`格式文件（一般以`JSON`为后缀），显而易见，服务器之所以要动态生成JSON文件，目的就在于把客户端需要的数据装入进去。
6. 客户端在对`JSON`文件调用成功之后，也就获得了自己所需的数据，剩下的就是按照自己需求进行处理和展现了，这种获取远程数据的方式看起来非常像`AJAX`，但其实并不一样。
7. 为了便于客户端使用数据，逐渐形成了一种非正式传输协议，人们把它称作`JSONP`，该协议的一个要点就是允许用户传递一个`callback`参数给服务端，然后服务端返回数据时会将这个`callback`参数作为函数名来包裹住`JSON`数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了。
如果对于`callback`参数如何使用还有些模糊的话，我们后面会有具体的实例来讲解。

**JSONP的客户端具体实现：**
不管`jQuery`也好，`extjs`也罢，又或者是其他支持`jsonp`的框架，他们幕后所做的工作都是一样的，下面我来循序渐进的说明一下`jsonp`在客户端的实现：
1. 我们知道，哪怕跨域`js`文件中的代码（当然指符合web脚本安全策略的），`web`页面也是可以无条件执行的。
远程服务器`remoteserver.com`根目录下有个`remote.js`文件代码如下：

```js
alert('我是远程文件');
```
本地服务器`localserver.com`下有个`jsonp.html`页面代码如下：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
</head>
<body>

</body>
</html>
```
毫无疑问，页面将会弹出一个提示窗体，显示跨域调用成功。
2. 现在我们在`jsonp.html`页面定义一个函数，然后在远程`remote.js`中传入数据进行调用。
jsonp.html页面代码如下：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript">
    var localHandler = function(data){
        alert('我是本地函数，可以被跨域的remote.js文件调用，远程js带来的数据是：' + data.result);
    };
    </script>
    <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
</head>
<body>

</body>
</html>
```
`remote.js`文件代码如下：
```js
localHandler({"result":"我是远程js带来的数据"});
```
运行之后查看结果，页面成功弹出提示窗口，显示本地函数被跨域的远程js调用成功，并且还接收到了远程js带来的数据。很欣喜，跨域远程获取数据的目的基本实现了，但是又一个问题出现了，我怎么让远程js知道它应该调用的本地函数叫什么名字呢？毕竟是jsonp的服务者都要面对很多服务对象，而这些服务对象各自的本地函数都不相同啊？我们接着往下看。
3. 聪明的开发者很容易想到，只要服务端提供的js脚本是动态生成的就行了呗，这样调用者可以传一个参数过去告诉服务端“我想要一段调用XXX函数的js代码，请你返回给我”，于是服务器就可以按照客户端的需求来生成js脚本并响应了。
看`jsonp.html`页面的代码：

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript">
    // 得到航班信息查询结果后的回调函数
    var flightHandler = function(data){
        alert('你查询的航班结果是：票价 ' + data.price + ' 元，' + '余票 ' + data.tickets + ' 张。');
    };
    // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码）
    var url = "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
    // 创建script标签，设置其属性
    var script = document.createElement('script');
    script.setAttribute('src', url);
    // 把script标签加入head，此时调用开始
    document.getElementsByTagName('head')[0].appendChild(script);
    </script>
</head>
<body>

</body>
</html>
```
这次的代码变化比较大，不再直接把远程js文件写死，而是编码实现动态查询，而这也正是`jsonp`客户端实现的核心部分，本例中的重点也就在于如何完成`jsonp`调用的全过程。
我们看到调用的`url`中传递了一个`code`参数，告诉服务器我要查的是`CA1998`次航班的信息，而`callback`参数则告诉服务器，我的本地回调函数叫做`flightHandler`，所以请把查询结果传入这个函数中进行调用。
OK，服务器很聪明，这个叫做`flightResult.aspx`的页面生成了一段这样的代码提供给`jsonp.html`（服务端的实现这里就不演示了，与你选用的语言无关，说到底就是拼接字符串）：

```js
flightHandler({
    "code": "CA1998",
    "price": 1780,
    "tickets": 5
});
```
我们看到，传递给`flightHandler`函数的是一个`json`，它描述了航班的基本信息。运行一下页面，成功弹出提示窗口，`jsonp`的执行全过程顺利完成！
4. 到这里为止的话，相信你已经能够理解`jsonp`的客户端实现原理了吧？剩下的就是如何把代码封装一下，以便于与用户界面交互，从而实现多次和重复调用。
什么？你用的是`jQuery`，想知道`jQuery`如何实现`jsonp`调用？好吧，那我就好人做到底，再给你一段`jQuery`使用`jsonp`的代码（我们依然沿用上面那个航班信息查询的例子，假定返回`jsonp`结果不变）：

```js
jQuery(document).ready(function(){
   $.ajax({
        type: "get",
        async: false,
        url: "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998",
        dataType: "jsonp",
        jsonp: "callback",//传递给请求处理程序或页面的，用以获得jsonp回调函数名的参数名(一般默认为:callback)
        jsonpCallback:"flightHandler",//自定义的jsonp回调函数名称，默认为jQuery自动生成的随机函数名，也可以写"?"，jQuery会自动为你处理数据
        success: function(json){
            alert('您查询到航班信息：票价： ' + json.price + ' 元，余票： ' + json.tickets + ' 张。');
        },
        error: function(){
            alert('fail');
        }
    });
});
```
[原文链接](http://www.cnblogs.com/dowinning/archive/2012/04/19/json-jsonp-jquery.html)
