# JavaScript

## JavaScript简介

Js代码要编写到script标签中

控制浏览器弹出一个警告窗

alert（）；

 

让计算机在页面中输入一个内容

document.write()可以向body中输入一个内容

 

向操作台输出一个内容

console.log()的作用是向控制台输出一个内容

```javascript
<script>
        alert("sb!?");
        document.write("sb?");
        console.log("sb!");
</script>
```

运算符typeof：用来检查一个变量的类型

```javascript
var a=123;
 console.log(typeof a);返回Number型
```

编写位置：

\1. 可以将js代码编写到标签的onclick属性中

当我们点击按钮时，js代码才会执行

但他们的属于结构与行为耦合，不方便维护，不推荐使用

\2. 可以将js代码写在超链接的href属性中，这样当点击超链接时，会执行js代码

```javascript
<body>
    <button onclick="alert('你为什么要点？')">click</button>
    <a href="javascript:alert('为什么要点');">点</a>
    <a href="javascript:;">点</a>
</body>
```

\3. 可以将js代码编写到外部js文件中，然后通过script标签引入

-写到外部文件中可以在不同的页面中同时引用，也可以利用到浏览器的缓存机制（推荐使用）

-但script标签一旦用于引用外部文件了，就不能再编写代码了，即使编写了浏览器也会忽略，如果需要则需要创建一个新的script标签用于编写内部代码

<script src="../script.js"></script><script src="../script.js"></script>

------



## JS基本语法

\1. js中严格区分大小写

\2. js的每一条语句要以分号；结尾

-如果不写，浏览器会自动添加，但会消耗一些系统资源，而且有些时候浏览器会加错分号

\3. Js中会忽略多个空格和换行

### 变量：

在js中使用var关键字来声明一个变量

```
var a=123;
```

标识符：变量名，函数名，属性名

命名规则：

\1. 可以含字母，数字，_下划线，$

\2. 不能以数字开头

\3. 不能是ES中的关键字，保留字

### 数据类型

Js中一共有六种数据类型

String，Number数值，Boolean，Null空值，Undefined未定义,Object对象

前五种为基本数据类型

 

Number数值型：包括整数和浮点数（小数）

Js中可以表示的数字的最大值

Number.MAX_VALUE

-如果使用Number表示的数字超过最大值，则会返回一个Infinity表示正无穷	，-Infinity表示负无穷

Number.MIN_VALUE

-大于0的最小值

NaN是一个特殊的数字，表示Not a Number

 

Null类型的值只有一个：就是null

Undefined类型的值只有一个就是undefined

### 强制类型转换

这里主要指String，Number，Boolean的转换

==将其它数据类型转换为String==

==方法一==：调用被转换数据类型的toString()方法

-该方法不会改变原变量的类型，它会将转换的结果返回

-但要注意null和undefined这两个值没有toString()方法

```
var a=123;
 var b=a.toString()
console.log(typeof b);
```

==方法二==：调用String（）方法，并用被转换的数据作为参数传递给函数

-对于null和undefined这两个值，可以直接使用此方法进行转换为String

```
var a=123;
a=String(a);
console.log(typeof a);
```

==将其它数据类型转换为Number==

==方法一==：使用Number（）函数

-字符串-->数字:

\1. 如果是纯数字，那直接转换

\2. 如果字符串中有非数字内容，则直接转换为NaN

\3. 如果字符串是空格，转换为0

```
var a="123";
a=Number(a);
console.log(typeof a);
```

-布尔-->数字

true 转换为1

false 转换为0

-null-->数字  0

-undefined-->数字 NaN

 

==方法二==：这种方法专门对付字符串

-parseInt()把一个字符串转换为一个整数

-parseInt()可以将字符串的有效整数取出，转换为Number

我们可以传递第二个参数，来指定数字的进制

```
var a="123px";
a=parseInt(a);
console.log(typeof a);
```

-parseFloat()把一个字符串转化为一个小数

-作用和parseInt()类似，但可以取出小数

如果对于非String类型的使用parseInt()和parseFloat()方法，那会先将其转换为String类型

进制：16进制以0x开头

8进制以0开头

2进制以0b开头



==将其它数据类型转换为Boolean==

-使用Boolean（）函数

```
var a="123px";
 a=Boolean(a);
console.log(typeof a);
 console.log(a);
```

-数字-->布尔

-除了0和NaN，其余都为true

 

-字符串-->布尔

-除了空串，其余都是true

 

-null和undefined都会转换为false

-对象也会转换为true

隐式类型转换

任意数据类型+一个“”即可转换为String

其它数据类型使用+，来转换为number



### 运算符

算数运算符

一元运算符

==逻辑运算符==

！非

对于非布尔值的数据进行逻辑非运算，则会向将其转换为布尔值，再取反

&& 与

如果第一个为false，则不会看第二个值

|| 或

如果第一个值为true，则不会看第二个值

&& || 非布尔值的情况

-&&

如果第一个值为true，则必然返回第二个值

如果第一个值为false，则直接返回第一个值

-||

如果第一个值为true，则直接返回第一个值

如果第一个值为false，则返回第二个值

==关系运算符==

非数值的情况

-对于非数值进行比较，会先转换为数字，然后比较

-如果是字符串之间的比较，不会转换为数字比较，而是分别比较字符串中的字符的Unicode编码

==相等运算符==

==相等：如果数据类型不同，会自动类型转换

