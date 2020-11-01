## es6新增内容
### 变量名的改变（块级作用域）
- let 用来声明变量
- cosnt 用来声明常量
### 模版字符串（``）
- 字符串拼接用${}来界定
- 字符串韩航
- 字符串新增方法
``````
// 1. includes()返回布尔值：表示是否找到了参数字符
let str = 'hahay'
console.log(str.includes('y')) // true
// 2. repeat(): 获取字符串重复n次
let s = 'he'
console.log(s.repeat(3)) // 'hehehe'
// 3. startsWith()返回布尔值：表示参数字符串是否在源字符串的头部
console.log("lxy".startsWith('l'));//true
console.log("lxy".startsWith('x'));//false
// 4. endsWith()返回布尔值，表示参数字符串是否在源字符串的尾部
console.log(str.includes('x'));//true
console.log(str.includes('z'));//false
``````
### 函数
#### 箭头函数
- 不需要function关键字
- 可以省略return关键字
- 继承当前上面文this关键字
#### 函数这只默认参数
`````
const people = (name='liming') => { `boy ${name}`}
// 替代 下面写法
name = name || 'liming'
`````
### 对象
#### 键值对重命名简写
#### 对象字面两方法赋值简写
#### 提供浅拷贝方法
`````
const obj = Object.assign({}, objA, objB)
`````
#### 数据解构
#### 数据展开
### 数组
#### forEach()
#### map()
#### filter()
#### reduce()
#### some()
#### every()

### import 和export
### Promise（承诺）
