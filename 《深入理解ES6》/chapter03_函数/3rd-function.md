# 第 3 章 -- 函数

## 目录 Table of Content
- 3.1 函数形参的默认值
	+ 3.1.1 在 ES5 中模拟默认参数
	+ 3.1.2 ES6 中的参数默认值
	+ 3.1.3 参数默认值对 arguments 对象的影响
	+ 3.1.4 默认参数表达式
	+ 3.1.5 默认参数的临时死区
- 3.2 使用不具名参数 (无命名参数)
	+ 3.2.1 ES5 中的 不具名参数
	+ 3.2.2 `剩余参数(rest parameter)` / `不定参数` 
- 3.3 增强的 `Function` 构造函数
- 3.4 展开运算符 / 扩展运算符
- 3.5 ES6 的 `name`(名称) 属性
    + 3.5.1 如何选择合适的名称
	+ 3.5.2 `name` 属性的特殊情况
- 3.6 明确函数的多重用途
	+ 3.6.1 在 ES5 中判断函数被调用的方法
	+ 3.6.2 `new.target` 元属性 (Metaproperty)
- 3.7 块级函数
	+ 3.7.1 块级函数的使用场景
	+ 3.7.2 非严格模式下的块级函数
- 3.8 箭头函数
	+ 3.8.1 箭头函数语法
	+ 3.8.2 创建立即执行函数表达式
	+ 3.8.3 箭头函数没有 `this` 绑定
	+ 3.8.4 箭头函数和数组
	+ 3.8.5 箭头函数没有 `arguments` 绑定
	+ 3.8.6 箭头函数的辨识方法
- 3.9 尾调用优化 (tail call optimization)
	+ 3.9.1 ES6 中的尾调用优化
	+ 3.9.2 如何利用尾调用优化
- 3.10 小结

---

