# ES6-summary
Summary of es6 knowledge points

1.变量的解构赋值让变量声明变得简单   
 
2.字符串方法：includes()、startsWith()、endsWith()

```js
let str = 'hello world!'; 
console.log(str.includes('he'));  // true
console.log(str.startsWith('hell'));  // true
console.log(str.endsWith('ld!'))  // true
```
3.字符串补全：padStart()、padEnd()

```js
'x'.padStart(5, 'ab')  // 'ababx'
'x'.padStart(4, 'ab')  // 'abax'

// 如果省略第二个参数，默认使用空格补全长度。
'x'.padStart(4)  // '   x'
'x'.padEnd(4)  // 'x   '
```

4.Array.from(obj) 与 [].slice.call(obj)

5.扩展运算符...

```js
// 不用for和while关键字构造1-100的数组，并且顺序打印出1到100
function* genArr(a) {
  if (a <= 10) {
    yield a;
    a++;
    yield* genArr(a);
  }
}
let array = [...genArr(1)];
array.map(function(item) {
  console.log('=', item);
});
```

```js
// 扩展运算符替代数组的apply方法
// ES5的写法
Math.max.apply(null, [14, 3, 77])

// ES6的写法
Math.max(...[14, 3, 77])

// 等同于
Math.max(14, 3, 77);
```

6.rest参数：利用 rest 参数改写数组push方法

```js
function push(array, ...items) {
  items.forEach(function(item) {
    array.push(item);
    console.log(item);
  });
}

var a = [];
push(a, 1, 2, 3)
```

7.箭头函数：     

+ 函数体内的this对象，就是定义时所在的对象，而不是使用时所在的对象。

+ 不可以当作构造函数，也就是说，不可以使用new命令，否则会抛出一个错误。

+ 不可以使用arguments对象，该对象在函数体内不存在。如果要用，可以用Rest参数代替。

+ 不可以使用yield命令，因此箭头函数不能用作Generator函数。

上面四点中，第一点尤其值得注意。this对象的指向是可变的，但是在箭头函数中，它是固定的。

8.使用toString()方法来检测对象类型：

可以通过toString() 来获取每个对象的类型。为了每个对象都能通过 Object.prototype.toString() 来检测，需要以 Function.prototype.call() 或者 Function.prototype.apply() 的形式来调用，把需要检测的对象作为第一个参数传入。

```js
var toString = Object.prototype.toString;

toString.call(new Date); // [object Date]
toString.call(new String); // [object String]
toString.call(Math); // [object Math]

//Since JavaScript 1.8.5
toString.call(undefined); // [object Undefined]
toString.call(null); // [object Null]
```

9.Object.assign方法实行的是浅拷贝，而不是深拷贝。也就是说，如果源对象某个属性的值是对象，那么目标对象拷贝得到的是这个对象的引用。

```js
var obj1 = {a: {b: 1}};
var obj2 = Object.assign({}, obj1);

obj1.a.b = 2;
obj2.a.b // 2
```
上面代码中，源对象obj1的a属性的值是一个对象，Object.assign拷贝得到的是这个对象的引用。这个对象的任何变化，都会反映到目标对象上面。

10.对象的扩展方法：Object.keys() Object.values()、Object.entries()

```js
let {keys, values, entries} = Object;
let obj = { a: 1, b: 2, c: 3 };

for (let key of keys(obj)) {
  console.log(key); // 'a', 'b', 'c'
}

for (let value of values(obj)) {
  console.log(value); // 1, 2, 3
}

for (let [key, value] of entries(obj)) {
  console.log([key, value]); // ['a', 1], ['b', 2], ['c', 3]
}
```

11.可以将任何部署了Iterator接口的数据结构，转为数组。也就是说，只要某个数据结构部署了Iterator接口，就可以对它使用扩展运算符，将其转为数组。

