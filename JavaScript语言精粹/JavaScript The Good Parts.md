##### 一，精华

取其精华去其糟粕——

一个method方法去定义一个新方法：

```javascript
Founction.prototype.method = function(name,func){
    this.prototype[name]=func;
    return this;
};
```

##### 二，语法

###### 空白

分隔字符序列（必须）

`var that = this` `var` 和 `that` 之间的空格不能移除

###### 注释

推荐使用行注释：多行注释可能和正则表达式冲突

###### 标识符

用于

- 语句
- 变量
- 参数
- 属性名
- 运算符
- 标志

###### 数字

内部被表示为**64位浮点数**

- 没有分离出整数类型，`1` 和 `1.0` 的值相同
- 避免了短整型溢出问题

`NaN`：不等于任何值（包括自己）

`Infinity` : 表示大于上界的值

###### 字符串

字符串字面量被包在一对单引号或双引号中，是**16位**的Unicode字符集

`length`属性

相同的字符串：

- 字符完全相同
- 字符顺序完全相同

###### 语句

没有块级作用域

函数有局部作用域

那些值被当做假（false）：

- false
- null
- undefined
- 空字符串` `
- 数字0
- 数字`NaN`

那些值被当做真（true）：

- true
- "false"
- 所有对象

JavaScript不允许

- 在`rerurn` 关键字和表达式之间换行

- 在`break`关键字和标签之间换行

###### 表达式

最简单的表达式是：

- 字面量值（字符串，数字）
- 变量
- 内置的值（true，false，null，undefined，NaN和Infinity）
- 以new开头的调用表达式
- 以delete开头的属性提取表表达式
- 包在圆括号中的表达式
- 以一个前置运算符作为前导的表达式

或者表达式后面跟着：

- 一个内置运算符与一个表达式
- 三元运算符
- 一个函数调用
- 一个属性提取表达式

运算符优先级

| `.` `[]` `()`                        |
| ------------------------------------ |
| `delete` `new` `typeof` `+` `-`  `!` |
| `*` `/` `%`                          |
| `+` `-`                              |
| `>=` `<=` `>` `<`                    |
| `===` `!==`                          |
| `&&`                                 |
| `||`                                 |
| `?:`                                 |

`typyof`产生的值：

- number
- string
- boolean
- undefined
- object or function

**数组和`null`的typeof操作都是object**

`&&`和`||`都有**短路操作**的特性

###### 字面量

- number literal
- string literal
- object literal
- array literal
- function
- regexp literal

###### 函数



##### 三，对象

**JavaScript中的对象是可变的键控集合（keyed collections）**

对象是属性的容器

- 属性名
  - 可以是包含空字符串在内的**任意字符串**（非法标识符...）
- 属性值
  - 除`uncdefined`外的**任意值**（函数...）

对象的属性的名字和值没有限制，原因是对象是无类型（JavaScript是弱类型）

对象适合于**汇集和管理数据**（json）轻易表示成

- 树形结构
- 图形结构

###### 对象字面量

```javascript
var stooge = {
    "first-name": "sara",
    "last-name": "how"
}
```

- 非常方便的创建新对象

###### 检索

如果属性名是合法的标识符且不是保留字，不强制要求用引号括住属性名

- 用`.` 语法访问对象的属性

如果属性名是非法的标识符，用引号括住属性名是必须得

- 用`[]`访问对象的属性

优先考虑使用`.` 表示法——更紧凑，可读性更好

检索一个并不存在的成员属性的值，将返回`undefined`

`||`运算符可以用来填充默认值

`var middle = stooge["middle-name"] || "(none)"`

###### 更新

如果属性名已经存在于对象中

- 属性值将被替换

如果之前没有拥有那个属性名，

- 该属性将被扩冲到到对象中

###### 引用

对象通过引用传递

###### 原型

所有通过对象字面量创建的对象都连接到`Object.prototype`——JavaScript的标配对象

**创建对象可以选择某个对象作为它的原型——为Object增加一个create方法**

```javascript
if(typeof Object.beget!=="function"){
    Object.create=function(o){
        var F = function(){}
        F.prototype=o;
        return new F();
    }
}
var another_stooge = Object(stooge);
```

向上搜索原型链——**委托**

原型关系是**动态关系**

###### 反射

处理掉不需要的属性

- 检查并丢弃（typeof）
- `hasOwnProperty()`:**对象私有的属性，不会检查原型链**

###### 枚举

`for in`语句遍历对象中**所有的属性名（包括函数和原型中的属性）**

确保属性以特定的顺序出现，避免使用`for in` 语句，而是创建一个数组以正确的顺序包含属性名

- 通过使用for循环
- 避免了原型链中的属性
- 按照自定义顺序取得了值

###### 删除

`delete`用来删除对象中的属性

- 不会触及原型链中的对象
- 可能会暴露原型链中同名属性

###### 减少全局变量污染

**创建唯一一个全局变量**

- 该变量成为一个容器
- 将全局性的资源纳入一个名称空间之下
- 降低与其他组件，类库的冲突

**使用闭包**

##### 四，函数

函数是JS的基础模块单元用于

- 代码复用
- 信息隐藏
- 组合调用

> 所谓编程就将需求分解为一组函数和数据结构的技能

###### 函数对象

函数也是对象——无序名值对的集合并且链接到对象原型

- 对象字面量产生的对象连接到`Object.prototype`
- 函数对象连接到`Founction.prototype`
  - 该原型对象连接到`Object.prototype`

