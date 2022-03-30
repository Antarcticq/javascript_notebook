[TOC]
##  js基本数据类型哪几个？引用类型有哪些？

##### 基本数据类型：

null, undefined, number, boolean, string, symbol(ES6)，bigint(ES10)；存储在栈中 

##### 引用数据类型：

引用类型统称为 object 类型，细分的话有：Object 类型、Array 类型、Date 类型、RegExp 类型、Function 类型，特殊的基本包装类型(String、Number、Boolean)以及单体内置对象(Global、Math)；存储在堆中 

JS的字符串是不能更改的，最大长度2^53 - 1

```javascript
1. typeof 可判断undefined, number, boolean, string, symbol, function且返回值为对应字符串；null, Array, object返回值为Object
2. new Number(2) instanceOf Number; 不能判断null和undefined；返回值为bool类型
3. Object.prototype.toString.call(obj) === '[object Object]' 都可判别
```



## JS中基本数据类型和引用类型在内存上有什么区别？

![](pics/内存图.png)

1. **基本类型** 基本数据类型变量保存在栈（stack）中,它们的值直接存储在变量访问的位置。这是因为这些原始类型占据的空间是固定的，所以可将它们存储在较小的内存区域 – 栈中。这样存储便于迅速查寻变量的值。
2. **引用类型** javascript 的引用数据类型是同时保存在栈内存和堆内存中的对象。与其它语言的不同是，你不可以直接访问堆内存空间中的位置和操作堆内存空间。只能操作对象在栈内存中的引用地址。准确地说，引用类型的存储需要内存的栈区和堆区（堆区是指内存里的堆内存）共同完成，栈区内存保存变量标识符和指向堆内存中该对象的指针，也可以说是该对象在堆内存的地址。由于引用值的大小会改变，所以不能把它放在栈中，否则会降低变量查寻的速度。



### **复制值**

**基本类型** 在将一个保存着原始值的变量复制给另一个变量时，会将原始值的副本赋值给新变量，此后这两个变量是完全独立的，它们只是拥有相同的 value 而已。

很显然，a 不全等 b

**引用类型** 在将一个保存着对象内存地址的变量复制给另一个变量时，会把这个内存地址赋值给新变量，也就是说这两个变量都指向了堆内存中的同一个对象，它们中任何一个作出的改变都会反映在另一个身上。（这里要理解的一点就是，复制对象时并不会在堆内存中新生成一个一模一样的对象，只是多了一个保存指向这个对象指针的变量罢了）。多了一个指针

结果然显然，a 全等 b，因为它们的指针指向同一个堆内存

### **传递值**

JS 高级程序设计—> 4.1.3 中提到: “ECMAScript 中所有函数的参数都是按值传递的” 结论：没有差别



## const、let和var的区别

![](pics/变量区别.jpg)



#### 简述：

1. var声明变量存在变量提升，let和const不存在变量提升
2. let、const都是块级局部变量
3. 同一作用域下let和const不能声明同名变量，而var可以

#### 详述：

1. var 定义**变量**，没有块的概念，可以跨块访问，但不能跨函数访问，不初始化会出现undefined，不会报错；有变量提升。
2. let定义**变量**，只能在当前块级作用域里访问，也不能跨函数访问，对函数外部无影响；无变量提升。
3. const定义**常量**，只能在当前块级作用域里访问，也不能跨函数访问，使用时**必须初始化**(即必须赋值)，而且不能修改(引用数据类型，如指向的对象，即内存地址不能修改，但是能修改里面的属性)；无变量提升。

#### 块级作用域

在ES6 之前, JavaScript中有三种作用域:

**1.** 全局作用域

**2.** 函数作用域

**3.** eval作用域

以上作用域内声明的变量或方法只在当前作用域内有效, 在其他作用域内引用则会返回 undefined;

而ES6则新增了一个作用域: **块级作用域**

块级作用域可以简单理解为是: 包在大括号{}里面的内容, 它可以自成一个作用域, 但ES5中也有大括号, 可ES5中并没有块级作用域, 这时该怎样判断 {} 是否具有块级作用域的特点?

这时就得用到 **let** 和 **const**

因此, 可以将块级作用域理解为: 使用let和const声明的变量, 只在当前大阔号内生效, 由此构建出了 **块级作用域** 这么个东西.

这里的 "大括号内" 主要指的下面几种情况: 

```js
// 条件语句
if () {}

// switch语句
switch () {}

// for / while循环语句
for () {}
while () {}

// try...catch语句
try () catch (err) {}

// 单大括号
{}

```

注意: 对象的大括号内不是一个块级作用域, 因为它里面不能直接声明变量; 



##  为什么0.1+0.2！=0.3 

