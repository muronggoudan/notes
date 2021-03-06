# :thinking:浏览器中的JavaScript执行机制

[[toc]]

## 主要执行流程
### 编译阶段
一段JS代码通过编译会生成两部分内容：执行上下文（Execution context）和可执行代码。
* 执行上下文：执行上下文是JavaScript执行一段代码时的运行环境，比如调用一个函数，就会进入这个函数的执 行上下文，确定该函数在执行期间用到的诸如this、变量、对象以及函数等。

### 执行上下文vs作用域

#### 编译过程中的变量提升现象

所谓的变量提升，是指在JavaScript代码执行过程中，JavaScript引擎把变量的声明部分和函数的声明部分提 升到代码开头的“行为”。变量被提升后，会给变量设置默认值，这个默认值就是我们熟悉的undefined。实际上变量和函数声明在代码里的位置是不会改变的，而且是在编译阶段被JavaScript引擎放入内存中。

### 函数表达式vs函数声明

#### 变量提升的部分规则

var的创建和初始化被提升，赋值不会被提升。
let的创建被提升，初始化和赋值不会被提升。
function的创建、初始化和赋值均会被提升。

#### 暂时性死区

暂时性死区是语法规定的，也就是说虽然通过let声明的变量已经在词法环境中了，但是在没有赋值之前，访问该变量JavaScript引擎就会抛出一个错误。

### 编译过程中的this指向问题
4种指向

（this指向是编译过程中确定还是执行过程中确定?）

## 执行阶段

JavaScript引擎会从变量环境中去查找自定义的变量和函数。

**执行过程中函数的执行** 

一段代码的执行过程：
1. 每调用一个函数，JavaScript引擎会为其创建执行上下文，并把该执行上下文压入调用栈，然后JavaScript引擎开始执行函数代码；
2. 如果在一个函数A中调用了另外一个函数B，那么JavaScript引擎会为B函数创建执行上下文，并将B函数的执行上下文压入栈顶；
3. 当前函数执行完毕后，JavaScript引擎会将该函数的执行上下文弹出栈；
4. 当分配的调用栈空间被占满时，会引发“堆栈溢出”问题。

栈溢出有哪些规避措施：
>1.递归调用的形式改造成其他形式（循环）；   
>2.不入栈，使用异步队列，使用setTimeout或者rAF；   
>3.尾递归优化   

# 块级作用域

ES6引入了 let和const关键字，由这两个关键字声明的变量，将会只在所在的块内可访问。

## 内存模型

在创建执行上下文的过程中，将会创建：
* 变量环境Variable Environment：可以看做是对象结构；
* 词法环境Lexical Environment：栈结构，栈底是函数最外层的变量，进入一个作用域块后，就会把该作用域块 内部的变量压到栈顶；当作用域执行完成之后，该作用域的信息就会从栈顶弹出，这就是词法环境的结构。

变量具体查找方式是：沿着词法环境的栈顶向下查询，如果在词法环境中的某个块中查找到了，就直接返回给JavaScript引擎，如果没有查找到，那么继续在变量环境中查找。

# 作用域链
在创建每个执行上下文的变量环境中，都包含了一个外部引用，用来指向外部的执行上下文，我们把这个外部引用 称为outero变量的查找规则就是在执行上下文内寻找，如果找不到将会沿着outer作用域链到外部执行上下文中寻找，直到查找到全局作用域。**需要注意的是作用域链根据是词法作用域规则确定的。**

## 词法作用域
词法作用域就是指作用域是由代码中函数声明的位置来决定的，所以词法作用域是静态的作用域，通过它就能够预测代码在执行过程中如何查找标识符。
也就是说，词法作用域是代码编译阶段就狭定好的，和函数是怎么调用的没有关系。

# 闭包
在JavaScript中，根据词法作用域的规则，内部函数总是可以访问其外部函数中声明的变量，当通过调用一个外部函数返回一个内部函数后，即使该外部函数已经执行结束了，但是内部函数引用外部函数的变量依然保存在内存中，我们就把这些变量的集合称为闭包。比如外部函数是foo，那么这些变量的集合就称为foo函数的闭包。

>老师提出的概念:内部函数引用外部函数的变量的集合。   
>高级程序设计中的概念：闭包是指有权访问另一个函数作用域中的变量的函数。   
>MDN上的概念：闭包是函数和声明该函数的词法环境的组合。   
