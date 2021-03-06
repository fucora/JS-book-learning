# 第 5 章 -- 引用类型

## 本章目录 (Catalog)
- 5.1 `Object` 类型
- 5.2 `Array` 类型
    * 5.2.1 检测数组
    * 5.2.2 转换方法
    * 5.2.3 栈方法
    * 5.2.4 队列方法
    * 5.2.5 重排序方法
    * 5.2.6 操作方法
    * 5.2.7 位置方法
    * 5.2.8 迭代方法
    * 5.2.9 归并方法
- 5.3 `Date` 类型
    * 5.3.1 继承的方法 
    * 5.3.2 日期格式化方法
    * 5.3.3 日期/时间组件方法
- 5.4 `RegExp` 类型
    * 5.4.1 RegExp 实例属性 
    * 5.4.2 RegExp 实例方法
    * 5.4.3 RegExp 构造函数属性
    * 5.4.4 模式的局限性
- 5.5 `Function` 类型
    * 5.5.1 没有重载 (深入理解)
    * 5.5.2 函数声明与函数表达式
    * 5.5.3 作为值的函数
    * 5.5.4 函数内部属性
    * 5.5.5 函数属性和方法
- 5.6 基本包装类型
    * 5.6.1 `Boolean` 类型
    * 5.6.2 `Number` 类型
    * 5.6.3 `String` 类型
- 5.7 单体内置对象
    * 5.7.1 `Global` 对象
    * 5.7.2 `Math` 对象


