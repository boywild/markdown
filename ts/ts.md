[TOC]

#字符串新特性
##多行字符串
语法
```js
``
```
    
##字符串模板
语法
```js
${}
```

##自动拆分字符串
用字符串模板调用test方法，在调用的时候注意，如果想要使用字符串自动拆分的特性 在用这个字符串模板调用test方法的时候不能使用圆括号的形式调用，直接在方法后面跟上字符串模板。例如
```js
var myName = 'chentian';
var getAge = function () { 
    return 27;
}
var test = function (template, name, age) { 
    console.log(template);
    console.log(name);
    console.log(age);
}
test`hellow my name is ${myName},i'm ${getAge()}`
```



#参数新特性
##参数类型
定义：在参数名称后面用冒号来制定参数的类型

1. :string 声明为字符串类型
2. :number 声明为数字类型
3. :boolean 声明为布尔类型
4. :void 方法不需要任何返回值若有返回值，报错
5. :any 任何类型

```js
var myName: string = 'chentian';
myName = 123;
//IDE会报错
```

```js
var myName = 'chentian';
myName = 123;
```

**类型推断机制**
IDE会报错,ts总是以第一个申明的类型为准推断这个变量以后的类型
```js
var alias:any='chentian'
```

参数可以声明类型 对函数返回值也可以申明类型
```js
function test(name:string):string(){
    return "";
}
```

自定义类型
```js
class person{
    name:string//字符串类型,
    age:number//数字类型
}
//如何调用自定义类型
var test:person=new person();
test.name='chentian';
test.age=27;
```

##默认参数
定义：在参数后面用等号来指定参数的默认值
```js
function person(name:string,age:number,email:any='88888888@qq.com'){
    console.log(name);
    console.log(age);
    console.log(email);
}
person('chentian',26);
person('chentian',26,'66666666@qq.com');
```
带默认值的参数一定要放在参数的末尾，否在就会被前面的参数覆盖

##可选参数
定义：在方法的参数声名后面用问号来标明此参数为可选参数
```js
function person(name:string,age?:number,email:any='88888888@qq.com'){
    console.log(name);
    console.log(age);
    console.log(email);
}
```
age为可选参数，可选参数一般声名在必传参数之后

#函数新特性
##Rest and Spread 操作符
定义：用来声名任意数量参数
```js
function test(...args){
    args.forEach(function (arg) {
        console.log(arg);
     });
}
var arr=[1,2,3],arr2[8,9,10,'aa','bb'];
test(arr);
test(arr2);
```

反向的用法
```js
function test(a,b,c){
	console.log(a);
	console.log(b);
	console.log(c);
}
var arg1=[1,2,3],arg2[8,9,10,'aa','bb'];
test(...arg1);
test(...arg2);
```

##genarator函数
定义：控制函数的执行过程，手工暂停和回复代码执行
```js
function* doSomething(){
	console.log('start');
    yield;
    console.log('finish');
}
var fun1=doSomething();
fun1.next();
fun1.next();
```

##destructuring析构表达式
定义：通过表达式将对象或数组拆解成任意数量的变量
析构也可称为解构

###在对象中进行析构
1、普通析构表达式
```js
var test=function(){
    return {
        name: "chentian",
        age: 27,
        email: '6666@qq.com'
    }
};
var {name, age } = test();
console.log(name);//chentian
console.log(age);//27
```

2、映射析构表达式
```js
var test=function(){
    return {
        name: "chentian",
        age: 27,
        email: '6666@qq.com'
    }
};

var {name:myname, age } = test();
console.log(name);//''
console.log(myname);//chentian
console.log(age);//27
```

3、嵌套析构表达式
```js
var test=function(){
    return {
        name: {
            bigName: 'xiaochen',
            smallName:'tiantian'
        },
        age: 27,
        email: '6666@qq.com'
    }
};

var { name: {bigName,smallName }, age } = test();
console.log(bigName);
console.log(smallName);