```js
// 不用for和while关键字打印1到10
function* genArr(a) {
  if (a <= 10) {
    yield a;
    a++;
    yield* genArr(a);
  }
}
let array = [...genArr(1)];
array.map(function(item) {
  console.log('=', item);
});
```

12.yield*

```js
let generator = function* () {
  yield 1;
  yield* [2,3,4];
  yield 5;
};

var iterator = generator();

iterator.next() // { value: 1, done: false }
iterator.next() // { value: 2, done: false }
iterator.next() // { value: 3, done: false }
iterator.next() // { value: 4, done: false }
iterator.next() // { value: 5, done: false }
iterator.next() // { value: undefined, done: true }
```

13.一个数据结构只要部署了Symbol.iterator属性，就被视为具有iterator接口，就可以用for...of循环遍历它的成员。也就是说，for...of循环内部调用的是数据结构的Symbol.iterator方法。

14.遍历数组的方法   
以数组为例，JavaScript提供多种遍历语法。最原始的写法就是for循环。

```js
for (var index = 0; index < myArray.length; index++) {
  console.log(myArray[index]);
}
```
这种写法比较麻烦，因此数组提供内置的forEach方法。

```js
myArray.forEach(function (value) {
  console.log(value);
});
```
这种写法的问题在于，无法中途跳出forEach循环，break命令或return命令都不能奏效。

for...in循环可以遍历数组的键名。

```js
for (var index in myArray) {
  console.log(myArray[index]);
}
```

for...in循环有几个缺点。

- 数组的键名是数字，但是for...in循环是以字符串作为键名“0”、“1”、“2”等等。
- for...in循环不仅遍历数字键名，还会遍历手动添加的其他键，甚至包括原型链上的键。
- 某些情况下，for...in循环会以任意顺序遍历键名。

总之，for...in循环主要是为遍历对象而设计的，不适用于遍历数组。

for...of循环相比上面几种做法，有一些显著的优点。

```js
for (let value of myArray) {
  console.log(value);
}
```

- 有着同for...in一样的简洁语法，但是没有for...in那些缺点。
- 不同用于forEach方法，它可以与break、continue和return配合使用。
- 提供了遍历所有数据结构的统一操作接口。

下面是一个使用break语句，跳出for...of循环的例子。

```js
for (var n of fibonacci) {
  if (n > 1000)
    break;
  console.log(n);
}
```
上面的例子，会输出斐波纳契数列小于等于1000的项。如果当前项大于1000，就会使用break语句跳出for...of循环。

15.执行 Generator 函数会返回一个遍历器对象，也就是说，Generator 函数除了状态机，还是一个遍历器对象生成函数。返回的遍历器对象，可以依次遍历 Generator 函数内部的每一个状态。

16.yield语句与return语句既有相似之处，也有区别。相似之处在于，都能返回紧跟在语句后面的那个表达式的值。区别在于每次遇到yield，函数暂停执行，下一次再从该位置继续向后执行，而return语句不具备位置记忆的功能。一个函数里面，只能执行一次（或者说一个）return语句，但是可以执行多次（或者说多个）yield语句。正常函数只能返回一个值，因为只能执行一次return；Generator函数可以返回一系列的值，因为可以有任意多个yield。从另一个角度看，也可以说Generator生成了一系列的值，这也就是它的名称的来历（在英语中，generator这个词是“生成器”的意思）。

17.下面是一个利用Generator函数和for...of循环，实现斐波那契数列的例子。

```js
function* fibonacci() {
  let [prev, curr] = [0, 1];
  for (;;) {
    [prev, curr] = [curr, prev + curr];
    yield curr;
  }
}

for (let n of fibonacci()) {
  if (n > 1000) break;
  console.log(n);
}
```

18.Generator 函数可以暂停执行和恢复执行，这是它能封装异步任务的根本原因。除此之外，它还有两个特性，使它可以作为异步编程的完整解决方案：函数体内外的数据交换和错误处理机制。




