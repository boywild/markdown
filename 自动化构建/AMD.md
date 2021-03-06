异步模块定义规范（AMD）制定了定义模块的规则，这样模块和模块的依赖可以被异步加载。这和浏览器的异步加载模块的环境刚好适应（浏览器同步加载模块会导致性能、可用性、调试和跨域访问等问题）。

此AMD与科技公司AMD 及其制造的AMD处理器无关。

##API说明
###define() 函数
本规范只定义了一个函数 "define"，它是全局变量。函数的描述为：

    define(id?, dependencies?, factory);

####id

**名字**
第一个参数，id，是个字符串。它指的是定义中模块的名字，这个参数是可选的。如果没有提供该参数，模块的名字应该默认为模块加载器请求的指定脚本的名字。如果提供了该参数，模块名必须是“顶级”的和绝对的（不允许相对名字）。

**模块名的格式**
模块名用来唯一标识定义中模块，它们同样在依赖数组中使用。`AMD`的模块名规范是`CommonJS`模块名规范的超集。引用如下：

* 模块名是由一个或多个单词以正斜杠为分隔符拼接成的字符串
* 单词须为驼峰形式，或者"."，".."
* 模块名不允许文件扩展名的形式，如".js"
* 模块名可以为 "相对的" 或 "顶级的"。如果首字符为"."或".."则为"相对的"模块名
* 顶级的模块名从根命名空间的概念模块解析
* 相对的模块名从 "require" 书写和调用的模块解析

上文引用的`CommonJS`模块`id`属性常被用于`JavaScript`模块。

**相对模块名解析示例：**

* 如果模块 "a/b/c" 请求 "../d", 则解析为"a/d"
* 如果模块 "a/b/c" 请求 "./e", 则解析为"a/b/e"
* 如果AMD的实现支持加载器插件(Loader-Plugins),则"!"符号用于分隔加载器插件模块名和插件资源名。由于插件资源名可以非常自由地命名，大多数字符都允许在插件资源名使用。


####依赖dependencies
第二个参数，dependencies，是个定义中模块所依赖模块的数组。依赖模块必须根据模块的工厂方法优先级执行，并且执行的结果应该按照依赖数组中的位置顺序以参数的形式传入（定义中模块的）工厂方法中。
依赖的模块名如果是相对的，应该解析为相对定义中的模块。换句话来说，相对名解析为相对于模块的名字，并非相对于寻找该模块的名字的路径。
本规范定义了三种特殊的依赖关键字。如果"`require`","`exports`", 或 "`module`"出现在依赖列表中，参数应该按照`CommonJS`模块规范自由变量去解析。
依赖参数是可选的，如果忽略此参数，它应该默认为["`require`", "`exports`", "`module`"]。然而，如果工厂方法的形参个数小于3，加载器会选择以函数指定的参数个数调用工厂方法。

####工厂方法factory
第三个参数，`factory`，为模块初始化要执行的函数或对象。如果为函数，它应该只被执行一次。如果是对象，此对象应该为模块的输出值。
如果工厂方法返回一个值（对象，函数，或任意强制类型转换为`true`的值），应该为设置为模块的输出值。

**简单的 CommonJS 转换**
如果依赖性参数被忽略，模块加载器可以选择扫描工厂方法中的`require`语句以获得依赖性（字面量形为`require("module-id")`）。第一个参数必须字面量为`require`从而使此机制正常工作。
在某些情况下，因为脚本大小的限制或函数不支持`toString`方法（`OperaMobile`是已知的不支持函数的`toString`方法），模块加载器可以选择扫描不扫描依赖性。
如果有依赖参数，模块加载器不应该在工厂方法中扫描依赖性。

**define.amd 属性**
为了清晰的标识全局函数（为浏览器加载script必须的）遵从`AMD`编程接口，任何全局函数应该有一个"`amd`"的属性，它的值为一个对象。这样可以防止与现有的定义了`define`函数但不遵从AMD编程接口的代码相冲突。
当前，`define.amd`对象的属性没有包含在本规范中。实现本规范的作者，可以用它通知超出本规范编程接口基本实现的额外能力。
`define.amd`的存在表明函数遵循本规范。如果有另外一个版本的编程接口，那么应该定义另外一个属性，如`define.amd2`，表明实现只遵循该版本的编程接口。

一个如何定义同一个环境中允许多次加载同一个版本的模块的实现：

    define.amd={
      multiversion: true
    }

最简短的定义：

    define.amd = {};

一次输出多个模块
在一个脚本中可以使用多次define调用。这些define调用的顺序不应该是重要的。早一些的模块定义中所指定的依赖，可以在同一脚本中晚一些定义。模块加载器负责延迟加载未解决的依赖，直到全部脚本加载完毕，防止没必要的请求。

##例子

###使用 require 和 exports

require 是一个方法，接受 模块标识 作为唯一参数，用来获取其他模块提供的接口。
中间包括两个概念
1、相对标识  以 . 开头，只出现在模块环境中（define 的 factory 方法里面）。相对标识永远相对当前模块的 URI 来解析：
2、顶级标识  不以点（.）或斜线（/）开始， 会相对模块系统的基础路径（即 Sea.js 的 base 路径）来解析：

创建一个名为"alpha"的模块，使用了require，exports，和名为"beta"的模块:

    define("alpha", ["require", "exports", "beta"], function (require, exports, beta) {
       exports.verb = function() {
           return beta.verb();
           //Or:
           return require("beta").verb();
       }
    });
一个返回对象的匿名模块：

     define(["alpha"], function (alpha) {
         return {
           verb: function(){
             return alpha.verb() + 2;
           }
         };
     });
一个没有依赖性的模块可以直接定义对象：

     define({
       add: function(x, y){
         return x + y;
       }
     });
一个使用了简单CommonJS转换的模块定义：

     define(function (require, exports, module) {
       var a = require('a'),
           b = require('b');

       exports.action = function () {};
     });

##全局变量
本规范保留全局变量`define`以用来实现本规范。包额外信息异步定义编程接口是为将来的`CommonJS `API保留的。模块加载器不应在此函数添加额外的方法或属性。
本规范保留全局变量`require`被模块加载器使用。模块加载器可以在合适的情况下自由地使用该全局变量。它可以使用这个变量或添加任何属性以完成模块加载器的特定功能。它同样也可以选择完全不使用`require`。

##使用注意
为了使静态分析工具（如`build`工具）可以正常工作，推荐使用字面上形如的`define(...)`。

##与CommonJS的关系
一个关于本`API`的`wiki`开始在`CommonJS` `wiki`中创建了，作为中转的格式，模块中转。但是为了包含模块定义接口，随着时间而不断改变。在`CommonJS`列表中关于推荐本`API`作为模块定义`API`尚未达成一致。本API被转移到它自己的`wiki`和讨论组中。
`AMD`可以作为`CommonJS`模块一个中转的版本只要`CommonJS`没有被用来同步的require调用。使用同步`require`调用的`CommonJS`代码可以被转换为使用回调风格的`AMD`模块加载器。


文章参考
[参考一](http://javascript.ruanyifeng.com/tool/requirejs.html)
[参考二](https://github.com/amdjs/amdjs-api/wiki/AMD-(%E4%B8%AD%E6%96%87%E7%89%88))
[参考三](http://www.runoob.com/w3cnote/requirejs-tutorial-1.html)
[参考四](http://www.runoob.com/w3cnote/requirejs-tutorial-2.html)