！=不相等：如果数据类型不同，会自动类型转换，如果转换后相等也会返回false

===全等：和相等类似，但不会自动类型转换，如果数据类型不同，直接false

！==不全等：和不等类似，不会自动类型转换，数据类型不同直接返回true

==条件运算符==

==基本语句==

------



## 对象

对象属于一种复合的数据类型，在对象中可以保存多个不同数据类型的属性

对象的分类：

\1. 内建对象

由ES标准中定义的对象，在任何的ES的实现中都可以使用

比如Math，String，...

\2. 宿主对象：

由JS的运行环境提供的对象，目前来讲主要是指由浏览器提供的对象

比如BOM，DOM

\3. 自定义对象：

由开发人员自己创建的对象



使用new关键字创建对象

```
var obj=new Object;
```

在对象中保存的值称为属性

​	==添加属性==

​		对象.属性名=属性值；

​	==读取属性==：如果对象没有这个属性，会返回undefined

​		对象.属性名

​	==修改属性==

​		对象.属性=新值

​	==删除属性==

​		delete 对象.属性名



如果要使用特殊的属性名，不能采用.的方式操作，

需要使用

对象[“属性名”]=属性值

读取时也要用这种方法

```
        obj["123"]=789;
        console.log(obj["123"]);
```

使用[]这种形式去操作属性，更加灵活

在[]中可以直接传递一个变量，这个变量是多少就会读取哪个属性



==in 运算符==

通过该运算符可以检查一个对象中是否含有指定的属性

如果有则返回true，没有则返回false

“属性名” in 对象

```
console.log("name" in obj);
```

==使用对象字面量来创建一个对象==

使用对象字面量来创建，在创建对象时可以直接指定对象中的属性

{属性名：属性值，属性名：属性值，属性名：属性值，...}

对象字面量的属性名可以加引号，也可以不加引号

如果一些特殊的名字，则必须加引号

```
        var obj={
            name:"Tom",
            age:18
        };
```

------

## 函数function

函数也是一个对象

函数中可以封装一些功能（代码），在需要时可以执行这些功能（代码）

### 函数创建

1.构造方法创建

```
var fun =new Function("console.log(111111);");
```

\2. 函数声明创建

function 函数名（[形参1，....]）{代码}

```
function fun2(){
            console.log(111111);
        }
```

\3. 使用函数表达式创建

```
var fun3=function(){
            console.log(222222)
        };
```

 

封装到函数里面的代码不会立即执行，在调用时执行

调用函数 函数对象（）

fun();



函数的参数

```
function sum(a,b){
            console.log(a+b);
        }
sum(1,2);
```

注：调用函数时解析器不会检查实参的类型



立即执行函数

函数定义完，立即被调用，往往只是执行一次

```
(function(){
            alert("11111634534523");
})();
```



枚举对象中的属性

使用for...in 语句

语法：for(var 变量 in 对象){}

for...in语句对象中有几个属性，循环体就会执行几次

每次执行时，会将对象中的一个属性的名字赋值给变量

```
        var obj={age:"1",abe:"2"};
        for(var n in obj){
            console.log(n);
        }
```



## 作用域：

-作用域指一个变量的作用的范围

-在JS中一共有两种作用域

### 1. 全局作用域

-直接编写在script标签中的JS代码，都在全局作用域

-全局作用域在页面打开时创建，在页面关闭时销毁

-在全局作用域中有一个全局对象window

window它代表的是一个浏览器的窗口，它由浏览器创建我们可以直接使用

-在全局作用域中：

创建的变量都会作为window对象的属性保存

创建的函数都会作为window对象的方法保存

-全局作用域中的变量都是全局变量

在页面的任意的部分都可以访问得到

 

 

### 2.函数作用域

-调用函数时创建函数作用域，函数执行完毕以后，函数作用域销毁

-每调用一次函数就会创建一个新的函数作用域，他们之间是互相独立的

-在函数作用域中可以访问到全局作用域的变量

在全局作用域中无法访问到函数作用域的变量

-当在函数作用域操作一个变量时，它会先在自身作用域中寻找，如果有就直接使用，如果没有则向上一级作用域中寻找，直到找到全局作用域，如果全局作用域中依然没有找到，则会报错ReferenceError

-在函数中要访问全局变量可以使用window对象

 

-函数的作用域也有声明提前的特性

使用var关键字声明的变量，会在函数中所有的代码执行前被声明

函数声明也会在函数中所有的代码执行前执行



## 变量的声明提前

-使用var关键字声明的变量，会在所有的代码执行之前被声明（但是不会赋值）

但是如果声明变量时不是用var关键字，则变量不会被声明提前

函数的声明提前

-使用函数声明形式创建的函数function函数（）{}

它会在所有的代码执行之前就被创建，所以我们可以在函数声明前来调用函数

使用函数表达式创建的函数，不会被声明提前，所以不能在声明前调用



## this 

解析器在调用函数每次都会向函数内部传递进一个隐含的参数

这个隐含的参数就是this，this指向的是一个对象

这个对象我们称为函数执行的上下文对象

根据函数的调用方式不同，this会指向不同的对象

\1. 以函数的形式调用时，this永远都是window

\2. 以方法的形式调用时，this就是调用方法的那个对象

```
        function fun(){
            console.log(this.name);
        }
        fun()
        var obj={
            name:"孙悟空",
            sayName:fun
        };
        var obj2={
            name:"猪八戒",
            sayName:fun
        };
        var name="全局的name属性";
        //方法的形式调用
        obj2.sayName();//结果是猪八戒
        //函数的形式调用
        fun()//结果是：全局的name属性
```



