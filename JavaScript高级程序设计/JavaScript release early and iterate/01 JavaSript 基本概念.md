###JavaScript简介

实现：

- ECMAScript——核心
  - 提供核心语言功能
- DOM——文档对象模型
  - 提供访问和**操作网页的方法和接口**
  - DOM1级
    - DOM核心——如何**映射**基于XML的**文档结构**
    - DOM HTML——**添加**针对**HTML的对象和方法**
  - DOM2级
    - DOM视图
    - **DOM事件**——定义事件和事件处理接口
    - **DOM样式**——基于CSS样式应用接口
    - DOM遍历和范围——遍历操作文档接口
  - DOM3级
    - 进一步扩充DOM核心，DOM加载，DOM验证
- BOM——浏览器对象模型
  - 提供**与浏览器交互的方法和接口**

在HTML中使用JavaScript：

- 使用<script>标签

  - 属性

    只要不含有defer和async属性，浏览器会按照<script>标签在页面中的先后顺序执行

    - async——异步脚本，html5
      - 立即下载，不妨碍页面的其他操作——（目的）不让页面等待两个脚本的加载和执行
      - 不按照指定的先后顺序执行
      - 只适用于外部脚本（只对外部文件有效）
    - defer——延迟脚本，html4.01
      - 立即下载，延迟到文档完全被解析后（遇到</html>标签后）执行
      - 多个延迟脚本不一定会按照先后顺序执行，最好只包含一个延迟脚本
      - 只适用于外部脚本
    - src——引用外部脚本文件
      - 带有src属性的<script>标签之间包含脚本会被忽略
      - src属性**可以包含外部域的脚本文件**
    - type——默认值：text/javascript

  - **位置**

    ```javascript
    <body>
        <!-- 这里放内容 -->
        <script type="text/javascript" src="example1.js"></script>
        <script type="text/javascript" src="example2.js"></script>
    </body>
    ```

- 嵌入代码
  - **可维护**
  - **可缓存**

- <noscript>元素 

  - 指定在不支持脚本的浏览器中显示替代内容，启用脚本是不显示

1. 语法

   - 类C语言

   - **区分大小写**（ECMAScript中的一切，变量，函数名，操作符）

   - 标识符

     - 指变量，函数，属性的名字，函数的参数
     - **格式规则**
       - 首字符必须是一个字母，下划线或美元符号（数字不能开头）
       - 其他字符可以是字母，数字，下划线，美元符号
       - **驼峰命名法**
       - 不能使用关键字和保留字
       - 变量名有意义，一般小写

   - 注释——C风格

   - 严格模式

     ```javas
     "use strict";
     ```

     在指定函数下使用严格模式

     ```javas
     function doSomething(){
         "use strict";
         //函数体
     }
     ```

   - 语句

     - **语句以分号结尾（养成习惯）**

2. 关键字和保留字

3. 变量

   - ECMAScript变量是**松散类型**——

     - 可以用来保存任何类型的数据（存储变量和操作变量——读，写）

       ```javas
       var message = "hi",
           found = false,
           age = 29;
       ```

       一条语句用定义多个变量，用逗号隔开

   - 定义变量用var关键字

     - 省略var表示隐式全局变量（**慎用**）