var test=function(){
    return {
        name: {
            bigName: 'xiaochen',
            smallName:'tiantian'
        },
        age: 27,
        email: '6666@qq.com'
    }
};
var { name: {bigName:nameOne,smallName:nameTwo }, age } = test();
console.log(nameOne);//xiaochen
console.log(nameTwo);//tiantian
```

###在数组中进行析构
定义：与在对象中进行析构不同的地方是，对象用()进行析构，而数组用[]进行析构

```js
var arr=[1,2,3];
var [aa,bb,cc]=arr;
console.log(aa);//1
console.log(bb);//2
console.log(cc);//3
```

```js
var arr = [1, 2, 3, 4, 5, 6, 7];

var [,,,aa, bb, cc] = arr;
console.log(aa);//4
console.log(bb);//5
console.log(cc);//6

var [,aa, bb, cc,...others] = arr;
console.log(aa);//2
console.log(bb);//3
console.log(cc);//4
console.log(others);//[5,6,7]

function test([aa,bb,cc,...other]) { 
    console.log(aa);//2
    console.log(bb);//3
    console.log(cc);//4
    console.log(other);//[5,6,7]
}
```

#表达式与循环
##箭头表达式
1、无参数
```js
var sun = () => { 
    return 'test';
}
//编译后
var sun = function () {
    return 'test';
};

```

2、只有一个参数
```js
var sun = arg1 => { 
    console.log(arg1);
}
sun('chentian');

//编译后
var sun = function (arg1) {
    console.log(arg1);
};
sun('chentian');//chentian

```

3、两个参数
```js
var sun=(arg1,arg2)=>arg1+arg2;
//编译后
var sun = function (arg1, arg2) { return arg1 + arg2; };
```

4、如果表达式是多行则需换行和return
```js
var sun = (arg1, arg2) => { 
    return arg1 + arg2;
}
//编译后
var sun = function (arg1, arg2) {
    return arg1 + arg2;
};
```

5、实例：
```js
var arr = [1, 2, 3, 4, 5, 6];
console.log(arr.filter(value=>value%2===0));
//编译后
var arr = [1, 2, 3, 4, 5, 6];
console.log(arr.filter(function (value) { return value % 2 === 0; }));//[2,4,6]

```

**箭头函数能解决this指向问题**
```js
function getStock(name: string) {
    this.name = name;
    setInterval(function () { 
        console.log('this name is '+this.name);
    },1000);
}
var test = new getStock('chentian');//this name is

function getStock(name: string) {
    this.name = name;
    setInterval(()=>console.log('this name is'+this.name),1000)
}
var test = new getStock('chentian');//this name is chentian
```

##forEach()/for in/for of

```js
var arr = [1, 2, 3, 4, 5];
arr.forEach(value=>console.log(value));
//编译后
var arr = [1, 2, 3, 4, 5];
arr.forEach(function (value) { return console.log(value); });

```

```js
var arr = [1, 2, 3, 4, 5];
for (var i in arr) {
    console.log(i);
    console.log(arr[i]);
 }
```

```js
var arr = [1, 2, 3, 4, 5];
for (var i of arr) { 
    if (i > 3) break;
    console.log(i)
}
```

#面向对象特性
##TypeScript类
###访问控制符

`public`共有 在类的内部和外部都能访问到
`private`私有 只能在类的内部访问在外部不能访问
`protected`受保护 在类的内部和类的子类中能访问到

```js
class Person { 
    constructor(name: string) { 
        console.log(this.name);
    }
    eat() { 
        console.log(this.name);
    }
}

var test1 = new Person('chentian');
test1.eat();//undefined

class Person { 
    constructor(public name: string) { 
        console.log(this.name);
    }
    name;
    eat() { 
        console.log(this.name);
    }
}

var test1 = new Person('chentian');
test1.eat();//chentian
```

`constructor`代表当前的类
上面两种写法的唯一区别就是有没有对参数`name`进行申明，声名了就有输出打印值，没有声名就输入`undefined`

###类的继承
`extends`用来声名类的继承关系
```js
class Person { 
    constructor(public name: string) { 
       
    }
    eat() { 
        console.log(this.name);
    }
}
class Employee extends Person { 
    code: string;
    work() { 
        console.log('111');
    }
}
var test1 = new Person('chentian');
test1.eat();