使用工厂方法创建对象：的对象都是Object类型，无法区分不同类型对象

通过此方法可以大批量的创建对象

```
        function createPerson(name,age,gender){
            var obj=new Object();
            obj.name=name;
            obj.age=age;
            obj.gender=gender;
            obj.sayName=function(){
                alert(this.name);
            };
            return obj;
        }
        var obj3=createPerson("孙悟空","18","男");
        var obj4=createPerson("猪八戒","18","男");
        var obj5=createPerson("沙和尚","18","男");
        var obj6=createPerson("唐僧","18","男");

        console.log(obj3);
        console.log(obj4);
        console.log(obj5);
        console.log(obj6);
```

## 构造函数

创建一个构造函数，专门用来创建Person对象的

构造函数就是一个普通的函数，创建方式和普通函数没什么不同

不同的是构造函数习惯上首字母大写

 

构造函数和普通函数的区别就是调用方式的不同

普通函数是直接调用，而构造函数需要使用new关键字来调用

 

### 构造函数的执行流程

\1. 立即创建一个新的对象

\2. 将创建的对象设置为函数中的this，在构造函数中可以使用this来引用新建的对象

\3. 运行执行函数中的代码

\4. 将新建的对象作为返回值返回

```
        function Person(name,age){
            this.name=name;
            this.age=age
        };
        function Dog(){};
        var per=new Person("孙悟空",18);
        var dog=new Dog();
        console.log(per.name);
        console.log(dog)
```

使用同一个构造函数创建的对象，我们称为一类对象，也将一个构造函数称为一个类，我们将通过一个构造函数创建的对象，称为该类的实例

 

使用instanceof可以检查一个对象是否是一个类的实例

语法： 对象 instanceof 构造函数

如果是，返回true，否则返回false

```
        console.log(per instanceof Person);
```



==垃圾回收机制==：我们只需要把不再需要到的对象设置为null，垃圾就会被自动回收



## 原型prototype

我们创建的每个函数，解析器都会向函数中添加员工属性prototype

这个属性对应着一个对象，这个对象就是原型对象

如果函数作为普通函数调用prototype没有任何作用

当函数以构造函数的形式调用时，它所创建的对象都有一个隐含的属性

指向改构造函数的原型对象，我们可以通过__proto__（注：两边各两个下滑线）来访问该属性

 

原型对象就相当于一个公共区域，所有同一个类的实例都可以访问到这个对象，我们可以将对象中共有的内容，统一设置到原型对象中

当我们访问对象的一个属性或方法时，他会先在对象自身中寻找，如果有则直接使用，如果没有则去原型对象中寻找，找到就直接使用

 

当我们使用in检查对象中是否有某个属性时，如果对象没有但原型有，也会返回true

可以使用hasOwnProperty()来检查对象自身中是否含有改属性

 

原型对象也是对象，所以它也有原型，

当我们使用一个对象的属性或方法时，会先在自身去寻找，

自身有直接使用，没有则去原型中找，找到就用

如果原型中也没有，再去原型的原型中找，直到找到Object对象的原型

Object对象的原型没有原型，如果在Object中依然找不到，则返回undifined

 

toString（）方法

当我们直接在页面中打印一个对象时，实际上是输出了对象的toString（）方法的返回值

如果我们希望在输出对象时不输出[object Object]，可以为对象添加一个toString（）方法



## 数组Array

### 数组的四个方法：

push()：

-该方法可以向数组的末尾添加一个或多个元素，并返回数组的新的长度

```
        var arr=["1","2","3"];
        var a=arr.push("4","5");
        console.log(arr);
        console.log(a)
```

pop():该方法可以删除数组的最后一个元素，并将删除的元素作为返回值返回

 

unshift():向数组的开头添加一个或多个元素，并返回新的数组长度

 

shift():删除并返回数组的第一个元素

 

### 数组的遍历

for

forEach()方法：

需要一个函数作为参数，像这种函数，由我们创建但不是由我们调用，称为回调函数

数组有几个元素，函数就执行几次，每次执行，浏览器会将遍历到的元素以实参的形式传递进来

浏览器会在回调函数中传递三个参数

第一个参数：就是当前正在遍历的元素

第二个参数：就是当前正在遍历的元素的索引

第三个参数：就是正在遍历的数组

```
        var arr=["a","b","c"];
        arr.forEach(function(q,w,e){
            console.log(q);
        });
```

### 方法：

slice()

-用来从数组提取指定元素，并将截取到的元素封装到新数组返回

数组对象.slice(start,end):从start索引位置开始，到end结束

包含开始索引位置的元素，不包含结束的

第二个参数可以不写，即截取到最后

可以传递负值，负值表示倒数第几个

```
        var arr=["a","b","c"];
        var s=arr.slice(0,1);
        console.log(s);
```

### 剩余方法

concat() 方法通过合并（连接）现有数组来创建一个新数组：

实例

var myGirls = ["Cecilie", "Lone"];

var myBoys = ["Emil", "Tobias", "Linus"];

var myChildren = myGirls.concat(myBoys);  // 连接 					myGirls 和 myBoys

join() 方法也可将所有数组元素结合为一个字符串。

它的行为类似 toString()，但是您还可以规定分隔符：

实例

var fruits = ["Banana", "Orange","Apple", "Mango"];

