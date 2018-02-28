## 参数传递
ECMAScript中所有函数的参数都是按值传递的。也就是说，把函数外部的值复制给函数内部的参数，就和把值从一个变量复制到另一个变量一样。基本类型值的传递如同基本类型变量的复制一样，而引用类型值的传递，则如同引用类型变量的复制一样，这个时候的值是地址拷贝（或者说是副本）。**故参数只能按值传递**。

在向参数传递基本类型的值的时候，被传递的值会被赋值给一个局部变量（即命名参数，或者**用ECMAScript的概念来说，就是arguments对象中的一个元素**）。在向参数传递引用类型的值时，会把这个值在内存中的地址赋值给一个局部变量，因此这个局部变量的变化会反映在函数的外部。**参数实际上是函数的局部变量，可以把ECMAScript函数的参数想象成局部变量**

1. 原始类型示例如下：
```js
function addTen(num){
    num += 10;
    return num;
}

var count = 20;
var result = addTen(count);
console.log(count);   //20, 没有变化
console.log(result); //30
```
上面代码可改装为：
```js
function addTen(num){
    var num = arguments[0];
    num += 10;
    return num;
}

var count = 20;
var result = addTen(count);
console.log(count);   //20, 没有变化
console.log(result); //30
```

2. 引用类型示例一如下： 
```JS
function setName(obj){
    obj.name = 'Nicholas';
}

var person = new Object();
setName(person);
console.log(person.name);  //  'Nicholas'
```
上面代码可改装为：
```JS
function setName(obj){
    var obj = arguments[0];
    obj.name = 'Nicholas';
}

var person = new Object();
setName(person);
console.log(person.name);  //  'Nicholas'
```
3. 引用类型示例二如下：
```js
function setName(obj){
    obj.name = 'Nicholas';
    obj = new Object();
    obj.name = 'Greg';
}

var person = new Object();

setName(person);
console.log(person.name); //  'Nicholas'
```
上面代码可改装为：
```js
function setName(obj){
    var obj = arguments[0];
    obj.name = 'Nicholas';
    obj = new Object();
    obj.name = 'Greg';
}

var person = new Object();
setName(person);
console.log(person.name); //  'Nicholas'
```
即使在函数内部修改了参数的值，但是原始的引用仍然保持未变。实际上，当在函数内部重写obj时，这个变量引用的就是一个局部变量了。而这个局部变量会在函数执行完毕后立即被销毁。
4. 综合类型示例如下：
```js
var count = 1;
var obj1 = {count:10};

incNumber(count)
console.log(count); // 1

incObject(obj1);
console.log(obj1.count); // 11

var obj2 = {count};  //相当于var obj2 = {count: count}，即var obj2 = {count: 1}
incObject(obj2);
console.log(obj2.count);  // 2
console.log(count);  // 1

function incNumber(count){
    return ++count;
}

function incObject(obj){
    obj.count++;
}
```

上面代码可改装为：
```js
var count = 1;
var obj1 = {count:10};

incNumber(count)
console.log(count); // 1

incObject(obj1);
console.log(obj1.count); // 11

var obj2 = {count};  
incObject(obj2);
console.log(obj2.count);  // 2
console.log(count);  // 1

function incNumber(count){
    var count = arguments[0]; // count里面存的是内容，拷贝的也是内容
    return ++count;
}

function incObject(obj){
    var obj = arguments[0]; //obj里面存储的是地址，拷贝的也是地址，操作的是同一地址指向的那片区域
    obj.count++;
}
```


## 参考阅读
1. [JavaScript高级程序设计-第四章>传递参数>70页]()
2. [引用类型
](https://xiedaimala.com/courses/b8b4c00c-6798-4caf-8bfe-ba9fbb4c6d3d/tasks/b5f1b753-cfe0-4a1b-b57a-4c60414bf2f0)
3. [求值策略(Evaluation strategy)](http://www.cnblogs.com/TomXu/archive/2012/02/08/2341439.html)