在小数点运算时，JavaScript将隐式的采取[IEEE754二进制浮点运算](https://link.zhihu.com/?target=https%3A//www.h-schmidt.net/FloatConverter/IEEE754.html)。而不是我们想象中的十进制运算。而十进制和二进制转换时，就可能出现精度丢失的问题。



那么应该怎样来解决0.1+0.2等于0.3呢? 最好的方法是设置一个误差范围值，通常称为”机器精度“，而对于Javascript来说，这个值通常是2^-52,而在ES6中，已经为我们提供了这样一个

属性：Number.EPSILON，而这个值正等于2^-52。这个值非常非常小，在底层计算机已经帮我们运算好，并且无限接近0，但不等于0,。这个时候我们只要判断Math.abs((0.1+0.2)-0.3)小于`Number.EPSILON`，在这个误差的范围内就可以判定0.1+0.2===0.3为true。

```js
function numbersequal(a,b){ 
    return Math.abs(a-b)<Number.EPSILON;
} 
var a=0.1+0.2， b=0.3;
console.log(numbersequal(a,b)); //true
```



## null和undefined的区别？

**null**表示“没有对象”，null值是一个空对象指针，既该处不应该有值。

  （1）作为函数的参数，表示该函数的参数不是对象。

  （2）作为对象原型链的终点。

​    object.getPrototypeOf(Object.prototype)

**undefind**表示“缺少值”，就是此处应该有一个值，但是没有定义。

   （1）变量被声明了，但是没有赋值时，就等undefined。

​        var i；  i // undefined

   （2）调用函数是，应该提供方的参数没有提供，该参数等于undefined。

​        

​        function f(x){ console.log(x) }

​          f() //undefined

   （3）对象没有赋值的属性，该属性的值为undefined。

​        var 0 = new Object();

​        0.p //undefined

   （4）函数没有返回值时，默认返回undefined。

​        var x = f()  x //undefined



## 比较typeof,instanceof,toString，valueOf 四种类型检测的区别

**解题思路:**

7种内置类型：Boolean、Null、Undefined、Number、String、Symbol

 (ECMAScript 6 新定义)和Object，除 Object 以外的所有类型都是不可变的（值本身无法被改变）

**一、typeof**

typeof操作符返回一个字符串,表示未经求值的操作数(unevaluated operand)的类型

**二、toString**

可以通过使用toString.call(obj)来检测对象类型。

可以用来检测Object、Number、Array、Date、Function、String、Boolean、Error和RegExp。

**三、instanceof**

instanceof 运算符可以用来判断某个构造函数的 prototype 属性是否存在另外一个要检测对象的原型链上，返回boolean值。

**四、valueOf**

1.如果存在任意原始值，它就默认将对象转换为表示它的原始值

2.对象是复合值，而大多数对象无法真正表示为一个原始值，因此默认的valueOf()方法简单地返回对象本身，而不是返回一个原始值



## == 和 === 的区别

```
==用于一般比较，===用于严格比较;
==在比较的时候可以转换数据类型，===严格比较，只要类型不匹配就返回flase。
```

==如果是基本数据类型类型之间的比较，会转换成数值类型进行比较，如果是存在引用类型，会先将引用类型转换原始类型，然后再进行比较。这里就涉及到另外两个函数`toString` 和`valueOf` 。

=== 比较运算符相比较`==` 运算符,不会做隐式转换，也就是不会自动调用`toString` `valueof` 方法。他会首先判断类型是否一致，然后在做比较



## 原型、原型链、作用域、作用域链

### 什么是作用域、作用域链？

**作用域概念**：在某一区域执行Js代码时，需对变量或函数的值进行访问，而这一区域又提供对变量或函数的查找，并确定是否可以访问，则称该区域为作用域

1、全局作用域

> 除了函数中定义的变量之外，都是全局作用域。

```js
var a = 10;
function bar(){
    console.log(a);
}
bar();//10
```

以上的a就是全局变量，到处可以访问a。 然鹅，

```js
var a = 10;
function bar(){
    console.log(a);
    var a = 20;
}
bar();//undefined
```

什么鬼？undefined？

*是的，你没看错。因为先搜索函数的变量看是否存在a，存在，又由于a被预解析（变量提升），提升的a绑定了这里的a作用域，所以结果就是undefined。*

2、局部作用域

> 函数里用var声明的变量。

```js
var a = 10;
function bar(){
   var a  = 20;
    console.log(a);
}
bar();//20
```

3、没有块级作用域（至ES5），ES6中有块级作用域

**作用域链概念**：变量取值会到创建这个变量的函数的作用域中取值，如果找不到，就会向上级作用域去查，直到查到全局作用域，这么一个查找过程形成的链条就叫做作用域链。



### 原型、原型链

**原型**

在JavaScript中，每当定义一个函数数据类型(普通函数、类)时候，都会天生自带一个prototype属性，这个属性指向函数的原型对象。上面定义的属性和方法可以被对象实例**共享**。

**原型链**

1.`__proto__`和constructor

（Chrome、Safari、Firefox）每一个对象数据类型(普通的对象、实例、prototype......)也天生自带一个属性`__proto__`，属性值是当前实例所属类的原型(prototype)。原型对象中有一个属性**constructor**, 它指向函数对象。

> **but  `__proto__` 不是实例的属性，也不是构造函数的属性，在大多数的浏览器中都支持这种非正式的访问方式。实际上 `__proto__` 来自 `Object.prototype`，当使用 `obj.__proto__` 时，可以理解成返回了 `Object.getPrototypeOf(obj)`**

![](pics/原型图.png)



在JavaScript中万物都是对象，对象和对象之间也有关系，并不是孤立存在的。对象之间的继承关系，在JavaScript中是通过prototype对象指向父类对象，直到指向Object对象为止，这样就形成了一个原型指向的链条，专业术语称之为**原型链**。

当我们访问对象的一个属性或方法时，它会先在对象自身中寻找，如果有则直接使用，如果没有则会去原型对象中寻找，如果找到则直接使用。如果没有则去原型的原型中寻找,直到找到Object对象的原型，Object对象的原型没有原型，如果在Object原型中依然没有找到，则返回undefined。

![](pics/原型链.png)

据类型的基类(最顶层的类)在Object.prototype上没有__proto__这个属性。

```js
console.log(Object.prototype.__proto__ === null) // true
```



##  JS 中数组、字符串和Object的常用方法

### 数组的常用方法 

#### **往数组里增加项**

`array.push(value, ...)` 给数组最后添加一个或多个元素

- 参数：`value`，要添加到 `array` 尾部的值，可以是一个或多个
- 返回值：把指定的值添加到数组后数组的新长度

`array.unshift(value, ...)` 给数组最前面添加一个或多个元素

- 参数：`value`，要插入数组头部的一个或多个值
- 返回值：把指定的值添加到数组后数组的新长度

#### **从数组里删除项**

`array.pop()` 删除并返回数组的最后一个元素

- 参数：无
- 返回值：`array` 的最后一个元素
- 方法 `pop()` 将删除 `array` 的最后一个元素，把数组长度减 `1`，并且返回它删除的元素的值。
- 如果数组已经为空，则 `pop()` 不改变数组，返回 `undefined`

`array.shift()` 将数组中的第一个元素移出数组

- 参数：无
- 返回值：数组原来的第一个元素
- 方法 `shift()` 将把 `array` 的第一个元素移出数组，返回那个元素的值，并且将余下的所有元素前移一位，以填补数组头部的空缺。
- 如果数组是空的，`shift()` 将不进行任何操作，返回 `undefined`
- 方法 `shift()` 和 方法 `pop()` 相似，只不过它在数组头部操作，而不是在尾部操作。该方法常常和 `unshift()` 一起使用

#### **更改数组项**

`array.reverse()` 颠倒数组中元素的顺序

- `array` 对象的方法 `reverse()` 将颠倒数组中元素的位置。
- 它在原数组上实现这一操作，即重排指定的 `array` 元素，但并不创建新数组。
- 如果对 `array` 有多个引用，那么通过所有引用都可以看到数组元素的新顺序。

`array.sort(function)` 对数组元素进行排序

- 参数：`function` 可以控制数字排序
- 返回值：对数组的引用。
- 注意：数组在原数组上进行排序，不制作副本
- 如果调用方法 `sort()` 时没有使用参数，将按字母顺序（按照字符编码的顺序）对数组中的元素进行排序

#### **查询数组项**

- `indexOf()`
- `lastIndexOf()`
- 这两个属性可以参照字符串的定义和介绍
- includes()
- find()
- findIndex()

#### 迭代器方法

- keys()
- values()
- entries()

#### **遍历数组**

```
array.forEach(item[, thisObject])
```

- 参数：
  - `item`：函数参数数组中每个元素
  - `thisObject`：对象作为该执行回调时使用
- 返回值：返回创建数组

```
array.map(item[, thisObject])
```

- 方法返回一个由原数组中的每个元素调用一个制定方法后的返回值组成的新数组

forEach 和 map 的区别

> `forEach` 是遍历，而 `map` 是映射

#### **截取数组值**

`array.slice(start, end)` 返回数组的一部分

- 参数：
  - `start`：数组片段开始处的数组下标。如果是负数，它声明从数组尾部开始算起的位置
  - `end`：数组片段结束处的后一个元素的数组下标
  - 如果没有指定 `end`，则默认包含从 `start` 开始到数组结束的所有元素。
- 返回值：一个新数组，包含从 `start` 到 `end` (不包括该元素)指定的 `array` 元素
- **注意**：不改变原数组！如果想删除数组中的一段元素，应该使用 `array.splice`

`array.splice(start, length, value, ...)` 插入、删除或替换数组中的元素

- 参数：
  - `start`：开始插入或删除的数组元素的下标
  - `length`：从 `start` 开始，包括 `start` 所指的元素在内要删除的元素个数。如果没有指定 `length`，`splice()` 将删除从 `start` 开始到原数组结尾的所有元素。
  - `value`：要插入数组的零个或多个值，从 `start` 所指的下标处开始插入。
- 返回值：如果从 `array` 中删除了元素，返回的是被删除元素的数组。

#### 其他数组方法

`array.concat(value, ...)` 拼接数组

- 参数：`value，...，` 要增加到 `array` 中的值，可以是任意多个
- 返回值：一个新数组
- 方法 `concat()` 将创建并返回一个新数组，这个数组是将所有参数添加到 `array` 中生成的。
- 不修改原数组 `array`
- **如果要进行 `concat()` 操作的参数是一个数组，那么添加的是数组中的元素，而不是数组。**

`array.join(separator)` 将数组元素连接起来以构建一个字符串

- 参数：`separator`，在返回的字符串中用于分割数组元素的字符或字符串，它是可选的。如果省略了这个参数，用逗号作为分隔符。
- 返回值：一个字符串，通过把 `array` 的每个元素转换成字符串，然后把这些字符串连接起来，在两个元素之间插入 `separator` 字符串而生成。

批量复制方法**copyWithin()**

填充数组方法**fill()**

**toLocalString()、toString()、valueOf()**

#### 迭代方法

**Array.filter()**方法在数组中查找**满足特定条件的所有元素**。返回的新数组，如果数组中没有项目符合条件，则返回一个空数组。

![img](https:////upload-images.jianshu.io/upload_images/20308335-e448b61017232477.png?imageMogr2/auto-orient/strip|imageView2/2/w/546/format/webp)

a、newArray是返回的新数组。

b、array 是我们要进行查找的数组本身。

c、callback 是应用于数组每个元素的回调函数。

**every() 方法**用于检测数组**所有元素是否都符合指定条件**。如果数组中检测到有一个元素不满足，则整个表达式返回*false*，且剩余的元素不会再进行检测。如果所有元素都满足条件，则返回 true。 every() 不会对空数组进行检测。every() 不会改变原始数组。

#### **归并方法**

reduce（）

reduceRight（）



### 2、Javscript字符串的常用方法 

#### **1）增**

除了常用的+以及${}进行字符串拼接之外，还可以通过contact()方法用于讲一个或多个字符串拼接成一个新字符串。

#### **2）删：substr()、slice()、substring()**

***string\*.substr(\*start\*,\*length\*) 方法**可在字符串中抽取从 ***开始*****下标开始的指定数目**的字符。*length*子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

**slice(start, end) 方法**可提取字符串的某个部分，并以新的字符串返回被提取的部分。使用 start（包含） 和 **end（不包含）参数**来指定字符串提取的部分。

**substring(from, to) 方法**用于提取字符串中介于两个指定下标之间的字符。返回的子串包括*开始*处的字符，**但不包括\*结束\* 处**的字符。

#### **3）改：不改变原有字符串，创建一个字符串副本**

**trim()、trimLeft()、trimRight() 方法**用于删除前、后或前后所有空格符。

**repeat() 方法**表示将字符串复制多少次，然后返回。接受一个整数参数。

**toLowerCase()、toUpperCase()****方法**用于大小写转换。

#### **4）查：charAt()、indexOf()、startWith()、includes()**

**charAt() 方法**返回给定索引位置的字符。需传入一个整数参数。

**indexOf() 方法**返回某个字符在字符串中的位置。需传入一个字符。

**startWith() 方法**返回一个布尔值，判断字符串是否以传入的字符开头。

**includes() 方法**返回一个布尔值，判断字符串是否包含传入的字符。

#### **5）转换方法：split()**

**split() 方法**把字符串按照指定的分隔符，返回一个数组。

#### **6）匹配方法：match()、search()、replace()**

**match() 方法**接受一个参数（正则表达式字符串或者正则表达式对象）。返回是一个数组。

**search() 方法**接受一个参数（正则表达式字符串或者正则表达式对象）。返回是布尔值。

**replace() 方法**接受两个参数，第一个是匹配的内容，第二个是替换的元素。替换的是第一次匹配到的。

### 如何去除字符串中的最后一个字符？ 

有三种方法: **str.slice(0,str.length-1)、substr(0,length-1)、str.substring(0,str.length-1) 
**

### **js 获取字符串最后一个字符？**

有三种：**str.charAt(str.length-1)、str.substr(str.length-1,1)、 str.substring(str.length-1) 、str.slice(str.length-1)、let res = str.split("")；res[str.length - 1]。**

#### 1）slice(start,end)

start : 要抽取的片断的起始下标。如果是负数，则该参数规定的是从字符串的尾部开始算起的位置。

end:要抽取的片段的结尾的下标。若未指定此参数，则要提取的子串包括 start 到原字符串结尾的字符串。如果该参数是负数，那么它规定的是从字符串的尾部开始算起的位置。

#### 2）substr(start,length) 

start : 必需。要抽取的子串的起始下标。必须是数值。如果是负数，那么该参数声明从字符串的尾部开始算起的位置。

length : 可选。子串中的字符数。必须是数值。如果省略了该参数，那么返回从 stringObject 的开始位置到结尾的字串。

#### 3）substring(start,stop)

**与 slice() 和 substr() 方法不同的是，substring() 不接受负的参数**

start : 必需。一个非负的整数，规定要提取的子串的第一个字符在 stringObject 中的位置

stop : 可选。一个非负的整数，**比要提取的子串的最后一个字符在 stringObject 中的位置多 1**。

#### js字符串拼接有哪些方法

1）使用 + 运算符；

2）使用concat()方法；

3）使用 join 方法。

![img](https:////upload-images.jianshu.io/upload_images/20308335-c6a9279102d56197.png?imageMogr2/auto-orient/strip|imageView2/2/w/517/format/webp)

### 3、Object的常用方法 

**1）Object.values()：**返回一个对象属性值的数组。

2）Object.keys()：返回一个对象属性名的数组。

3）Object.entries()：创建一个数组，其中包含一个对象的键/值对数组。

4）Object.is()：相等运算符（==）和严格相等运算符（===）。它们都有缺点，前者会自动转换数据类型，后者的NaN不等于自身，以及+0等于-0，Object.is就是用来解决这个问题，与“===”基本一致。

5）Object.assign() 浅拷贝：用于对象的合并。

6）Object spread (对象展开)：展开一个对象，允许向一个对象添加新的属性和值。



## forEach（）和map（）区别

### forEach

forEach方法用于调用数组的每个元素，并将元素传递给回调函数。

```
array.forEach(function(currentValue, index, arr), thisValue)
```

### map

返回一个新数组，并且照原始数组元素顺序依次处理元素，数组中的元素为原始数组元素调用函数处理后的值。

```
array.map(function(currentValue,index,arr), thisValue)
```

### 共同点

- 对空数组都是不会执行回调函数的
- 都能够对数组进行循环

### 区别

- map() 不会改变原始数组，并且会返回一个新的数组
- forEach() 会改变原始数组，返回值为undefined

### 总结

forEach适合于你并不打算改变数据的时候，而只是想用数据做一些事情 – 比如存入数据库或则打印出来。

```html
let arr = [ 'a' , 'b' , 'c' , 'd' ];
arr.forEach((letter) => {
  console.log(letter);
});
// a
// b
// c
// d
12345678
```

map()适用于你要改变数据值的时候。不仅仅在于它更快，而且返回一个新的数组。这样的优点在于你可以使用复合(composition)(map(), filter(), reduce()等组合使用)来玩出更多的花样。

```html
let arr = [1, 2, 3, 4, 5];
let arr2 = arr.map(num => num * 2).filter(num => num > 5);
// arr2 = [6, 8, 10]
123
```

我们首先使用map将每一个元素乘以2，然后紧接着筛选出那些大于5的元素。最终结果赋值给arr2。

核心要点

能用forEach()做到的，map()同样可以。反过来也是如此。

map()会分配内存空间存储新数组并返回，forEach()不会返回数据。

forEach()允许callback更改原始数组的元素。map()返回新的数组。





## 常用八种继承方案

### 原型链继承

构造函数、原型和实例之间的关系：每个构造函数都有一个原型对象，原型对象都包含一个指向构造函数的指针，而实例都包含一个原型对象的指针。

继承的本质就是**复制，即重写原型对象，代之以一个新类型的实例**。

```js
function SuperType() {
    this.property = true;
}

SuperType.prototype.getSuperValue = function() {
    return this.property;
}

function SubType() {
    this.subproperty = false;
}

// 这里是关键，创建SuperType的实例，并将该实例赋值给SubType.prototype
SubType.prototype = new SuperType(); 

SubType.prototype.getSubValue = function() {
    return this.subproperty;
}

var instance = new SubType();
console.log(instance.getSuperValue()); // true

```

![](pics/原型链继承.png)

原型链方案存在的缺点：多个实例对引用类型的操作会被篡改。

```js
function SuperType(){
  this.colors = ["red", "blue", "green"];
}
function SubType(){}

SubType.prototype = new SuperType();

var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"

var instance2 = new SubType(); 
alert(instance2.colors); //"red,blue,green,black"

```

### 借用构造函数继承

使用父类的构造函数来增强子类**实例**，等同于复制父类的实例给子类（不使用原型）

```js
function  SuperType(){
    this.color=["red","green","blue"];
}
function  SubType(){
    //继承自SuperType
    SuperType.call(this);
}
var instance1 = new SubType();
instance1.color.push("black");
alert(instance1.color);//"red,green,blue,black"

var instance2 = new SubType();
alert(instance2.color);//"red,green,blue"

```

核心代码是`SuperType.call(this)`，创建子类实例时调用`SuperType`构造函数，于是`SubType`的每个实例都会将SuperType中的属性复制一份

缺点：

- 只能继承父类的**实例**属性和方法，不能继承原型属性/方法
- 无法实现复用，每个子类都有父类实例函数的副本，影响性能

### 组合继承

组合上述两种方法就是组合继承。用原型链实现对**原型**属性和方法的继承，用借用构造函数技术来实现**实例**属性的继承。

```js
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};

function SubType(name, age){
  // 继承属性
  // 第二次调用SuperType()
  SuperType.call(this, name);
  this.age = age;
}

// 继承方法
// 构建原型链
// 第一次调用SuperType()
SubType.prototype = new SuperType(); 
// 重写SubType.prototype的constructor属性，指向自己的构造函数SubType
SubType.prototype.constructor = SubType; 
SubType.prototype.sayAge = function(){
    alert(this.age);
};

var instance1 = new SubType("Nicholas", 29);
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
instance1.sayName(); //"Nicholas";
instance1.sayAge(); //29

var instance2 = new SubType("Greg", 27);
alert(instance2.colors); //"red,blue,green"
instance2.sayName(); //"Greg";
instance2.sayAge(); //27

```

![](pics/组合继承.png)

缺点：

- 第一次调用`SuperType()`：给`SubType.prototype`写入两个属性name，color。
- 第二次调用`SuperType()`：给`instance1`写入两个属性name，color。

实例对象`instance1`上的两个属性就屏蔽了其原型对象SubType.prototype的两个同名属性。所以，组合模式的缺点就是在使用子类创建实例对象时，其原型中会存在两份相同的属性/方法。

### 原型式继承(后面没看)

```js
function object(obj){
  function F(){}
  F.prototype = obj;
  return new F();
}
```

object()对传入其中的对象执行了一次`浅复制`，将构造函数F的原型直接指向传入的对象

```js
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"]
};

var anotherPerson = object(person);
anotherPerson.name = "Greg";
anotherPerson.friends.push("Rob");

var yetAnotherPerson = object(person);
yetAnotherPerson.name = "Linda";
yetAnotherPerson.friends.push("Barbie");

alert(person.friends);   //"Shelby,Court,Van,Rob,Barbie"

```

缺点：

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能。
- 无法传递参数

另外，ES5中存在`Object.create()`的方法，能够代替上面的object方法。

### 寄生式继承

核心：在原型式继承的基础上，增强对象，返回构造函数

```js
function createAnother(original){
  var clone = object(original); // 通过调用 object() 函数创建一个新对象
  clone.sayHi = function(){  // 以某种方式来增强对象
    alert("hi");
  };
  return clone; // 返回这个对象
}
```

函数的主要作用是为构造函数新增属性和方法，以**增强函数**

```js
var person = {
  name: "Nicholas",
  friends: ["Shelby", "Court", "Van"]
};
var anotherPerson = createAnother(person);
anotherPerson.sayHi(); //"hi"

```

缺点（同原型式继承）：

- 原型链继承多个实例的引用类型属性指向相同，存在篡改的可能。
- 无法传递参数

### 寄生组合式继承

结合借用构造函数传递参数和寄生模式实现继承

```js
function inheritPrototype(subType, superType){
  var prototype = Object.create(superType.prototype); // 创建对象，创建父类原型的一个副本
  prototype.constructor = subType;                    // 增强对象，弥补因重写原型而失去的默认的constructor 属性
  subType.prototype = prototype;                      // 指定对象，将新创建的对象赋值给子类的原型
}

// 父类初始化实例属性和原型属性
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name);
};

// 借用构造函数传递增强子类实例属性（支持传参和避免篡改）
function SubType(name, age){
  SuperType.call(this, name);
  this.age = age;
}

// 将父类原型指向子类
inheritPrototype(SubType, SuperType);

// 新增子类原型属性
SubType.prototype.sayAge = function(){
  alert(this.age);
}

var instance1 = new SubType("xyc", 23);
var instance2 = new SubType("lxy", 23);

instance1.colors.push("2"); // ["red", "blue", "green", "2"]
instance1.colors.push("3"); // ["red", "blue", "green", "3"]

```

![](pics/寄生组合式继承.png)

这个例子的高效率体现在它只调用了一次`SuperType` 构造函数，并且因此避免了在`SubType.prototype` 上创建不必要的、多余的属性。于此同时，原型链还能保持不变；因此，还能够正常使用`instanceof` 和`isPrototypeOf()`

**这是最成熟的方法，也是现在库实现的方法**

### 混入方式继承多个对象

```js
function MyClass() {
     SuperClass.call(this);
     OtherSuperClass.call(this);
}

// 继承一个类
MyClass.prototype = Object.create(SuperClass.prototype);
// 混合其它
Object.assign(MyClass.prototype, OtherSuperClass.prototype);
// 重新指定constructor
MyClass.prototype.constructor = MyClass;

MyClass.prototype.myMethod = function() {
     // do something
};

```

`Object.assign`会把 `OtherSuperClass`原型上的函数拷贝到 `MyClass`原型上，使 MyClass 的所有实例都可用 OtherSuperClass 的方法。

### ES6类继承extends



## new操作符

### new是什么

**`new` 运算符**创建一个用户定义的对象类型的实例或具有构造函数的内置对象的实例。

```js
function Person (name,age) {
    this.name = name
    this.age = age
}
Person.prototype.sayName  = function () {
    console.log(this.name)
}
let man = new Person('xl',20)
console.log(man) // Person { name: 'xl', age: 20 }
man.sayName() // 'xl'

```

- new通过Person创建出来的实例可以访问到构造函数中的属性
- new通过Person创建出来的实例可以访问到构造函数原型链上的方法和属性



- **构造函数如果返回原始值（虽然例子中只有返回了 1，但是你可以试试其他的原始值，结果还是一样的），那么这个返回值毫无意义**
- **构造函数如果返回值为对象，那么这个返回值会被正常使用**

> 这两个例子告诉了我们一点，构造函数尽量不要返回值。因为返回原始值不会生效，返回对象会导致 new 操作符没有作用。



### new的流程

new做了什么工作呢？

- 新建一个对象`obj`
- 把obj的和构造函数通过原型链连接起来
- 将构造函数的`this`指向obj
- 如果该函数没有返回对象，则返回`this`

![](pics/new流程.png)、

### 实现一个new

```js
function create(Con, ...args) {
  let obj = {}
  Object.setPrototypeOf(obj, Con.prototype)
  let result = Con.apply(obj, args)
  return result instanceof Object ? result : obj
}

```

这就是一个完整的实现代码，我们通过以下几个步骤实现了它：

1. 首先函数接受不定量的参数，第一个参数为构造函数，接下来的参数被构造函数使用
2. 然后内部创建一个空对象 `obj`
3. 因为 `obj` 对象需要访问到构造函数原型链上的属性，所以我们通过 `setPrototypeOf` 将两者联系起来。这段代码等同于 `obj.__proto__ = Con.prototype`
4. 将 `obj` 绑定到构造函数上，并且传入剩余的参数
5. 判断构造函数返回值是否为对象，如果为对象就使用构造函数返回的值，否则使用 `obj`，这样就实现了忽略构造函数返回的原始值

接下来我们来使用下该函数，看看行为是否和 `new` 操作符一致

```js
function Test(name, age) {
  this.name = name
  this.age = age
}
Test.prototype.sayName = function () {
    console.log(this.name)
}
const a = create(Test, 'yck', 26)
console.log(a.name) // 'yck'
console.log(a.age) // 26
a.sayName() // 'yck'

```



## this,bind,call,apply

### this的指向

在 ES5 中，其实 this 的指向，始终坚持一个原理：**this 永远指向最后调用它的那个对象**，记住这句话，this 你已经了解一半了。

例 1：

```js
var name = "windowsName";
function a() {
    var name = "Cherry";

    console.log(this.name);          // windowsName

    console.log("inner:" + this);    // inner: Window
}
a();
console.log("outer:" + this)         // outer: Window
```

这个相信大家都知道为什么 log 的是 windowsName，因为根据刚刚的那句话“**this 永远指向最后调用它的那个对象**”，我们看最后调用 `a` 的地方 `a();`，前面没有调用的对象那么就是全局对象 window，这就相当于是 `window.a()`；注意，这里我们没有使用严格模式，如果使用严格模式的话，全局对象就是 `undefined`，那么就会报错 `Uncaught TypeError: Cannot read property 'name' of undefined`。

例 2：

```js
var name = "windowsName";
var a = {
    name: "Cherry",
    fn : function () {
        console.log(this.name);      // Cherry
    }
}
a.fn();
```

在这个例子中，函数 fn 是对象 a 调用的，所以打印的值就是 a 中的 name 的值。是不是有一点清晰了呢~
例 3：

```js
var name = "windowsName";
    var a = {
        name: "Cherry",
        fn : function () {
            console.log(this.name);      // Cherry
        }
    }
    window.a.fn();
```

这里打印 Cherry 的原因也是因为刚刚那句话“**this 永远指向最后调用它的那个对象**”，最后调用它的对象仍然是对象 a。

我们再来看一下这个例子：

```js
    var name = "windowsName";
    var a = {
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // undefined
        }
    }
    window.a.fn();
```

这里为什么会打印 `undefined` 呢？这是因为正如刚刚所描述的那样，调用 fn 的是 a 对象，也就是说 fn 的内部的 this 是对象 a，而对象 a 中并没有对 name 进行定义，所以 log 的 `this.name` 的值是 `undefined`。

这个例子还是说明了：**this 永远指向最后调用它的那个对象**，因为最后调用 fn 的对象是 a，所以就算 a 中没有 name 这个属性，也不会继续向上一个对象寻找 `this.name`，而是直接输出 `undefined`。

再来看一个比较坑的例子：
例 5：

```js
    var name = "windowsName";
    var a = {
        name : null,
        // name: "Cherry",
        fn : function () {
            console.log(this.name);      // windowsName
        }
    }

    var f = a.fn;
    f();
```

这里你可能会有疑问，为什么不是 `Cherry`，这是因为虽然将 a 对象的 fn 方法赋值给变量 f 了，但是没有调用，再接着跟我念这一句话：“**this 永远指向最后调用它的那个对象**”，由于刚刚的 f 并没有调用，所以 `fn()` 最后仍然是被 window 调用的。所以 this 指向的也就是 window。

由以上五个例子我们可以看出，this 的指向并不是在创建的时候就可以确定的，在 es5 中，永远是**this 永远指向最后调用它的那个对象**。

```js
  var name = "windowsName";

    function fn() {
        var name = 'Cherry';
        innerFunction();
        function innerFunction() {
            console.log(this.name);      // windowsName
        }
    }

    fn()

```

### 改变 this 的指向

改变 this 的指向我总结有以下几种方法：

- 使用 ES6 的箭头函数
- 在函数内部使用 `_this = this`
- 使用 `apply`、`call`、`bind`
- new 实例化一个对象

例 7：

```js
    var name = "windowsName";

    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            },100);
        }

    };

    a.func2()     // this.func1 is not a function
```

在不使用箭头函数的情况下，是会报错的，因为最后调用 `setTimeout` 的对象是 window，但是在 window 中并没有 func1 函数。

### 箭头函数

众所周知，ES6 的箭头函数是可以避免 ES5 中使用 this 的坑的。**箭头函数的 this 始终指向函数定义时的 this，而非执行时。**，箭头函数需要记着这句话：“箭头函数中没有 this 绑定，必须通过查找作用域链来决定其值，如果箭头函数被非箭头函数包含，则 this 绑定的是最近一层非箭头函数的 this，否则，this 为 undefined”。

例 8 ：

```js
    var name = "windowsName";

    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            setTimeout( () => {
                this.func1()
            },100);
        }

    };

    a.func2()     // Cherry
```

### 在函数内部使用 `_this = this`

如果不使用 ES6，那么这种方式应该是最简单的不会出错的方式了，我们是先将调用这个函数的对象保存在变量 `_this` 中，然后在函数中都使用这个 `_this`，这样 `_this` 就不会改变了。

例 9：

```js
    var name = "windowsName";

    var a = {

        name : "Cherry",

        func1: function () {
            console.log(this.name)     
        },

        func2: function () {
            var _this = this;
            setTimeout( function() {
                _this.func1()
            },100);
        }

    };

    a.func2()       // Cherry
```

这个例子中，在 func2 中，首先设置 `var _this = this;`，这里的 `this` 是调用 `func2` 的对象 a，为了防止在 `func2` 中的 setTimeout 被 window 调用而导致的在 setTimeout 中的 this 为 window。我们将 `this(指向变量 a)` 赋值给一个变量 `_this`，这样，在 `func2` 中我们使用 `_this` 就是指向对象 a 了。

### 使用 apply、call、bind

#### 使用 apply

例 10：

```js
    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.apply(a),100);
        }

    };

    a.func2()            // Cherry
```

#### 使用 call

例 11：

```js
    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.call(a),100);
        }

    };

    a.func2()            // Cherry
```

#### 使用 bind

例 12：

```js
    var a = {
        name : "Cherry",

        func1: function () {
            console.log(this.name)
        },

        func2: function () {
            setTimeout(  function () {
                this.func1()
            }.bind(a)(),100);
        }

    };

    a.func2()            // Cherry
```

### apply、call、bind 区别

刚刚我们已经介绍了 apply、call、bind 都是可以改变 this 的指向的，但是这三个函数稍有不同。

**语法：**

```js
fun.call(thisArg, param1, param2, ...)
fun.apply(thisArg, [param1,param2,...])
fun.bind(thisArg, param1, param2, ...)
```

**返回值：**

call/apply：`fun`执行的结果 bind：返回`fun`的拷贝，并拥有指定的`this`值和初始参数

**参数**

### 调用`call`/`apply`/`bind`的必须是个函数

call、apply和bind是挂在Function对象上的三个方法,只有函数才有这些方法。

只要是函数就可以，比如: `Object.prototype.toString`就是个函数，我们经常看到这样的用法：`Object.prototype.toString.call(data)`

### 作用：

改变函数执行时的this指向，目前所有关于它们的运用，都是基于这一点来进行的。

### 区别：

call与apply的唯一区别

传给`fun`的参数写法不同：

- `apply`是第2个参数，这个参数是一个数组：传给`fun`参数都写在数组中。

- `call`从第2~n的参数都是传给`fun`的。

- call/apply与bind的区别

  **执行**：

  - call/apply改变了函数的this上下文后马上**执行该函数**
  - bind则是返回改变了上下文后的函数,**不执行该函数**

  **返回值**:

  - call/apply 返回`fun`的执行结果
  - bind返回fun的拷贝，并指定了fun的this指向，保存了fun的参数。

  返回值这段在下方bind应用中有详细的示例解析。

  ```js
  function fn(a, b) {
      console.log(this, a, b)
  }
  
  var obj = {
      name: '林一一'
  }
  
  fn.call(obj, 20, 23)   // {name: "林一一"} 20 23
  
  fn.call(20, 23) // Number {20} 23 undefined
  
  fn.call()   //Window {0: global, window: …} undefined undefined     | 严格模式下为 undefined
  
  fn.call(null)   //Window {0: global, window: …} undefined undefined       | 严格模式下为 null
  
  fn.call(undefined)  //Window {0: global, window: …} undefined undefined     | 严格模式下为 undefined
  ```

  `fn`调用了`call`，`fn` 的 `this` 指向 `obj`，最后 `fn` 被执行；`this` 指向的值都是引用类型，在非严格模式下，不传参数或传递 `null/undefined`，`this` 都指向 `window`。传递的是原始值，原始值会被包装。严格模式下，`call` 的一个参数是谁就指向谁

  ```js
  var obj1 = {
      a: 10,
      fn: function(x) {
          console.log(this.a + x)
      }
  }
  
  var obj2 = {
      a : 20,
      fn: function(x) {
          console.log(this.a - x)
      }
  }
  
  obj1.fn.call(obj2, 20) //40
  ```

  稍微变量一下，原理不变`obj1.fn` 中 `fn`的 `this` 指向到 `obj2`，最后还是执行 `obj1.fn` 中的函数。

### 手写call/apply、bind：（没看）

#### 你能手写实现一个`call`吗？

**思路**

1. 根据call的规则设置上下文对象,也就是`this`的指向。
2. 通过设置`context`的属性,将函数的this指向[隐式绑定](https://juejin.cn/post/6844903630592540686#heading-4)到context上
3. 通过隐式绑定执行函数并传递参数。
4. 删除临时属性，返回函数执行结果

```
Function.prototype.myCall = function (context, ...arr) {
    if (context === null || context === undefined) {
       // 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中为window)
        context = window 
    } else {
        context = Object(context) // 值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的实例对象
    }
    const specialPrototype = Symbol('特殊属性Symbol') // 用于临时储存函数
    context[specialPrototype] = this; // 函数的this指向隐式绑定到context上
    let result = context[specialPrototype](...arr); // 通过隐式绑定执行函数并传递参数
    delete context[specialPrototype]; // 删除上下文对象的属性
    return result; // 返回函数执行结果
};
复制代码
```

**判断函数的上下文对象：**

很多人判断函数上下文对象，只是简单的以`context`是否为false来判断,比如：

```
// 判断函数上下文绑定到`window`不够严谨
context = context ? Object(context) : window; 
context = context || window; 
复制代码
```

经过测试,以下三种为false的情况,函数的上下文对象都会绑定到`window`上：

```
// 网上的其他绑定函数上下文对象的方案: context = context || window; 
function handle(...params) {
    this.test = 'handle'
    console.log('params', this, ...params) // do some thing
}
handle.elseCall('') // window
handle.elseCall(0) // window
handle.elseCall(false) // window
复制代码
```

而`call`则将函数的上下文对象会绑定到这些原始值的实例对象上：



![原始值的实例对象](https://p1-jj.byteimg.com/tos-cn-i-t2oaga2asx/gold-user-assets/2019/8/4/16c5bdb742a5f2b0~tplv-t2oaga2asx-zoom-in-crop-mark:1304:0:0:0.awebp)



所以正确的解决方案，应该是像我上面那么做：

```
// 正确判断函数上下文对象
    if (context === null || context === undefined) {
       // 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中为window)
        context = window 
    } else {
        context = Object(context) // 值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的实例对象
    }
复制代码
```

**使用`Symbol`临时储存函数**

尽管之前用的属性是`testFn`但不得不承认，还是有跟上下文对象的原属性冲突的风险,经网友提醒使用`Symbol`就不会出现冲突了。

考虑兼容的话,还是用尽量特殊的属性，比如带上自己的ID：`OBKoro1TestFn`。

#### 你能手写实现一个`apply`吗？

思路：

1. 传递给函数的参数处理，不太一样，其他部分跟`call`一样。
2. `apply`接受第二个参数为类数组对象, 这里用了JavaScript权威指南中判断是否为类数组对象的方法。

```
Function.prototype.myApply = function (context) {
    if (context === null || context === undefined) {
        context = window // 指定为 null 和 undefined 的 this 值会自动指向全局对象(浏览器中为window)
    } else {
        context = Object(context) // 值为原始值（数字，字符串，布尔值）的 this 会指向该原始值的实例对象
    }
    // JavaScript权威指南判断是否为类数组对象
    function isArrayLike(o) {
        if (o &&                                    // o不是null、undefined等
            typeof o === 'object' &&                // o是对象
            isFinite(o.length) &&                   // o.length是有限数值
            o.length >= 0 &&                        // o.length为非负值
            o.length === Math.floor(o.length) &&    // o.length是整数
            o.length < 4294967296)                  // o.length < 2^32
            return true
        else
            return false
    }
    const specialPrototype = Symbol('特殊属性Symbol') // 用于临时储存函数
    context[specialPrototype] = this; // 隐式绑定this指向到context上
    let args = arguments[1]; // 获取参数数组
    let result
    // 处理传进来的第二个参数
    if (args) {
        // 是否传递第二个参数
        if (!Array.isArray(args) && !isArrayLike(args)) {
            throw new TypeError('myApply 第二个参数不为数组并且不为类数组对象抛出错误');
        } else {
            args = Array.from(args) // 转为数组
            result = context[specialPrototype](...args); // 执行函数并展开数组，传递函数参数
        }
    } else {
        result = context[specialPrototype](); // 执行函数 
    }
    delete context[specialPrototype]; // 删除上下文对象的属性
    return result; // 返回函数执行结果
};
复制代码
```

#### 你能手写实现一个`bind`吗？

**划重点**：

手写`bind`是大厂中的一个高频的面试题，如果面试的中高级前端，只是能说出它们的区别，用法并不能脱颖而出，理解要有足够的深度才能抱得offer归！

**思路**

1. 拷贝源函数:
   - 通过变量储存源函数
   - 使用`Object.create`复制源函数的prototype给fToBind
2. 返回拷贝的函数
3. 调用拷贝的函数：
   - new调用判断：通过`instanceof`判断函数是否通过`new`调用，来决定绑定的`context`
   - 绑定this+传递参数
   - 返回源函数的执行结果

**2019/8/26更新：修复函数没有`prototype`的情况**

```
Function.prototype.myBind = function (objThis, ...params) {
    const thisFn = this; // 存储源函数以及上方的params(函数参数)
    // 对返回的函数 secondParams 二次传参
    let fToBind = function (...secondParams) {
        const isNew = this instanceof fToBind // this是否是fToBind的实例 也就是返回的fToBind是否通过new调用
        const context = isNew ? this : Object(objThis) // new调用就绑定到this上,否则就绑定到传入的objThis上
        return thisFn.call(context, ...params, ...secondParams); // 用call调用源函数绑定this的指向并传递参数,返回执行结果
    };
    if (thisFn.prototype) {
        // 复制源函数的prototype给fToBind 一些情况下函数没有prototype，比如箭头函数
        fToBind.prototype = Object.create(thisFn.prototype);
    }
    return fToBind; // 返回拷贝的函数
};
复制代码
```

**对象缩写方法没有`prototype`**

箭头函数没有`prototype`，这个我知道的，可是`getInfo2`就是一个缩写，为什么没有`prototype`。

谷歌/`stack overflow`都没有找到原因，有大佬指点迷津一下吗？？

```
var student = {
    getInfo: function (name, isRegistered) {
        console.log('this1', this)
    },
    getInfo2(name, isRegistered) {
        console.log('this2', this) // 没有prototype
    },
    getInfo3: (name, isRegistered) => {
        console.log('this3', this) // 没有prototype
    }
}
```

## 闭包

```js
var n = 10
function fn(){
    var n =20
    function f() {
       n++;
       console.log(n)
     }
    f()
    return f
}

var x = fn()
x()
x()
console.log(n)
/* 输出
*  21
    22
    23
    10
/
```

> **思路：`fn` 的返回值是什么变量 `x` 就是什么，这里 `fn` 的返回值是函数名 `f` 也就是 `f` 的堆内存地址，`x()` 也就是执行的是函数 `f()`，而不是 `fn()`，输出的结果显而易见**

**JS 堆栈内存释放**

堆内存：存储引用类型值，对象类型就是键值对，函数就是代码字符串。

堆内存释放：将引用类型的空间地址变量赋值成 `null`，或没有变量占用堆内存了浏览器就会释放掉这个地址

栈内存：提供代码执行的环境和存储基本类型值。

栈内存释放：一般当函数执行完后函数的私有作用域就会被释放掉。

> **但栈内存的释放也有特殊情况：① 函数执行完，但是函数的私有作用域内有内容被栈外的变量还在使用的，栈内存就不能释放里面的基本值也就不会被释放。② 全局下的栈内存只有页面被关闭的时候才会被释放**

### 闭包定义

> **闭包是指有权访问另一个函数作用域中变量的函数**，**通常在嵌套函数中实现**。

### 形成闭包的原因

> **内部的函数存在外部作用域的引用就会导致闭包**。从上面介绍的上级作用域的概念中其实就有闭包的例子 `return f`就是一个表现形式。

```js
var a = 0
function foo(){
    var b =14
    function fo(){
        console.log(a, b)
    }
    fo()
}
foo()
//这里的子函数 fo 内存就存在外部作用域的引用 a, b，所以这就会产生闭包
```

### 闭包变量存储的位置

> 直接说明：**闭包中的变量存储的位置是堆内存。**
>
> 假如闭包中的变量存储在栈内存中，那么栈的回收 会把处于栈顶的变量自动回收。所以闭包中的变量如果处于栈中那么变量被销毁后，闭包中的变量就没有了。所以闭包引用的变量是出于堆内存中的。

### 闭包的作用

- 保护函数的私有变量不受外部的干扰。形成不销毁的栈内存。
- 保存，把一些函数内的值保存下来。闭包可以实现方法和属性的私有化

### 闭包经典使用场景

1. `return` 回一个函数

   ```js
   var n = 10
   function fn(){
       var n =20
       function f() {
          n++;
          console.log(n)
        }
       return f
   }
   
   var x = fn()
   x() // 21
   //这里的 return f, f()就是一个闭包，存在上级作用域的引用。
   ```

2. 函数作为参数

   ```js
   var a = '林一一'
   function foo(){
       var a = 'foo'
       function fo(){
           console.log(a)
       }
       return fo
   }
   
   function f(p){
       var a = 'f'
       p()
   }
   f(foo())
   /* 输出
   *   foo
   / 
   //使用 return fo 返回回来，fo() 就是闭包，f(foo()) 执行的参数就是函数 fo，因为 fo() 中的 a 的上级作用域就是函数foo()，所以输出就是foo
   ```

3. IIFE（自执行函数）

   ```js
   var n = '林一一';
   (function p(){
       console.log(n)
   })()
   /* 输出
   *   林一一
   / 
   //同样也是产生了闭包p()，存在 window下的引用 n。
   ```

   

4. 循环赋值

   ```js
   for(var i = 0; i<10; i++){
     (function(j){
          setTimeout(function(){
           console.log(j)
       }, 1000) 
     })(i)
   }
   //因为存在闭包的原因上面能依次输出1~10，闭包形成了10个互不干扰的私有作用域。将外层的自执行函数去掉后就不存在外部作用域的引用了，输出的结果就是连续的 10。为什么会连续输出10，因为 JS 是单线程的遇到异步的代码不会先执行(会入栈)，等到同步的代码执行完 i++ 到 10时，异步代码才开始执行此时的 i=10 输出的都是 10。
   ```

5. 使用回调函数就是在使用闭包

   ```js
   window.name = '林一一'
   setTimeout(function timeHandler(){
     console.log(window.name);
   }, 100)
   ```

6. 节流防抖

   ```js
   // 节流
   function throttle(fn, timeout) {
       let timer = null
       return function (...arg) {
           if(timer) return
           timer = setTimeout(() => {
               fn.apply(this, arg)
               timer = null
           }, timeout)
       }
   }
   
   // 防抖
   function debounce(fn, timeout){
       let timer = null
       return function(...arg){
           clearTimeout(timer)
           timer = setTimeout(() => {
               fn.apply(this, arg)
           }, timeout)
       }
   }
   ```

7. 柯里化实现

```js
function curry(fn, len = fn.length) {
    return _curry(fn, len)
}

function _curry(fn, len, ...arg) {
    return function (...params) {
        let _arg = [...arg, ...params]
        if (_arg.length >= len) {
            return fn.apply(this, _arg)
        } else {
            return _curry.call(this, fn, len, ..._arg)
        }
    }
}

let fn = curry(function (a, b, c, d, e) {
    console.log(a + b + c + d + e)
})

fn(1, 2, 3, 4, 5)  // 15
fn(1, 2)(3, 4, 5)
fn(1, 2)(3)(4)(5)
fn(1)(2)(3)(4)(5)
```

### 使用闭包需要注意什么

> ```
> 容易导致内存泄漏。闭包会携带包含其它的函数作用域，因此会比其他函数占用更多的内存。过度使用闭包会导致内存占用过多，所以要谨慎使用闭包。
> ```

### 内存泄漏

#### **定义**

> 内存泄漏（Memory leak）是在计算机科学中，由于疏忽或错误造成程序未能释放已经不再使用的内存。并非指内存在物理上的消失，而是应用程序分配某段内存后，由于设计错误，导致在释放该段内存之前就失去了对该段内存的控制，从而造成了内存的浪费

#### **引起内存泄漏的情况**

1. **意外的全局变量**

全局变量的生命周期最长，直到页面关闭前，它都存活着，所以全局变量上的内存一直都不会被回收

当全局变量使用不当，没有及时回收（手动赋值 null），或者拼写错误等将某个变量挂载到全局变量时，也就发生内存泄漏了

2. **遗忘的定时器**

setTimeout 和 setInterval 是由浏览器专门线程来维护它的生命周期，所以当在某个页面使用了定时器，当该页面销毁时，没有手动去释放清理这些定时器的话，那么这些定时器还是存活着的

也就是说，定时器的生命周期并不挂靠在页面上，所以当在当前页面的 js 里通过定时器注册了某个回调函数，而该回调函数内又持有当前页面某个变量或某些 DOM 元素时，就会导致即使页面销毁了，由于定时器持有该页面部分引用而造成页面无法正常被回收，从而导致内存泄漏了

如果此时再次打开同个页面，内存中其实是有双份页面数据的，如果多次关闭、打开，那么内存泄漏会越来越严重

而且这种场景很容易出现，因为使用定时器的人很容易遗忘清除

**3. 使用不当的闭包**

函数本身会持有它定义时所在的词法环境的引用，但通常情况下，使用完函数后，该函数所申请的内存都会被回收了

但当函数内再返回一个函数时，由于返回的函数持有外部函数的词法环境，而返回的函数又被其他生命周期东西所持有，导致外部函数虽然执行完了，但内存却无法被回收

所以，返回的函数，它的生命周期应尽量不宜过长，方便该闭包能够及时被回收

正常来说，闭包并不是内存泄漏，因为这种持有外部函数词法环境本就是闭包的特性，就是为了让这块内存不被回收，因为可能在未来还需要用到，但这无疑会造成内存的消耗，所以，不宜烂用就是了。

**4. 遗漏的 DOM 元素**

DOM 元素的生命周期正常是取决于是否挂载在 DOM 树上，当从 DOM 树上移除时，也就可以被销毁回收了

但如果某个 DOM 元素，在 js 中也持有它的引用时，那么它的生命周期就由 js 和是否在 DOM 树上两者决定了，记得移除时，两个地方都需要去清理才能正常回收它

5. **网络回调**

某些场景中，在某个页面发起网络请求，并注册一个回调，且回调函数内持有该页面某些内容，那么，当该页面销毁时，应该注销网络的回调，否则，因为网络持有页面部分内容，也会导致页面部分内容无法被回收。

#### 怎么检查内存泄露

- performance 面板 和 memory 面板可以找到泄露的现象和位置

#### 浏览器解决方法：

垃圾回收机制



## 垃圾回收

### 概念

`GC` 即 `Garbage Collection` ，程序工作过程中会产生很多 `垃圾`，这些垃圾是程序不用的内存或者是之前用过了，以后不会再用的内存空间，而 `GC` 就是负责回收垃圾的，因为他工作在引擎内部，所以对于我们前端来说，`GC` 过程是相对比较无感的，这一套引擎执行而对我们又相对无感的操作也就是常说的 `垃圾回收机制` 了

#### 可达性

JavaScript 中内存管理的主要概念是可达性。

简单地说，“可达性” 值就是那些以某种方式可访问或可用的值，它们被保证存储在内存中。

**1. 有一组基本的固有可达值，由于显而易见的原因无法删除。例如:**

- 本地函数的局部变量和参数
- 当前嵌套调用链上的其他函数的变量和参数
- 全局变量
- 还有一些其他的，内部的

**这些值称为根。**

**2. 如果引用或引用链可以从根访问任何其他值，则认为该值是可访问的。**

例如，如果局部变量中有对象，并且该对象具有引用另一个对象的属性，则该对象被视为**可达性**， 它引用的那些也是可以访问的，详细的例子如下。

JavaScript 引擎中有一个后台进程称为[垃圾回收器](https://link.segmentfault.com/?enc=oacf9nqaahoxkw9sIAeKcw%3D%3D.w6cXsKFVJX7ldc8qs8n%2F10i2NTudcOP3T0spgYtM8CJw9HF5YWx0Y1QGHvPkgBDrXRQooaPtN254zorhhq8hpeyoLA0JenJ%2BdRFLhcjjWao%3D)，它监视所有对象，并删除那些不可访问的对象。



### 垃圾回收策略

我们都可以 Get 到这之中的重点，那就是怎样找出所谓的垃圾？

这个流程就涉及到了一些算法策略，有很多种方式，我们简单介绍两个最常见的

- 标记清除算法
- 引用计数算法

**标记清除法步骤**：

- 垃圾回收器获取根并**“标记”**(记住)它们。
- 然后它访问并“标记”所有来自它们的引用。
- 然后它访问标记的对象并标记它们的引用。所有被访问的对象都被记住，以便以后不再访问同一个对象两次。
- 以此类推，直到有未访问的引用(可以从根访问)为止。
- 除标记的对象外，所有对象都被删除。

综上所述，标记清除算法或者说策略就有两个很明显的缺点

- **内存碎片化**，空闲内存块是不连续的，容易出现很多空闲内存块，还可能会出现分配所需内存过大的对象时找不到合适的块
- **分配速度慢**，因为即便是使用 `First-fit` 策略，其操作仍是一个 `O(n)` 的操作，最坏情况是每次都要遍历到最后，同时因为碎片化，大对象的分配效率会更慢

而 **标记整理（Mark-Compact）算法** 就可以有效地解决，它的标记阶段和标记清除算法没有什么不同，只是标记结束后，标记整理算法会将活着的对象（即不需要清理的对象）向内存的一端移动，最后清理掉边界的内存

**引用计数算法步骤**

它的策略是跟踪记录每个变量值被使用的次数

- 当声明了一个变量并且将一个引用类型赋值给该变量的时候这个值的引用次数就为 1
- 如果同一个值又被赋给另一个变量，那么引用数加 1
- 如果该变量的值被其他的值覆盖了，则引用次数减 1
- 当这个值的引用次数变为 0 的时候，说明没有变量在使用，这个值没法被访问了，回收空间，垃圾回收器会在运行的时候清理掉引用次数为 0 的值占用的内存

**优点**

引用计数算法的优点我们对比标记清除来看就会清晰很多，首先引用计数在引用值为 0 时，也就是在变成垃圾的那一刻就会被回收，所以它可以立即回收垃圾

而标记清除算法需要每隔一段时间进行一次，那在应用程序（JS脚本）运行过程中线程就必须要暂停去执行一段时间的 `GC`，另外，标记清除算法需要遍历堆里的活动以及非活动对象来清除，而引用计数则只需要在引用时计数就可以了

**缺点**

引用计数的缺点想必大家也都很明朗了，首先它需要一个计数器，而此计数器需要占很大的位置，因为我们也不知道被引用数量的上限，还有就是无法解决**循环引用**无法回收的问题，这也是最严重的



### 优化

- **分代回收**——对象分为两组:“新对象”和“旧对象”。许多对象出现，完成它们的工作并迅速结 ，它们很快就会被清理干净。那些活得足够久的对象，会变“老”，并且很少接受检查。
- **增量回收**——如果有很多对象，并且我们试图一次遍历并标记整个对象集，那么可能会花费一些时间，并在执行中会有一定的延迟。因此，引擎试图将垃圾回收分解为多个部分。然后，各个部分分别执行。这需要额外的标记来跟踪变化，这样有很多微小的延迟，而不是很大的延迟。
- **空闲时间收集**——垃圾回收器只在 CPU 空闲时运行，以减少对执行的可能影响。





## 正则表达式 RegExp

### TBC



## 如何判断数组类型的数据

因为数组属于引用类型，所以常规的typeof方法并不能判断数组类型。

### **instanceof**

instanceof用于检测**构造函数**的prototype属性是否出现在某个对象的原型链上

```js
var arr = [1,2,3];

console.log(arr instanceof Array);
```

### constructor

判断对象的**构造函数**是否是数组。

**`constructor`** 是一种用于创建和初始化`class`创建的对象的特殊方法。如果我们判断该对象的构造函数就是`Array`，那就是说该对象可以通过数组的构造函数（`Array`）创建得到，也就说明它是数组了。

```js
var arr = [1,2,3];

console.log(arr.constructor == Array);
```

### Object.prototype.toString.call()

`toString() ` 方法返回一个表示该对象的字符串。

每个对象都有一个 `toString()` 方法，当该对象被表示为一个文本值时，或者一个对象以预期的字符串方式引用时自动调用。默认情况下，`toString()` 方法被每个 `Object` 对象继承。如果此方法在自定义对象中未被覆盖，`toString()` 返回 "[object *type*]"，其中 `type` 是对象的类

```js
var arr = [1,2,3];
console.log(Object.prototype.toString.call(arr);
console.log(Object.prototype.toString.call(arr) === '[object Array]');
```

![](pics/Object.prototype.toString.call().png)

### Array.prototype.isPrototypeOf()

`isPrototypeOf() ` 方法用于测试一个对象是否存在于另一个对象的原型链上。

> `isPrototypeOf()` 与 [`instanceof`](https://link.juejin.cn?target=https%3A%2F%2Fdeveloper.mozilla.org%2Fzh-CN%2Fdocs%2FWeb%2FJavaScript%2FReference%2FOperators%2Finstanceof) 运算符不同。在表达式 "`object instanceof AFunction`"中，`object` 的原型链是针对 `AFunction.prototype` 进行检查的，而不是针对 `AFunction` 本身

### Array.isArray

在ES6中提供了Array.isArray()方法来检测数组类型。

```js
var arr = [1,2,3];
console.log(Array.isArray(arr));
```



## 生成器Generator

### 介绍

- generator:可以将生成器视为可以暂停和恢复的进程（代码段),代码在执行的过程中可以主要交出控制权，是 ES6 提供的一种异步编程解决方案

- genearator 语法: function* 是一个新的关键字用于生成器函数（也有生成器方法）。
  yield是generator可以自行暂停的运算符。此外，generator还可以通过yield接收输入和发送输出。

```js
function* example() {
 yield 1;
 yield 2;
 yield 3;
}
var iter=example();
iter.next();//{value:1，done:false}
iter.next();//{value:2，done:false}
iter.next();//{value:3，done:false}
iter.next();//{value:undefined，done:true}
```

Generator函数有多个返回值状态（每个yield关键字后跟一个状态），只有调用next()函数时才会返回值，每调用一次next()函数就返回一个对象{value: xxx, done: false}，直到没有对应的yield了就返回done状态为true的对象{value: undefined, done: true}。

与普通函数相比二者有如下区别:

- 普通函数使用 function 声明，生成器函数用 function*声明
- 普通函数使用 return 返回值，生成器函数使用 yield 返回值
- 普通函数是 run to completion 模式，即普通函数开始执行后，会一直执行到该函数所有语句完成，在此期间别的代码语句是不会被执行的；生成器函数是 run-pause-run 模式，即生成器函数可以在函数运行中被暂停一次或多次，并且在后面再恢复执行，在**暂停期间允许其他代码语句被执行**

### 使用案例

https://blog.csdn.net/qq_39903567/article/details/115188020

#### 1.给普通对象添加[遍历](https://so.csdn.net/so/search?q=遍历&spm=1001.2101.3001.7020)器

一个普通的对象obj默认是没有遍历器的，意味着不能使用for…of遍历，且不能使用…操作符解构。
可见都是报错obj is not iterable，我们通过Generator函数给其添加遍历器。

```js
let obj = {
    name:'zhangsan',
    age:18,
    sex:'man'
}
obj[Symbol.iterator]=function* (){
    for(var key in obj){
        yield obj[key];
    }
}
for(let value of obj){
    console.log(value);//zhangsan 18 man
}
console.log([...obj]);//["zhangsan", 18, "man"]
```

#### 2.将[ajax](https://so.csdn.net/so/search?q=ajax&spm=1001.2101.3001.7020)请求转成类似的 let a = ajax()的同步赋值形式

经常碰见一个业务内多个请求串联依赖，即后者依赖前者的请求结果，目前只能有3种做法，

- 回调函数嵌套

- 使用promise的.then进行链式调用

- 但现在我们学习了Generator函数后可以有第三种选择
  将前一个网络请求的返回值赋值给一个变量，在下一个请求中使用
  即以下形式：

- ```js
  let a = 请求1(){}
  let b = 请求2(a){}
  
  let res = 0;
  //封装一个网络请求函数，不做实际动作，就打印一下参数param
  function ajaxMy(method,url,param,varibal){
      console.log(param);
      //使用延时计时器模拟网络请求1秒后返回正确结果response
      setTimeout(function(){
          let response = res++;
          varibal.next(response);
      },300)
  }
  
  let k;
  let tell = function* (){
      //网络请求1
      let a = yield ajaxMy('get','www.baidu.com',10,k);
      console.log(a);//0
      //网络请求2
      let b = yield ajaxMy('get','www.baidu.com',a,k);
      console.log(b);//1
      //网络请求3
      let c = yield ajaxMy('get','www.baidu.com',b,k);
      console.log(c);//2
      //网络请求4
      let d = yield ajaxMy('get','www.baidu.com',c,k);
      console.log(d);//3
  }
  k = tell()
  k.next();
  ```

  以上代码let a = yield ajaxMy(‘get’,‘www.baidu.com’,10,k);类似于将一个网络请求的返回值赋值给了a变量，可在下一步请求yield ajaxMy(‘get’,‘www.baidu.com’,a,k);中作为参数使用，这样一来编写业务逻辑就会清晰很多。

#### 3.实现[状态机]

```js
let state = function* (){
    while(1){
        yield 'block';
        yield 'none';
    }
}
let displayClass = state();
console.log(displayClass.next().value);//block
console.log(displayClass.next().value);//none
console.log(displayClass.next().value);//block
console.log(displayClass.next().value);//none
```

### 实现原理

先简单回顾一下JS的运行规则：

1. JS是单线程的，只有一个主线程
2. 函数内的代码从上到下顺序执行，遇到被调用的函数先进入被调用函数执行，待完成后继续执行
3. 遇到异步事件，浏览器另开一个线程，主线程继续执行，待结果返回后，执行回调函数

那么，Generator函数是如何进行异步化为同步操作的呢？ 实质上很简单，* 和 yield 是一个标识符，在浏览器进行软编译的时候，遇到这两个符号，自动进行了代码转换：

```js
// 异步函数
function asy() {
    $.ajax({
        url: 'test.txt',
        dataType: 'text',
        success() {
            console.log("我是异步代码");
        }
    })
}

function* gener() {
    let asy = yield asy();
    yield console.log("我是同步代码");
}
let it = gener().next();
it.then(function() {
    it.next();
})
// 我是异步代码
// 我是同步代码
```

浏览器编译之后:

```js
function gener() {
    // let asy = yield asy(); 替换为
    $.ajax({
        url: 'test.txt',
        dataType: 'text',
        success() {
            console.log("我是异步代码");
            // next 之后执行以下
            console.log("我是同步代码");
        }
    })
    // yield console.log("我是同步代码");
}
```

整个过程类似于，浏览器遇到标识符 * 之后，就明白这个函数是生成器函数，一旦遇到 yield 标识符，就会将以后的函数放入此异步函数之内，待异步返回结果后再进行执行。

更深一步，从内存上来讲：
普通函数在被调用时，JS 引擎会创建一个栈帧，在里面准备好局部变量、函数参数、临时值、代码执行的位置（也就是说这个函数的第一行对应到代码区里的第几行机器码），在当前栈帧里设置好返回位置，然后将新帧压入栈顶。待函数执行结束后，这个栈帧将被弹出栈然后销毁，返回值会被传给上一个栈帧。

当执行到 yield 语句时，Generator 的栈帧同样会被弹出栈外，但Generator在这里耍了个花招——它在堆里保存了栈帧的引用（或拷贝）！这样当 it.next 方法被调用时，JS引擎便不会重新创建一个栈帧，而是把堆里的栈帧直接入栈。因为栈帧里保存了函数执行所需的全部上下文以及当前执行的位置，所以当这一切都被恢复如初之时，就好像程序从原本暂停的地方继续向前执行了。
而因为每次 yield 和 it.next 都对应一次出栈和入栈，所以可以直接利用已有的栈机制，实现值的传出和传入。



## 期约Promise

是异步编程的一种解决方案，比传统的解决方案（回调函数）更加合理和更加强大

在 Promise 出现以前，在我们处理多个异步请求嵌套时，可能产生**回调地狱**，产生**回调地狱**的原因归结起来有两点：

1. **嵌套调用**，第一个函数的输出往往是第二个函数的输入；
2. **处理多个异步请求并发**，开发时往往需要同步请求最终的结果。

原因分析出来后，那么问题的解决思路就很清晰了：

1. **消灭嵌套调用**：通过 Promise 的链式调用可以解决；
2. **合并多个任务的请求结果**：使用 Promise.all 获取合并多个任务的错误处理。

### 状态

`promise`对象仅有三种状态

- `pending`（进行中）
- `fulfilled`（已成功）
- `rejected`（已失败）

### 特点

- 对象的状态不受外界影响，只有异步操作的结果，可以决定当前是哪一种状态
- 一旦状态改变（从`pending`变为`fulfilled`和从`pending`变为`rejected`），就不会再变，任何时候都可以得到这个结果

![](pics/Promise周期.png)

`Promise`对象是一个构造函数，用来生成`Promise`实例

```
const promise = new Promise(function(resolve, reject) {});
Promise`构造函数接受一个函数作为参数，该函数的两个参数分别是`resolve`和`reject
```

- `resolve`函数的作用是，将`Promise`对象的状态从“未完成”变为“成功”
- `reject`函数的作用是，将`Promise`对象的状态从“未完成”变为“失败”

### 实例方法

`Promise`构建出来的实例存在以下方法：

- then()
- catch()
- finally()

#### then()

**“promise本身不是异步的，是用来管理异步的，但是then方法是异步的「微任务」”**

`then`是实例状态发生改变时的回调函数，第一个参数是`resolved`状态的回调函数，第二个参数是`rejected`状态的回调函数

`then`方法**返回的是一个新的`Promise`实例**，也就是`promise`能链式书写的原因

```js
getJSON("/posts.json").then(function(json) {
  return json.post;
}).then(function(post) {
  // ...
});
```

#### catch

`catch()`方法是`.then(null, rejection)`或`.then(undefined, rejection)`的别名，用于指定发生错误时的回调函数

```js
getJSON('/posts.json').then(function(posts) {
  // ...
}).catch(function(error) {
  // 处理 getJSON 和 前一个回调函数运行时发生的错误
  console.log('发生错误！', error);
});
```

`Promise `对象的错误具有“冒泡”性质，会一直向后传递，直到被捕获为止

```js
getJSON('/post/1.json').then(function(post) {
  return getJSON(post.commentURL);
}).then(function(comments) {
  // some code
}).catch(function(error) {
  // 处理前面三个Promise产生的错误
});
```

一般来说，使用`catch`方法代替`then()`第二个参数

`Promise `对象抛出的错误不会传递到外层代码，即不会有任何反应

```js
const someAsyncThing = function() {
  return new Promise(function(resolve, reject) {
    // 下面一行会报错，因为x没有声明
    resolve(x + 2);
  });
};
```

浏览器运行到这一行，会打印出错误提示`ReferenceError: x is not defined`，但是不会退出进程

`catch()`方法之中，还能再抛出错误，通过后面`catch`方法捕获到

#### finally()

`finally()`方法用于指定不管 Promise 对象最后状态如何，都会执行的操作

```js
promise
.then(result => {···})
.catch(error => {···})
.finally(() => {···});
```

### 构造函数方法

`Promise`构造函数存在以下方法：

- all()
- race()
- allSettled()
- resolve()
- reject()
- try()

#### all()

`Promise.all()`方法用于将多个 `Promise `实例，包装成一个新的 `Promise `实例

```js
const p = Promise.all([p1, p2, p3]);
```

接受一个**数组**（迭代对象）作为参数，数组成员都应为`Promise`实例

实例`p`的状态由`p1`、`p2`、`p3`决定，分为两种：

- 只有`p1`、`p2`、`p3`的状态都变成`fulfilled`，`p`的状态才会变成`fulfilled`，此时`p1`、`p2`、`p3`的返回值组成一个数组，传递给`p`的回调函数
- 只要`p1`、`p2`、`p3`之中有一个被`rejected`，`p`的状态就变成`rejected`，此时第一个被`reject`的实例的返回值，会传递给`p`的回调函数

注意，如果作为参数的 `Promise` 实例，自己定义了`catch`方法，那么它一旦被`rejected`，并不会触发`Promise.all()`的`catch`方法

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result)
.catch(e => e);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result)
.catch(e => e);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// ["hello", Error: 报错了]
```

如果`p2`没有自己的`catch`方法，就会调用`Promise.all()`的`catch`方法

```js
const p1 = new Promise((resolve, reject) => {
  resolve('hello');
})
.then(result => result);

const p2 = new Promise((resolve, reject) => {
  throw new Error('报错了');
})
.then(result => result);

Promise.all([p1, p2])
.then(result => console.log(result))
.catch(e => console.log(e));
// Error: 报错了
```

#### race()

`Promise.race()`方法同样是将多个 Promise 实例，包装成一个新的 Promise 实例

```js
const p = Promise.race([p1, p2, p3]);
```

只要`p1`、`p2`、`p3`之中有一个实例**率先**改变状态，`p`的状态就跟着改变

率先改变的 Promise 实例的返回值则传递给`p`的回调函数

```js
const p = Promise.race([
  fetch('/resource-that-may-take-a-while'),
  new Promise(function (resolve, reject) {
    setTimeout(() => reject(new Error('request timeout')), 5000)
  })
]);

p
.then(console.log)
.catch(console.error);
```

#### allSettled()

`Promise.allSettled()`方法接受一组 Promise 实例作为参数，包装成一个新的 Promise 实例

只有等到所有这些参数实例**都返回结果**，不管是`fulfilled`还是`rejected`，包装实例才会结束

```js
const promises = [
  fetch('/api-1'),
  fetch('/api-2'),
  fetch('/api-3'),
];

await Promise.allSettled(promises);
removeLoadingIndicator();
```

#### resolve()

将现有对象转为 `Promise `对象

```js
Promise.resolve('foo')
// 等价于
new Promise(resolve => resolve('foo'))
```

参数可以分成四种情况，分别如下：

- 参数是一个 Promise 实例，`promise.resolve`将不做任何修改、原封不动地返回这个实例
- 参数是一个`thenable`对象，`promise.resolve`会将这个对象转为 `Promise `对象，然后就立即执行`thenable`对象的`then()`方法
- 参数不是具有`then()`方法的对象，或根本就不是对象，`Promise.resolve()`会返回一个新的 Promise 对象，状态为`resolved`
- 没有参数时，直接返回一个`resolved`状态的 Promise 对象

#### reject()

```js
Promise.reject(reason)`方法也会返回一个新的 Promise 实例，该实例的状态为`rejected
const p = Promise.reject('出错了');
// 等同于
const p = new Promise((resolve, reject) => reject('出错了'))

p.then(null, function (s) {
  console.log(s)
});
// 出错了
```

`Promise.reject()`方法的参数，会原封不动地变成后续方法的参数

```js
Promise.reject('出错了')
.catch(e => {
  console.log(e === '出错了')
})
// true
```

### 手写Promise（未看）（听说标配）

**Promise/A+**

我们想要手写一个 Promise，就要遵循 [Promise/A+](https://link.juejin.cn?target=https%3A%2F%2Fpromisesaplus.com%2F) 规范，业界所有 Promise 的类库都遵循这个规范。

其实 Promise/A+ 规范对如何实现一个符合标准的 Promise 类库已经阐述的很详细了。每一行代码在 Promise/A+ 规范中都有迹可循，所以在下面的实现的过程中，我会尽可能的将代码和 Promise/A+ 规范一一对应起来。

下面开始步入正题啦～

**基础版 Promise**

我们先来回顾下最简单的 Promise 使用方式：

```js
const p1 = new Promise((resolve, reject) => {
  console.log('create a promise');
  resolve('成功了');
})

console.log("after new promise");

const p2 = p1.then(data => {
  console.log(data)
  throw new Error('失败了')
})

const p3 = p2.then(data => {
  console.log('success', data)
}, err => {
  console.log('faild', err)
})
复制代码
```

控制台输出：

```js
"create a promise"
"after new promise"
"成功了"
"faild Error: 失败了"
复制代码
```

- 首先我们在调用 Promise 时，会返回一个 Promise 对象。
- 构建 Promise 对象时，需要传入一个 **executor 函数**，Promise 的主要业务流程都在 executor 函数中执行。
- 如果运行在 excutor 函数中的业务执行成功了，会调用 resolve 函数；如果执行失败了，则调用 reject 函数。
- Promise 的状态不可逆，同时调用 resolve 函数和 reject 函数，默认会采取第一次调用的结果。

以上简单介绍了 Promise 的一些主要的使用方法，结合 [Promise/A+](https://link.juejin.cn?target=https%3A%2F%2Fpromisesaplus.com%2F) 规范，我们可以分析出 Promise 的基本特征：

> 1. promise 有三个状态：`pending`，`fulfilled`，or `rejected`；「规范 Promise/A+ 2.1」
> 2. `new promise`时， 需要传递一个`executor()`执行器，执行器立即执行；
> 3. `executor`接受两个参数，分别是`resolve`和`reject`；
> 4. promise  的默认状态是 `pending`；
> 5. promise 有一个`value`保存成功状态的值，可以是`undefined/thenable/promise`；「规范 Promise/A+ 1.3」
> 6. promise 有一个`reason`保存失败状态的值；「规范 Promise/A+ 1.5」
> 7. promise 只能从`pending`到`rejected`, 或者从`pending`到`fulfilled`，状态一旦确认，就不会再改变；
> 8. promise 必须有一个`then`方法，then 接收两个参数，分别是 promise 成功的回调 onFulfilled, 和 promise 失败的回调 onRejected；「规范 Promise/A+ 2.2」
> 9. 如果调用 then 时，promise 已经成功，则执行`onFulfilled`，参数是`promise`的`value`；
> 10. 如果调用 then 时，promise 已经失败，那么执行`onRejected`, 参数是`promise`的`reason`；
> 11. 如果 then 中抛出了异常，那么就会把这个异常作为参数，传递给下一个 then 的失败的回调`onRejected`；

按照上面的特征，我们试着勾勒下 Promise 的形状：

```js
// 三个状态：PENDING、FULFILLED、REJECTED
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

class Promise {
  constructor(executor) {
    // 默认状态为 PENDING
    this.status = PENDING;
    // 存放成功状态的值，默认为 undefined
    this.value = undefined;
    // 存放失败状态的值，默认为 undefined
    this.reason = undefined;

    // 调用此方法就是成功
    let resolve = (value) => {
      // 状态为 PENDING 时才可以更新状态，防止 executor 中调用了两次 resovle/reject 方法
      if(this.status ===  PENDING) {
        this.status = FULFILLED;
        this.value = value;
      }
    } 

    // 调用此方法就是失败
    let reject = (reason) => {
      // 状态为 PENDING 时才可以更新状态，防止 executor 中调用了两次 resovle/reject 方法
      if(this.status ===  PENDING) {
        this.status = REJECTED;
        this.reason = reason;
      }
    }

    try {
      // 立即执行，将 resolve 和 reject 函数传给使用者  
      executor(resolve,reject)
    } catch (error) {
      // 发生异常时执行失败逻辑
      reject(error)
    }
  }

  // 包含一个 then 方法，并接收两个参数 onFulfilled、onRejected
  then(onFulfilled, onRejected) {
    if (this.status === FULFILLED) {
      onFulfilled(this.value)
    }

    if (this.status === REJECTED) {
      onRejected(this.reason)
    }
  }
}
复制代码
```

写完代码我们可以测试一下：

```js
const promise = new Promise((resolve, reject) => {
  resolve('成功');
}).then(
  (data) => {
    console.log('success', data)
  },
  (err) => {
    console.log('faild', err)
  }
)
复制代码
```

控制台输出：

```js
"success 成功"
复制代码
```

现在我们已经实现了一个基础版的 Promise，但是还不要高兴的太早噢，这里我们只处理了同步操作的 promise。如果在 `executor()`中传入一个异步操作的话呢，我们试一下：

```js
const promise = new Promise((resolve, reject) => {
  // 传入一个异步操作
  setTimeout(() => {
    resolve('成功');
  },1000);
}).then(
  (data) => {
    console.log('success', data)
  },
  (err) => {
    console.log('faild', err)
  }
)
复制代码
```

执行测试脚本后发现，promise 没有任何返回。

因为 promise 调用 then 方法时，当前的 promise 并没有成功，一直处于 pending 状态。所以如果当调用 then 方法时，当前状态是 pending，我们需要先将成功和失败的回调分别存放起来，在`executor()`的异步任务被执行时，触发 resolve 或 reject，依次调用成功或失败的回调。

结合这个思路，我们优化一下代码：

```js
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

class Promise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;
    // 存放成功的回调
    this.onResolvedCallbacks = [];
    // 存放失败的回调
    this.onRejectedCallbacks= [];

    let resolve = (value) => {
      if(this.status ===  PENDING) {
        this.status = FULFILLED;
        this.value = value;
        // 依次将对应的函数执行
        this.onResolvedCallbacks.forEach(fn=>fn());
      }
    } 

    let reject = (reason) => {
      if(this.status ===  PENDING) {
        this.status = REJECTED;
        this.reason = reason;
        // 依次将对应的函数执行
        this.onRejectedCallbacks.forEach(fn=>fn());
      }
    }

    try {
      executor(resolve,reject)
    } catch (error) {
      reject(error)
    }
  }

  then(onFulfilled, onRejected) {
    if (this.status === FULFILLED) {
      onFulfilled(this.value)
    }

    if (this.status === REJECTED) {
      onRejected(this.reason)
    }

    if (this.status === PENDING) {
      // 如果promise的状态是 pending，需要将 onFulfilled 和 onRejected 函数存放起来，等待状态确定后，再依次将对应的函数执行
      this.onResolvedCallbacks.push(() => {
        onFulfilled(this.value)
      });

      // 如果promise的状态是 pending，需要将 onFulfilled 和 onRejected 函数存放起来，等待状态确定后，再依次将对应的函数执行
      this.onRejectedCallbacks.push(()=> {
        onRejected(this.reason);
      })
    }
  }
}
复制代码
```

测试一下：

```js
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('成功');
  },1000);
}).then(
  (data) => {
    console.log('success', data)
  },
  (err) => {
    console.log('faild', err)
  }
)
复制代码
```

控制台等待 `1s` 后输出：

```js
"success 成功"
复制代码
```

ok！大功告成，异步问题已经解决了！

熟悉设计模式的同学，应该意识到了这其实是一个**发布订阅模式**，这种`收集依赖 -> 触发通知 -> 取出依赖执行`的方式，被广泛运用于发布订阅模式的实现。

**then 的链式调用&值穿透特性**

我们都知道，promise 的优势在于可以链式调用。在我们使用 Promise 的时候，当 then 函数中 return 了一个值，不管是什么值，我们都能在下一个 then 中获取到，这就是所谓的**then 的链式调用**。而且，当我们不在 then 中放入参数，例：`promise.then().then()`，那么其后面的 then 依旧可以得到之前 then 返回的值，这就是所谓的**值的穿透**。那具体如何实现呢？简单思考一下，如果每次调用 then 的时候，我们都重新创建一个 promise 对象，并把上一个 then 的返回结果传给这个新的 promise 的 then 方法，不就可以一直 then 下去了么？那我们来试着实现一下。这也是手写 Promise 源码的重中之重，所以，打起精神来，重头戏来咯！

有了上面的想法，我们再结合 [Promise/A+](https://link.juejin.cn?target=https%3A%2F%2Fpromisesaplus.com%2F) 规范梳理一下思路：

> 1. then 的参数 `onFulfilled` 和 `onRejected` 可以缺省，如果 `onFulfilled` 或者 `onRejected`不是函数，将其忽略，且依旧可以在下面的 then 中获取到之前返回的值；「规范 Promise/A+ 2.2.1、2.2.1.1、2.2.1.2」
> 2. promise 可以 then 多次，每次执行完 promise.then 方法后返回的都是一个“新的promise"；「规范 Promise/A+ 2.2.7」
> 3. 如果 then 的返回值 x 是一个普通值，那么就会把这个结果作为参数，传递给下一个 then 的成功的回调中；
> 4. 如果 then 中抛出了异常，那么就会把这个异常作为参数，传递给下一个 then 的失败的回调中；「规范 Promise/A+ 2.2.7.2」
> 5. 如果 then 的返回值 x 是一个 promise，那么会等这个 promise 执行完，promise 如果成功，就走下一个 then 的成功；如果失败，就走下一个 then 的失败；如果抛出异常，就走下一个 then 的失败；「规范 Promise/A+ 2.2.7.3、2.2.7.4」
> 6. 如果 then 的返回值 x 和 promise 是同一个引用对象，造成循环引用，则抛出异常，把异常传递给下一个 then 的失败的回调中；「规范 Promise/A+ 2.3.1」
> 7. 如果 then 的返回值 x 是一个 promise，且 x 同时调用 resolve 函数和 reject 函数，则第一次调用优先，其他所有调用被忽略；「规范 Promise/A+ 2.3.3.3.3」

我们将代码补充完整：

```js
const PENDING = 'PENDING';
const FULFILLED = 'FULFILLED';
const REJECTED = 'REJECTED';