4. 数据类型

   - **5中简单数据类型（基本数据类型）**

     - number，string，boolean，undefined，null

   - **一种复杂类型**

     - object——无序的名值对组成

   - **typeof操作符**——

     - 检测变量的数据类型

     ```javascript
     var num = 10;
     var str = "ha";
     var flag = true;
     var nll = null;
     var undef;
     var obj = new Object();
     
     console.log(typeof (num));//number
     console.log(typeof (str));//string
     console.log(typeof (flag));//boolean
     console.log(typeof (nll));//object
     console.log(String(nll));//null
     console.log(typeof (undef));//undefined
     console.log(typeof (obj));//object
     ```

     - typeof是一个操作符**不是函数**，

     ```javas
     //typeof 变量；
     
     //typeof(变量)；
     ```

   - Undefined

     - 未初始化的变量执行typeof操作返回undefined
     - 未声明的变量执行typeof操作返回undefined
     - 函数return没有带回返回值为undefined

   - Null

     - 空对象指针
     - **定义变量用于保存对象可初始化为null**
       - 作为空对象指针的惯例，有助于区分null和undefined

   - Boolean

     - true（不一定是等于1）（注意大小写）

     - 和false（不一定等于0）（注意大小写）

     - ECMAScript中所有类型的值都有与Boolean等价的值

       - 转型函数Boolean()

         ```javascript
         var message = "Hello world!";
         var messageAsBoolean = Boolean(message);
         ```

       转换规则：

       | 数据类型 |      转换为ture的值      | 转换为false的值 |
       | :------: | :----------------------: | :-------------: |
       | Boolean  |           true           |      false      |
       |  String  |   非空字符串(注意" ")    | ""（空字符串）  |
       |  Number  | 非零数字（-1，**负数**） |   0，-0，NaN    |
       |  Objec   |         任何对象         |      null       |
       | Udefined |            -             |    undefined    |

       转换规则对if语句和while语句自动执行Boolean转换非常重要

       ```javascript
       var message = "Hello world!";
           if (message){
           alert("Value is true");
       }
       
       var flag=1;
       while(flag){
             //do something
       }
       ```

   - Number

     - 使用**IEEE754 格式**来表示整数和浮点数（0.1+0.2==0.3，**小数计算误差**）

     - 范围

       - Number.MIN_VALUE
       - Number.MAX_VALUE
       - Infinity
       - -Infinity
       - isFinite()
         - 判断数值是否有穷（**是否位于最大值和最小值之间**）

     - NaN(Not a Number)

       - 涉及NaN的操作都会返回NaN（**多步计算出错**）

       - NaN和任何值都不相等（包括NaN本身）

       - isNaN()

         - **不能被转化为数值的值为true**

         ```javascript
         var number;
         
         console.log(number + 10);//NaN
         console.log(number + 10 == NaN);//false
         console.log(isNaN(number + 10));//true
         ```

     - 类型转换

       - 将非数值转换为数值

         - **Number()——转型函数，可用于任何数据类型**

           - 一元加操作符的操作和Number()函数相同

           - **空字符串返回“0”**

           ```javascript
           //Number()
           
           //Boolean 值：true为1；false为0
           console.log(Number(true));//1
           console.log(Number(false));//0
           
           //数字：简单的传入和返回
           console.log(Number(0));//0
           console.log(Number(1));//1
           console.log(Number(-100));//-100
           console.log(Number(100));//100
           
           //null：0
           console.log(Number(null));//0
           
           //undefined：NaN
           console.log(Number(undefined));//NaN
           
           //字符串：
           //只包含数字，转换为十进制，忽略前导零，有浮点则转换为浮点，忽略字符串前面的空格直到找到第一个非空格字符
           console.log(Number("		10"));//10
           console.log(Number("10.001"));//10.001
           console.log(Number("00010.001"));//10.001
           console.log(Number("-00010.001"));//-10.001
           //含十六进制格式转换为十进制
           console.log(Number("0xf"));//16
           //空字符串转换为零
           console.log(Number(""));//0
           //tab
           console.log(Number(" "));//0
           //除了以上格式以下都为NaN
           console.log(Number("10tttm"));//NaN
           console.log(Number("gg10"));//NaN
           console.log(Number("1ddd10"));//NaN
           console.log(Number("10.99ssd"));//NaN
           console.log(Number("12.12ddf12.01"));//NaN
           
           //对象，则调用对象的valueof()方法，按上述规则，返回Nan则调用对象的toString()方法，按上述方法
           ```

         - parseInt()——专用于字符串转数字

           - 从第一个字符（位置0）开始解析直到遇到一个无效的整型数字字符为止
           - 小数点不被视为有效的数字
           - **空字符串返回NaN**

           ```javascript
           //parseInt()
           
           //忽略字符串前面的空格直到找到第一个非空格字符
           console.log(parseInt("      10"));//10
           //第一个字符不是数字或者负号则返回NaN-----空字符串返回NaN，Number（）为0
           console.log(parseInt("      gg10"));//NaN
           console.log(parseInt("      -10"));//-10
           console.log(parseInt("      "));//NaN
           //是数字继续解析直到遇到非数字
           console.log(parseInt("10110"));//10110
           console.log(parseInt("10tttm"));//10
           console.log(parseInt("1ddd10"));//1
           //在parseInt（）中小数点不被视为有效的数字
           console.log(parseInt("10.98"));//10
           console.log(parseInt("10.99ssd"));//10
           console.log(parseInt("12.12ddf12.01"));//12
           //进制转换
           console.log(parseInt("0xf"));//15
           console.log(parseInt("f"));//NaN
           console.log(parseInt("f",16));//15
           //忽略了前导零被当做十进制,因此用第二个参数加以区别
           console.log(parseInt("070"));//70
           console.log(parseInt("70"));//70
           console.log(parseInt("70",2));//NaN
           console.log(parseInt("10",2));//2
           console.log(parseInt("70",8));//56
           //忽略了前导零
           console.log(parseInt("000070"));//70
           ```

         - parseFloat()——专用于字符串转数字

           - 和parseInt()类似从第一个字符开始（位置0）开始解析直到遇到一个无效的浮点数字字符为止
           - 字符串中第一个小数点有效，第二个无效

           ```javascript
           //parseFoat()
           
           console.log(parseFloat("    -000010"));//-10
           console.log(parseFloat("10tttm"));//10
           console.log(parseFloat("gg10"));//NaN
           console.log(parseFloat("1ddd10"));//1
           console.log(parseFloat("10.98"));//10.98
           console.log(parseFloat("10.99ssd"));//10.99
           console.log(parseFloat("12.12ddf12.01"));//12.12
           ```

   - String

     - 零个和多个16位Unicode字符组成的字符序列——字符串
     - 用单引号或者双引号表示
     - **字符串不可变**
     - 转化为字符串
       - toString()方法，
         - 数值，布尔值，对象，字符串都有toString()方法
         - **null和undefined值没有toString()方法**
       - String()——转型函数
         - 可将任何类型的值转换为字符串（不知道是不是null和undefined的情况下）
         - 转换规则
           - 有toString（）则调用（没有参数），返回结果
           - 值是null则返回null
           - 值是undefined返回undefined

   - Object

     - 数据（属性）和功能（方法）的（无序）集合

     - 通过new操作符创建对象

     - 在ECMAScript中Object是所有对象的基础，Object类型所具有的属性和方法同样存在于更具体的对象中

       - Object实例对象都具有下列属性和方法
       - constructor
         - 保存创建当前对象的函数——构造函数（constructor）
       - hasOwnProperty(propertyName)
         - 给定属性是否在当前实例对象中（而不是在实例原型中）
       - isPrototypeOf(object)
         - 是否是传入对象的原型
       - propertyIsEnumerable(propertyName)
         - 给定属性能否使用for-in语句枚举
       - toLocaleString()
         - 返回对象的字符串表示，与地区对应
       - toString()
         - 返回对象的字符串表示
       - valueOf()
         - 返回对象的字符串，数值，布尔值表示

