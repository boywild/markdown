
一个文件就是一个模块
每个模块都有自己的作用于
我们使用var定义的一个变量，他并不是全局的，而是属于当前模块下
__filename

require


模块路径
1、绝对路径
2、相对路径
如果直接是文件名形式会默认加载node核心模块，或者是node_modules

模块加载
1、首先按照加载的模块的名称进行查找
2、如果没有找到，则会在文件名称后加上.js后缀进行查找
3、如果仍然没有找到，则会在文件名称后加上.json后缀进行查找
4、如果还没有找到，则会在文件名后加上.node的后缀进行查找
文件名称 -> .js -> .json -> .node

在一个模块中通过var定义的变量，其作用域范围是当前模块，外部不能够直接访问
如果我们想一个模块访问另外一个模块中定义的变量，可以：
1.把变量作为global对象的一个属性，但是这种做法不推荐
2.使用模块对象module，在这个module对象，有一个子对象，exports对象，我们可以通过这个对象把一个模块中的局部变量对象提供访问

require('./1.js');
global.a=100;
console.log(a);


var a=100;
module.exports.a=a;
var a=require('./1.js');
console.log(a);


global.process 
process是一个全局对象，可以在任何地方都能访问到它，通过这个对象提供的属性和方法，使我们可以对当前运行的程序进行访问和控制

##process
argv 一组包含命令行参数的数组
execPath开启当前进程的绝对路径
env返回用户环境信息
version 返回node版本信息
versions 返回node以及node依赖包版本信息
pid当前进程的pid
title当前进程的显示名称
arch返回当前cpu处理器架构
platform 返回当前操作系统频台
cwd() 返回当前进程的工作目录
chdir(directory) 改变当前进程的工作目录
memoryUsage() 返回node进程的内存使用情况，单位是byte
exit(code) 退出
kill(pid) 向进程发送信息

stdin/stdout标准输入输出流(IO)
stdin/stdout 提供了操作输入数据和输出数据的方法，通常也成为IO操作
stdin 标准输入流
默认情况下输入流是关闭的，要监听处理输入流数据，首先要开启输入流
process.stdin.resume();
process.stdin.on('data',function (chunk) {
    console.log('用户输入了：'+chunk);
});
stdout标准输出流

```js
var a;
var b;

process.stdout.write('请输入a的值：');

process.stdin.on('data',function (chunk) {
    if(!a){
        a=Number(chunk);
        process.stdout.write('请输入b的值：');
    }else{
        b=Number(chunk);
        process.stdout.write('结果是：'+(a+b))
    }
});
```