函数在创建时会附加**两个隐藏属性**

- 函数执行上下文
- 实现函数行为的代码
  - JS创建一个函数对象时，会给该对象设置一个**调用属性**
  - 调用一个函数时，可以理解为：调用此函数的调用属性

函数对象在创建时会配一个`prototype`属性

- 拥有`constructor`属性，且值就是该函数的的对象
- 意义：to do

由于函数是对象，所以函数可以向任何其他的值一样被使用

- 函数可以保存在
  - 变量
  - 对象
  - 数组
- 函数可以被当做参数传递
- 函数可以再返回函数
- 函数可以拥有方法

###### 函数字面量

函数对象可以通过函数字面量来创建

```javascript
var add = function(a,b){
    return a+b;
}
```

第一部分：保留字`function`

第二部分：函数名（**可以省略**）。没有函数名被称为匿名函数

第三部分：包围在圆括号中的参数（多个参数用逗号隔开），参数的名字将被定义为函数中的变量

第四部分：包围在花括号中的语句——函数体

> 函数字面量可以出现在任何表达式出现的地方

通过字面量创建的函数对象包含一个连接到外部上下文的的连接——闭包（**强大之处**）

###### 调用

调用运算符是跟在**任何产生一个函数值的表达式**之后的一对圆括号

当实际参数（arguments）的个数和形式参数（parameters）不匹配时：

- 如果实参过多，超出的参数值会被忽略
- 如果实参过少，缺少的值会被替换为`undefined`
- 参数值不会进行类型检查（任何类型的值都可以被传递给任何参数）

调用一个函数时，会暂停当前函数的执行，传递控制权和参数给新函数

- 除了声明时定义的形式参数
- 每个函数还接收两个附加的参数
  - this（取决于调用模式（如何初始化this）——4种）
    - 方法调用模式
    - 函数调用模式
    - 构造器调用模式
    - apply调用模式
  - arguments

###### 方法调用模式

方法：一个函数被保存为对象的一个属性

当一个方法调用时`this`被绑定到**该对象**

- 方法可以使用`this`访问自己所属的对象
  - 从对象中提取值
  - 或修改对象
- `this`到对象的绑定发生在调用的时候——**延迟绑定（vary late binding）**

通过`this`可取得他们所属对象的上下文的方法——**公共方法（public method）**

```javascript
var myObject = {
    value:0,
    increment:function(num){
        this.value+= typeof num=== "number" ? num : 1;
    }
};

myObject.increment();
myObject.value;//1

myObject.increment(2);
myObject.value;//3
```

###### 函数调用模式

函数不是一个对象的属性时，就可以当做一个函数来调用；

函数调用模式，`this`被被绑定到**全局对象**

- 这是语言设计的一个错误：
  - 内部函数被调用时，this应该绑定到外部函数的this变量
  - 这个实际的结果是，方法不能利用内部函数来帮助它工作（this被绑定到了全局对象）

解决方法：`that=this`

```javascript
myObject.double = function(){
    var that = this;
    
    var helper = function(){
        that.value = add(that.value,that.value);
    };
    
    helper();
};

myObject.doulbe();
```

###### 构造器调用模式

如果在一个函数前面带上`new`操作符来调用，那么

- 将创建一个连接到该函数原型（prototype）的新对象
- this会绑定到新对象上
- `new`也会改变`return`语句的行为

```javascript
var Quo = function(string){
    this.status = string;
};

Que.prototype.get_status=function(){
    return this.status;
};

var myQue = new Que("confused");
```

如果函数创建的目的就是结合new前缀来调用，这样的函数被称为构造函数

- 构造函数名-首字母大写（约定）

###### apply调用模式

JS是函数式的面向对象编程语言，函数可以拥有方法（函数也是对象）

```javascript
var statusObject = {
    status:"a-ok"
};
Quo.prototype.get_status.apply(statusObject);//改变了this的指向
```

###### 参数

参数列表——`arguments`

- 伪数组（array-like）
- 拥有一个`lengths`属性
- 没有数组的方法
  - 可以添加
  - 或者调用（apply）

###### 返回

函数返回时，会把控=控制权交还给调用该函数的程序

- return执行时，立即返回
- 没有指定返回值，则返回undefined
- 如果函数用new操作符调用，返回this（新对象）

###### 扩充类型的功能

JS允许给语言的基本类型扩充功能

- 对象
- 函数
- 数组
- 字符串
- 数字
- 正则表达式
- 布尔值

给所有函数增加方法：

```javascript
Function.prototype.method = function(name,func){
    this.prototype[name] = func;
    return this;
}
```

提取数字中的整数部分：

```javascript
Number.method("integer",function(){
    return Math[this<0?"ceil":"floor"](this);
});
```

通过给基本添加方法，极大的增加了语言的变现力

只有确定没有方法才添加

```javascript
Function.prototype.method = function(name,func){
    if(!this.prototypr[name]){
        this.prototype[name] = func;
    }
    
    return this;
}
```

`hasOwnProperty`删选继承而来的属性

###### 递归

直接或间接调用自己的函数

> 汉诺塔问题

**尾递归**：函数返回自身调用的结果（可以**被替换为循环**）

```javascript
//to do
```

###### 作用域

**作用域控制着变量与参数的可见性和生命周期**

JS中没有块级作用域

- JS中有函数作用域
  - 定义在函数中的参数和变量在函数外是不可见的
  - 在函数内部定义的变量，在该函数内部任何位置都可见
