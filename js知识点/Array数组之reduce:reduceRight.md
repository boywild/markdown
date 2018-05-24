# Array数组之reduce/reduceRight

# reduce
`reduce()`方法对累加器和数组中的每个元素（从左到右）应用一个函数，将其减少为单个值。

##语法
###`arr.reduce(callback[, initialValue])`

`callback`
执行数组中每个值的函数，包含四个参数：

1. `accumulator`-累加器累加回调的返回值; 它是上一次调用回调时返回的累积值，或initialValue（如下所示）。
2. `currentValue`-数组中正在处理的元素
3. `currentIndex`(可选)-数组中正在处理的当前元素的索引。 如果提供了initialValue，则索引号为0，否则为索引为1。
4. `array`-调用reduce的数组
5. `initialValue`(可选)-用作第一个调用 callback的第一个参数的值。 如果没有提供初始值，则将使用数组中的第一个元素。 在没有初始值的空数组上调用 reduce 将报错。


##作用
`reduce`为数组中的每一个元素依次执行callback函数，不包括数组中被删除或从未被赋值的元素，接受四个参数：

1. `accumulator`
2. `currentValue` 
3. `currentIndex`
4. `array`

回调函数第一次执行时，`accumulator`和`currentValue`的取值有两种情况：调用`reduce`时提供`initialValue`，`accumulator`取值为`initialValue`，`currentValue`取数组中的第一个值；没有提供 `initialValue`，`accumulator`取数组中的第一个值，`currentValue`取数组中的第二个值
如果数组为空且没有提供`initialValue`，会抛出`TypeError` 。如果数组仅有一个元素（无论位置如何）并且没有提供`initialValue`， 或者有提供`initialValue`但是数组为空，那么此唯一值将被返回并且`callback`不会被执行


##例子
一、求和

```js
var sum = [0, 1, 2, 3].reduce(function (a, b) {
  return a + b;
}, 0);
```

二、将二维数组转化为一维

```js
var flattened = [[0, 1], [2, 3], [4, 5]].reduce(
  function(a, b) {
    return a.concat(b);
  },
  []
);
// flattened is [0, 1, 2, 3, 4, 5]
```

三、计算数组中每个元素出现的次数

```js
var names = ['Alice', 'Bob', 'Tiff', 'Bruce', 'Alice'];

var countedNames = names.reduce(function (allNames, name) { 
  if (name in allNames) {
    allNames[name]++;
  }
  else {
    allNames[name] = 1;
  }
  return allNames;
}, {});
// countedNames is:
// { 'Alice': 2, 'Bob': 1, 'Tiff': 1, 'Bruce': 1 }
```

四、数组去重

```js
let arr = [1,2,1,2,3,5,4,5,3,4,4,4,4];
let result = arr.sort().reduce((init, current)=>{
    if(init.length===0 || init[init.length-1]!==current){
        init.push(current);
    }
    return init;
}, []);
console.log(result); //[1,2,3,4,5]
```

五、使用扩展运算符和initialValue绑定包含在对象数组中的数组

```js
// friends - an array of objects 
// where object field "books" - list of favorite books 
var friends = [{
  name: 'Anna',
  books: ['Bible', 'Harry Potter'],
  age: 21
}, {
  name: 'Bob',
  books: ['War and peace', 'Romeo and Juliet'],
  age: 26
}, {
  name: 'Alice',
  books: ['The Lord of the Rings', 'The Shining'],
  age: 18
}];

// allbooks - list which will contain all friends' books +  
// additional list contained in initialValue
var allbooks = friends.reduce(function(prev, curr) {
  return [...prev, ...curr.books];
}, ['Alphabet']);

// allbooks = [
//   'Alphabet', 'Bible', 'Harry Potter', 'War and peace', 
//   'Romeo and Juliet', 'The Lord of the Rings',
//   'The Shining'
// ]
```

# ReduceRight

`Array.prototype.reduce()`的执行方向相反。语法和参数和`reduce`一样，只是执行的方向不一样，`reduce`从左到右，`reduceRight`从右到左

## 例子

一、求一个数组中所有值的和

```js
var total = [0, 1, 2, 3].reduceRight(function(a, b) {
    return a + b;
});
// total == 6
```

二、扁平化（`flatten`）一个元素为数组的数组

```js
var flattened = [[0, 1], [2, 3], [4, 5]].reduceRight(function(a, b) {
    return a.concat(b);
}, []);
// flattened is [4, 5, 2, 3, 0, 1]
```

三、`reduce`与`reduceRight`之间的区别

```js
var a = ['1', '2', '3', '4', '5']; 
var left  = a.reduce(function(prev, cur)      { return prev + cur; }); 
var right = a.reduceRight(function(prev, cur) { return prev + cur; }); 

console.log(left);  // "12345"
console.log(right); // "54321"
```