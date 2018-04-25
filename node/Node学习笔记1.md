
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

##Buffer类
一个用于更好操作二进制数据的类
我们在操作文件或者网络数据的时候其实操作的就是二进制数据流，node为我们提供了一个更加方便的去操作这种数据流的类buffer，他是一个全局的类
当我们为一个Buffer对象分配一个空间大小以后，其长度是固定，不能更改
一.创建Buffer
new Buffer(size) 
size[Number]创建一个buffer对象，并为这个对象分配一个大小size
```js
var bf=new Buffer(5);
console.log(bf);
```

new Buffer(array)
array[Array]
分配一个新的buffer使用一个8位字节array数组

new Buffer(size)

```js
var str1='miaov';
var bf=new Buffer(str1);
console.log(str1.length);//5
console.log(bf.length);//5

var str2='妙味';
var bf2=new Buffer(str2);
console.log(str2.length);//2
console.log(bf2.length);//6
中文字，1个字占3个字节
```
new Buffer(size,[encoding])

一.Buffer对象提供的toString、JSON的使用
buf.length
buffer对象的bytes大小

buf.write(string,[offset],[length],[encoding])
string 要写入的字符串
offset 从Buffer对象中的第几位开始写入
length 写入的字符串的长度，写入字符串的编码
encoding 编码格式

buf.toString([encoding],[star],[end])
根据encoding参数(默认是utf-8)返回一个解码的string类型
从star位开始截取到end位，不包含end位

buf.toJSON()
返回一个JSON表示buffer实例。JSON.stringify将会默认调用字符串序列化这个buffer实力

buf.slice(start,end)
返回一个新的buffer，这个buffer将会和老的buffer引用相同的的内存地址
注意：修改新的buffer实例也会改变原来的buffer

buf.copy(targetBuffer,[targetStart],[sourceStart],[sourceEnd])
进行buffer拷贝

二、Buffer中静态方法的使用
Buffer.isEncoding(encoding)
如果给定的编码 encoding 是有效的，返回 true，否则返回 false

Buffer.isBuffer(obj)
测试这个 obj 是否是一个 Buffer.

Buffer.byteLength(string,[encoding])
将会返回这个字符串真实byte长度。 encoding 编码默认是： 'utf8'

Buffer.concat(list,[totalLength])
返回一个保存着将传入buffer数组中所有buffer对象拼接在一起的buffer对象

##文件系统
该模块是核心模块，需要使用require导入后使用

fs.open(path,flags,[mode],callback)
异步版的打开一个文件

fs.openSync(path,flags)
fs.open() 的同步版

fs.read(fd,buffer,offset,length,position,callback)
从指定的文档标识符fd读取文件数据
fd 通过open
buffer
offset
length
position
callback

fs.readSync(fd, buffer, offset, length, position)
fs.read 函数的同步版本。 返回bytesRead的个数

fs.write(fd, buffer, offset, length[, position], callback)
通过文件标识fd，向指定的文件中写入buffer

fs.write(fd, data[, position[, encoding]], callback)
把data写入到文档中通过指定的fd,如果data不是buffer对象的实例则会把值强制转化成一个字符串。

fs.writeSync(fd, buffer, offset, length[, position])
fs.write()的同步版本

fs.writeSync(fd, data[, position[, encoding]])
fs.write()的同步版

fs.close(fd, callback)
关闭一个打开的文件

fs.closeSync(fd)
fs.close() 的同步版本

fs.writeFlie(filename, data, [options], callback)
异步的将数据写入一个文件,如果文件不存在则新建, 如果文件原先存在，会被替换。 data 可以是一个string，也可以是一个原生buffer

fs.writeFileSync(filename, data, [options])
fs.writeFile的同步版本。注意：没有callback，也不需要

fs.appendFile(filename, data, [options], callback)
异步的将数据添加到一个文件的尾部，如果文件不存在，会创建一个新的文件。 data 可以是一个string，也可以是原生buffer

fs.appendFileSync(filename, data, [options])
fs.appendFile的同步版本

fs.readFile(filename, [options], callback)
异步读取一个文件的全部内容

fs.readFileSync(filename, [options])
fs.readFile的同步版本

fs.exists(path, callback)
检查指定路径的文件或者目录是否存在

fs.existsSync(path)
fs.exists的同步版本

fs.unlink(path, callback)
删除一个文件

fs.unlinkSync(path)
fs.unlink() 的同步版本

fs.rename(oldPath, newPath, callback)
重命名

fs.renameSync(oldPath, newPath)
fs.rename() 的同步版本

fs.stat(path, callback)
读取文件信息

fs.statSync(path, callback)
fs.stat() 的同步版本

fs.watch(filename, [options], [listener])
观察指定路径的改变，filename 路径可以是文件或者目录

fs.mkdir(path, [mode], callback)
创建文件夹

fs.mkdirSync(path, [mode])
fs.mkdir的同步版本

fs.readdir(path, callback)
读取文件夹

fs.readdirSync(path)
fs.readdir同步版本

fs.rmdir(path, callback)
删除文件夹

fs.rmdirSync(path)
fs.rmdir的同步版本