- （建议）在函数的顶部声明函数中可能用到的所有变量
  - 其他语言中推荐尽肯能的延迟声明变量

###### 闭包

作用域的好处：

- 内部函数可以访问定义它们的外部函数的参数和变量（出了this和arguments）
- 内部函数拥有比它的外部函数更长的生命周期

```javascript
var myObject = (function(){
    var value = 0;
    
    return {
        increment:function(num){
        	this.value+= typeof num=== "number" ? num : 1;
    	},
        getValue:function(){
            return value;
        }
    };
}());

//字面量的方式
var myObject = {
    value:0,
    increment:function(num){
        this.value+= typeof num=== "number" ? num : 1;
    },
    getValue:function(){
        return value;
    }
};
```

和以对象的字面量的方式去初始化myObject不同

- 该函数返回一个包含increment和getValue方法的字面量对象去初始化myObject
- myObject对increment和getValue方法总是可用的（可继续访问value）（why？—**作用域链**）
- **函数的作用域使increment和getValue方法对其他程序不可见**

another simple：

```javascript
//闭包的方式
var quo = function(status){
    return {
        get_status:function(){
            return status;
        }
    }
}

var myQue = quo("amazed");


//通过Quo构造器产生一个带有status属性和get_status方法的对象
var Quo = function(string){
    this.status = string;
};

Que.prototype.get_status=function(){
    return this.status;
};

var myQue = new Que("confused");

```

`quo()`函数被设计成无需new操作符调用（因此名字没有大写）

当调用`quo`函数时，

- 返回一个包含get_status方法的新对象
- 新对象的一个引用保存在myQuo
- 即使quo已经被返回了，但是get_status仍然享有访问quo对象的status属性的特权

一个糟糕的例子：

```javascript
var add_the_handlers = function(nodes){
    var i;
    for(i=0;i<nodes.length;i++){
        nodes[i].onclick = function(e){
            alert(i);
        }
    }
}
```

- 事件处理函数绑定了`i`本身

结束糟糕的例子：

```javascript
var add_the_handlers = function(nodes){
    var i;
    for(i=0;i<nodes.length;i++){
        nodes[i].onclick = function(i){
            return function(e){
                alert(i);
            }
        }(i);
    }
}
```

###### 回调

**函数使不连续事件的处理变得更容易**

- 异步请求

```javascript
//同步请求，假死现象
request = prepare_the_request();
response = send_request_synchronously(request);
display(response);

//异步请求,一接收到响应立即调用
request = prepare_the_request();
send_request_synchronously(request，function(){
                  display(response);         
         });
```

###### 模块

**模块是一个提供接口却隐藏状态与实现的函数或对象**——促进了信息隐藏和其他优秀的设计实践

- 比如应用程序的封装
- 和构造单例对象

可是使用函数和闭包来构造模块

- 这样就可以**完全摒弃全局变量**

simple1：给String增加一个deentityify方法——寻找字符串中的HTML字符实体并将它们替换为对应字符串

- 需要在一个对象中保存字符实体的名字和它们对应的字符（**在哪里保存呢？**）
  - 放到一个全局变量中（魔鬼）
  - 把它定义到函数内部，但是会带来运行时的损耗
    - 每次执行该函数的时候该字面量都会被求值一次
  - 放入闭包中（理想的办法）
    - 还可以增强更多字符创实体的方法

```javascript
String.method("deentityify",function(){
    //字符实体表，映射字符实体名字到对应的字符
    var entity = {
        quot:`"`,
        lt:`<`,
        gt:`>`
    };
    //返回deentityify方法
    return function(){
        return this.replace(...,function(...){
            ...
        })
    }
}());
```

模块模式也可以用来**产生安全的对象**：

simple2：构造一个产生序列号的对象

```javascript
var serial_maker = function (){
    var prefic = "";//前缀
    var seq = 0;//序列号
    
    return {//返回产生字符串的对象
        
        set_prefix:function(){//设置前缀的方法
            ...
        },
        set_seq:function(){//设置序列号的方法
            ...
        },
        gensym:function(){//产生字符串的方法
            ...
        }
    }
}
            
var seqer = serial_maker();
```

seqer包含的方法都没有用到this和that，因此没有办法损害seqer

seqer对象是可变的

- 它的方法可以被替换掉，但是替换掉的方法不能访问私有变量

###### 级联

有一些方法没有返回值

- 比如设置和修改对象的某个状态却不返回任何值的方法
  - 可以让这些方法返回this
  - 而不是undefined
  - 就可以启用级联

###### 柯里化

允许把函数与传递给它的参数相结合产生一个新的函数

to do

###### 记忆

函数可以将先前操作的结果记录在某个对象里，从而避免无谓的重复运算

- 这种优化被称为——**记忆（memoization）**

- 可以通过对象和数组实现

```javascript
var fibonacci = function(n){
    return n<2?n:fibonacci(n-1)+fibonacci(n-2);
}
//做了很多重复的工作