5. 操作符

   ECMAScript的操作符适用于字符串，数字，布尔甚至对象（通常会调用对象的valueOf()或者toString()方法）

   - 一元操作符

     - 递增，递减操作符

       ```javascript
       //递增递减操作符
       
       var s1 = "2";
       var s2 = "z";
       var b = false;
       var f = 1.1;
       var o = {
           valueOf: function() {
               return -1;
           }
       };
       
       //包含有效数字的字符串，先转换为数字值，再加减1
       s1++; // 值变成数值3
       //不包含有效数值的字符串，变量的值NaN
       s2++; // 值变成NaN
       //布尔false时，先转换为0；布尔true时先转换为1
       b++; // 值变成数值1
       //浮点值时，直接加减
       f--; // 值变成0.10000000000000009（由于浮点舍入错误所致）
       //对象时，先调用valuleOf()方法，返回NaN则继续调用toString()方法
       o--; // 值变成数值-2
       ```

     - 一元加减操作符

       - 一元加减操作符
         - 放数值前面，不会产生影响
         - 在非数字前面，**和Number()转型函数一样**

       ```javascript
       //一元加减操作符
       
       //
       var num = 25;
       num = +num; // 仍然是25
       //在非数字前面，和Number()转型函数一样
       var s1 = "01";
       var s2 = "1.1";
       var s3 = "z";
       var b = false;
       var f = 1.1;
       var o = {
           valueOf: function() {
               return -1;
           }
       };
       
       s1 = +s1; // 值变成数值1
       s2 = +s2; // 值变成数值1.1
       s3 = +s3; // 值变成NaN
       b = +b; // 值变成数值0
       f = +f; // 值未变，仍然是1.1
       o = +o; // 值变成数值-1
       ```

       - 一元减操作符
         - 放数值前面，主要用于表示负数
         - 在非数字前面，和Number()转型函数一样

   - 位运算

     - 符号位
       - 0表示整数
       - 1表示负数
     - 二进制补码
       - 数值的绝对值的二进制码
       - 反码
       - 反码加1
     - 按位非
     - 按位或
     - 按位与
     - 按位异或
     - 左移
     - 右移

   - 布尔操作符

     - 重要性堪比相等操作

     - 主要用在条件语句和循环语句——测试两个值关系

     - 逻辑非

       - 适用于任何值
       - 先将操作数转换为布尔值，再求反
       - `！！`——将值转换为对应的布尔值，和Boolean()转型函数相同

     - 逻辑与——**短路操作**（如果第一个操作数是false，就不会对第二个操作数求值了）

       - 适用于任何类型操作数

       - 有一个操作数不是布尔值时，不一定返回布尔值

         ```javascript
         //如果第一个操作数是对象，则返回第二个操作数；
         //如果第二个操作数是对象，则只有在第一个操作数的求值结果为true 的情况下才会返回该对象；
         //如果两个操作数都是对象，则返回第二个操作数；
         //如果有一个操作数是null，则返回null；
         //如果有一个操作数是NaN，则返回NaN；
         //如果有一个操作数是undefined，则返回undefined
         ```

     - 逻辑或——**短路操作**（如果第一个操作数是true，就不会对第二个操作数求值了）

       -  有一个操作数不是布尔值时，不一定返回布尔值

         ```javascript
         //如果第一个操作数是对象，则返回第一个操作数；
         //如果第一个操作数的求值结果为false，则返回第二个操作数；
         //如果两个操作数都是对象，则返回第一个操作数；
         //如果两个操作数都是null，则返回null；
         //如果两个操作数都是NaN，则返回NaN；
         //如果两个操作数都是undefined，则返回undefined
         ```

   - 乘性操作符

     - 操作数在非数值的情况下会执行自动的类型转换（**Number()转型函数**）

     - 乘法

       ```javascript
       //如果操作数都是数值，执行常规的乘法计算，如果乘积超过了ECMAScript 数值的表示范围，则返回Infinity 或-Infinity；
       //如果有一个操作数是NaN，则结果是NaN；
       //如果是Infinity 与0 相乘，则结果是NaN；
       //如果是Infinity 与非0 数值相乘，则结果是Infinity 或-Infinity，取决于有符号操作数的符号；
       //如果是Infinity 与Infinity 相乘，则结果是Infinity；
       //如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则
       ```

     - 除法

       ```javascript
       //如果操作数都是数值，执行常规的除法计算，如果商超过了ECMAScript 数值的表示范围，则返回Infinity 或-Infinity；
       //如果有一个操作数是NaN，则结果是NaN；
       //如果是Infinity 被Infinity 除，则结果是NaN；
       //如果是零被零除，则结果是NaN；
       //如果是非零的有限数被零除，则结果是Infinity 或-Infinity，取决于有符号操作数的符号；
       //如果是Infinity 被任何非零数值除，则结果是Infinity 或-Infinity，取决于有符号操作数的符号；
       //如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则
       ```

     - 求摸（余数）

       ```javascript
       //如果操作数都是数值，执行常规的除法计算，返回除得的余数；
       //如果被除数是无穷大值而除数是有限大的数值，则结果是NaN；
       //如果被除数是有限大的数值而除数是零，则结果是NaN；
       //如果是Infinity 被Infinity 除，则结果是NaN；
       //如果被除数是有限大的数值而除数是无穷大的数值，则结果是被除数；
       //如果被除数是零，则结果是零；
       //如果有一个操作数不是数值，则在后台调用Number()将其转换为数值，然后再应用上面的规则
       ```

   - 加性操作符

     - 加法

       - 如果两个操作符都是数值,常规计算

       ```javascript
       //如果有一个操作数是NaN，则结果是NaN；
       //如果是Infinity 加Infinity，则结果是Infinity；
       //如果是-Infinity 加-Infinity，则结果是-Infinity；
       //如果是Infinity 加-Infinity，则结果是NaN；
       //如果是+0 加+0，则结果是+0；
       //如果是-0 加-0，则结果是-0；
       //如果是+0 加+0，则结果是+0
       ```

       - 如果一个操作数是字符串

       ```javascript
       //如果两个操作数都是字符串，则将第二个操作数与第一个操作数拼接起来；
       //如果只有一个操作数是字符串，则将另一个操作数转换为字符串，然后再将两个字符串拼接起来
       ```

       - ​	**对象，数值，布尔值调用toString()方法**，得到相应的字符串	
       - **undefined和null调用String()函数**得到"undefined"和"null"

     - 减法

       ```javascript
       //如果两个操作符都是数值，则执行常规的算术减法操作并返回结果；
       //如果有一个操作数是NaN，则结果是NaN；
       //如果是Infinity 减Infinity，则结果是NaN；
       //如果是-Infinity 减-Infinity，则结果是NaN；
       //如果是Infinity 减-Infinity，则结果是Infinity；
       //如果是-Infinity 减Infinity，则结果是-Infinity；
       //如果是+0 减+0，则结果是+0；
       //如果是+0 减-0，则结果是-0
       //如果是-0 减-0，则结果是+0；
       //如果有一个操作数是字符串、布尔值、null 或undefined，则先在后台调用Number()函数将其转换为数值，然后再根据前面的规则执行减法计算。如果转换的结果是NaN，则减法的结果就是NaN；
       //如果有一个操作数是对象，则调用对象的valueOf()方法以取得表示该对象的数值。如果得到的值是NaN，则减法的结果就是NaN。如果对象没有valueOf()方法，则调用其toString()方法并将得到的字符串转换为数值。
       ```

   - 关系操作符

     - 关系操作符的操作数是非数值时，也要进行数据转换

       ```javascript
       //如果两个操作数都是数值，则执行数值比较。
       //如果两个操作数都是字符串，则比较两个字符串对应的字符编码值。
       //如果一个操作数是数值，则将另一个操作数转换为一个数值，然后执行数值比较。
       //如果一个操作数是对象，则调用这个对象的valueOf()方法，用得到的结果按照前面的规则执行比较。如果对象没有valueOf()方法，则调用toString()方法，并用得到的结果根据前面的规则执行比较。
       //如果一个操作数是布尔值，则先将其转换为数值，然后再执行比较
       ```

     - 比较字符串时，比较的是两个字符串对应位置的编码（**大写字母的编码值小于小写字母**）

     - 按照常理，如果一个值不小于另一个值，则一定是大于或等于那个值。然而，在**与NaN 进行比较时**，
       这两个比较操作的结果**都返回了false**

       ```javascript
       var result1 = NaN < 3; //false
       var result2 = NaN >= 3; //false
       ```

   - 相等操作符

     - 相等(==)和不相等(!=)——**先转换再比较**

       - **强制转换**

       - 如果一个操作数是布尔值，比较相等性之前先转换为数值

         - true——1
         - false——0

       - 如果一个操作数是字符串，另一个是数值，比较相等性之前先将字符串转换为数值

       - 如果一个操作数是对象，另一个操作数不是，则调用对象的valueOf()方法，再比较

       - null 和undefined 相等

       - **要比较相等性之前，不能将null 和undefined 转换成其他任何值**

       - 有一个操作数是NaN，则相等操作符返回false，而不相等操作符返回true

       - 如果两个操作数都是对象，则比较它们是不是同一个对象。如果两个操作数都指向同一个对象，
         则相等操作符返回true；否则，返回false

         ```javascript
         console.log(null == undefined);// true 
         console.log("NaN" == NaN);// false 
         console.log(5 == NaN);// false 
         console.log(NaN == NaN);// false 
         console.log(NaN != NaN);// true 
         console.log(false == 0);// true
         console.log(true == 1);// true
         console.log(true == 2);// false
         console.log(undefined == 0);// false
         console.log(null == 0);// false
         console.log("5"==5);// true
         ```

     - 全等(===)和不全等(!==)——**仅比较而不转换**

       - 除了在比较之前不转换之外，全等操作符和不全等操作符没有区别

         ```javascript
         console.log(undefined==null);//true
         console.log(undefined===null);//false
         ```

       - 为了保持数据类型的完整性，推荐使用全等和不全等操作符

   - 条件操作符

   - 赋值操作符

   - 逗号操作符

     - 在一条语句中执行执行多个操作

       ```javascript
       //声明多个变量
       var num1=1, num2=2, num3=3;
       ```

     - 赋值(返回表达式中的最后一项)

       ```javascript
       var num = (5, 1, 4, 8, 0); // num 的值为0
       ```

