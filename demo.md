
> 变量提升：在当前作用域下，会把带 `var` 和 `function` 进行提前声明或定义  
> __注意：__ 函数提升要比变量提升的优先级要高，且不会被变量声明覆盖，但是会被变量赋值之后覆盖  
> __注意：__ 块级作用域只对 `let/const/function` 有效，对 `var` 无效  
> __注意：__ 变量没有 `var` 声明为全局变量，有 `var` 声明且是 `function` 则是 `function` 的私有变量

### 变量提升块级作用域题目一

```javascript

console.log(a)

var a = 0

if(true){

  console.log(a)
  a = 1
  console.log(a)
  function a() {}
  console.log(a)
  a = 21
  console.log(a)

}

console.log(a)

```



### 变量提升块级作用域题目二

```javascript


var a = 10
var b = 11
var c = 12

console.log(test)

function test(a){
  a= 1
  var b =2
  c=3
}

test(5)
console.log(a)
console.log(b)
console.log(c)

```


### 变量提升块级作用域题目三

```javascript

var n = 0

function a() {
  var n = 10
  function b() {
    n++
    console.log(n)
  }
  b()
  return b
}

var c = a()
c()
console.log(n)

```



### 变量提升块级作用域题目四

```javascript

var foo = 1

function bar() {

  if(!foo) {
    var foo = 10
  }
  console.log(foo)

}

bar()


```



### 变量提升块级作用域题目五

```javascript

console.log(a)
a = 12

function fn() {

  console.log(a)
  a = 13

}

fn()
console.log(a)

```




### 变量提升之新旧浏览器差异


```javascript

console.log(a)

var a = 0

if(true){

  console.log(a)
  function a() {}
  console.log(a)

}

console.log(a)

```





### 分析题目一

打印结果

```javascript

undefined
ƒ a(){}
1
1
21
1

```

```javascript

// 变量提升var a , function a
console.log(a)  // undefined

var a = 0

if(true){

  console.log(a)  // 块级作用域function a提升
  a = 1
  console.log(a)  // 1
  function a() {} // 把a之前的变化，映射全局a = 1
  console.log(a)  // 1
  a = 21  // 私有，未映射到全局
  console.log(a)  // 21

}

console.log(a)  // 1

```

### 分析题目二

打印结果

```javascript

ƒ test(a) {
  a = 1
  var b =2
  c = 3
}

10
11
3

```

```javascript

// 变量提升：var a ，function test() {}
var a = 10
var b = 11
var c = 12

console.log(test) // 没有块级作用域，声明并定义

function test(a) {
  a = 1 // 传入的形参为私有变量，不会影响公有
  var b = 2 // var声明function的私有变量
  c = 3 // 没有var，全局
}

test(5)
console.log(a)
console.log(b)
console.log(c)

```


### 分析题目三

打印结果

```javascript

11
12
0

```

```javascript

// 变量提升
// var n ; var c ; function a() {}

var n = 0

function a() {

  // 私有作用域， var n ,function b() {}
  var n = 10
  function b() {
    n++   // 操作自己上级的作用域，不操作全局
    console.log(n)
  }
  b()
  return b

}

var c = a()
c() // 相当于调用b函数
console.log(n)  // 全局变量不受影响，function a操作的是 `var n = 10` 这里的私有变量

```

### 分析题目四

// 打印结果
// 10


> 分析：私有作用域中的 `if` 块级作用域中 `var foo` 变量提升，此时的foo为 `undefined` ，`!foo为true`，符合条件 `foo = 10` ,结果打印 `10`


### 分析题目五

> 结果报错：`a is not defined`


### 分析变量提升之新旧浏览器差异

> Tips: 变量提升之新旧浏览器差异，旧版本无视块级作用域

新版本打印

```javascript

undefined
function a() {}
function a() {}
function a() {}

```

IE10及以下打印结果

```javascript

function a() {}
0
0
0

```