const resolvePromise = (promise2, x, resolve, reject) => {
  // 自己等待自己完成是错误的实现，用一个类型错误，结束掉 promise  Promise/A+ 2.3.1
  if (promise2 === x) { 
    return reject(new TypeError('Chaining cycle detected for promise #<Promise>'))
  }
  // Promise/A+ 2.3.3.3.3 只能调用一次
  let called;
  // 后续的条件要严格判断 保证代码能和别的库一起使用
  if ((typeof x === 'object' && x != null) || typeof x === 'function') { 
    try {
      // 为了判断 resolve 过的就不用再 reject 了（比如 reject 和 resolve 同时调用的时候）  Promise/A+ 2.3.3.1
      let then = x.then;
      if (typeof then === 'function') { 
        // 不要写成 x.then，直接 then.call 就可以了 因为 x.then 会再次取值，Object.defineProperty  Promise/A+ 2.3.3.3
        then.call(x, y => { // 根据 promise 的状态决定是成功还是失败
          if (called) return;
          called = true;
          // 递归解析的过程（因为可能 promise 中还有 promise） Promise/A+ 2.3.3.3.1
          resolvePromise(promise2, y, resolve, reject); 
        }, r => {
          // 只要失败就失败 Promise/A+ 2.3.3.3.2
          if (called) return;
          called = true;
          reject(r);
        });
      } else {
        // 如果 x.then 是个普通值就直接返回 resolve 作为结果  Promise/A+ 2.3.3.4
        resolve(x);
      }
    } catch (e) {
      // Promise/A+ 2.3.3.2
      if (called) return;
      called = true;
      reject(e)
    }
  } else {
    // 如果 x 是个普通值就直接返回 resolve 作为结果  Promise/A+ 2.3.4  
    resolve(x)
  }
}