## 生词 (New Words)
- **character ['kærəktɚ] --n.字符; 人物; 性格; 性质.**
    + Chinese character. 汉字.
    + moral character. 品德
    + main character. 主要人物; 主要角色
    + His character is set. 他的性格已定型.


## 本章内容 (Content)
- 引用类型的值(对象) 是 **引用类型** 的一个实例. 在 ECMAScript 中, 引用类型是一种
  数据结构, 用于将数据和功能组织在一起. 它也常被称为**类**, 但这种称呼并不妥当. 尽管
  ECMAScript 从技术上讲是一门面向对象的语言, 但它不具备传统的面型对象语言所支持的 类
  和 接口 等基本结构. 引用类型有时候也被称为**对象定义**, 因为它们描述的是一类对象所
  具有的属性和方法.
- Hint: *虽然引用类型与类看起来相似, 但它们并不是相同的概念. 为避免混淆, 本书将不再*
  *使用类这个概念.*
- 如前所述，**对象是某个特定引用类型的实例**。新对象是使用 `new` 操作符后跟一个
  **构造函数**来创建的。
- 构造函数本身就是一个函数，只不过该函数是出于创建新对象的目的而定义的。(Q: 构造函数本省
  就是函数, 但函数实际上是对象, 那就是说构造函数也是对象. 只是说必须通过 new 来调用罢了.)
  请看下面这行代码:
  `var person = new Object();`
  这行代码创建了 Object 引用类型的一个新实例，然后把该实例保存在了变量 person 中。使用
  的构造函数是 Object，它只为新对象定义了默认的属性和方法。 ECMAScript 提供了很多
  原生引用类型（例如 Object），以便开发人员用以实现常见的计算任务。  

### 5.1 `Object` 类型
- 一般来说，访问对象属性时使用的都是点表示法，这也是很多面向对象语言中通用的语法。不过，在 
  JavaScript 中也可以使用方括号表示法来访问对象的属性。在使用方括号语法时，应该将要访问的
  **属性以字符串的形式放在方括号中**，如下面的例子所示。
  ```javascript
    let person = {
        name : "Nicholas",
        age : 29
    };
    console.log(person["name"]);    // "Nicholas"
    console.log(person.name);   // "Nicholas"
  ```

### 5.2 `Array` 类型
#### 5.2.1 检测数组
#### 5.2.2 转换方法
- `join()` 方法：数组方法。只接收一个参数，即用作分隔符的字符串，然后返回包含所有
  数组项的字符串.
   ```javascript
        let colors = ["red", "green", "blue"];
        console.log(colors.join(",")); //red,green,blue
        console.log(colors.join("||")); //red||green||blue
        
        // - 字符串通过"借用"数组的非变更方法来处理字符串: 
        let a = "foo";
        let b = ['f', 'o', 'o'];
        console.log(Array.prototype.join.call(a, '-'));    // "f-o-o"
        let c = Array.prototype.map.call(a, function(item) {
            return item.toUpperCase() + ".";
        }).join("");
        console.log(c)    // "F.O.O"
   ```
#### 5.2.3 栈方法
- ECMAScript 数组也提供了一种让数组的行为类似于其他数据结构的方法。具体说来，数组可以
  表现得就像栈一样，后者是一种可以限制插入和删除项的数据结构。栈是一种 **LIFO
  (Last-In-First-Out，后进先出)** 的数据结构，也就是最新添加的项最早被移除。而栈中
  项的插入(叫做推入)和移除(叫做 弹出)，只发生在一个位置——栈的顶部。ECMAScript 为数组
  专门提供了 `push()` 和 `pop()` 方法，以便 实现类似栈的行为。
#### 5.2.4 队列方法
#### 5.2.5 重排序方法
#### 5.2.6 操作方法
- 主要有 3 个操作方法: `concat()`, `slice()`, `splice()`
- 数组的 slice() 方法: slice() 方法可以接受一或两个参数，即要返回项的起始和结束位置。
   在只有一个参数的情况下，slice() 方法返回从该参数指定位置开始到当前数组末尾的所有项。
   如果有两个参数该方法返回起始和结束位置之间的项——但不包括结束位置的项。
   注意， slice() 方法不会影响原始数组。
- **`splice()`**的主要用途是向数组的中部插入项，但使用这种方法的方式则有如下 3 种。
    + (1)、删除:可以删除任意数量的项，只需指定 2 个参数:要删除的第一项的位置和要删除的项数。
      例如，splice(0,2)会删除数组中的前两项。
    + (2)、插入:可以向指定位置插入任意数量的项，只需提供 3 个参数:起始位置、0(要删除的项数) 
      和要插入的项。如果要插入多个项，可以再传入第四、第五，以至任意多个项。
      例如，splice(2,0,"red","green")会从当前数组的位置 2 开始插入字符串"red"和"green"。
    + (3)、替换:可以向指定位置插入任意数量的项，且同时删除任意数量的项，只需指定 3 个参数:
      起始位置、要删除的项数 和 要插入的任意数量的项。插入的项数不必与删除的项数相等。
      例如，splice (2,1,"red","green")会删除当前数组位置 2 的项，然后再从位置 2 开始
      插入字符串 "red"和"green"。
    + splice()方法始终都会返回一个数组，该数组中包含从原始数组中删除的项(如果没有删除任何项，
      则返回一个空数组)。下面的代码展示了上述 3 种使用 splice()方法的方式。   
    + ```javascript
          let colors = ["red", "green", "blue"];
          let removed = colors.splice(0,1);
          console.log('colors: ', colors);    // ['green', 'blue']
          console.log('removed: ', removed);  // ['red']
          removed = colors.splice(1, 0, "yellow", "orange");
          removed = colors.splice(1, 1, "red", "purple");
      ```
#### 5.2.7 位置方法
- 位置方法 ES5 为数组实例添加了两个位置方法：`indexOf()` 和 `lastIndexOf()`. 
  这两个方法都接受 2 个参数: (1):是要查找的项 和 (2):(可选的)表示查找起点位置的索引.
  其中, `indexOf()` 方法从数组的开头(位置`0`)开始向后查找, `lastIndexOf()`
  方法则从数组的末尾开始向前查找.
- 这两个方法都`返回要查找的项在数组中的位置`, 或者在没有的情况下返回 -1.
  ```javascript
        const aa = "Hello World!";
        if (aa.indexOf('lo') !== -1) {
            // 找到匹配
        }
        if (aa.indexOf('lo') === -1) {
            // 没找到匹配
        }
  ```
#### 5.2.8 迭代方法
-  ECMAScript 5 为数组定义了 5 个迭代方法。每个方法都接受 2 个参数: 
    1. 要在每一项上运行的函数;
        - 第一个参数运行的函数接受 3 个参数: 
            + (1) 数组项的值 (item). 
            + (2) 该项在数组种的位置 (index) 
            + (3) 和 数组对象本身。(array)
        - Tip: 根据使用的方法不同, 这个函数执行后的返回值可能会也可能不会影响方法的
          返回值.    
    2. 和（可选的）运行该函数的作用域对象——影响 this 的值。
    
- 以下是 5 个迭代方法的作用。: (Note: 下面 5 个方法发都不会修改数组中包含的值)
    + `every()`:
    + `filter()`: 对数组中的每一项运行给定函数, 返回该函数会返回 true 的项组成的数组.
    + `forEach()`: 对数组中的每一项运行给定函数. 这个方法没有返回值.
    + `map()`: 对数组中的每一项运行给定函数, 返回每次函数调用的结果组成的数组.
    + `some()`:
    ```javascript
        let numbers = [1,2,3,4,5,4,3,2,1];
        let mapResult = numbers.map(function(item, index, array) {
            return item * 2;
        });
        console.log(mapResult); // [2,4,6,8,10,8,6,4,2]
    ```
#### 5.2.9 归并方法

### 5.3 `Date` 类型
#### 5.3.1 继承的方法 
#### 5.3.2 日期格式化方法
#### 5.3.3 日期/时间组件方法

### 5.4 `RegExp` 类型 [正则表达式 (Regular Expression)]
- 此章节详细笔记见: 
  `DataStructure-Algorithm-Learning/正则表达式/README.MD`
- 更多 `RegExp` 的讲解见:
  `DataStructure-Algorithm-Learning/正则表达式/正则表达式-特殊字符.md`
#### 5.4.1 RegExp 实例属性 
- `RegExp` 的每个`实例`都具有下列属性, 通过这些属性可以取得有关模式的各种信息.
    1. **`global`**: `布尔值`, 表示是否设置了 g 标志.
    1. **`ignoreCase`**: `布尔值`, 表示是否设置了 i 标志.
    1. **`lastIndex`**: `整数`, 表示开始搜索下一个匹配项的字符位置, 从 0 算起.
    1. **`multiline`**: `布尔值`, 表示是否设置了 m 标志.
    1. **`source`**: 正则表达式的字符串表示, 
       按照字面量形式而非传入构造函数中的字符串模式返回.

  通过这些属性可以获知一个正则表达式的各方面信息, 但却没有多大用处, 
  因为这些信息全都包含在模式声明中. 例如:
  ```js
    var pattern1 = /\[bc\]at/i;
    console.log(pattern1.global);       // false
    console.log(pattern1.ignoreCase);   // true
    console.log(pattern1.multiline);    // false
    console.log(pattern1.lastIndex);    // 0
    console.log(pattern1.source);       // "\[bc\]at"

    var pattern2 = new RegExp("\\[bc\\]at", "i");

    console.log(pattern2.global);       // false
    console.log(pattern2.ignoreCase);   // true
    console.log(pattern2.multiline);    // false
    console.log(pattern2.lastIndex);    // 0
    console.log(pattern2.source);       // "\[bc\]at
  ```
  我们注意到, 尽管第一个模式使用的是字面量, 第二个模式使用了 RegExp 构造函数, 
  但它们的 `source` 属性是相同的。可见, `source` 属性保存的是规范形式的字符串,
  即`字面量`形式所用的字符串.
#### 5.4.2 RegExp 实例方法
#### 5.4.3 RegExp 构造函数属性
#### 5.4.4 模式的局限性


### 5.5 `Function` 类型
- - 在 ECMAScript 中函数实际上是对象。每个函数都是 Function 类型的实例, 而且都与其他
  引用类型一样具有属性和方法。由于函数是对象，因此函数名实际上也是一个指向函数对象的指针，
  不会与某个函数绑定。
    + (1)、函数通常使用 **"函数声明"** 语法定义，如下
      ```javascript
          function sum(num1, num2) {
              return num1 + num2;
          }
      ```
    + (2)、这与下面使用 **"函数表达式"** 定义函数的方式几乎相差无几。
      ```javascript
          let sum = function(num1, num2) {
              return num1 + num2;
          };
      ```
    + (3)、最后一种定义函数的方式是 **"使用 Function 构造函数"**。 Function 构造函数
      可以接收任意数量的参数，但最后一个参数始终都被看成是函数体，而前面的参数则枚举除了
      新函数的参数。来看下面的例子：
      ```javascript
          // 不推荐，因为这种语法会导致解析两次代码(第一次是解析常规 ECMAScript 代码，
          // 第二次是解析传入构造函数中的字符串)从而影响性能。不过这种语法对于理解 
          // "函数是对象，函数名是指针" 的概念到时非常直观的。
          let sum = new Function("num1", "num2", "return num1 + num2"); 
      ```
    + **Tip:通过 函数声明/函数表达式 定义的普通函数应看作是 Function 构造函数的直接实例**
- 立即 调用/(执行) 函数表达式 (Immediately-Invoked Function Expression. IIFE) 
 ```javascript
   // - 这段代码会导致语法错误，因为 js 将 function 关键字当作一个函数声明的开始，而函数声明
   //   后面不能跟圆括号。
   // function(){
      // - 这里是块级作用域
   // }();

   // - 然而，函数表达式的后面可以跟圆括号。要将函数声明转换成函数表达式，只要像下面这样给它加上
   //   一对圆括号即可。
   (function(){
        // - 这里是块级作用域
   })();

   // - 立即执行的匿名函数声明方式还有
   (function(){
   }());
 ```
#### 5.5.1 没有重载 (深入理解)
#### 5.5.2 函数声明与函数表达式
#### 5.5.3 作为值的函数
#### 5.5.4 函数内部属性
#### 5.5.5 函数属性和方法
- 在 ECMAScript 核心所定义的全部属性中，最耐人寻味的就要数 `prototype` 属性了。对于
  ECMAScript 中的引用类型而言， prototype 是保存它们所有实例方法的真正所在。换句话说，
  诸如 `toString()` 和 `valueOf()` 等方法实际上都保存在 prototype 名下，只不过是通过
  各自对象的实例访问罢了。在创建自定义引用类型以及实现继承时， prototype 属性的作用是极为
  重要的（第 6 章将详细介绍）。在 ECMAScript 5 中， prototype 属性是不可枚举的，因此
  使用 `for-in` 无法发现。
- 每个函数都包含两个非继承而来的方法: `apply()`和 `call()`。这两个方法的用途是在特定的
  作用域中调用函数，实际上等于设置函数体内 `this` 对象的值。
    + apply() 接受2个参数，
        - 第 1 个参数制定了函数体内 this 对象的指向。
        - 第 2 个参数为一个带下标的集合，这个集合可以是数组，也可以为类数组，
          apply 方法把这个集合中的元素作为参数传递给被调用的函数。


### 5.6 基本包装类型
#### 5.6.1 `Boolean` 类型
#### 5.6.2 `Number` 类型
#### 5.6.3 `String` 类型
- `String` 类型是字符串的对象包装类型, 可以像下面这样使用 `String` 构造函数来创建.
  ```js
    var stringObject = new String('Hello world');
  ```
  `String` 对象的方法也可以在所有基本的字符串值中访问到. 其中, 继承的 `valueOf()`,
  `toLocaleString()` 和 `toString()` 方法, 都返回对象所表示的基本字符串值.

  `String` 类型的每个实例都有一个 `length` 属性, 表示字符串中包含多个字符.
   来看下面的例子.
   ```js
    var stringValue = "hello world";
    console.log(stringValue.length);    // "11"
   ```
  这个例子输出了字符串 "hello world" 中的字符数量, 即 "11". 应该注意的是,
  即使字符串中包含双字节字符 (不是占一个字节的 `ASCII` 字符), 每个字符也仍然算一个字符.

  `String` 类型提供了很多方法，用于辅助完成对 ECMAScript 中字符串的解析和操作。
- (1). 字符方法
    + 2 个用于访问字符串中特定字符的方法: 这两个方法都接收一个参数, 即`基于 0 的字符位置`.
        + `charAt(num)`: 以单字符字符串的形式返回给定位置的那个字符.(ES 中没有字符类型.)
          例如: 
          ```js
            var stringValue = 'Hello world';
            console.log(stringValue.charAt(1));     // "e"
          ```
        + `charCodeAt(num)`: 返回给定位置的字符编码.
           ```js
            var stringValue = 'Hello world';
            console.log(stringValue.charCodeAt(1));     // "101"
          ```
- (2). 字符串操作方法
    + (a.) `concat()`: 用于拼接一或多个字符串, 返回拼接后的新字符串.
      ```js
        var stringValue = "hello ";
        var result = stringValue.concat("world");
        console.log(result);        // "hello world"
        console.log(stringValue);   // "hello"
      ```
    + (b.) ES 还提供了 3 个基于子字符串创建新字符串的方法: 这 3
      个方法都会返回被操作字符串的一个子字符串, 而且都接受 1 或 2 个参数.
      第 1 个参数 `指定子字符串的开始位置`, 第 2 个参数
      `指定的是子字符串最后一个字符后面的位置.`
        - `slice()`
        - `substr()`
        - `subString()`
- (3). 字符串位置方法
    + 有 2 个可以从字符串中查找子字符串的方法: 这 2
      个方法都是从一个字符串中 `搜索给定的子字符串`, 然后返回子字符串的位置
      (如果没有找到该子字符串, 则返回 `-1`). 这 2 个方法也可以接受第二个参数,
      表示`从字符串中的那个位置开始搜索`.
        - `indexOf()`
        - `lastIndexOf()`
- (4). `trim()` 方法
- (5). 字符串大小写转换方法
    + `toLowerCase()`, toLocalLowerCase()
    + `toUpperCase()`, toLocalUpperCase()
- (6). 字符串的模式匹配方法
    + (1) `match()`: 只接受一个参数(正则表达式 / RegExp对象)
    + (2) `search()`: 参数与 match 方法相同。 search() 方法返回字符串中第一个匹配项
      的索引; 如果没有返回 -1.
    + (3) `replace()`: 接受2个参数: 第一个为 "正则表示 / RegExp对象", 第二个参数为
      "一个字符串 / 一个函数"。
      
        - 更多 `replace()` 的使用示例见:
          `DataStructure-Algorithm-Learning/正则表达式/replace方法和正则表达式.md`
    + (4) `split()`: 基于指定的分隔符将一个字符串分割为多个子字符串，并将结果放在一个
      数组中。(Tip: 简短说法: `split()` 方法: 把字符串转换为数组.)
      ```js
        // (1.) match()
        let word = "cot, bot, sot, fot";
        let pattern = /.ot/;
        let matches = word.match(pattern);
        console.log(matches.index);   // 0
        console.log(matches[0]);      // cot
        console.log(pattern.lastIndex);   // 0

        // (2.) search()
        let font = "cat, bat, dat, eat";
        let pos = font.search(/at/);
        console.log(pos);     // 1
        
        // (3.) replace()
        let text = "cat, bat, sat, fat";
        let result = text.replace(/at/g, "ond");
        console.log(result);  // replace 返回的是字符串:  cond, bond, sond, fond
        
        // (4.) split()
        const colorText = "red, blue, green, yellow";
        let colorArr = colorText.split(",");
        console.log(colorArr);  // [ 'red', ' blue', ' green', ' yellow' ]
      ```
- (7). `localCompare()` 方法
- (8). `fromCharCode()` 方法
    + `fromCharCode()`: 接收一或多个字符编码, 然后将它们转换成一个字符串. 
      从本质上来看, 这个方法与实例方法 `charCodeAt()` 执行的是相反的操作。
      ```js
        console.log(String.fromCharCode(104, 101, 108, 108, 111));  // "hello"
      ```

### 5.7 单体内置对象
#### 5.7.1 `Global` 对象
#### 5.7.2 `Math` 对象

### 小结