document.getElementById("demo").innerHTML = fruits.join(" * "); 

 

Reverse（）

语法：array.reverse()

reverse( )方法：用于颠倒数组中元素的顺序。。

代码示例如下：

var fruits = ["Banana", "Orange", "Apple", "Mango"];

fruits.reverse();

console.log(fruits)//Mango,Apple,Orange,Banana

 

sort( )

语法：array.sort(sortfunction)

用于对数组的元素进行排序。按照Unicode编码排序

代码示例如下：

var Array = [1,2,3,4,5];

var fruits = Array.sort(function(a,b){

​	//return a - b; //从小到大

​	return b-a; //从大到小

})



### 函数的方法：

call（）和apply（）

-这两个方法都是函数对象的方法，需要函数去调用

当对函数调用以上两个方法时，都会调用函数执行

相当于fun();

不同的是在调用以上两个方法时可以将一个对象指定为第一个参数

此时这个对象将会成为函数执行时的this

call（）方法可以将实参在对象之后依次传递

```
function fun(){
            alert("我是fun函数");
        }
        fun.apply();
        fun.call();
        fun();
```

this的情况：

\1. 以函数的形式调用时，this永远是window

\2. 以方法的形式调用时，this是调用方法的对象

\3. 以构造函数的形式调用时，this是新创建的那个对象

\4. 使用call和apply调用时，this是指定的那个对象

### arguments

在调用函数时，浏览器每次都会传递隐含的两个参数

\1. 上下文对象this

\2. 封装实参的对象arguments

-arguments是一个类数组的对象，它可以通过索引来操作数据，也可以获取长度

-在调用函数时，我们所传递的实参都会被保存在arguments中

-arguments.length可以用来获取实参的长度

-即使我们不定义形参，也可以通过arguments来使用实参

-它里面有一个属性：callee

这个属性对应一个函数对象，就是当前正在指向的函数的对象

### Data类

如果直接使用构造函数创建一个Data对象，则会封装为当前代码执行的时间

创建一个指定时间的对象，需要在构造函数中传递一个表示时间的字符串作为参数。格式：月/日/年 时：分：秒

 

### Math类

Math和其它的对象不同，它不是一个构造函数

属于一个工具类，里面有属性和方法

 

### 包装类

：通过三个包装类可以将基本数据类型的数据转换为对象

String（）

Number（）

Boolean（）

但注意，我们在实际应用中不会使用基本数据类型的对象

## String 对象方法