class Promise {
  constructor(executor) {
    this.status = PENDING;
    this.value = undefined;
    this.reason = undefined;
    this.onResolvedCallbacks = [];
    this.onRejectedCallbacks= [];

    let resolve = (value) => {
      if(this.status ===  PENDING) {
        this.status = FULFILLED;
        this.value = value;
        this.onResolvedCallbacks.forEach(fn=>fn());
      }
    } 

    let reject = (reason) => {
      if(this.status ===  PENDING) {
        this.status = REJECTED;
        this.reason = reason;
        this.onRejectedCallbacks.forEach(fn=>fn());
      }
    }

    try {
      executor(resolve,reject)
    } catch (error) {
      reject(error)
    }
  }

  then(onFulfilled, onRejected) {
    //解决 onFufilled，onRejected 没有传值的问题
    //Promise/A+ 2.2.1 / Promise/A+ 2.2.5 / Promise/A+ 2.2.7.3 / Promise/A+ 2.2.7.4
    onFulfilled = typeof onFulfilled === 'function' ? onFulfilled : v => v;
    //因为错误的值要让后面访问到，所以这里也要跑出个错误，不然会在之后 then 的 resolve 中捕获
    onRejected = typeof onRejected === 'function' ? onRejected : err => { throw err };
    // 每次调用 then 都返回一个新的 promise  Promise/A+ 2.2.7
    let promise2 = new Promise((resolve, reject) => {
      if (this.status === FULFILLED) {
        //Promise/A+ 2.2.2
        //Promise/A+ 2.2.4 --- setTimeout
        setTimeout(() => {
          try {
            //Promise/A+ 2.2.7.1
            let x = onFulfilled(this.value);
            // x可能是一个proimise
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            //Promise/A+ 2.2.7.2
            reject(e)
          }
        }, 0);
      }

      if (this.status === REJECTED) {
        //Promise/A+ 2.2.3
        setTimeout(() => {
          try {
            let x = onRejected(this.reason);
            resolvePromise(promise2, x, resolve, reject);
          } catch (e) {
            reject(e)
          }
        }, 0);
      }

      if (this.status === PENDING) {
        this.onResolvedCallbacks.push(() => {
          setTimeout(() => {
            try {
              let x = onFulfilled(this.value);
              resolvePromise(promise2, x, resolve, reject);
            } catch (e) {
              reject(e)
            }
          }, 0);
        });

        this.onRejectedCallbacks.push(()=> {
          setTimeout(() => {
            try {
              let x = onRejected(this.reason);
              resolvePromise(promise2, x, resolve, reject)
            } catch (e) {
              reject(e)
            }
          }, 0);
        });
      }
    });
  
    return promise2;
  }
}
复制代码
```

测试一下：

```js
const promise = new Promise((resolve, reject) => {
  reject('失败');
}).then().then().then(data=>{
  console.log(data);
},err=>{
  console.log('err',err);
})