//改进
//memo数组保存存储结果，隐藏在闭包中
var fibonacci = function(){
    var memo = [0,1];
    var fib = function(n){
        var rerult = memo[n];
        if(typeof result!== "number"){
            result = fib(n-1)+fib(n-2);
            memo[n] = rerult;
        }
        return rerult;
    }
    return fib;
}();
```

推而广之：编写一个函数来构造带记忆功能的函数

```javascript
var memoizer = function(memo,formula){
    var recur = function(n){
        if(typeof result!== "number"){
            result = formula(recur,n);
            memo[n] = rerult;
        }
        return rerult;
    }
    return recur;
}
//memoizer函数取得memo数组和formula函数
//返回一个管理memo存储和在需要时调用formula函数的recur函数
```

通过设计这种产生另一个函数的函数，极大的减少了工作量：

例如产生一个可记忆的阶乘函数，只需提供阶乘公式

```javascript
var fatorial = memoizer([1,1],functon(recur,n){
           return n*recur(n-1);            
});
```



##### 五，继承

继承提供两个有用的服务

- 代码重用的一种形式
  - （新的类和已经存在的类大部分相似，只需说明不同点）
  - 显著的提高开发效率（非常重要）
- 引入了一套类型系统的规范

JS是一种基于原型的语言，可以模拟那些基于类的模式

- 方法很多
- **保持简单**

###### 伪类

JS中不让，**不直接让对象从其他对象继承**，**反而插入一个多余的间接层**：通过构造函数产生对象

当一个函数对象被创建时，`Function构造器`产生的函数对象会运行类似的代码：

```javascript
this.prototype = {
    constructor:this
}
```

- `constructor`属性没什么用
- `prototype`是存放继承特征的地方

**模拟`new操作符`**:

```javascript
Function.method("new",function(){
    //创建一个新对象，继承自构造函数的原型对象
    var that = Object.create(this.prototype);
    //调用构造函数，绑定this到新对象上
    var other = this.apply(that,arguments);
    //如果返回值不是一个对象，就返回该新对象
    return (typeof other ==="object" && other) || that;
});
```

定义一个构造器并扩充它的原型:

```javascript
var Mammal = function(name){
    this.name = name;
};

Mammal.prototype.get_name = function(){
    return this.name;
};

Mammal.prototype.says = function(){
    ...
}
```

构造另一个伪类来继承Mammal

```javascript
var Cat = function(){
    ...
};

//替换Cat.prototype为新的Mammal实例
Cat.prototype = new Mammal();
    
//扩充其原型对象，增加方法
Cat.prototype.get_name= function(){
	...
};
```

伪类模式**本意是向面向对象靠拢**，却看起来格格不入，可以**隐藏一些“丑陋的细节”**

```javascript
Founction.prototype.method = function(name,func){
    this.prototype[name]=func;
    return this;
};

Function.method("inherits",function(Parent){
   this.prototype = new Parent();
    return this;
});
```

- inherits和method方法都返回this
  - 可以启用**级联**的形式编程

```javascript
var Cat = function(){
    
}
.inherits(Mammal)//继承Mammal
.method()//增强方法
.method()；//增强方法

//注意和原型模式对比
```

- 像“类”
- 但是没有私有私有环境
- 使用构造函数的一个严重的缺陷
  - 调用构造函数时，忘记使用new操作符
  - this将会被绑定到全局对象上
  - 破坏全局变量环境
  - 既没有编译时的警告也没有运行时警告（很危险）
- 为了降低这个风险，**所有的构造函数命名首字母大写（约定）**
- 不使用new

###### 对象说明符

当构造器接收一大串的参数时

- 记住参数的顺序非常困难
- 可以**接收一个对象说明符**

```javascript
var myObject = maker(f,l,m,c,s);

//改进
var myObject = maker({
    first:f,
    middle:m,
    last:l,
    state:s,
    city:c
});
```

多个参数可以按任何顺序排列

这种形式还可以有一个间接的好处

- 与JSON一起工作
  - JSON描述的数据可以是一个对象
  - 将该数据和它的方法关联起来
    - 构造器取得一个对象说明符，就能返回一个构造完全的对象

###### 原型

基于原型的继承相比于基于类的继承在概念上更简单：新对象继承旧对象的属性

```javascript
//先用字面量构造一个有用的对象
var myMammal = {
    name:"sara",
    get_name:function(){
        return this.name;
    },
    says:function(){
        ...
    }
}

//用Object.create方法构造更多的实例
var myCat = Object.create(myMammal);
myCat.name = "lari";
mayCat.get_name = function(){
    ...
}
```

这是一种**"差异化继承"**——定义一个新对象指明所基于对象的区别

- 有时候对某些数据结构继承于其他数据结构的情形非常有用

```javascript
//花括号指示作用域：内部作用域会继承外部作用域

//当遇到一个左括号是block函数被调用，parse函数从scope中寻找字符

var block = function(){
    //记住当前作用域
    var oldScope = scope;
    //构造一个包含当前作用域中所有对象的新的作用域
    scope = Object.create(scope);
    
    //传递左括号作为参数调用advance
    advance("{");
    
    //使用新的作用域进行解析
    parse(scope);
    
    //传递右括号作为参数调用advance
    advance("}");
    
    //抛弃新的作用域，恢复原来的作用域
    scope = oldScope;
};
```

###### 函数化

伪类模式的缺点是不能隐私保护（有时候很麻烦）

更好的选择——**模块模式**（构造一个生成对象的函数）（四个模式）

- 创建一个新对象
  - 通过对象字面量的方式
  - 通过new操作符调用构造函数
  - 使用自定义Object.create()方法
  - 通过调用任意一个返回一个对象的函数
- 有选择的定义私有变量和方法
- 给新对象扩充方法（拥有访问私有属性的特权方法）
- 返回新对象

伪代码描述：

```javascript
var constructor = function(spec,my){
    var that,其他的私有实例变量;
    my = my || {};
    
    把共享的变量和函数添加到my中
    
    that = 一个新的对象
    
    给that添加特权方法
    
    return that;
};
```

- spec对象包含构造器需要构造一个新实例的所有信息
  - 可能被复制到私有变量中
  - 可能被其他函数改变
  - 可能方法在需要的时候访问spec的信息
- my对象是为继承链中的构造器提供的秘密共享容器

需要注意的是：**扩充that时采用分配一个新函数成为其成员方法**

```javascript
var methodical = function(){
    ...
};