6. 语句

   - if语句
     - 语法：

       ```javascript
       if (condition) statement1 else statement2
       ```

     - condition:

       - 任意表达式
       - 不是布尔值，会**调用Boolean()转换函数**

   - do-while语句

     - 语法

       ```javascript
       do {
       	statement
       } while (expression);
       ```

     - 循环体中的代码**至少执行一次**

   - while语句

     - 语法

       ```javascript
       while(expression) statement
       ```

   - for语句

     - 语法

       ```javascript
       for (initialization; expression; post-loop-expression) statement
       ```

     - **和while等价**

     - for语句的初始化表达式，控制表达式和循环表达式都是**可选**的

       ```javascript
       for (;;) { // 无限循环
       	doSomething();
       }
       ```

   - for-in语句

     - 语法

       ```javascript
       for (property in expression) statement
       ```

     - 精准的**迭代**语句——用来枚举对象大的属性
       - 检测对象的值是否是null和undefined

   - label语句

   - break和continue语句

   - switch语句

     - 语法

       ```javascript
       switch (expression) {
           case value: statement
           	break;
           case value: statement
           	break;
           case value: statement
           	break;
           case value: statement
           	break;
           default: statement
       }
       ```

       - 状态机转换

7. 函数

   - 语言的核心

   - 语法

     ```javascript
     function functionName(arg0, arg1,...,argN) {
     	statements
     }
     ```

   - 参数

     - 在内部以数组来表示——arguments（**不是Array的实例**）
       - 可以传入任意多个参数，原因参数始终以数组传入
     - 命名的参数只提供便利，但**不是必需的**
     - 不需要函数签名
     - 参数和arguments的内存空间独立，但是它们的**值会同步**
     - arguments的长度由传入参数个数决定，不是由定义函数时的命名参数的个数决定的——**实参个数**
       - length——定义时命名参数的个数——**形参个数**
       - 没有传递值的命名参数自动被赋予undefined

   - 没有重载

     - 后定义的函数覆盖先定义的函数
     - **可以根据传入参数的类型和数量，模拟重载**

8. 小结

   黑体是重点内容