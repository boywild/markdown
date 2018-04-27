##node
一个文件就是一个模块
每个模块都有自己的作用于
我们使用var定义的一个变量，他并不是全局的，而是属于当前模块下
`__filename`

`require(path)` 加载一个node模块


path模块路径

1. 绝对路径:
2. 相对路径
3. 如果直接是文件名形式会默认加载node核心模块，或者是node_modules

模块加载

1. 首先按照加载的模块的名称进行查找
2. 如果没有找到，则会在文件名称后加上.js后缀进行查找
3. 如果仍然没有找到，则会在文件名称后加上.json后缀进行查找
4. 如果还没有找到，则会在文件名后加上.node的后缀进行查找

>文件名称 -> .js -> .json -> .node

*注意：*在一个模块中通过var定义的变量，其作用域范围是当前模块，外部不能够直接访问。如果我们想一个模块访问另外一个模块中定义的变量，可以：

1. 把变量作为global对象的一个属性，但是这种做法不推荐
2. 使用模块对象module，在这个module对象，有一个子对象，exports对象，我们可以通过这个对象把一个模块中的局部变量对象提供访问

```JS
require('./1.js');
global.a=100;
console.log(a);

var a=100;
module.exports.a=a;
var a=require('./1.js');
console.log(a);
```

##global.process 
process是一个全局对象，可以在任何地方都能访问到它，通过这个对象提供的属性和方法，使我们可以对当前运行的程序进行访问和控制

###process
* `argv`一组包含命令行参数的数组
* `execPath`开启当前进程的绝对路径
* `env`返回用户环境信息
* `version`返回node版本信息
* `versions`返回node以及node依赖包版本信息
* `pid`当前进程的pid
* `title`当前进程的显示名称
* `arch`返回当前cpu处理器架构
* `platform`返回当前操作系统频台
* `cwd()`返回当前进程的工作目录
* `chdir(directory)`改变当前进程的工作目录
* `memoryUsage()`返回node进程的内存使用情况，单位是byte
* `exit(code)`退出
* `kill(pid)`向进程发送信息
* `stdin/stdout`标准输入输出流(IO)
* `stdin/stdout` 提供了操作输入数据和输出数据的方法，通常也成为IO操作
* `stdin`标准输入流

默认情况下输入流是关闭的，要监听处理输入流数据，首先要开启输入流