that.methodical = methodical;
```

两部定义的好处是：

- 其他方法想调用methodical可以直接调用methodical而不是that.methodical
- 如果实例被修改或者that.methodical被替换，methodical仍然可以工作

mammal的例子：

```javascript
var mammal = function(spec){
    var that = {};
    
    that.get_name = function(){
        return spec.name;
    }
    that.says = function(){
        return spec.saying || "";
    }
    
    return that;
};

var cat = function(){
    spec.saying = spec.saying || "meow";
    var that = mammal(spec);
    that.purr = function(){
        ...
    };
    that.get_name(){
        ...
    }
    
    return that;
};
 
var myCat = cat({name:"sara"});
```

与伪类模式的区别是，

- **函数化模式中cat只需关注自身的差异，不需要重复构造器Mammal已经完成的工作**
- **没有new操作符**
- 提供处理父类方法的方法

```javascript
Object.method("superior",function(name){
    var that = this,
        method = that[name];
    
    return function(){
        return method.apply(that,arguments);
    }
});
```

函数化模式相对于伪类模式

- 灵活，工作量小
- 更好的封装和信息隐藏
- 访问父类方法的能力

###### 部件

to do

##### 六，数组

数组是一段 线性分配的内存，通过整数计算偏移并访问其中的元素

JS中没有像数组的数据结构，提供了**类数组(array-like)**特性的对象

- 把下标转化为字符串，作为属性
- 属性的检索和更新方式与对象一模一样（多了一个**可以用整数作为属性名的特性**）
- 数组元素元素允许包含任意混合类型的值
- 动态数组
- 比真正的数组慢，但更方便

###### 数组字面量

在一对方括号中包围零个或多个用逗号分隔的值的表达式

`var enpty = []`

数组字面量允许出现在任何表达式出现的地方

```javascript
var numbers = [
    "zero",
    "one",
    "two",
    ...
    "nine"
];

var number_object = {
    "0":"zero",
    "1":"one",
    ...
    "9":"nine"
};
```

数组字面量和对象字面量相似：

- 属性刚好有相同的名字
- numbers继承自Array.prototype
  - 继承大量有用的方法
  - 有一个诡异的length属性
- numbers_object继承自Object.prototype

###### 长度

每一个数组都有一个length属性

- JS的length没有边界
  - 用大于或等于length的数字下标来存储元素，**不会发生数组越界错误**
- length属性的值是数组的最大整数属性名加1

*通过把下标指定为数组当前length，可以追加一个新元素到数组末尾*

```javascript
numbers[numbers.lenght];
//可以用push（）
```

###### 删除

由于JS的数组就是对象，因此可以使用delete操作符

数组中某个元素被删除后，悔留下空洞（需要递减后面每个元素的属性）

- 数组提供了一个`splice()`方法

###### 枚举

JS中数组就是对象，因此可以用`for-in`来遍历数组，但

- 无法保证属性的顺序
- 可能从原型链中得到属性

因此为了期望按照阿拉伯数字的顺序来产生数组元素，

- 使用`for 循环`遍历数组

###### 容易混淆的地方

数组和对象的区别是混乱的

- `typeof`数组的类型是`object`
- 当属性名，小而连续用数组，否则用对象

如何区分对象和数组（JS中没有好的机制）

- 自定义`is_array()`函数

```javascript
var is_array(value){
    return value && typeof value === "object" && value.constructor === Array;
}

//一个更好的方法
var is_array = function(value){
    return Object.prototype.toString.apply(value) === "[object Array]";
}
```

###### 方法

JS提供了一套数组可用的方法

- 存储在`Array.prototype`中
- 可增强
  - 给数组增加属性（非数字）length不会增加

###### 指定初始值

JS数组不会预置值，访问不存在的元素则是`undefined`

一些场景初始化数组为指定初始值很有必要

```JavaScript
Array.dim = function(dimension,initial){
    var a = [],i;
    
    for(i=0;i<a.dimension;i++){
        a[i]=initial;
    }
    return a;
}
```

JS中没有二位数组，但是支持元素为数组中的数组（对象中的对象-嵌套）

初始化二维数组：

```javascript
Array.matrix = function(m,n,initial){
    var a,i,j,mat=[];
    
    for(i=0;i<m;i++){
        a = [];
        for(j=0;j<n;j++){
            a[j] = initial;
        }
        mat[i]=a;
    }
    return mat;
}

