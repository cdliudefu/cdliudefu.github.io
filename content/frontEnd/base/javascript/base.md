### 一、JavaScript基础整理
1、JavaScript规定了8种语言类型：根据堆栈存储方式的不同可分为原始类型（简单类型）和复杂类型（对象类型）
- 简单类型（7种）：字符串(string)、数值(number)、布尔(boolean)、null、undefined、symbol(es6新增,表示独一无二)、大整数(BigInt,es2020引入)
- 复杂类型（1种）：object(Function、Array、Date、RegExp)

2、Symbol类型的应用：  
Symbol表示一个独一无二的值，如果我们使用了一个他人提供的对象，如果要扩展中给对象，那么必须保证属性名不能冲突，使用Symbol可以很好保证这点，Symbol与任何值都不相等。
```js
let s1 = Symbol('foo')
let s2 = Symbol('bar')
console.log(s1) // Symbol(foo)
console.log(s2)// Symbol(bar)
```
3、JavaScript中的变量在内存中的具体存储形式：
- 变量存储方式取决于变量的类型是基本类型还是引用类型
- 基本类型是键值对的方法存储在栈内存中
- 引用类型会在堆内存中开辟一块空间，存储这个对象的值，并同时在栈内存中存储变量和指向对象的指针或对象的地址

4、基本类型对应的内置对象，以及他们之间的装箱拆箱操作：  
  常见的基本类型对应的内置对象有：
  - 字符串string --> String对象
  - 数值number --> Number
  - 布尔boolean -->Boolean
  
  它们之间把数据类型转换为对应的引用类型的操作称为装箱，把引用类型转换为基本数据类型称为拆箱。  
  因为基本数据类型不是对象，所以没有方法，但是JS内部为我们完成了一系列处理（即装箱过程），使得它们能够调用方法，基本类型装箱实现机制如下：  
  1、创建String类型的实例  
  2、在实例上调用指定的方法  
  3、执行完后销毁这个实例
  ```js
  var s1 = new String('some text') // 创建实例对象
  var s2 = s1.substring(2) // 调用对象上的方法
  s1 = null // 销毁该对象
  ```
  拆箱：将引用类型对象转换为对应的值类型，它们通过引用类型的valueOf()或toString()方法来实现的，如果是自定义的对象，也可以自定义它们的valueOf()和toString()方法，实现对这个对象的拆箱，比如：
  ```js
  var objNum = new Number(123)
  var objStr = new String('123')

  console.log(typeOf objNum) // object
  console.log(typeof objStr) // object

  console.log(typeof objNum.valueOf()) //123 number
  console.log(typeof objNum.toString()) // '123' string
  console.log(typeof objStr.valueOf()) // '123' string
   console.log(typeof objStr.toString()) // '123' string
   ```
5、null和undefined:
- null表示空对象指针，将null赋值给变量，就表示该变量指向空对象
- undefined表示未定义，声明一个变量但不初始化，那么它的值就是undefined
- null主要表示一个变量还没有真正保存对象的时候，值为null，而undefined通常表示意料之外的内容，比如未初始化的变量

6、几种判断JavaScript数据类型的方式，以及它们的优缺点，如何准确的判断数组类型： 
1.  typeof操作符  
   可以判断基本数据类型，对于引用数据类型全部返回Object,但除函数外，typeof function()返回的是"function"
2. instanceof操作符  
   (obj instanceof Object) 检测object.prototype是否存在于参数obj的原型链上，主要用来判断变量是否是某个构造函数的实例，但是Object是所有对象的原型，所以在obj instanceof Object中，无论参数obj是数组还是函数都是返回true
3. constructor构造函数  
  constructor是prototype对象上的属性，指向构造函数，根据实例对象寻找属性的顺序，若实例对象上没有实例属性或方法，就会去原型链上寻找，因此，实例对象也可以使用constructor属性的，统一是这样也值能输出构造函数  
4. prototype原型对象上的方法

    总之：准确判断数组类型可以使用:
    ```js
    var a = "iamstring.";
    var b = 222;
    var c= [1,2,3];
    var d = new Date();
    var e = function(){alert(111);};
    var f = function(){this.name="22";};　
    　
    // typeof
    alert(typeof a) ------------> string
    alert(typeof b) ------------> number
    alert(typeof c) ------------> object
    alert(typeof d) ------------> object
    alert(typeof e) ------------> function
    alert(typeof f) ------------> function

    // instanceof
    alert(c instanceof Array) ---------> true
    alert(d instanceof Date)
    alert(f instanceof Function) ------> true
    alert(f instanceof function) ------> false

    // constructor
    alert(c.constructor === Array) ----> true
    alert(d.constructor === Date) ------> true
    alert(e.constructor === Function) ----> true

    // prototype
    alert(Object.prototype.toString.call(a) === ‘[object String]') --> true;
    alert(Object.prototype.toString.call(b) === ‘[object Number]') --> true;
    alert(Object.prototype.toString.call(c) === ‘[object Array]') --> true;
    alert(Object.prototype.toString.call(d) === ‘[object Date]') --> true;
    alert(Object.prototype.toString.call(e) === ‘[object Function]') --> true;
    alert(Object.prototype.toString.call(f) === ‘[object Function]') --> true;


    Array.isArray(obj)
    或
    Object.prototype.toString().call(obj) ==='[object Array]'
    ```