复制代码
```

控制台输出：

```js
"失败 err"
复制代码
```

至此，我们已经完成了 promise 最关键的部分：then 的链式调用和值的穿透。搞清楚了 then 的链式调用和值的穿透，你也就搞清楚了 Promise。

**测试 Promise 是否符合规范**

Promise/A+规范提供了一个专门的测试脚本，可以测试所编写的代码是否符合Promise/A+的规范。

首先，在 promise 实现的代码中，增加以下代码:

```js
Promise.defer = Promise.deferred = function () {
  let dfd = {};
  dfd.promise = new Promise((resolve,reject)=>{
      dfd.resolve = resolve;
      dfd.reject = reject;
  })
  return dfd;
}
复制代码
```

安装测试脚本:

```js
npm install -g promises-aplus-tests
复制代码
```

如果当前的 promise 源码的文件名为 promise.js

那么在对应的目录执行以下命令:

```js
promises-aplus-tests promise.js
复制代码
```

promises-aplus-tests 中共有 872 条测试用例。以上代码，可以完美通过所有用例。

------

**感谢小伙伴的提醒，由于文章中使用 setTimeout 实现 promise 的异步，会对大家造成误解。所以这里添加一些标注哈！**

由于原生的 Promise 是V8引擎提供的微任务，我们无法还原V8引擎的实现，所以这里使用 setTimeout 模拟异步，所以原生的是微任务，这里是宏任务。

Promise A+ 规范3.1 中也提到了：

> 这可以通过“宏任务”机制（例如setTimeout或setImmediate）或“微任务”机制（例如MutatonObserver或）来实现process.nextTick。

如果你想实现 promise 的微任务，可以 mutationObserver 替代 seiTimeout 来实现微任务。

有小伙伴说可以使用 queueMicrotask 实现微任务，我也查阅了一些资料，是可以的。不过 queueMicrotask 兼容性不是很好，IE 下完全不支持。据我所知 queueMicrotask 的 polyfill 是基于 promise 实现的，如果不支持 promise 会转成 setTimeout。

总的来说，queueMicrotask 和 mutationObserver 都可以实现微任务机制，不过更建议有执念的小伙伴用 mutationObserver 实现一下，没有执念的小伙伴了解 promise 的微任务机制就好了～

**Promise 的 API**

虽然上述的 promise 源码已经符合 Promise/A+ 的规范，但是原生的 Promise 还提供了一些其他方法，如:

- Promise.resolve()
- Promise.reject()
- Promise.prototype.catch()
- Promise.prototype.finally()
- Promise.all()
- Promise.race(）

下面具体说一下每个方法的实现:

**Promise.resolve**

默认产生一个成功的 promise。

```js
static resolve(data){
  return new Promise((resolve,reject)=>{
    resolve(data);
  })
}
复制代码
```

这里需要注意的是，**promise.resolve 是具备等待功能的**。如果参数是 promise 会等待这个 promise 解析完毕，在向下执行，所以这里需要在 resolve 方法中做一个小小的处理：

```js
let resolve = (value) => {
  // ======新增逻辑======
  // 如果 value 是一个promise，那我们的库中应该也要实现一个递归解析
  if(value instanceof Promise){
      // 递归解析 
      return value.then(resolve,reject)
  }
  // ===================
  if(this.status ===  PENDING) {
    this.status = FULFILLED;
    this.value = value;
    this.onResolvedCallbacks.forEach(fn=>fn());
  }
}
复制代码
```

测试一下：

```js
Promise.resolve(new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('ok');
  }, 3000);
})).then(data=>{
  console.log(data,'success')
}).catch(err=>{
  console.log(err,'error')
})
复制代码
```

控制台等待 `3s` 后输出：

```js
"ok success"
复制代码
```

**Promise.reject**

默认产生一个失败的 promise，Promise.reject 是直接将值变成错误结果。

```js
static reject(reason){
  return new Promise((resolve,reject)=>{
    reject(reason);
  })
}
复制代码
```

**Promise.prototype.catch**

Promise.prototype.catch 用来捕获 promise 的异常，**就相当于一个没有成功的 then**。

```js
Promise.prototype.catch = function(errCallback){
  return this.then(null,errCallback)
}
复制代码
```

**Promise.prototype.finally**

finally 表示不是最终的意思，而是无论如何都会执行的意思。 如果返回一个 promise 会等待这个 promise 也执行完毕。如果返回的是成功的 promise，会采用上一次的结果；如果返回的是失败的 promise，会用这个失败的结果，传到 catch 中。

```js
Promise.prototype.finally = function(callback) {
  return this.then((value)=>{
    return Promise.resolve(callback()).then(()=>value)
  },(reason)=>{
    return Promise.resolve(callback()).then(()=>{throw reason})
  })  
}
复制代码
```

测试一下：

```js
Promise.resolve(456).finally(()=>{
  return new Promise((resolve,reject)=>{
    setTimeout(() => {
        resolve(123)
    }, 3000);
  })
}).then(data=>{
  console.log(data,'success')
}).catch(err=>{
  console.log(err,'error')
})
复制代码
```

控制台等待 `3s` 后输出：

```js
"456 success"
复制代码
```

**Promise.all**

promise.all 是解决并发问题的，多个异步并发获取最终的结果（如果有一个失败则失败）。

```js
Promise.all = function(values) {
  if (!Array.isArray(values)) {
    const type = typeof values;
    return new TypeError(`TypeError: ${type} ${values} is not iterable`)
  }
  
  return new Promise((resolve, reject) => {
    let resultArr = [];
    let orderIndex = 0;
    const processResultByKey = (value, index) => {
      resultArr[index] = value;
      if (++orderIndex === values.length) {
          resolve(resultArr)
      }
    }
    for (let i = 0; i < values.length; i++) {
      let value = values[i];
      if (value && typeof value.then === 'function') {
        value.then((value) => {
          processResultByKey(value, i);
        }, reject);
      } else {
        processResultByKey(value, i);
      }
    }
  });
}
复制代码
```

测试一下：

```js
let p1 = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve('ok1');
  }, 1000);
})

