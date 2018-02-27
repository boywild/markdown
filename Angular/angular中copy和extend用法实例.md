#angular.copy() 深拷贝
定义：
复制一个对象或者一个数组
1. 如果省略了destination，一个新的对象或数组将会被创建出来； 
2. 如果提供了destination，则source对象中的所有元素和属性都会被复制到destination中； 
3. 如果source不是对象或数组（例如是null或undefined）, 则返回source； 
4. 如果source和destination类型不一致，则会抛出异常。 注意：这个是单纯复制覆盖，不是类似继承。

使用方法：
```js
angular.copy(source, [destination]);
```

参数：
| 参数名称               | 参数类型     | 描述                                                       |
| -                      |              |                                                            |
| source                 | *            | 被copy的对象. 可以使任意类型, 包括null和undefined.         |
| destination (optional) | Object,array | copy去的目的地. 可以省略, 如果不省略, 其必须和source是同类 |


**source的值会完全把destination的值覆盖掉**
```js
angular.module('copyApp', [])
    .controller('CopyController', ['$scope', function($scope) {
         var source={
           name:'chentian',
           age:27,
           email:'666@qq.com',
           clothse:{
             clothse1:'aaa',
             clothse2:'bbb',
             clothse3:'ccc',
             clothse4:'ddd',
           }
         };
      var destination={
        money:'10000',
        house:'1',
        car:'2'
      };
      console.warn('destination',destination);
      var test=angular.copy(source,destination);
      console.warn('test',test);
      console.warn('source',source);
      console.warn('destination',destination);
    }]);
```

##angular.extend()
定义：
依次将第二个参数及后续的参数的第一层属性（不管是简单属性还是对象）拷贝赋给第一个参数的第一层属性，如果第一层属性是对象，则引用的是同一个对象，并返回第一个参数对象。

使用方法：
```js
angular.extend(destination, [source]);
```

实例一：`var r = angular.extend(b, a)`;将对象`a`的第一层属性（不管是简单属性还是对象）拷贝赋给对象`b`的第一层属性，如果是对象，则是引用的是同一个对象，并返回对象`b`
```js
var a = {
    name : 'bijian',
    address : 'shenzhen',
    family : {
        num : 6,
        amount : '80W'
    }
};
var b = {};

var r = angular.extend(b, a);
console.log('a:' + JSON.stringify(a));
console.log('b:' + JSON.stringify(b));
console.log('r:' + JSON.stringify(r));

b.address = 'hanzhou';
b.family.amount = '180W';
console.log('a:' + JSON.stringify(a));
console.log('b:' + JSON.stringify(b));
console.log('r:' + JSON.stringify(r));

//运行结果：

a:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"80W"}}
b:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"80W"}}
r:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"80W"}}

a:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"180W"}}
b:{"name":"bijian","address":"hanzhou","family":{"num":6,"amount":"180W"}}
r:{"name":"bijian","address":"hanzhou","family":{"num":6,"amount":"180W"}}
```

实例二：`var r = angular.extend(b, a, z)`;相继将对象`a、z`的第一层属性（不管是简单属性还是对象）拷贝赋给对象`b`的第一层属性，即如果是对象，则是引用的是同一个对象，并返回对象`b`

```js
var a = { name : 'bijian',
    address : 'shenzhen',
    family : {
        num : 6,
        amount : '80W'
    }
};
var z = {
    family : {
        amount : '150W',
        mainSource : '经营公司'
    }
};
var b = {};

var r = angular.extend(b, a, z);
console.log('a:' + JSON.stringify(a));
console.log('b:' + JSON.stringify(b));
console.log('r:' + JSON.stringify(r));
b.address = 'hanzhou';
b.family.amount = '180W';//这里需要注意的是，z作为最后一个参数b.family.amount会改变z的值，但不会改变a的值。如果将a作为最后一个参数，那么情况反之，z的值不会被修改，a的值会被修改
console.log('a:' + JSON.stringify(a));
console.log('b:' + JSON.stringify(b));
console.log('r:' + JSON.stringify(r));
//运行结果：

a:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"80W"}}
b:{"name":"bijian","address":"shenzhen","family":{"amount":"150W","mainSource":"经营公司"}} 
r:{"name":"bijian","address":"shenzhen","family":{"amount":"150W","mainSource":"经营公司"}} 

a:{"name":"bijian","address":"shenzhen","family":{"num":6,"amount":"80W"}} 
b:{"name":"bijian","address":"hanzhou","family":{"amount":"180W","mainSource":"经营公司"}}
r:{"name":"bijian","address":"hanzhou","family":{"amount":"180W","mainSource":"经营公司"}}
```