//构造一个单位矩阵
Array.identity = function(n){
    var i,mat = Array.matrix(n,n,0);
    
    for(i=0;i<n;i++){
        mat[i][i] = 1;
    }
    return mat;
}
```

##### 七，正则表达式

正则表示的作用：

- 对字符串中的信息实现查找，替换和提取操作

处理正则表达式的方式有：

- `regexp.exec`
- `regexp.test`
- `string.match`
- `string.replace`
- `string.split`

正则表达式相较于等效的字符串处理有显著的性能优势

正则表达式的特点：

- 趋于极致的简洁
- 不容含义模糊
- 不支持注释和空白
- 在JS中必须写在一行中

###### 元字符

`.`	\n外任意一个字符

`[]`	表示范围

`[1-8]`	1-8中任意一个数字

`[a-z]` a-z任意一个字母

`[A-Z]` A-Z任意一个字母

`[0-9a-zA-z]`  任意一个数字或者字母

`[.]` 一个“.”



`*` 前面的表达式出现0次或多次

`+` 前面的表达式出现1次或多次

`?` 前面的表达式出现0次或1次



`{}` 限定符，限定前面表达式出现的次数

`{0,}` 0次到多次，和`{*}`一样

`{1,}`1次到多次，和`{+}`一样

`{0,1}`0次或1次，和`{+}`一样

`{5,10}` 54次到10次

`{10}` 10次



`^` 以什么开始或取反

`^[0-9]  ` 以非数字开头

`[^0-9a-zA-Z_]`   特殊符号｛注意下划线｝



`$` 以什么结束

`[a-z]$  ` 以小写字母结束



`()` 表示分组

`(?: . . .)?` 表示可选的非捕获数组



`[0-9a-zA-Z_.-]+[@][0-9a-zA-Z_.-]+([.][a-zA-Z]+){1,2}` 邮箱正则表达式

###### 结构

`g`   表示全局模式

`i`   表示忽略大小写

`m`  	表示多行



创建方式：

- 通过字面量的方式
  - **共享同一个单例**
- 通过`RegExp()`构造函数
  - 适用于用于必须在运行时动态生成正则表达式的情形

###### 因子

可以是

- 字符
- 组
- 字符类
- 转义序列

###### 正则表达式分支

`|` 包含一个或多个正则表达式序列

- 任何一项符合匹配条件，就匹配

###### 正则表达式转义

转义（字面化）用反斜杠

- 数字
- 和字母不能字面化

`\u` Unicode字符表示一个十六进制的常量

`\d` 任意一个数字

`\D` 任意一个非数字

`\s `  任意一个空白符

`\S` 任意一个非空白符

`\w`  任意一个非特殊符号（包含下划线）

`\W  ` 任意一个特殊符号（没有下划线）

`\b  ` 单词边界

`\1`  指向分组1所捕获到的文本的引用（能再次被匹配）

`\2` 指向分组2的引用

###### 正则表示分组

4种：

- 捕获型
  - `{}`
  - 匹配的字符都会被捕获
  - 每个捕获型分组都会指定一个数字（1，2，...）
- 非捕获型
  - `(?:`
  - 不会捕获所匹配的文本
  - 不会干扰捕获型分组的编号
- 向前正向匹配（不好）
  - `(?=`
  - 类似非捕获型分组
  - 文本倒回到开始的地方
- 向后负向匹配（不好）
  - `(?!`
  - 只有匹配失败继续向前匹配

###### 正则表达式字符集

如果想匹配一个元音字母：可以这样做

`(?:a|e|i|o|u)`

可以方便的写成一个类：

`[aeiou]`

类提供两个遍历

- 可以指定字符范围`[a-z]`
- 方便类的求反`[^]`

###### 正则表达式字符转义

`\b` 不表示退格

`[\b]` 表示退格

###### 正则表达式量词

`{}` 可以用一个正则表达式量词后缀决定前面因子被匹配的次数



`{}` 限定符，限定前面表达式出现的次数

`{0,}` 0次到多次，和`{*}`一样

`{1,}`1次到多次，和`{+}`一样

`{0,1}`0次或1次，和`{+}`一样

`{5,10}` 54次到10次

`{10}` 10次

贪婪模式：

- 只有一个量词，匹配尽可能多的副本直至达到上限

非贪婪模式

- 量词附加`?`后缀
- 只匹配必要的副本

坚持使用贪婪模式

##### 八，方法

###### Array

`array.concat(item...)`

- 产生一个新数组，包含array的浅复制

- item是数组将分别添加其中的元素

- 类似`array.push()`


`array.join(separator)`

- 构造一个字符串
- 将array中每个元素构造为一个字符串，然后用separator分隔符将它们链接起来
- 默认的separator是“，”（逗号）
- 无间隔连接使用空白符



`array.pop()`

- 移除array中最后一个元素并且返回

实现：

```javascript
Array.method("pop",function(){
    return this.splice(this.length-1,1)[0];
})
```



`array.push(itme...)`

- 将一个或多个元素追加到数组末尾
-  item是数组则会将其视为单个元素追加到数组末尾
- 返回array的新长度

实现：

```javascript
Array.method("push",function(){
    this.splice.apply(this,[this.length,0].concat(Array.prototype.slice.apply(aguments)));
    return this.length;
})
```

to do

`array.reverse()`

-  返回数组本身

`array.shift()`

- 移除array的第一个元素并返回该元素
- 空数组则返回undefined

实现：

```javascript
Array.method("shift",function(){
    return this.splice(0,1)[0];
})
```



`array.unshift(itme...)`

- 将item添加到array的开始部分
- 返回array的新长度

实现

```javascript
Array.method("unshift",function(){
    this.splice.apply(this,[0,0].concat(Array.prototype.slice.apply(aguments)));
    return this.length;
})
```



`array.slice(start,end)`

- 对array做一次浅复制
- end参数可选

`array.sort(conparefn)`

- JS中默认的比较函数把要排序的比价元素都视为字符串
  - 错的离谱的结果
- 自定义排序函数

to do

`array.splice(start,deleteCount,item...)`

- 返回一个数组

实现：

to do

###### Funtion

`function.apply(thisArg,argArray)`

实现`bind()`方法：

```javascript
Function.method("bind",function(that){
    var method = this,
        slice = Array.prototype.slice,
        args = slice.apply(arguments,[1]);
    
    return function(){
        return method.apply(that,args.concat(slice.apply(arguments,[0])));
    };
});

```



###### Number

`number.toExponerial(fractionDigits)`



`number.toFixed(fractionDigits)`



`number.toPrecision(precision)`



`number.toString(radix)`



###### Object

`object.hasOwnPrototype(name)`

- 返回布尔值
- 原型链中的同名属性不会被检查

###### RegExp

`regexp.exec(string)`

- 返回一个数组
- 数组下标为0的元素将包含正则表达式regexp匹配的子字符串
- 下标为1的元素是分组1的捕获文本
- 下标为2的元素是分组2的捕获文本
- ...
- 匹配失败则返回null



`regexp.test(string)`

- 返回布尔量

实现：

```javascript
RegExp.method("text",function(string){
    return this.exec(string) !== null;
})
```



###### String

`string.charAt(pos)`



`string.chaCodeAt(pos)`



`string.concat(string...)`

- 推荐使用`+` 



`string.indexOf(searchString,pos)`

- 找到searchString则返回其位置
- 否则返回`-1`



`string.lastIndexOf(searchString,pos)`



`string.localeCompare(that)`



`sring.match(regexp)`

- 和`regexp,exec()`相同



`string.replace(searchValue,replaceValue)`



`string.search(regexp)`

- 和indecof方法类似
- 找到返回第一个匹配的首字符位置



`string.slice(start，end)`



`string.split(separator,limit)`

- 返回一个字符串数组



`string.substring()`

- 使用slice替代



`string.toLowerCase()`



`string.toUpperCase()`



##### 九，代码风格

优秀的程序具备

- 前瞻性的结构，
- 清晰的表达方式

代码块内容和对象字面量缩进4个空格

`if`和`(`之间放一个空格

- 不会看起来像一个函数调用

逗号和冒号后都放一个空格

每行最多一条语句

- 放不下则在冒号或者三元运算符后拆开

`if`和`while`这样的结构化语句中始终使用代码块

```javascript
//不要这样
if (a)
    b();
c();

//建议这样
if (a){
    b();
}
c();
```

把`{`放在每一行的结尾而不是下一行的开头

- 避免JS中`return`语句的设计错误

JS没有块级作用域，有函数作用域

- 因此在函数的开头部分声明所有需要的变量

不要在if的条件部分使用赋值语句

不要使用switch语句块中的条件穿越到下一个case语句

一个脚本应用或工具库，只使用唯一一个全局变量

- 每个对象都有自己的命名空间，很容易用对象去管理代码

使用闭包提供进一步信息隐藏，增强模块的健壮性

#####十，优美的特性

###### 函数是顶级对象

###### 基于原型继承的动态对象

###### 对象字面量和数组字面量

JavaScript中字面量是数据交换格式JSON的灵感之源

##### 附：毒瘤

一些难以避免的特性

###### 全局变量

在微型程序中可能很方便，但在大型程序中难以管理，（全局变量可以被程序的任何部分在任何时间修改）

全局变量和子程序导致命名冲突

三种声明全局变量的方式：

- 在函数外使用`var`关键字

- 直接给全局变量添加属性（Web浏览器中是`window`）
- 直接使用未声明的变量（隐式全局变量）

###### 作用域

JavaScript采用块语法（类c）却没有块级作用域：

- 代码块中声明的变量在包含此代码块的函数的任何位置都是可见的

一般来说，声明变量的最好地方是在第一次用到它的地方

- 但这种做法，在JavaScript中却是个坏习惯
- 更好的方式是：在每个函数的开头部分声明所有变量

###### 自动插入分号

```javascript
rerturn 
{
    status:true;
};
```

- 自动插入分号，返回了undefined
- 将`{`放在上一行的末尾（return后），而不是下一行的开头

```javascript
return {
    status:true;
};
```

###### 保留字

保留字不能用来命名变量和参数

当保留字被用作对象字面量的键值时：

- 必须用引号括起来
- 不能使用点语法，只能使用括号语法访问

###### Unicode

JavaScript的字符是16位的

Unicode把一对字符视为单一的字符，JavaScript认为一对字符是两个不同的字符

###### typeof

`typeof`不能辨认`null`和对象

可以这样：

```javascript
if (value && typeof value === "object"){
    //value是一个对象或者数组
}
```

正则表达式

```javascript
typeof /a/;

//一些返回“object”，其他返回“function”

//期望返回“regexp”，但是标准不是这样
```

###### parseInt

`parseInt()`遇到非数字停止解析

```javascript
parseInt("15");//15
parseInt("15 ddd");//15

//如果字符串的第一个字符是0，会基于八进制求值
//八进制中8，9不是数字
parseInt("08");//0
parseInt("09");//0
//导致解析日期时出现问题
```

`parseInt()`可以接收一个基数作为参数

```javascript
parseInt("08",10);//8
parseInt("09",10);//9
```

建议加上基数参数

###### +

可用于加法运算或字符串拼接（取决于参数的类型）

- 两个运算数都是数字，返回两者之和
- 否则，都将转化为字符串拼接

这个复杂的行为是bug之源

###### 浮点数

二进制浮点数不能正确的处理十进制的的小数（0.1+0.1不等于0.3）

JavaScript遵循二进制浮点数算数标准（IEEE 754）有意导致的结果

浮点数中的整数运算是准确的，

小数的问题可以通过指定精确度来避免或者转化为整数运算然后再转化为小数（例如货币换算）

###### NaN

试图将非数字形式的字符串转化为数字

```javascript
+ "0"	//0
+ "oop"	//NaN
```

`typeof`不能辨别数字和`NaN`

`NaN`不等于任何值包括本身

`isNaN()`可以辨别数字和`NaN`

```javascript
isNaN(NaN);//true
isNaN(0);//false
isNaN("0");//false
```

判断一个值是否可以做数字的最佳方法是`isFinite()`函数

- 会筛除`NaN`和`Infinity`
- (缺陷)试图将运算数转化为一个数字

自定义`isNumber()`函数

```javascript
var isNumber = function(value){
    return typeof value === "number" && isFinite(value);
}
```

###### 伪数组

JavaScript没有真正的数组

- JavaScript中的数组本质是对象
- 简单易用（不用设置维度，不会产生越界错误）
- 性能相比真正的数组差

`typeof`不能辨别数组和对象

判断是否是数组

```javascript
if (value && typeof value === "object" && value.constructor === Array){
    //value是一个数组
}
```

更可靠的方法

```javascript
if (Object.prototype.toString.apply(value) === '[object Array]'){
    //value是数组
}
```

`aguments`不是数组，只是拥有length成员属性的对象

###### 假值

JavaScript中拥有的假值

|       值        |   类型    |
| :-------------: | :-------: |
|       `0`       |  Number   |
| `NaN`（非数字） |  Number   |
| `""`(空字符串)  |  String   |
|     `false`     |  String   |
|     `null`      |  Object   |
|   `undefined`   | undefined |

虽然这些值都等同于假，但不可相互转换

`undefined`和`NaN`不是常量（ES5之前，并且可以更改它们的值，千万不要这么做），是全局变量

###### hasOwnProperty

`hasOwnProperty`可以被用作过滤器避开`for in`语句的隐患

（缺陷）作为一个方法，而不是运算符，在对象中可以被任意值替换

```javascript
another_stooge.hasOwnProperty = null;//埋雷

another_stooge.hasOwnProperty(name);//触雷
```

###### 对象

JavaScript的对象不会是真的空对象，因为可以从原型链中取得成员的属性

- 有时候会带来麻烦

##### 附：糟粕

一些容易避免的特性

###### ==

`===`和`!==`

- 类型一致，值相同时，前者返回true后者返回false

和邪恶的`==`和`!=`

- 强制转换值的类型

规则复杂难以记忆，一些例子

```javascript
console.log("" == "0");//false
console.log(0 == "");//true
console.log(0 == "0");//true

console.log(false == "false");//false
console.log(false == "0");//ture  ???

console.log(false == undefined);//fasle
console.log(false == null);//false
console.log(null == undefined);//ture

console.log("\t\r\n" == 0);//true
```

永远不要使用`==`使用`===`

###### with语句

本意是用来快捷访问对象的属性（结果不可预料，不要使用）

```javascript
with(obj){
    a = b;
}

//等效于
if (obj.a === undefined){
    a = obj.b === undefined ? b : obj.b;
}else{
    obj.a = obj.b === undefined ? b : obj.b;
}
```

###### eval

传递一个字符串给JavaScript编译器，并执行其结果

```javascript
eval("value = myObject." + key + ";");

//等价于
value = myObject[key];
```



缺点诸多

- 代码难以阅读
- 性能显著下降
- 减弱了程序的安全性

`Function构造器`是`eval`的另一种形式，避免使用

浏览器提供的`setTimeout`和`setInterval`会像`eval`那样去处理，避免使用字符串参数形式

###### continue语句

###### switch穿越

建议不要使用

###### 缺少块的语句

`if` `while` `do` `for` 可以接收括在花括号中的代码块，也可以接受单行语句

```javascript
if (ok)
t = true;
//可能变成
if (ok)
    t = true;
    advance();
//看起来像这样
if(ok){
    t = ok;
    advance();
}
//实际上，本意是
if (ok){
    t =true;
}
advance();
```

单行语句模糊了程序结构，不要使用

###### ++ --

不够严谨的编程风格

###### 位运算符

由于JavaScript中只有双精度浮点数，因此位运算符现将数字转换为整数，然后执行运算，然后再转换回去

JavaScript的执行环境接触不到硬件，所以非常慢

######function语句对比function表达式

function语句在解析时会发生被提升的情况

- 放宽了函数必须先声明再使用的要求
- 导致混乱

建议使用函数表达式

一个语句不能以一个函数表达式开头（官方的语法假定以function开头的语句是一个函数语句）

- 解决方法：将函数调用括在括在圆括号中

```javascript
(function(){
    var hidden_variable;
    //不会影响引入新的全局变量
}());
```

###### 类型的包装对象

例如:

```javascript
new Boolean(false);
```

避免使用`new Object` 和 `new Array` 使用`{}` 和`[]`

###### new

new运算符创建一个继承其运算数的原型的对象，把新创建的对象绑定给this

- 如果忘记使用此new运算符，得到的就是一个普通函数调用
- this将被绑定到全局对象上
- 意味着函数尝试去初始化新成员属性时将会污染全局变量（没有编译时的警告，也没有运行时的警告）

按照惯例：

与new结合使用的函数应该以首字母大写的形式命名，首字母大写只用来命名构造器函数

更好的策略：

不使用new

###### void

不要使用