let p2 = new Promise((resolve, reject) => {
  setTimeout(() => {
    reject('ok2');
  }, 1000);
})

Promise.all([1,2,3,p1,p2]).then(data => {
  console.log('resolve', data);
}, err => {
  console.log('reject', err);
})
复制代码
```

控制台等待 `1s` 后输出：

```js
"resolve [ 1, 2, 3, 'ok1', 'ok2' ]"
复制代码
```

**Promise.race**

Promise.race 用来处理多个请求，采用最快的（谁先完成用谁的）。

```js
Promise.race = function(promises) {
  return new Promise((resolve, reject) => {
    // 一起执行就是for循环
    for (let i = 0; i < promises.length; i++) {
      let val = promises[i];
      if (val && typeof val.then === 'function') {
        val.then(resolve, reject);
      } else { // 普通值
        resolve(val)
      }
    }
  });
}
复制代码
```

特别需要注意的是：因为**Promise 是没有中断方法的**，xhr.abort()、ajax 有自己的中断方法，axios 是基于 ajax 实现的；fetch 基于 promise，所以他的请求是无法中断的。

这也是 promise 存在的缺陷，我们可以使用 race 来自己封装中断方法：

```js
function wrap(promise) {
  // 在这里包装一个 promise，可以控制原来的promise是成功还是失败
  let abort;
  let newPromise = new Promise((resolve, reject) => { // defer 方法
      abort = reject;
  });
  let p = Promise.race([promise, newPromise]); // 任何一个先成功或者失败 就可以获取到结果
  p.abort = abort;
  return p;
}

