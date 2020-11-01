### 数据类型
#### 基本类型
- undefined
- null
- string
- number
- boolean
- symbol(es6)
#### 引用数据类型
- object

### 定义函数的方法
- 函数声明表达式
`````
function a(){}
`````
- function 操作符
`````
const a = fucntion(){}
`````
- Funtion 构造函数
- ES6: arrow function
``````
()=>{}
``````

### 类数组=>数组
``````
// 最经典
var arr = [].slice.call(arguments)
// es6
var arr = Array.from(arguments);
var args = [...arguments];
// jquery
var arr = $.makeArray(arguments);
``````