| ***\*方法\****                                               | ***\*描述\****                                       |
| ------------------------------------------------------------ | ---------------------------------------------------- |
| [big()](https://www.w3school.com.cn/jsref/jsref_big.asp)     | 用大号字体显示字符串。                               |
| [blink()](https://www.w3school.com.cn/jsref/jsref_blink.asp) | 显示闪动字符串。                                     |
| [bold()](https://www.w3school.com.cn/jsref/jsref_bold.asp)   | 使用粗体显示字符串。                                 |
| [charAt()](https://www.w3school.com.cn/jsref/jsref_charAt.asp) | 返回在指定位置的字符。                               |
| [charCodeAt()](https://www.w3school.com.cn/jsref/jsref_charCodeAt.asp) | 返回在指定的位置的字符的 Unicode 编码。              |
| [concat()](https://www.w3school.com.cn/jsref/jsref_concat_string.asp) | 连接字符串。                                         |
| [fixed()](https://www.w3school.com.cn/jsref/jsref_fixed.asp) | 以打字机文本显示字符串。                             |
| [fontcolor()](https://www.w3school.com.cn/jsref/jsref_fontcolor.asp) | 使用指定的颜色来显示字符串。                         |
| [fontsize()](https://www.w3school.com.cn/jsref/jsref_fontsize.asp) | 使用指定的尺寸来显示字符串。                         |
| [fromCharCode()](https://www.w3school.com.cn/jsref/jsref_fromCharCode.asp) | 从字符编码创建一个字符串。                           |
| [indexOf()](https://www.w3school.com.cn/jsref/jsref_indexOf.asp) | 检索字符串。                                         |
| [italics()](https://www.w3school.com.cn/jsref/jsref_italics.asp) | 使用斜体显示字符串。                                 |
| [lastIndexOf()](https://www.w3school.com.cn/jsref/jsref_lastIndexOf.asp) | 从后向前搜索字符串。                                 |
| [link()](https://www.w3school.com.cn/jsref/jsref_link.asp)   | 将字符串显示为链接。                                 |
| [localeCompare()](https://www.w3school.com.cn/jsref/jsref_localeCompare.asp) | 用本地特定的顺序来比较两个字符串。                   |
| [match()](https://www.w3school.com.cn/jsref/jsref_match.asp) | 找到一个或多个正则表达式的匹配。                     |
| [replace()](https://www.w3school.com.cn/jsref/jsref_replace.asp) | 替换与正则表达式匹配的子串。                         |
| [search()](https://www.w3school.com.cn/jsref/jsref_search.asp) | 检索与正则表达式相匹配的值。                         |
| [slice()](https://www.w3school.com.cn/jsref/jsref_slice_string.asp) | 提取字符串的片断，并在新的字符串中返回被提取的部分。 |
| [small()](https://www.w3school.com.cn/jsref/jsref_small.asp) | 使用小字号来显示字符串。                             |
| [split()](https://www.w3school.com.cn/jsref/jsref_split.asp) | 把字符串分割为字符串数组。                           |
| [strike()](https://www.w3school.com.cn/jsref/jsref_strike.asp) | 使用删除线来显示字符串。                             |
| [sub()](https://www.w3school.com.cn/jsref/jsref_sub.asp)     | 把字符串显示为下标。                                 |
| [substr()](https://www.w3school.com.cn/jsref/jsref_substr.asp) | 从起始索引号提取字符串中指定数目的字符。             |
| [substring()](https://www.w3school.com.cn/jsref/jsref_substring.asp) | 提取字符串中两个指定的索引号之间的字符。             |
| [sup()](https://www.w3school.com.cn/jsref/jsref_sup.asp)     | 把字符串显示为上标。                                 |
| [toLocaleLowerCase()](https://www.w3school.com.cn/jsref/jsref_toLocaleLowerCase.asp) | 把字符串转换为小写。                                 |
| [toLocaleUpperCase()](https://www.w3school.com.cn/jsref/jsref_toLocaleUpperCase.asp) | 把字符串转换为大写。                                 |
| [toLowerCase()](https://www.w3school.com.cn/jsref/jsref_toLowerCase.asp) | 把字符串转换为小写。                                 |
| [toUpperCase()](https://www.w3school.com.cn/jsref/jsref_toUpperCase.asp) | 把字符串转换为大写。                                 |
| [toString()](https://www.w3school.com.cn/jsref/jsref_toString_string.asp) | 返回字符串。                                         |
| [valueOf()](https://www.w3school.com.cn/jsref/jsref_valueOf_string.asp) | 返回某个字符串对象的原始值。                         |

## ==正则表达式==

用于定义一些字符串的规则

计算机可以根据正则表达式，来检查一个字符串是否符合规则

获取将字符串中符合规则的内容提取出来

 

创建正则表达式的对象：

var 变量=new RegExp(“正则表达式”，“匹配模式”)

test（）

-使用这个方法可以用来检查一个字符串是否符合正则表达式的规则，

如果符合则返回true，否则返回false

在构造函数中可以传递一个匹配模式作为第二参数

可以是：

i 忽略大小写

g 全局匹配模式

```
        var reg=new RegExp("a","i");//检查字符串中是否含有字符a,并忽略大小写
        var str="a";
        var result=reg.test(str);
        console.log(result);
```

使用字面量来创建正则表达式

var 变量=/正则表达式/匹配模式

这个方法更简单，构造函数方法更灵活

```
        reg=/a/i;
        console.log(reg.test("abc"));
```

使用|表示或者的意思

```
        reg=/a|b/i;
        console.log(reg.test("adc"));
```

使用[]也或者的意思，[ab]==a|b

[a-z]表示任意小写的字母	

[^ ]表示除了



==字符串和正则表达式相关的方法==

split()-可以将一个字符串拆分为一个数组

在此方法中传入正则表达式作为参数

这个方法即使没设置全局匹配模式也会全部拆分

```
        var str="1a2b3c4d5"
        var result=str.split(/[A-z]/);
        console.log(result);
```

search()-可以搜索字符串是否含有指定内容

返回索引，没找到就返回-1

match()-根据正则表达式，从一个字符串中将符合条件的内容提取出来

默认情况下match只会找到第一个符合的返回然后停止

我们可以设置全局匹配模式

可以为一个正则表达式设置多个匹配模式

match()返回的是数组

```
        var str="1a2b3c4d5E6"
        var result=str.match(/[A-z]/ig);
        console.log(result);
```

replace()-可以将字符串中指定的内容替换为新内容

参数：1.被替换的内容2.新的内容

默认只会替换一个

```
        var str="1a2b3c4d5E6a7a8"
        result=str.replace(/a/g,"@@@");//设置为全局匹配模式
        console.log(result)
```

## 量词

-通过量词可以设置一个内容出现的次数

-只对它前边的一个内容起作用

-{n}正好出现n次

-{n,m}出现n到m次

-{n,}出现n次以上

+至少一个，相当于{1，}

\* 0个或多个，相当于{0，}

？ 0个或1个，相当于{0，1}

```
        var str="aaa";
        var reg=/a{3}/;
        console.log(reg.test(str));
```

^表示开头

$表示结尾

检查字符串是否以a开头

​    reg=/^a/;

​    reg=/a$/;

 

\w	-任意字母，数字，_

\W -除了任意字母，数字，_

\d -任意数字

\D -除了数字

\s -空格

\S -除了空格

\b -单词边界

\B -除了单词边界

\. 点

\\\ 斜杠



## DOM文档对象模型

JS中通过DOM来对HTML文档进行操作，通过DOM就可以随心所欲的操作web页面

文档：整个HTML页面文档

对象：网页的每个部分都可以转换为一个对象

模型：表示对象之间的关系

![image-20230225153045784](C:\Users\ZWK\AppData\Roaming\Typora\typora-user-images\image-20230225153045784.png)



## 事件

就是用户和浏览器之间的交互行为

比如点击按钮，鼠标移动等

我们可以在事件对应的属性中设置一些js代码

这样当事件被触发时，这些代码将会执行

 

这种写法我们称为结果和行为耦合，不方便维护，不推荐使用

 

可以为按钮的对应事件绑定处理函数的形式来响应事件

这样当事件被触发时，其对应的函数将会被调用

```
<button id="btn" >我是一个按钮</button>
    <script type="text/javascript">
        var btn =document.getElementById("btn");
        btn.onclick=function(){
            alert("你点什么？");
        };
    </script>
```

浏览器加载页面时是按自上向下的顺序加载的

读取一行就运行一行，如果将script标签写到页面的上边，在代码执行时，页面还没有加载，页面没有加载DOM对象也没有加载会导致无法获取到DOM对象

 

将js代码编写到页面的下面就是为了，可以在页面加载完毕以后在执行js代码



### onload

事件会在整个页面加载完成之后才触发

为window绑定一个onload事件

该事件对应的响应函数将会在页面加载完成之后执行

这样可以确保我们的代码执行时所有的DOM对象已经加载完毕了



## 获取元素节点

：通过document对象调用

\1. getElementById()-通过id属性获取一个元素节点对象

\2. getElementsByTagName()-通过标签名获取一组元素节点对象，返回一个类数组对象

\3. getElementsByName()-通过name属性获取一组元素节点对象



==innerHTML== 通过这个属性可以获取到元素内部的html代码

-用于获取元素内部的代码，所有自结束标签没有内部，所以不能用

==innerText==

-该属性可以获取到元素内部的文本内容

-和innerHTML类似，不同的是它会自动将html去除

如果需要读取元素节点的属性，直接使用元素.属性名

-例如;元素.id	

但class属性不能用这种方式，需要使用元素.className



### 获取元素节点的子节点

通过具体的元素节点调用

\1. getElementsByTagName()

-方法，返回当前节点的指定标签名后代节点

\2. childNodes

-属性，表示当前节点的所有子节点

会获取包括文本节点在内的所有节点

根据DOM标签标签空白也会当成文本节点

-children属性可以获取当前元素的所有子元素

\3. firstChild

-属性，表示当前节点的第一个子节点（包括空白文本节点）

\4. lastChild

-属性，表示当前节点的最后一个子节点



### 获取父节点和兄弟节点

：通过具体的节点调用

\1. parentNode

-属性，表示当前节点的父节点

\2. previousSibling

-属性，表示当前节点的前一个兄弟节点(也可能获取到空白的文本节点)

\3. nextSibling

-属性，表示当前节点的后一个兄弟节点



在document中有一个属性body，它保存的是body的引用

document.documentElement保存的是html根标签

document.all代表的是页面中的所有的元素

 

根据元素的class属性来查询一组元素

getElementsByClassName(“class名”)



document.querySelector();

需要一个选择器的字符串作为参数，可以根据一个css选择器来查询元素节点对象

使用该方法总会返回唯一一个元素，如果满足条件的元素有多个，那么它只是会返回第一个

 

而document.querySelectorAll();它会将符合条件的元素封装到一个数组中返回，即使只有一个符合条件也会返回数组



## ==Dom增删改==

 

### document.createElement()

-可以创建一个元素节点对象

-需要一个标签名作为参数，将会根据该标签名创建元素节点对象，并将创建好的对象作为返回值返回

 

### document.createTextNode()

可以用来创建一个文本节点对象

需要一个文本内容作为参数，将会根据内容创建文本节点，并将新的节点返回

 

### appendChild()

向一个父节点中添加一个新节点

用法：父节点.appendChild(子节点)；

 

### insertBefore(新子，旧子)

可以在指定的子节点前插入新的子节点

语法：父节点.insertBefore(新子，旧子)

 

### replaceChild()

可以使用指定的子节点替换已有的子节点

语法：父节点.replaceChild(新节点，旧节点)

 

### removeChild()

可以删除一个子节点

语法：父节点.removeChild(子节点)

 

常用：子节点.parentNode.removeChild(子节点)

-此方法不用单独获取父节点



使用innerHtml也可以完成DOM的增删改相关操作

-但一般不使用该方法，我们会结合两种方式使用

```javascript
window.onload=function(){
            // 创建广州节点，添加到city下
            myClick("btn01",function(){
                // 创建广州节点
                    // 创建元素节点
                    var li = document.createElement("li");
                    // 创建广州文本节点
                    var gzText=document.createTextNode("广州");
                    // 将广州设置为li的文本
                    li.appendChild(gzText);
                    
                // 添加到city下
                    var city=document.getElementById("city");
                    city.appendChild(li);
            });

            // 将广州节点插入到#bj前面
            myClick("btn02",function(){
                // 创建广州节点
                var li = document.createElement("li");
                var gzText=document.createTextNode("广州");
                li.appendChild(gzText);
                // 获取bj节点
                var bj=document.getElementById("bj");
                // 插入bj前面
                var city=document.getElementById("city");
                city.insertBefore(li,bj)
            });
            // 使用广州节点替换bj节点
            myClick("btn03",function(){
                // 创建广州节点
                var li = document.createElement("li");
                var gzText=document.createTextNode("广州");
                li.appendChild(gzText);
                // 获取bj节点
                var bj=document.getElementById("bj");
                var city=document.getElementById("city");
                city.replaceChild(li,bj);
            });
            
            // 删除#bj节点
            myClick("btn04",function(){
                var bj=document.getElementById("bj");
                var city=document.getElementById("city");
                city.removeChild(bj);
                
            });
            // 读取#city内的HTML代码
            myClick("btn05",function(){
                var city=document.getElementById("city");
                alert(city.innerHTML);
            });
            // 设置#bj内的HTML代码
            myClick("btn06",function(){
                var bj=document.getElementById("bj");
                bj.innerHTML="海南";
            });
        };
        function myClick(idStr ,fun){
            var btn=document.getElementById(idStr);
            btn.onclick=fun;
        }
```



### 添加删除记录

 

点击超链接以后，超链接会跳转页面，这个是超链接的默认行为

但我们不需要默认行为，可以通过响应函数的最后return false来取消默认行为

 

confirm()用于弹出一个带有确认和取消按钮的提示框

需要一个字符串作为参数，该字符串将作为提示文字显示出来

如果用户点击确认则会返回true，如果点击取消则返回false

 

for循环会在页面加载完成之后立即执行

而响应函数会在超链接被点击时才执行

当响应函数执行时，for循环早已经执行完毕



## ==使用DOM操作css样式==

通过JS修改元素的样式：修改的是内联样式

语法：元素.style.样式名=”样式值”

注意：如果css的样式名中含有减号，这种名称在js中是不合法的

如：background-color

需要将这种样式名修改为驼峰命名法

即去掉-，然后将-后面的字母大写（单词首字母）

 

读取元素的样式

语法：元素.style.样式名

这个方法读取的也是内联样式，无法读取样式表中的样式



读取元素当前显示的样式

语法：getComputedStyle()这是window的方法可直接使用

必须要两个参数

第一个：要获取样式的元素

第二个：可以传递一个伪元素，一般都传null

该方法会返回一个对象，对象中封装了当前元素对应的样式



ClientWidth

ClientHeight

-这两个属性可以获取元素的可见宽度和高度

-这些属性都是不带px的，返回都是一个数字，可以直接进行计算

-会获取元素的宽度和高度，包括内容区和内边距

-这些属性都是只读的，不能修改

 

offsetWidth

offsetHeight

-获取元素的整个的宽度和高度，包括内容区和内边距和边框

 

offsetParent

-可以用来获取当前元素的定位元素父元素

-会获取到离当前元素最近的开启了定位的祖先元素

-如果所有的祖先元素都没有开启定位，则返回body

 

offsetLeft

-当前元素相对于其定位元素的水平偏移量

offsetTop

-当前元素相对于其定位元素的垂直偏移量

 

scrollWidth

scrollHeight

-可以获取元素整个滚动区的宽度和高度

 

scrollLeft

-可以获取水平滚动条滚动的距离

scrollTop

-可以获取垂直滚动条滚动的距离

 

当满足scrollHeight-scrollTop==clientHeight

说明垂直滚动条滚动到底了

当满足scrollWidth-scrollLeft==clienWidth

说明水平滚动条滚动到底了

 

Onscroll

-该事件会在元素的滚动条滚动时触发

 

disabled属性可以设置一个元素是否禁用

如果设置为true，则元素禁用

如果是false，则元素可用



## 事件对象

-当我们的事件响应函数被触发时，浏览器每次都会将一个事件对象作为实参传递进响应函数

-在事件对象中封装了当前事件相关的一切信息，比如，鼠标的坐标，键盘按下哪个按键，鼠标滚轮滚动的方向

onmousemove

该事件会在鼠标在元素中移动时被触发

clientX可以获取鼠标指针的水平坐标

clientY可以获取鼠标指针的垂直坐标

 

pageX和pageY可以获取鼠标相对于当前页面的坐标



## 事件的冒泡

：是事件的向上传导，当后代元素上的事件被触发时，其祖先元素的相同事件也会被触发

-在开发中冒泡大多数情况下都是有用的

-如果不希望发生冒泡，可以通过事件对象来取消冒泡

```
       // 可以将事件对象的cancelBubble设置为true，即可取消冒泡
                event.cancelBubble=true;
```

## 事件的委派：

-指将事件统一绑定给元素的共同的祖先元素，这样当后代元素上的事件触发时，会一直冒泡到祖先元素，从而通过祖先元素的响应来处理事件

-事件委派是利用了冒泡，通过委派可以减少事件绑定的次数，提高程序的性能

target

-event中的target表示的触发事件的对象

 

事件的委派

 

使用 对象.事件 = 函数 的形式绑定响应函数

它只能同时为一个元素的一个事件绑定一个响应函数

不能绑定多个，如果绑定了多个，则后面会覆盖掉前边的

addEventListener()

-通过这个方法可以为元素绑定响应函数

-参数：

\1. 事件的字符串，不要on

\2. 回调函数，当事件触发时该函数会被调用

\3. 是否在捕获阶段触发事件，需要一个布尔值，一个都传false

-使用这个方法可以同时为一个元素的相同事件同时绑定多个响应函数

-这样当事件被触发时，响应函数将会按照函数的绑定顺序执行

 

## 事件的传播

-关于事件的传播网景公司和微软公司理解不同

-微软认为事件应该是由里向外传播的，也就是当事件触发时，应该先触发当前元素的响应事件，然后再向其祖先元素传播，也就就是说事件应该再冒泡阶段执行

-网景公司认为事件应该是由外向内传播的，也就是事件触发时，应该先触发当前元素的最外层的祖先元素的事件，然后再向内传播给后代元素

 

W3c中和了两个公司的看法，将事件传播分为三个阶段

\1. 捕获阶段

在捕获阶段时从最外层的祖先元素，向目标元素进行事件的捕获，但是默认此时不会触发事件

 

\2. 目标阶段

事件捕获到目标元素，捕获结束开始在目标元素上触发事件

 

\3. 冒泡阶段

事件从目标元素向他的祖先元素传递，依次触发祖先元素上的事件

 

如果希望在捕获阶段就触发事件，可以将addEventListener()的第三个参数设置为true

-一般情况下不会希望在捕获阶段触发事件，所有这个参数一般都是false



## 拖拽

流程：

\1. 当鼠标被拖拽元素按下时开始拖拽，onmousedown

\2. 当鼠标移动时，被拖拽元素跟随鼠标移动,onmousemove

\3. 当鼠标松开时，被拖拽元素固定在此时的位置,onmouseup



## 键盘事件

onkeydown

-按键被按下

-对于onkeydown来说如果一直按着某个按键不松手，则事件会一直触发

-当onkeydown连续触发时，第一次和第二次之间隔限会稍微长一点，其他的会非常的快，这样设计是为了防止误触发发生

 

在文本框中输入内容，属于onkeydown的默认行为

如果在其中取消了默认行为，则输入的内容，不会出现在文本框中



==onkeyup==

-按键被松开

 

键盘事件一般都会绑定给一些可以获取焦点的对象或是document

 

可以通过keyCode来获取按键的编码

通过他可以判断哪个按键被按下

除了keyCode，事件对象中还提供了几个属性

altKey

ctrlKey

shiftKey

-这三个用来判断各自对应的按钮是否被按下

按下则返回true，否则返回false



## BOM -浏览器对象模型

-BOM可以使我们通过JS来操作浏览器

-在BOM中为我们提供了一组对象，用来完成对浏览器的操作

-BOM对象

 

Window

-该对象代表的是整个浏览器的窗口，同时window也是网页的全局对象

 

 

Navigator

-代表的当前浏览器的信息，提供该对象可以识别不同的浏览器

由于历史原因，Navigator对象的大部分属性都已经不能帮助我们识别浏览器了

一般我们只会使用userAgent来判断浏览器的信息

userAgent是一个字符串，这个字符串包含有用来描述浏览器信息的内容，不同的浏览器会有不同的userAgent

如果userAgent不能判断，那可以用ActiveXObject来判断

 

 

Location

-代表当前浏览器的地址栏信息，通过它可以获取地址栏信息，或者操作浏览器跳转页面

如果直接打印Location可以获取到地址栏的信息（当前页面的完整路径）

如果直接将location属性修改为一个完整的路径，或相对路径，则	我们页面会自动跳转到该路径，并且会生成相应的历史记录

assign（）

-用来跳转到其他页面，作用和直接修改location一样

reload（）

-重新加载当前页面，作用相当于刷新按钮一样

reload（true）：表示强制清空缓存刷新

replace()

-可以使用一个新的页面替换当前页面，调用完毕也会跳转页面		但不会生成历史记录，即不能返回

 

History

-代表浏览器的历史记录，可以通过该对象来操作历史记录

由于隐私的原因，该对象不能获取到具体的历史记录，只能操作浏览器向前向后翻页，而且该操作只在当次访问有效

-length-属性，可以获取到当前访问的链接数量

-back（）-可以用来回退到上一个页面，作用和浏览器的回退按钮一样

-forward()-可以跳转到下一个页面，作用和浏览器的前进按钮一样

-go()-可以用来跳转到指定的页面，

-它需要一个整数作为参数

1：表示向前跳转一个页面 相当于forward()

2：表示向前跳转两个页面

-1：表示向后跳转一个页面

 

Screen

-代表用户屏幕的信息，通过该对象可以获取到用户的显示器的相关信息

 

 

这些BOM对象在浏览器中都是作为window对象的属性保存的，

我们可以通过window对象使用，也可以直接使用



## ==定时器==

简介：

JS的程序的执行速度是非常非常快的

如果希望一段程序，可以每间隔一段时间执行一次，可以使用定时调用

 

setInterval()

-定时调用

可以将一个函数每隔一段时间执行一次

参数：

\1. 回调函数，该函数会每隔一段时间被调用一次

\2. 每次调用间隔的时间，单位是毫秒

这个方法会有一个返回值

返回一个Number类型的数据

这个数字用来作为定时器的唯一标识

 

clearInterval()

-用来关闭定时器，方法中需要一个定时器的标识，这样其会关闭标识对应的定时器

可以接收任意参数，如果参数是一个有效的定时器标识，则停止对应定时器，如果不是有效标识，则什么也不做

```
<script>
        window.onload=function(){
            var count=document.getElementById("count");
            var num=1;
            var timer=setInterval(function(){
                count.innerHTML=num++;
                if(num==11){
                    clearInterval(timer);
                };
            },1000)
        };
    </script>
</head>
<body>
    <h1 id="count">0</h1>
```

目前：我们每点一次按钮，就会开启一个定时器

点击多次会开启多个，而且只能关闭最后一个定时器



## 延时调用

setTimeout()

-一个函数不马上执行，而是隔一段时间之后执行，而且它只是会执行一次

和定时调用区别，定时会执行多次

 

clearTimeout()

-关闭一个延时调用

延时调用和定时调用是可以互相代替的，在开发中可以根据开发需要去选择



## 类的操作

 

通过style属性来修改元素的样式，每修改一个样式，浏览器都会重新进行一次渲染页面，

这样的执行的性能是比较差的，而且这种形式当我们要修改多个样式时，也不方便

我们可以通过修改元素的class属性来间接的修改样式，这样一来，我们只需要修改一次，即可同时修改多个样式，浏览器也只需要重新渲染一次页面，性能较好，并且这种方式，可以使表现与行为进一步的分离