const promise = new Promise((resolve, reject) => {
  setTimeout(() => { // 模拟的接口调用 ajax 肯定有超时设置
      resolve('成功');
  }, 1000);
});

let newPromise = wrap(promise);

setTimeout(() => {
  // 超过3秒 就算超时 应该让 proimise 走到失败态
  newPromise.abort('超时了');
}, 3000);

newPromise.then((data => {
    console.log('成功的结果' + data)
})).catch(e => {
    console.log('失败的结果' + e)
})
复制代码
```

控制台等待 `1s` 后输出：

```js
"成功的结果成功"
复制代码
```

**promisify**

promisify 是把一个 node 中的 api 转换成 promise 的写法。 在 node 版本 12.18 以上，已经支持了原生的 promisify 方法：`const fs = require('fs').promises`。

```js
const promisify = (fn) => { // 典型的高阶函数 参数是函数 返回值是函数 
  return (...args)=>{
    return new Promise((resolve,reject)=>{
      fn(...args,function (err,data) { // node中的回调函数的参数 第一个永远是error
        if(err) return reject(err);
        resolve(data);
      })
    });
  }
}
复制代码
```

如果想要把 node 中所有的 api 都转换成 promise 的写法呢：

```js
const promisifyAll = (target) =>{
  Reflect.ownKeys(target).forEach(key=>{
    if(typeof target[key] === 'function'){
      // 默认会将原有的方法 全部增加一个 Async 后缀 变成 promise 写法
      target[key+'Async'] = promisify(target[key]);
    }
  });
  return target;
}
```