var test2 = new Employee('yangli');
test2.eat();
```

`super`调用父类的构造函数或者方法
注意：子类的构造函数必须要调用父类的构造函数
```js
class Person { 
    constructor(public name: string) { 
        console.log('haha');
    }
    eat() { 
        console.log(this.name);
    }
}
class Employee extends Person { 
    constructor(name: string, code: string) {
        super(name);//子类的构造函数必须要调用父类的构造函数
        this.code = code;
        console.log('xixi');
     }
    code: string;
    work() { 
        console.log('111');
    }
}
var test2 = new Employee('yangli','1');//打印出haha xixi
```

调用父类的方法
```js
class Person { 
    constructor(public name: string) { 
        console.log('haha');
    }
    eat() { 
        console.log('im eating');
    }
}
class Employee extends Person { 
    constructor(name: string, code: string) {
        super(name);
        this.code = code;
        console.log('xixi');
     }
    code: string;
    work() { 
        super.eat();
        this.doWork();
    }
    doWork() { 
        console.log('im working');
    }
}
var test2 = new Employee('yangli', '1');
test2.work();//haha  xixi  blankim eating im working
test2.doWork();//im working
```

如果给`doWork`加上访问控制符之后就不能直接在外部调用了
```js
class Person { 
    constructor(public name: string) { 
        console.log('haha');
    }
    eat() { 
        console.log('im eating');
    }
}
class Employee extends Person { 
    constructor(name: string, code: string) {
        super(name);
        this.code = code;
        console.log('xixi');
     }
    code: string;
    work() { 
        super.eat();
        this.doWork();
    }
    private doWork() { 
        console.log('im working');
    }
}
var test2 = new Employee('yangli', '1');
test2.work();//haha  xixi  blankim eating im working
test2.doWork();//不能直接调用
```

##泛型
定义：参数化的类型，一般用来限制集合的内容
```js
//Person即上文中的Person
var workers:Array<Person>=[];
workers[0]=new Person('chentian');//正确因为workers是Person的泛型
workers[1]=new Employee('yangli','1');//正确，因为Employee继承自Person
workers[2]=2;//错误
```

##接口
定义：用来建立某种代码约定，使得其他开发者在调用某个方法或者创建新的类时必须遵循接口所定义的代码约定

1. `interface`用来定义或者声名一个接口
2. `implements` 用来声名某一个类实现了一个特定的接口

**接口声名属性**
```js
interface iPerson{
    name:string;
    age:number;
}
class person{
    constructor(public param:iPerson){
        console.log('用接口声名属性');
    }
}
var test=new person({
    name:'yangli',
    age:26
    });
```

**接口声名方法**
当一个类实现某个接口时，必须得实现这个接口里声名得方法
```js
interface Animal{
    eat();
}
class sheep implements Animal{
    eat(){
        console.log('im eat grass');
    }
}
class tigger implements Animal{
    eat(){
        console.log('im eat meat');
    }
}
```

##模块
关键字`module` `import`模块可以帮助开发者将代码分割为可重用得单元。开发者可以自己决定将模块中的哪些资源暴露出去供外部使用，哪些资源只在模块内使用。
和es6中的模块类似
例如
```js
import{xxx,xxx,xxx} from './a';
export var a;
export function fun1(){}
export class test{}
```

##注解
注解为程序的元素加上更直观更明了的说明，这些说明信息与程序的业务逻辑无关，而是提供指定的工具或者框架使用的说明
```js
@Component({
    selector:'app-root',
    templateUrl:'./app.component.html',
    styleUrls:['./app.component.css']
    })
```

##类型定义文件
类型定义文件用来帮助开发者在TypeScript中使用已有的JavaScript的工具包
例如JQuery
文件格式为`*.d.ts`