```js
var a;
var b;
process.stdout.write('请输入a的值：');
process.stdin.resume();
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
我们在操作文件或者网络数据的时候其实操作的就是二进制数据流，`node`为我们提供了一个更加方便的去操作这种数据流的类`buffer`，他是一个全局的类
当我们为一个`Buffer`对象分配一个空间大小以后，其长度是固定，不能更改
###一.创建Buffer
* `new Buffer(size)` `size[Number]`创建一个`buffer`对象，并为这个对象分配一个大小`size`

```js
var bf=new Buffer(5);
console.log(bf);
```

* `new Buffer(array)` `array[Array]`分配一个新的`buffer`使用一个8位字节`array`数组

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

* `new Buffer(size,[encoding])`

###二.Buffer对象提供的toString、JSON的使用

1. `buf.length`
buffer对象的bytes大小

2. `buf.write(string,[offset],[length],[encoding])`
`string` 要写入的字符串
`offset` 从Buffer对象中的第几位开始写入
`length` 写入的字符串的长度，写入字符串的编码
`encoding` 编码格式

3. `buf.toString([encoding],[star],[end])`
根据`encoding`参数(默认是utf-8)返回一个解码的`string`类型
从`star`位开始截取到`end`位，不包含`end`位

4. `buf.toJSON()`
返回一个`JSON`表示`buffer`实例。`JSON.stringify`将会默认调用字符串序列化这个`buffer`实力

5. `buf.slice(start,end)`
返回一个新的`buffer`，这个`buffer`将会和老的`buffer`引用相同的的内存地址
注意：修改新的`buffer`实例也会改变原来的`buffer`

6. `buf.copy(targetBuffer,[targetStart],[sourceStart],[sourceEnd])`
进行`buffer`拷贝

###三、Buffer中静态方法的使用

1. `Buffer.isEncoding(encoding)`
如果给定的编码`encoding`是有效的，返回`true`，否则返回`false`

2. `Buffer.isBuffer(obj)`
测试这个`obj`是否是一个`Buffer`.

3. `Buffer.byteLength(string,[encoding])`
将会返回这个字符串真实`byte`长度。 `encoding`编码默认是： 'utf8'

4. `Buffer.concat(list,[totalLength])`
返回一个保存着将传入`buffer`数组中所有`buffer`对象拼接在一起的`buffer`对象

##文件系统
该模块是核心模块，需要使用require导入后使用

1. `fs.open(path,flags,[mode],callback)`
异步版的打开一个文件

2. `fs.openSync(path,flags)`
fs.open() 的同步版

3. `fs.read(fd,buffer,offset,length,position,callback)`
从指定的文档标识符`fd`读取文件数据，存放到`buffer`对象中生成一个新的`buffer`对象
`fd`：通过`open`方法成功打开一个文件返回的一个数字编号
`buffer`：`buffer`对象
`offset`：新的内容添加到`buffer`中的起始位置
`length`：添加到`buffer`中的内容长度
`position`：读取文件中的起始位置
`callback`：回调函数接受3个参数`err`,`len`,`newBf`分别是错误信息、`buffer`长度、新的`buffer`对象

4. `fs.readSync(fd, buffer, offset, length, position)`
`fs.read` 函数的同步版本。 返回`bytesRead`的个数

5. `fs.write(fd, buffer, offset, length, position, callback)`
向指定的文件`fd`中写入`buffer`
`fd`：通过`open`方法成功打开一个文件返回的一个数字编号
`buffer`：要写入数据的buffer对象
`offset`：`buffer`对象中要写入的数据的起始位置
`length`：要写入的`buffer`数据的长度
`position`：向`fd`文件写入内容的起始位置
`callback`：回调函数接受3个参数`err`,`len`,`newBf`分别是，写入数据的错误信息 写入数据的长度，写入数据的`buffer`

6. `fs.write(fd, data, position, encoding, callback)`
把`data`写入到文档中通过指定的`fd`,如果`data`不是`buffer`对象的实例则会把值强制转化成一个字符串。
`fd`：通过`open`方法成功打开一个文件返回的一个数字编号
`data`：字符串，向`fd`文件写入的内容
`position`：向`fd`文件写入内容的起始位置
`encoding`：编码格式`utf-8`等
`callback`：回调函数接受3个参数`err`,`len`,`newBf`分别是，写入数据的错误信息 写入数据的长度，写入数据的`buffer`

7. `fs.writeSync(fd, buffer, offset, length[, position])`
`fs.write()`的同步版本

8. `fs.writeSync(fd, data, position, encoding)`
`fs.write()`的同步版

9. `fs.close(fd, callback)`
关闭一个打开的文件

10. `fs.closeSync(fd)`
`fs.close()`的同步版本

11. `fs.writeFlie(filename, data, options, callback)`
异步的将数据写入一个文件,如果文件不存在则新建, 如果文件存在则新的内容会覆盖原有文件的的内容。 `data` 可以是一个`string`，也可以是一个原生`buffer`
`filename`：需要写入或者新建的文件名
`data`：需要写入的数据
`options`：设置编码格式等配置项
`callback`：回掉函数接受一个参数`err`，为`true`表示读取文件或者创建文件失败

12. `fs.writeFileSync(filename, data, [options])`
fs.writeFile的同步版本。注意：没有callback，也不需要

13. `fs.appendFile(filename, data, options, callback)`
异步的将数据添加到一个文件的尾部，如果文件不存在，会创建一个新的文件。 `data` 可以是一个`string`，也可以是原生`buffer`
`filename`：需要写入或者新建的文件名
`data`：需要添加的数据
`options`：设置编码格式等配置项
`callback`：回掉函数接受一个参数`err`，为`true`表示读取文件或者创建文件失败

14. `fs.appendFileSync(filename, data, options)`
`fs.appendFile`的同步版本

15. `fs.readFile(filename, options, callback)`
异步读取一个文件的全部内容
`filename`：要读取的文件名
`options`：配置项，可选
`callback`：回掉函数，接受2个参数`(err,data)`，分别表示读取错误，成功读取的数据

16. `fs.readFileSync(filename, [options])`
`fs.readFile`的同步版本

17. `fs.exists(path, callback)`
检查指定路径的文件或者目录是否存在
`path`：需要检查的文件路径
`callback`：回掉函数接受一个参数isExists,为true表示文件存在，为false表示文件不存在

18. `fs.existsSync(path)`
`fs.exists`的同步版本

19. `fs.unlink(path, callback)`
删除一个文件
`callback`：回到函数接受一个参数`err`，为`true`表示删除失败

20. `fs.unlinkSync(path)`
fs.unlink() 的同步版本

21. `fs.rename(oldPath, newPath, callback)`
重命名
`oldPath`：需要重命名的文件路径
`newPath`：将旧文件更名为新文件的路径
`callback`：更名成功回掉，不接受任何参数

22. `fs.renameSync(oldPath, newPath)`
`fs.rename()`的同步版本

23. `fs.stat(path, callback)`
读取文件信息
`path`：需要读取的文件
`callback`：读取成功回掉，接受2个参数`(err,info)`，分别表示`(错误,读取的信息)`

24. `fs.statSync(path, callback)`
`fs.stat()`的同步版本

25. `fs.watch(filename, options, listener)`
观察指定路径的改变，`filename`路径可以是文件或者目录
*注意：*不是完全跨平台，其在某些情况下不可用
`filename`：要监听的文件
`options`：设置，可选，如果`option`现象被提供那么它应当是一个只包含成员`persistent`的对象，`persistent`为`boolean`类型。`persistent`制定了进程是否只要文件被见识就继续执行，缺省值为`{persistent:true}`
`listener`：状态改变回掉函数接受2个参数`(ev,fn)`，分别表示事件`(change/rename)`，文件是否改变

26. `fs.mkdir(path, mode, callback)`
创建文件夹
`callback`：接受一个参数err 为true创建目录失败

27. `fs.mkdirSync(path,mode)`
`fs.mkdir`的同步版本

28. `fs.readdir(path, callback)`
读取文件夹
`callback`：接受两个参数`(err,fileList)`，分别为`(读取失败,文件夹包含内容list)`

29. `fs.readdirSync(path)`
`fs.readdir`同步版本

30. `fs.rmdir(path, callback)`
删除文件夹
`callback`：接受一个参数err 为true创建目录失败

31. `fs.rmdirSync(path)`
`fs.rmdir`的同步版本

用node进行项目构建

```js
var projectData={
    name:'miaov',
    fileData:[
        {
            name:'css',
            type:'dir'
        },
        {
            name:'image',
            type:'dir'
        },
        {
            name:'js',
            type:'dir'
        },
        {
            name:'index.html',
            type:'file',
            content:'<!DOCTYPE html>\n<html lang="en">\n <head>\n\t<meta charset="UTF-8">\n\t<title>Document</title>\n</head>\n<body>\n</body>\n</html>'
        }
    ]
};
var fs=require('fs');

if(projectData.name){
    fs.mkdirSync(projectData.name);
    var fileData=projectData.fileData;
    if(fileData){
        fileData.forEach(function (f) {
            f.path=projectData.name+'/'+f.name;
            f.content=f.content || '';
            switch (f.type){
                case 'dir':
                    fs.mkdirSync(f.path);
                    break;
                case 'file':
                    fs.writeFileSync(f.path,f.content);
                    break;
                default:
                    break;
            }
        });
    }

}
```