## 生词
- parametric [ˌpærə'metrik]  adj.参数的
- parameter  [pə'ræmɪtə]     n. 参数
- statement  ['steɪtm(ə)nt]  n. 声明, 陈述
- expression [ɪk'spreʃ(ə)n]  n. 表达， 表现

---

## 本章内容 (Content)

### 3.1 函数形参的默认值
#### 3.1.1 在 ES5 中模拟默认参数
- ```javascript
	function; makeRequest(url, timeout, callback); {
        // - 添加 typeof 判断是排除传入 timeout 值为 0 时 js 认为 0 不是
        //   合法的值, 仍然把 2000 赋值给 timeout 的情况.
        timeout = (typeof timeout !== "undefined") ? timeout: 2000;
        callback = (typeof callback !== "undefined") ? callback: function() {};

        // 函数的其他代码
	}
  ```
#### 3.1.2 ES6 中的参数默认值
- (0) ES6 参数默认值示例: 
  ```javascript
    // - ES6 参数默认值示例: 只有第一个参数 url 是必须的, 其他 2 个参数都有默认值
    // - Tip: 由于书上的示例无法运行, 所以从 internet 上找到示例.
    function  num(x = 1, y = 2, z = 3) {
        console.log(x, y, z);
    }
    num(6, 7);  // output: 6 7 3
  ```
- (1) 使用 `undefined` 传参: 如果想让某个参数使用默认值, 我们可以使用 `undefined`
  进行赋值, 例如
  ```javascript 
    num(6, undefined, 7);   // 6 2 7
  ```
- (2) 使用 `null` 代替默认值
  ```javascript 
    num(9, null, 12);   // 6 null 12
  ```
- (3) 参数运算: 
  ```javascript
    function sum(x = 1, y = 2, z = x + y) {
        console.log(x, y, z);
    }
    sum(6, 7);
  ```
#### 3.1.3 参数默认值对 `arguments` 对象的影响
- 在使用 `ES6` 参数默认值的函数中, `arguments` 对象的表现总是会与 ES5 的严格模式一致,
  无论此时函数是否明确运行在严格模式下. 参数默认值的存在触发了 `arguments`
  对象与`具名参数`的分离. 这是个细微但重要的细节, 因为 `arguments`
  对象的使用方式发生了变化. 研究如下代码: 
  ```js
    // - 非严格模式
    function mixArgs(first, second = 'b') {
        console.log(arguments.length);          // 1
        console.log(first === arguments[0]);    // true
        console.log(second === arguments[1]);   // false

        first = 'c';
        second = 'd';

        console.log('first:', first);           // first: c
        console.log(first === arguments[0]);    // false
        console.log(second === arguments[1]);   // false
    }

    mixArgs('a');
  ```
#### 3.1.4 默认参数表达式
- 参数默认值最有意思的特性或许就是默认值并不要求一定是基本类型的值. 例如,
  你可以执行一个函数来产生参数的默认值, 就像这样:
  ```js
    function getValue() {
        return 5;
    }
    function add(first, second = getValue()) {
        return first + second;
    }
    console.log(add(1, 1)); // 2
    console.log(add(1));    // 6
  ```
#### 3.1.5 默认参数的临时死区
- 略

### 3.2 使用不具名参数 (无命名参数)
#### 3.2.1 ES5 中的 不具名参数
+ ```javascript
    let book = {
        title: "Understanding ECMAScript 6",
        author: "Nicholas C. Zakas",
        year: 2016
    };
    function pick(obj) {
        // Javascript设计模式与编程实践\第1部分--基础知识\第1章\P14-Object.create.js
        let result = Object.create(null);   // 克隆一个空对象

        // start at the second parameter
        for (let i = 1, len = arguments.length; i < len; i++) {
            result[arguments[i]] = obj[arguments[i]];
        }
        return result;
    }
    
    let bookData = pick(book, "author", "year");
    console.log(bookData.author);
    console.log(bookData.year);
  ```
#### 3.2.2 ES6 剩余参数(Rest parameters) / 不定参数
- 在函数的命名参数前添加三个点 (`...`) 就表明这是一个剩余参数, 该参数为一个数组, 
  包含着自它之后传入的所有参数, 通过这个数组名即可逐一访问里面的参数.
  e.g. 重写上面的 `pick()`
  函数:
  ```javascript
    function pick2(obj, ...keys) {
        let result = Object.create(null);
        for (let i = 0, len = keys.length; i < len; i++) {
            result[keys[i]] = obj[keys[i]]
        }
        return result;
    }

    let bookData2 = pick2(book, "author", "year");
    // bookData2.author: Nicholas C. Zakas
    console.log("bookData2.author: " + bookData2.author); 
    
    // 不定参数的使用示例
    let func = function(a, b, ...args) {
        console.log(args);
    };
    func(1, 2, 3, 4, 5, 6); // [3, 4, 5, 6]
  ```


### 3.3 增强的 Function 构造函数

### 3.4 扩展运算符 / 展开运算符
- 与神域参数关系最密切的就是扩展运算符. 剩余参数允许你把多个独立的参数河滨到一个数组总;
  而扩展运算符则允许将一个数组分割, 并将各个项作为分离的参数传给函数. 
  ```js
    // - ES5 中返回最大值
    let values = [25, 50, 75, 100];
    console.log(Math.max.apply(Math, values));  // 100

    // - ES6 利用展开运算符实现返回最大值
    console.log(Math.max(...values));       // 100

    // - ES6 展开运算符示例2
    let values2 = [-25, -70, -50, -100];
    console.log(Math.max(...values2, 0));   // 0

    // - 展开运算符使用示例3
    let { x, y, ...z } = { x: 1, y: 2, a: 3, b: 4 };
    console.log("x", x);
    console.log("y", x);
    // console.log("...z", ...z);
  ```

### 3.5 ES6 的 name(名称) 属性
#### 3.5.1 如何选择合适的名称
- ES6 程序中所有的函数的 name 属性都有一个合适的值.如下面函数声明和函数表达式定义的
  函数, 都打印了各自的 name 属性:
  ```javascript
    function doSomething() {}
    var doAnotherThing = function() {};
    console.log(doSomething.name);  // "doSomething"
    console.log(doAnotherThing.name); // "doAnotherThing"
  ```
#### 3.5.2 name 属性的特殊情况
- ES6 中为所有的函数新增了 name 属性. 函数声明/函数表达式. 例如: 
  ```javascript
    let doSomething = function doSomethingElse() {};
    const person = {
        // - getter 函数，名称为"get firstName", setter 函数的名称有前缀 "set"
        get firstName() {
            return "Nicholas";
        },
        sayName: function () {
            console.log(this.name);
        }
    };
    console.log(doSomething.name);      // doSomething
    console.log(person.sayName.name);   // sayName

    // - 这里和书上有出入，这是发现的第二处修改了
    // - getOwnPropertyDescriptor 取得自身属性描述符
    const descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
    console.log(descriptor.get.name);   // get firstName

    // P53: 通过 bind() 函数创建的函数，其名称将带有 "bound" 前缀; 通过 Function 
    // 构造函数创建的函数，其名称
    // 将是 "anonymous"(/ə'nɒnɪməs/ adj.匿名)。 示例如下:
    const doSome = function () {
        // 空函数
    };
    console.log(doSomething.bind().name);   // bound doSomethingElse
    console.log((new Function()).name);     // anonymous
  ```

### 3.6 明确函数的多重用途
#### 3.6.1 在 ES5 中判断函数被调用的方法
- 在ES5中如果想确定一个函数是否通过 new 关键字被调用(或者说，判断该函数是否作为
  构造函数被调用),最流行的方式是使用 `instanceof` (instance 实例), 例如:
  ```javascript
    function Person(name) {
        if (this instanceof Person) {
        this.name = name;
        } else {
        throw new Error("必须通过new关键字来调用Person.");
        }
    }

    // (2.) 还有一种在库中常见的写法是:  add-20180428
    function SendVerCode() {
        if (
            typeof this === "undefined" 
            || Object.getPrototypeOf(this) !== SendVerCode.prototype
        ) {
            return new SendVerCode();
        }
    }
  ```
#### 3.6.2 元属性 (Metaproperty) new.target
- 元属性 (Metaproperty) `new.target`
    + P55: 元属性 new.target: 检测一个函数是否通过 new 关键字来调用。
      ```javascript
        function Person2(name) {
            if (typeof new.target !== "undefined") {
            	console.log(this.name = name);
            } else {
            	console.log("必须通过 new 关键字来调用 Person.");
            }
        }

        let person2 = new Person2("Nicholas"); // Nicholas
        // - 必须通过 new 关键字来调用 Person.
        let anotherPerson = Person2.call(person, "Michael"); 
      ```


### 3.7 块级函数 
#### 3.7.1 块级函数的使用场景
#### 3.7.2 非严格模式下的块级函数

### 3.8 箭头函数
- 在 ECMAScript6 中，箭头函数是其中最有趣的新增特性。顾名思义，箭头函数是一种使用箭头
  `=>` 定义函数的新语法，但是它与传统的 Javascript 函数有些许不同，主要集中在以下方面:
    + (1) **没有 `this`、`super`、`arguments` 和 `new.target` 绑定**, 箭头函数
      中的 this、super、arguments 及 new.target 这些值 
      **由外围最近一层非箭头函数决定**.(super 将在第 4 章进行讲解。)
        - 示例 1
          ```js
            const obj = {
                a: function () {
                    console.log(this);
                    window.setTimeout(() => {
                    console.log(this)
                    }, 1000)
                }
            };
            // - 第一个 this 是 obj 对象，第二个 this 还是 obj 对象
            obj.a.call(obj); 
          ```
        - 示例 2
          ```js
            const obj = {
                a: function () {
                    console.log(this)
                },
                b: {
                    c: function () {
                        console.log(this)
                    }
                }
            };
            obj.a();  // 打出的是obj对象, 相当于obj.a.call(obj)
            obj.b.c(); //打出的是obj.b对象, 相当于obj.b.c.call(obj.b)
          ```
    + (2) **不能通过 `new 关键字调用** 箭头函数没有`[Construct]` 方法，所以
      不能被用作构造函数，如果通过 new 关键字调用箭头函数，程序会抛出错误.
    + (3) **没有原型** 由于不可以通过 new 关键字调用箭头函数, 因而没有构造原型的需求,
      所以箭头函数不存在 prototype 这个属性.
    + (4) **不可以改变 `this` 的绑定** 函数内部的 this 值不可被改变，
      在函数的生命周期内始终保持一致。
    + (5) **不支持 `arguments` 对象** 箭头函数没有 arguments 绑定，所以你必须通过
      命名参数和不定参数(Rest parameters) 这 2 种形式访问函数的参数.
    + (6) **不支持重复的命名参数** 无论在严格还是非严格模式下，箭头函数都不支持
      重复的命名参数；而在传统函数的规定中，只有在严格模式下才不能有重复的命名参数。
      没有原型由于不可以通过 new 关键字调用箭头函数，因而没有构建原
- Note: 箭头函数同样也有一个 name 属性, 这与其他函数的规则相同. 
- Added: 箭头函数: 所谓箭头函数，目的其实就是为了实现函数式的 lambda 表达式的，它本身
  就是为了函数式而添加进去的新概念，所谓“方便写”只是附带的特性。然而，函数式和面向对象
  两种编程语言范式是冲突的，冲突的点在于数据组织的方式不一致。面向对象是利用 “对象” 来
  集合一组数据和方法，而函数式是通过函数来集合一组数据，并且他的方法是和数据分开的。所以
  在函数式里面不会存在 this 这种上下文概念。
#### 3.8.1 箭头函数语法
- 箭头函数的语法多变, 根据实际的使用场景有多种形式. 所有变种都由 函数参数, 箭头, 函数体
  组成, 根据使用的需求, 参数和函数体可以分别采取多种不同的形式. 下面举例
  ```js
    // ### 示例 1
    let reflect = value => value;
    // - 上一行相当于:
    let reflect01 = function(value) {
        return value;
    };

    // ### 示例 2 
    let sum = (num1, num2) => num1 + num2;
    // - 上一行相当于
    let sum01 = function(num1, num2) {
        return num1 + num2;
    };

    // ### 示例 3 
    let plus = (num1, num2) => {
        return num1 + num2;
    };
    // - 上面相当于
    let plus01 = function(num1, num2) {
        return num1 + num2;
    };

    // #### 示例 4
    let doNothing = () => {};
    // - 上一行相当于 
    let doNothing01 = function() {};

    // #### 示例 5
    // - Tip: 将对象字面量包裹在小括号中是为了将其与函数体区分开来.
    let getTempItem = id => ({id: id, name: 'Temp'});
    // - 上一行相当于 
    let getTempItem01 = function(id) {
        return {
            id: id,
            name: 'Temp'
        }
    }
  ```
#### 3.8.2 创建立即执行函数表达式
- ```javascript
    let people = ((name) => {
        return {
            getName: function () {
                return name;
            }
        }
    })("Nicholas");
    console.log(people.getName());
  ```
#### 3.8.3 箭头函数没有 this 绑定
#### 3.8.4 箭头函数和数组
- ```javascript
    let numArr = [1, 6, 4, 8, 2, -1, -20];
    let result = numArr.sort((a, b) => a - b);
    console.log(numArr);   // [ -20, -1, 1, 2, 4, 6, 8 ]
  ```
#### 3.8.5 箭头函数没有 arguments 绑定
- ```javascript
    // P66 - 箭头函数没有 arguments 绑定: 始终可以访问外围函数的 arguments 对象。
    function createArrowFunReturningFirstArg() {
        return () => arguments[0];
    }
  ```
#### 3.8.6 箭头函数的辨识方法

### 3.9 尾调用优化 (tail call optimization)
#### 3.9.1 ES6 中的尾调用优化
#### 3.9.2 如何利用尾调用优化
- ```javascript
    // P69 - 如何利用尾调优化 factorial /fæk'tɔːrɪəl/ n.阶乘
    function factorial(n, p = 1) {
        if (n <= 1) {
            return p;
        } else {
        let result = n * p;
            // 优化后
            return factorial(n - 1, result)
        }
    }
  ```

### 3.10 小结


