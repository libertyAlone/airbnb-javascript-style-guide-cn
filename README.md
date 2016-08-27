# Airbnb JavaScript编码规范指南(ES6)中文版

*个人翻译，一些个人见解会用"注："的形式标出*

## 内容
1. [类型](#types)
2. [引用](#references)
3. [对象](#objects)
4. [数组](#arrays)
5. [解构](#destructuring)
6. [字符串](#strings)
7. [函数](#functions)
8. [箭头函数](#arrow-functions)
9. [类 & 构造函数](#classes--constructors)
10. [模块](#modules)
11. [迭代器和生成器](#iterators-and-generators)
12. [属性](#properties)
13. [变量](#variables)
14. [变量提升](#hoisting)
15. [比较运算符 & 相等](#comparison-operators--equality)
16. [(代码)块](#blocks)
17. [注释](#comments)
18. [空格](#whitespace)
19. [逗号](#commas)
20. [分号](#semicolons)
21. [类型转换](#type-casting-coercion)
22. [命名规则](#naming-conventions)
23. [存取器](#accesssors)
24. [事件](#events)
25. [jQuery](#jquery)
26. [ECMAScript 5兼容性](#ecmascript-5-compatibility)
27. [ECMAScript 6+风格](#ecmascript-6-styles)
28. [测试](#testing)
29. [性能](#performance)
30. [资源](#resources)
31. [谁在使用?](#in-the-wild)
32. [翻译](#translation)
33. [JavaScript编码规范指南](#the-javascript-style-guide-guide)
34. [和我们谈论Javascript](#chat-with-us-about-javascript)
35. [贡献者](#contributors)
36. [许可](#license)

## 类型

  <a name="types--primitives"></a><a name="1.1"></a>
  - [1.1](#types--primitives) **原始类型**: 存取原始类型直接作用于值本身.

    + `string`
    + `number`
    + `boolean`
    + `null`
    + `undefined`

    ```javascript
    const foo = 1;
    let bar = foo;

    bar = 9;

    console.log(foo, bar); // => 1, 9
    ```

  <a name="types--complex"></a><a name="1.2"></a>
  - [1.2](#types--complex)  **复杂类型**: 存取复杂类型作用于值的引用.

    + `object`
    + `array`
    + `function`

    ```javascript
    const foo = [1, 2];
    const bar = foo;

    bar[0] = 9;

    console.log(foo[0], bar[0]); // => 9, 9
    ```

**[⬆ back to top](#table-of-contents)**

## 引用

  <a name="references--prefer-const"></a><a name="2.1"></a>
  - [2.1](#references--prefer-const) 对引用使用`const`;避免使用`var`.eslint: [`prefer-const`](http://eslint.org/docs/rules/prefer-const.html), [`no-const-assign`](http://eslint.org/docs/rules/no-const-assign.html)

    > 原因：会确保你不会改变引用的指向，这可能会产生bug并且代码难以理解

    ```javascript
    // bad
    var a = 1;
    var b = 2;

    // good
    const a = 1;
    const b = 2;
    ```

  <a name="references--disallow-var"></a><a name="2.2"></a>
  - [2.2](#references--disallow-var) 如果要改变引用的指向，使用`let`而非`var`.eslint: [`no-var`](http://eslint.org/docs/rules/no-var.html) jscs: [`disallowVar`](http://jscs.info/rule/disallowVar)

    > 原因：`let`是块级作用于而`var`是函数作用域.

    ```javascript
    // bad
    var count = 1;
    if (true) {
      count += 1;
    }

    // good, use the let.
    let count = 1;
    if (true) {
      count += 1;
    }
    ```

  <a name="references--block-scope"></a><a name="2.3"></a>
  - [2.3](#references--block-scope) 注意`let`和`const`都是块级作用域.

    ```javascript
    // const and let only exist in the blocks they are defined in.
    {
      let a = 1;
      const b = 1;
    }
    console.log(a); // ReferenceError
    console.log(b); // ReferenceError
    ```

**[⬆ back to top](#table-of-contents)**

## 对象

  <a name="objects--no-new"></a><a name="3.1"></a>
  - [3.1](#objects--no-new) 使用字面语法创建对象. eslint: [`no-new-object`](http://eslint.org/docs/rules/no-new-object.html)

    ```javascript
    // bad
    const item = new Object();

    // good
    const item = {};
    ```

  <a name="es6-computed-properties"></a><a name="3.4"></a>
  - [3.4](#es6-computed-properties) 创建动态属性名字的对象时使用计算后的属性名.

    > Why? 原因：允许你在一个地方定义一个对象的全部属性.

    ```javascript

    function getKey(k) {
      return `a key named ${k}`;
    }

    // bad
    const obj = {
      id: 5,
      name: 'San Francisco',
    };
    obj[getKey('enabled')] = true;

    // good
    const obj = {
      id: 5,
      name: 'San Francisco',
      [getKey('enabled')]: true,
    };
    ```

  <a name="es6-object-shorthand"></a><a name="3.5"></a>
  - [3.5](#es6-object-shorthand) 使用对象方法简写. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    ```javascript
    // bad
    const atom = {
      value: 1,

      addValue: function (value) {
        return atom.value + value;
      },
    };

    // good
    const atom = {
      value: 1,

      addValue(value) {
        return atom.value + value;
      },
    };
    ```

  <a name="es6-object-concise"></a><a name="3.6"></a>
  - [3.6](#es6-object-concise) 使用简写属性. eslint: [`object-shorthand`](http://eslint.org/docs/rules/object-shorthand.html) jscs: [`requireEnhancedObjectLiterals`](http://jscs.info/rule/requireEnhancedObjectLiterals)

    > 原因：书写描述更简短.

    ```javascript
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      lukeSkywalker: lukeSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
    };
    ```

  <a name="objects--grouped-shorthand"></a><a name="3.7"></a>
  - [3.7](#objects--grouped-shorthand) 将简写属性统一放到对象声明开头.

    > 原因：容易区分使用简写的属性.

    ```javascript
    const anakinSkywalker = 'Anakin Skywalker';
    const lukeSkywalker = 'Luke Skywalker';

    // bad
    const obj = {
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      lukeSkywalker,
      episodeThree: 3,
      mayTheFourth: 4,
      anakinSkywalker,
    };

    // good
    const obj = {
      lukeSkywalker,
      anakinSkywalker,
      episodeOne: 1,
      twoJediWalkIntoACantina: 2,
      episodeThree: 3,
      mayTheFourth: 4,
    };
    ```

  <a name="objects--quoted-props"></a><a name="3.8"></a>
  - [3.8](#objects--quoted-props) 只有加引号的属性是合法的标识符. eslint: [`quote-props`](http://eslint.org/docs/rules/quote-props.html) jscs: [`disallowQuotedKeysInObjects`](http://jscs.info/rule/disallowQuotedKeysInObjects)

  > 原因：通常我们主观上认为更易读.有利于语法高亮，更容易被许多JS引擎优化.

  ```javascript
  // bad
  const bad = {
    'foo': 3,
    'bar': 4,
    'data-blah': 5,
  };

  // good
  const good = {
    foo: 3,
    bar: 4,
    'data-blah': 5,
  };
  ```

  <a name="objects--prototype-builtins"></a>
  - [3.9](#objects--prototype-builtins) 不要直接使用`Object.prototype`,`hasOwnProperty`, `propertyIsEnumerable`和 `isPrototypeOf`等方法.

  >  原因：这些方法可能会被对象自身的同名属性覆盖-考虑`{ hasOwnProperty: false }`或者对象是null.

  ```javascript
  // bad
  console.log(object.hasOwnProperty(key));

  // good
  console.log(Object.prototype.hasOwnProperty.call(object, key));

  // best
  const has = Object.prototype.hasOwnProperty; // cache the lookup once, in module scope.
  /* or */
  const has = require('has');
  …
  console.log(has.call(object, key));
  ```

**[⬆ back to top](#table-of-contents)**

## 数组

  <a name="arrays--literals"></a><a name="4.1"></a>
  - [4.1](#arrays--literals) 使用字面量语法创建数组. eslint: [`no-array-constructor`](http://eslint.org/docs/rules/no-array-constructor.html)

    ```javascript
    // bad
    const items = new Array();

    // good
    const items = [];
    ```

  <a name="arrays--push"></a><a name="4.2"></a>
  - [4.2](#arrays--push) 使用 [Array#push](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/push) 而不是直接给数组赋值.

    ```javascript
    const someStack = [];

    // bad
    someStack[someStack.length] = 'abracadabra';

    // good
    someStack.push('abracadabra');
    ```

  <a name="es6-array-spreads"></a><a name="4.3"></a>
  - [4.3](#es6-array-spreads) 使用数组扩展运算符`...`拷贝数组.

    ```javascript
    // bad
    const len = items.length;
    const itemsCopy = [];
    let i;

    for (i = 0; i < len; i++) {
      itemsCopy[i] = items[i];
    }

    // good
    const itemsCopy = [...items];
    ```

  <a name="arrays--from"></a><a name="4.4"></a>
  - [4.4](#arrays--from) 将类数组对象转换成数组使用[Array.from](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_Objects/Array/from).

    ```javascript
    const foo = document.querySelectorAll('.foo');
    const nodes = Array.from(foo);
    ```

  <a name="arrays--callback-return"></a><a name="4.5"></a>
  - [4.5](#arrays--callback-return) 在数组方法的回调函数中使用return.如果函数体只有一条声明则可以省略return[8.2](#8.2). eslint: [`array-callback-return`](http://eslint.org/docs/rules/array-callback-return)

    ```javascript
    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map(x => x + 1);

    // bad
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
    });

    // good
    const flat = {};
    [[0, 1], [2, 3], [4, 5]].reduce((memo, item, index) => {
      const flatten = memo.concat(item);
      flat[index] = flatten;
      return flatten;
    });

    // bad
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      } else {
        return false;
      }
    });

    // good
    inbox.filter((msg) => {
      const { subject, author } = msg;
      if (subject === 'Mockingbird') {
        return author === 'Harper Lee';
      }

      return false;
    });
    ```

**[⬆ back to top](#table-of-contents)**

##  解构

  <a name="destructuring--object"></a><a name="5.1"></a>
  - [5.1](#destructuring--object) 使用对象的多个属性时使用对象解构. jscs: [`requireObjectDestructuring`](http://jscs.info/rule/requireObjectDestructuring)

    > 原因：解构可以避免创建属性的临时引用.

    ```javascript
    // bad
    function getFullName(user) {
      const firstName = user.firstName;
      const lastName = user.lastName;

      return `${firstName} ${lastName}`;
    }

    // good
    function getFullName(user) {
      const { firstName, lastName } = user;
      return `${firstName} ${lastName}`;
    }

    // best
    function getFullName({ firstName, lastName }) {
      return `${firstName} ${lastName}`;
    }
    ```

  <a name="destructuring--array"></a><a name="5.2"></a>
  - [5.2](#destructuring--array) 使用数组解构. jscs: [`requireArrayDestructuring`](http://jscs.info/rule/requireArrayDestructuring)

    ```javascript
    const arr = [1, 2, 3, 4];

    // bad
    const first = arr[0];
    const second = arr[1];

    // good
    const [first, second] = arr;
    ```

  <a name="destructuring--object-over-array"></a><a name="5.3"></a>
  - [5.3](#destructuring--object-over-array) 对于多个返回值使用对象解构而不是数组解构. jscs: [`disallowArrayDestructuringReturn`](http://jscs.info/rule/disallowArrayDestructuringReturn)

    > 原因：可以非破坏性地随时增加或者改变属性顺序.

    ```javascript
    // bad
    function processInput(input) {
      // then a miracle occurs
      return [left, right, top, bottom];
    }

    // the caller needs to think about the order of return data
    const [left, __, top] = processInput(input);

    // good
    function processInput(input) {
      // then a miracle occurs
      return { left, right, top, bottom };
    }

    // the caller selects only the data they need
    const { left, top } = processInput(input);
    ```


**[⬆ back to top](#table-of-contents)**

## 字符串

  <a name="strings--quotes"></a><a name="6.1"></a>
  - [6.1](#strings--quotes) 字符串使用单引号 `''` . eslint: [`quotes`](http://eslint.org/docs/rules/quotes.html) jscs: [`validateQuoteMarks`](http://jscs.info/rule/validateQuoteMarks)

    ```javascript
    // bad
    const name = "Capt. Janeway";

    // bad - template literals should contain interpolation or newlines
    const name = `Capt. Janeway`;

    // good
    const name = 'Capt. Janeway';
    ```

  <a name="strings--line-length"></a><a name="6.2"></a>
  - [6.2](#strings--line-length) 一行超过100个字符的字符串不应该使用字符串连接符来跨行书写.

    > 原因：截断的字符串很难维护并且代码不易被搜索查找.

    ```javascript
    // bad
    const errorMessage = 'This is a super long error that was thrown because \
    of Batman. When you stop to think about how Batman had anything to do \
    with this, you would get nowhere \
    fast.';

    // bad
    const errorMessage = 'This is a super long error that was thrown because ' +
      'of Batman. When you stop to think about how Batman had anything to do ' +
      'with this, you would get nowhere fast.';

    // good
    const errorMessage = 'This is a super long error that was thrown because of Batman. When you stop to think about how Batman had anything to do with this, you would get nowhere fast.';
    ```

  <a name="es6-template-literals"></a><a name="6.4"></a>
  - [6.4](#es6-template-literals) 动态构建字符串时使用模板字符串而不是拼接. eslint: [`prefer-template`](http://eslint.org/docs/rules/prefer-template.html) [`template-curly-spacing`](http://eslint.org/docs/rules/template-curly-spacing) jscs: [`requireTemplateStrings`](http://jscs.info/rule/requireTemplateStrings)

    > 原因：模板字符串有可读性强，语法明确，换行合理和字符串插值的特性.

    ```javascript
    // bad
    function sayHi(name) {
      return 'How are you, ' + name + '?';
    }

    // bad
    function sayHi(name) {
      return ['How are you, ', name, '?'].join();
    }

    // bad
    function sayHi(name) {
      return `How are you, ${ name }?`;
    }

    // good
    function sayHi(name) {
      return `How are you, ${name}?`;
    }
    ```

  <a name="strings--eval"></a><a name="6.5"></a>
  - [6.5](#strings--eval) 不要对字符串使用`eval()`，漏洞太多.

  <a name="strings--escaping"></a>
  - [6.6](#strings--escaping) 不要在字符串中使用不必要的转义字符. eslint: [`no-useless-escape`](http://eslint.org/docs/rules/no-useless-escape)

    > 原因：反斜线不利于阅读，应该只在必要的时候出现.

    ```javascript
    // bad
    const foo = '\'this\' \i\s \"quoted\"';

    // good
    const foo = '\'this\' is "quoted"';
    const foo = `'this' is "quoted"`;
    ```

**[⬆ back to top](#table-of-contents)**


## 函数

  <a name="functions--declarations"></a><a name="7.1"></a>
  - [7.1](#functions--declarations) 使用函数声明而非函数表达式. eslint: [`func-style`](http://eslint.org/docs/rules/func-style) jscs: [`requireFunctionDeclarations`](http://jscs.info/rule/requireFunctionDeclarations)

    > Why? Function declarations are named, so they're easier to identify in call stacks. Also, the whole body of a function declaration is hoisted, whereas only the reference of a function expression is hoisted. This rule makes it possible to always use [Arrow Functions](#arrow-functions) in place of function expressions.

    ```javascript
    // bad
    const foo = function () {
    };

    // good
    function foo() {
    }
    ```

  <a name="functions--iife"></a><a name="7.2"></a>
  - [7.2](#functions--iife) 用圆括号包裹自执行匿名函数. eslint: [`wrap-iife`](http://eslint.org/docs/rules/wrap-iife.html) jscs: [`requireParenthesesAroundIIFE`](http://jscs.info/rule/requireParenthesesAroundIIFE)

    > Why? An immediately invoked function expression is a single unit - wrapping both it, and its invocation parens, in parens, cleanly expresses this. Note that in a world with modules everywhere, you almost never need an IIFE.

    ```javascript
    // immediately-invoked function expression (IIFE)
    (function () {
      console.log('Welcome to the Internet. Please follow me.');
    }());
    ```

  <a name="functions--in-blocks"></a><a name="7.3"></a>
  - [7.3](#functions--in-blocks) 不要在非函数块（if,while等）中声明函数.要将函数赋给变量.浏览器允许你这么做，但是处理方式不同，这不是好消息. eslint: [`no-loop-func`](http://eslint.org/docs/rules/no-loop-func.html)

  <a name="functions--note-on-blocks"></a><a name="7.4"></a>
  - [7.4](#functions--note-on-blocks) **Note:** ECMA-262 defines a `block` as a list of statements. A function declaration is not a statement. [Read ECMA-262's note on this issue](http://www.ecma-international.org/publications/files/ECMA-ST/Ecma-262.pdf#page=97).

    ```javascript
    // bad
    if (currentUser) {
      function test() {
        console.log('Nope.');
      }
    }

    // good
    let test;
    if (currentUser) {
      test = () => {
        console.log('Yup.');
      };
    }
    ```

  <a name="functions--arguments-shadow"></a><a name="7.5"></a>
  - [7.5](#functions--arguments-shadow) 不要将参数命名为 `arguments`. 会导致该参数的优先级高于每个函数作用域内原先存在的`arguments`对象.

    ```javascript
    // bad
    function nope(name, options, arguments) {
      // ...stuff...
    }

    // good
    function yup(name, options, args) {
      // ...stuff...
    }
    ```

  <a name="es6-rest"></a><a name="7.6"></a>
  - [7.6](#es6-rest) 不要使用 `arguments`, 使用扩展语法 `...` . eslint: [`prefer-rest-params`](http://eslint.org/docs/rules/prefer-rest-params)

    > 原因：`...`是你想获得的参数列表，因此，扩展语法的参数是真正的数组，而`arguments`是类数组.

    ```javascript
    // bad
    function concatenateAll() {
      const args = Array.prototype.slice.call(arguments);
      return args.join('');
    }

    // good
    function concatenateAll(...args) {
      return args.join('');
    }
    ```

  <a name="es6-default-parameters"></a><a name="7.7"></a>
  - [7.7](#es6-default-parameters) 使用参数默认值语法而不是修改函数参数.

    ```javascript
    // really bad
    function handleThings(opts) {
      // No! We shouldn't mutate function arguments.
      // Double bad: if opts is falsy it'll be set to an object which may
      // be what you want but it can introduce subtle bugs.
      opts = opts || {};
      // ...
    }

    // still bad
    function handleThings(opts) {
      if (opts === void 0) {
        opts = {};
      }
      // ...
    }

    // good
    function handleThings(opts = {}) {
      // ...
    }
    ```

  <a name="functions--default-side-effects"></a><a name="7.8"></a>
  - [7.8](#functions--default-side-effects) 避免参数默认值的副作用.

    > 原因：难以理解.

    ```javascript
    var b = 1;
    // bad
    function count(a = b++) {
      console.log(a);
    }
    count();  // 1
    count();  // 2
    count(3); // 3
    count();  // 3
    ```

  <a name="functions--defaults-last"></a><a name="7.9"></a>
  - [7.9](#functions--defaults-last) 将参数默认值置于最后.

    ```javascript
    // bad
    function handleThings(opts = {}, name) {
      // ...
    }

    // good
    function handleThings(name, opts = {}) {
      // ...
    }
    ```

  <a name="functions--constructor"></a><a name="7.10"></a>
  - [7.10](#functions--constructor) 不要使用Function构造函数创建函数. eslint: [`no-new-func`](http://eslint.org/docs/rules/no-new-func)

    > 原因：此方式创建函数和对字符串使用eval()一样会产生漏洞.

    ```javascript
    // bad
    var add = new Function('a', 'b', 'return a + b');

    // still bad
    var subtract = Function('a', 'b', 'return a - b');
    ```

  <a name="functions--signature-spacing"></a><a name="7.11"></a>
  - [7.11](#functions--signature-spacing) 在函数签名中使用空格. eslint: [`space-before-function-paren`](http://eslint.org/docs/rules/space-before-function-paren) [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks)

    > 原因：统一为先，不应该在增删名字的同时增删空格.

    ```javascript
    // bad
    const f = function(){};
    const g = function (){};
    const h = function() {};

    // good
    const x = function () {};
    const y = function a() {};
    ```

  <a name="functions--mutate-params"></a><a name="7.12"></a>
  - [7.12](#functions--mutate-params) 不要改变参数. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > 原因：操作参数中的对象可能在原始调用中造成意想不到的变量副作用.

    ```javascript
    // bad
    function f1(obj) {
      obj.key = 1;
    };

    // good
    function f2(obj) {
      const key = Object.prototype.hasOwnProperty.call(obj, 'key') ? obj.key : 1;
    };
    ```

  <a name="functions--reassign-params"></a><a name="7.13"></a>
  - [7.13](#functions--reassign-params) 不要给参数重新赋值. eslint: [`no-param-reassign`](http://eslint.org/docs/rules/no-param-reassign.html)

    > 原因：参数重新赋值可能会导致无法预期的行为，尤其是当操作`arguments`对象时.也可能导致优化问题，尤其是在V8引擎中

    ```javascript
    // bad
    function f1(a) {
      a = 1;
    }

    function f2(a) {
      if (!a) { a = 1; }
    }

    // good
    function f3(a) {
      const b = a || 1;
    }

    function f4(a = 1) {
    }
    ```

  <a name="functions--spread-vs-apply"></a><a name="7.14"></a>
  - [7.14](#functions--spread-vs-apply) 调用可变参数函数时建议使用扩展运算符`...`. eslint: [`prefer-spread`](http://eslint.org/docs/rules/prefer-spread)

    > 原因：显然你无需使用上下文，很难结合`new`和`apply`.

    ```javascript
    // bad
    const x = [1, 2, 3, 4, 5];
    console.log.apply(console, x);

    // good
    const x = [1, 2, 3, 4, 5];
    console.log(...x);

    // bad
    new (Function.prototype.bind.apply(Date, [null, 2016, 08, 05]));

    // good
    new Date(...[2016, 08, 05]);
    ```

**[⬆ back to top](#table-of-contents)**

## 箭头函数

  <a name="arrows--use-them"></a><a name="8.1"></a>
  - [8.1](#arrows--use-them) 当你必须使用函数表达式（传递匿名函数）时，使用箭头函数标记. eslint: [`prefer-arrow-callback`](http://eslint.org/docs/rules/prefer-arrow-callback.html), [`arrow-spacing`](http://eslint.org/docs/rules/arrow-spacing.html) jscs: [`requireArrowFunctions`](http://jscs.info/rule/requireArrowFunctions)

    > 原因：这会创建一个能在其中使用`this`上下文的函数，这是你想要的，语法也更明确.

    > 不适用的地方：如果有一个相当复杂的函数，应该把逻辑放到自己的函数声明里.

    ```javascript
    // bad
    [1, 2, 3].map(function (x) {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--implicit-return"></a><a name="8.2"></a>
  - [8.2](#arrows--implicit-return) 如果函数体只包含一条表达语句，可以省略花括号并使用隐式的return. 否则保留花括号并使用`return`语句. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html), [`arrow-body-style`](http://eslint.org/docs/rules/arrow-body-style.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam), [`requireShorthandArrowFunctions`](http://jscs.info/rule/requireShorthandArrowFunctions)

    > 原因：语法糖.当进行多个函数的链式操作时可读性好.

    ```javascript
    // bad
    [1, 2, 3].map(number => {
      const nextNumber = number + 1;
      `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map(number => `A string containing the ${number}.`);

    // good
    [1, 2, 3].map((number) => {
      const nextNumber = number + 1;
      return `A string containing the ${nextNumber}.`;
    });

    // good
    [1, 2, 3].map((number, index) => ({
      index: number
    }));
    ```

  <a name="arrows--paren-wrap"></a><a name="8.3"></a>
  - [8.3](#arrows--paren-wrap) 一旦语句跨多行，使用圆括号包裹以便更好阅读.

    > 原因：清晰展示了函数的开始和结束.

    ```js
    // bad
    [1, 2, 3].map(number => 'As time went by, the string containing the ' +
      `${number} became much longer. So we needed to break it over multiple ` +
      'lines.'
    );

    // good
    [1, 2, 3].map(number => (
      `As time went by, the string containing the ${number} became much ` +
      'longer. So we needed to break it over multiple lines.'
    ));
    ```

  <a name="arrows--one-arg-parens"></a><a name="8.4"></a>
  - [8.4](#arrows--one-arg-parens) 函数如果只有一个参数并且没用花括号，则省略圆括号. 否则总是使用圆括号包裹参数. eslint: [`arrow-parens`](http://eslint.org/docs/rules/arrow-parens.html) jscs:  [`disallowParenthesesAroundArrowParam`](http://jscs.info/rule/disallowParenthesesAroundArrowParam)

    > 原因：看上去不混乱.

    ```js
    // bad
    [1, 2, 3].map((x) => x * x);

    // good
    [1, 2, 3].map(x => x * x);

    // good
    [1, 2, 3].map(number => (
      `A long string with the ${number}. It’s so long that we’ve broken it ` +
      'over multiple lines!'
    ));

    // bad
    [1, 2, 3].map(x => {
      const y = x + 1;
      return x * y;
    });

    // good
    [1, 2, 3].map((x) => {
      const y = x + 1;
      return x * y;
    });
    ```

  <a name="arrows--confusing"></a><a name="8.5"></a>
  - [8.5](#arrows--confusing) 避免把箭头函数语法 (`=>`) 和比较运算符 (`<=`, `>=`)弄混. eslint: [`no-confusing-arrow`](http://eslint.org/docs/rules/no-confusing-arrow)

    ```js
    // bad
    const itemHeight = item => item.height > 256 ? item.largeSize : item.smallSize;

    // bad
    const itemHeight = (item) => item.height > 256 ? item.largeSize : item.smallSize;

    // good
    const itemHeight = item => (item.height > 256 ? item.largeSize : item.smallSize);

    // good
    const itemHeight = (item) => {
      const { height, largeSize, smallSize } = item;
      return height > 256 ? largeSize : smallSize;
    };
    ```

**[⬆ back to top](#table-of-contents)**


## 类 & 构造函数

  <a name="constructors--use-class"></a><a name="9.1"></a>
  - [9.1](#constructors--use-class) 总是使用 `class`. 避免直接操作 `prototype` .

    > 原因：`class`语法更明确和合理.

    ```javascript
    // bad
    function Queue(contents = []) {
      this.queue = [...contents];
    }
    Queue.prototype.pop = function () {
      const value = this.queue[0];
      this.queue.splice(0, 1);
      return value;
    };


    // good
    class Queue {
      constructor(contents = []) {
        this.queue = [...contents];
      }
      pop() {
        const value = this.queue[0];
        this.queue.splice(0, 1);
        return value;
      }
    }
    ```

  <a name="constructors--extends"></a><a name="9.2"></a>
  - [9.2](#constructors--extends) 使用 `extends` 继承.

    > 原因：是实现继承功能而不会破坏`instanceof`的内置方法.

    ```javascript
    // bad
    const inherits = require('inherits');
    function PeekableQueue(contents) {
      Queue.apply(this, contents);
    }
    inherits(PeekableQueue, Queue);
    PeekableQueue.prototype.peek = function () {
      return this._queue[0];
    }

    // good
    class PeekableQueue extends Queue {
      peek() {
        return this._queue[0];
      }
    }
    ```

  <a name="constructors--chaining"></a><a name="9.3"></a>
  - [9.3](#constructors--chaining) 方法可以返回`this`来进行链式调用.

    ```javascript
    // bad
    Jedi.prototype.jump = function () {
      this.jumping = true;
      return true;
    };

    Jedi.prototype.setHeight = function (height) {
      this.height = height;
    };

    const luke = new Jedi();
    luke.jump(); // => true
    luke.setHeight(20); // => undefined

    // good
    class Jedi {
      jump() {
        this.jumping = true;
        return this;
      }

      setHeight(height) {
        this.height = height;
        return this;
      }
    }

    const luke = new Jedi();

    luke.jump()
      .setHeight(20);
    ```


  <a name="constructors--tostring"></a><a name="9.4"></a>
  - [9.4](#constructors--tostring) 在确保表现正常没有副作用的情况下可以重写传统的toString()方法.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        this.name = options.name || 'no name';
      }

      getName() {
        return this.name;
      }

      toString() {
        return `Jedi - ${this.getName()}`;
      }
    }
    ```

  <a name="constructors--no-useless"></a><a name="9.5"></a>
  - [9.5](#constructors--no-useless) 如果未声明构造函数，则类会有一个默认的构造函数. 没必要用空的构造函数或者将其委托给父类. eslint: [`no-useless-constructor`](http://eslint.org/docs/rules/no-useless-constructor)

    ```javascript
    // bad
    class Jedi {
      constructor() {}

      getName() {
        return this.name;
      }
    }

    // bad
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
      }
    }

    // good
    class Rey extends Jedi {
      constructor(...args) {
        super(...args);
        this.name = 'Rey';
      }
    }
    ```

  <a name="classes--no-duplicate-members"></a>
  - [9.6](#classes--no-duplicate-members) 避免类成员重复. eslint: [`no-dupe-class-members`](http://eslint.org/docs/rules/no-dupe-class-members)

    > 原因：重复的类成员声明会默认使用最后声明的-有重复值基本就是bug.

    ```javascript
    // bad
    class Foo {
      bar() { return 1; }
      bar() { return 2; }
    }

    // good
    class Foo {
      bar() { return 1; }
    }

    // good
    class Foo {
      bar() { return 2; }
    }
    ```


**[⬆ back to top](#table-of-contents)**


## 模块

  <a name="modules--use-them"></a><a name="10.1"></a>
  - [10.1](#modules--use-them) 在非标准模块系统的基础上使用模块 (`import`/`export`) . 你可以将其转化为你更喜欢的模块系统.

    > 原因：模块是未来，让我们现在开始使用未来的特性.

    ```javascript
    // bad
    const AirbnbStyleGuide = require('./AirbnbStyleGuide');
    module.exports = AirbnbStyleGuide.es6;

    // ok
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    export default AirbnbStyleGuide.es6;

    // best
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-wildcard"></a><a name="10.2"></a>
  - [10.2](#modules--no-wildcard) 不要使用通配符导入.

    > 原因：这会确保你只有一个默认的export.

    ```javascript
    // bad
    import * as AirbnbStyleGuide from './AirbnbStyleGuide';

    // good
    import AirbnbStyleGuide from './AirbnbStyleGuide';
    ```

  <a name="modules--no-export-from-import"></a><a name="10.3"></a>
  - [10.3](#modules--no-export-from-import) 不要从import中export.

    > 原因：很显然有一个明确的方法来import和export会保持一致性.

    ```javascript
    // bad
    // filename es6.js
    export { es6 as default } from './airbnbStyleGuide';

    // good
    // filename es6.js
    import { es6 } from './AirbnbStyleGuide';
    export default es6;
    ```

  <a name="modules--no-duplicate-imports"></a>
  - [10.4](#modules--no-duplicate-imports) 只允许在一个地方从一个路径import.
 eslint: [`no-duplicate-imports`](http://eslint.org/docs/rules/no-duplicate-imports)
    > 原因：从一个路径import多次会使代码难维护.

    ```javascript
    // bad
    import foo from 'foo';
    // … some other imports … //
    import { named1, named2 } from 'foo';

    // good
    import foo, { named1, named2 } from 'foo';

    // good
    import foo, {
      named1,
      named2,
    } from 'foo';
    ```

  <a name="modules--no-mutable-exports"></a>
  - [10.5](#modules--no-mutable-exports) 不要export可变的绑定值.
 eslint: [`import/no-mutable-exports`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/no-mutable-exports.md)
    > 原因：通常来说可变应该被避免，尤其是在export绑定值的时候.通常来说，这个技术只在常量引用被导出的时候使用.

    ```javascript
    // bad
    let foo = 3;
    export { foo }

    // good
    const foo = 3;
    export { foo }
    ```

  <a name="modules--prefer-default-export"></a>
  - [10.6](#modules--prefer-default-export) 在单一expoort的模块中，建议export default而不是具名export.
 eslint: [`import/prefer-default-export`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/prefer-default-export.md)

    ```javascript
    // bad
    export function foo() {}

    // good
    export default function foo() {}
    ```

  <a name="modules--imports-first"></a>
  - [10.7](#modules--imports-first) 将所有的`import`放在非`import`语句前.
 eslint: [`import/imports-first`](https://github.com/benmosher/eslint-plugin-import/blob/master/docs/rules/imports-first.md)
    > 原因：由于`import`会被提前，将其全部置于顶部会避免意外行为.

    ```javascript
    // bad
    import foo from 'foo';
    foo.init();

    import bar from 'bar';

    // good
    import foo from 'foo';
    import bar from 'bar';

    foo.init();
    ```

**[⬆ back to top](#table-of-contents)**

## 迭代器和生成器

  <a name="iterators--nope"></a><a name="11.1"></a>
  - [11.1](#iterators--nope) 不要使用迭代器.建议使用JS更高优先级的函数代替`for-in` 或 `for-of`循环. eslint: [`no-iterator`](http://eslint.org/docs/rules/no-iterator.html) [`no-restricted-syntax`](http://eslint.org/docs/rules/no-restricted-syntax)

    > 原因：这会强化不可变的规则.处理返回值的纯函数要比消除副作用容易.

    > 使用 `map()` / `every()` / `filter()` / `find()` / `findIndex()` / `reduce()` / `some()` / ... 来遍历数组, 用 `Object.keys()` / `Object.values()` / `Object.entries()` 来制造数组以便能对对象进行迭代.

    ```javascript
    const numbers = [1, 2, 3, 4, 5];

    // bad
    let sum = 0;
    for (let num of numbers) {
      sum += num;
    }

    sum === 15;

    // good
    let sum = 0;
    numbers.forEach(num => sum += num);
    sum === 15;

    // best (use the functional force)
    const sum = numbers.reduce((total, num) => total + num, 0);
    sum === 15;
    ```

  <a name="generators--nope"></a><a name="11.2"></a>
  - [11.2](#generators--nope) 现在不要使用生成器.

    > 原因：不能很好地转译为ES5.

  <a name="generators--spacing"></a>
  - [11.3](#generators--spacing) 如果你非得使用生成器或者无视 [建议](#generators--nope), 请确保函数签名中的空格正确. eslint: [`generator-star-spacing`](http://eslint.org/docs/rules/generator-star-spacing)

    > 原因：`function` 和 `*`是同一个概念 - `*`不是`function`的修饰符，`function*是不同于`function`的独特的构造函数`.

    ```js
    // bad
    function * foo() {
    }

    const bar = function * () {
    }

    const baz = function *() {
    }

    const quux = function*() {
    }

    function*foo() {
    }

    function *foo() {
    }

    // very bad
    function
    *
    foo() {
    }

    const wat = function
    *
    () {
    }

    // good
    function* foo() {
    }

    const foo = function* () {
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Properties

  <a name="properties--dot"></a><a name="12.1"></a>
  - [12.1](#properties--dot) 访问属性时使用`.`. eslint: [`dot-notation`](http://eslint.org/docs/rules/dot-notation.html) jscs: [`requireDotNotation`](http://jscs.info/rule/requireDotNotation)

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    // bad
    const isJedi = luke['jedi'];

    // good
    const isJedi = luke.jedi;
    ```

  <a name="properties--bracket"></a><a name="12.2"></a>
  - [12.2](#properties--bracket) 当访问的属性是变量时使用 `[]` .

    ```javascript
    const luke = {
      jedi: true,
      age: 28,
    };

    function getProp(prop) {
      return luke[prop];
    }

    const isJedi = getProp('jedi');
    ```

**[⬆ back to top](#table-of-contents)**


## Variables

  <a name="variables--const"></a><a name="13.1"></a>
  - [13.1](#variables--const) 总是使用 `const` 声明变量. 不这么做会导致全局变量问题. 要避免污染全局命名空间. eslint: [`no-undef`](http://eslint.org/docs/rules/no-undef) [`prefer-const`](http://eslint.org/docs/rules/prefer-const)

    ```javascript
    // bad
    superPower = new SuperPower();

    // good
    const superPower = new SuperPower();
    ```

  <a name="variables--one-const"></a><a name="13.2"></a>
  - [13.2](#variables--one-const) 每个变量声明前都要使用`const`. eslint: [`one-var`](http://eslint.org/docs/rules/one-var.html) jscs: [`disallowMultipleVarDecl`](http://jscs.info/rule/disallowMultipleVarDecl)

    > 原因：这种方式更易添加变量声明，不用考虑将`;`变成`,`.用debugger逐个调试也很方便，而不是一次跳过所有变量.

    ```javascript
    // bad
    const items = getItems(),
        goSportsTeam = true,
        dragonball = 'z';

    // bad
    // (compare to above, and try to spot the mistake)
    const items = getItems(),
        goSportsTeam = true;
        dragonball = 'z';

    // good
    const items = getItems();
    const goSportsTeam = true;
    const dragonball = 'z';
    ```

  <a name="variables--const-let-group"></a><a name="13.3"></a>
  - [13.3](#variables--const-let-group) 将 `const` 和 `let` 分别放到一起.

    > 原因：当需要给依赖前一个已经赋值的变量的变量赋值时很有用.

    ```javascript
    // bad
    let i, len, dragonball,
        items = getItems(),
        goSportsTeam = true;

    // bad
    let i;
    const items = getItems();
    let dragonball;
    const goSportsTeam = true;
    let len;

    // good
    const goSportsTeam = true;
    const items = getItems();
    let dragonball;
    let i;
    let length;
    ```

  <a name="variables--define-where-used"></a><a name="13.4"></a>
  - [13.4](#variables--define-where-used) 在需要的地方给变量赋值，但位置要合理.

    > 原因：`let` 和 `const` 是块级作用域而不是函数级作用域.

    ```javascript
    // bad - unnecessary function call
    function checkName(hasName) {
      const name = getName();

      if (hasName === 'test') {
        return false;
      }

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }

    // good
    function checkName(hasName) {
      if (hasName === 'test') {
        return false;
      }

      const name = getName();

      if (name === 'test') {
        this.setName('');
        return false;
      }

      return name;
    }
    ```
  <a name="variables--no-chain-assignment"></a><a name="13.5"></a>
  - [13.5](#variables--no-chain-assignment) 变量不要进行链式赋值.

    > 原因：变量链式赋值会创建隐藏的全局变量.

    ```javascript
    // bad
    (function example() {
      // JavaScript interprets this as
      // let a = ( b = ( c = 1 ) );
      // The let keyword only applies to variable a; variables b and c become
      // global variables.
      let a = b = c = 1;
    }());

    console.log(a); // undefined
    console.log(b); // 1
    console.log(c); // 1

    // good
    (function example() {
      let a = 1;
      let b = a;
      let c = a;
    }());

    console.log(a); // undefined
    console.log(b); // undefined
    console.log(c); // undefined

    // the same applies for `const`
    ```

**[⬆ back to top](#table-of-contents)**


## 变量提升

  <a name="hoisting--about"></a><a name="14.1"></a>
  - [14.1](#hoisting--about) `var`声明会被提升到作用域的顶部, 但是赋值不会. `const` 和 `let` 声明有一个新的概念叫 [临时死区 (TDZ)](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/let#Temporal_dead_zone_and_errors_with_let). 了解 [typeof不再安全](http://es-discourse.com/t/why-typeof-is-no-longer-safe/15)很重要.

    ```javascript
    // we know this wouldn't work (assuming there
    // is no notDefined global variable)
    function example() {
      console.log(notDefined); // => throws a ReferenceError
    }

    // creating a variable declaration after you
    // reference the variable will work due to
    // variable hoisting. Note: the assignment
    // value of `true` is not hoisted.
    function example() {
      console.log(declaredButNotAssigned); // => undefined
      var declaredButNotAssigned = true;
    }

    // the interpreter is hoisting the variable
    // declaration to the top of the scope,
    // which means our example could be rewritten as:
    function example() {
      let declaredButNotAssigned;
      console.log(declaredButNotAssigned); // => undefined
      declaredButNotAssigned = true;
    }

    // using const and let
    function example() {
      console.log(declaredButNotAssigned); // => throws a ReferenceError
      console.log(typeof declaredButNotAssigned); // => throws a ReferenceError
      const declaredButNotAssigned = true;
    }
    ```

  <a name="hoisting--anon-expressions"></a><a name="14.2"></a>
  - [14.2](#hoisting--anon-expressions) 匿名函数表达式会提升变量名字而不是函数赋值.

    ```javascript
    function example() {
      console.log(anonymous); // => undefined

      anonymous(); // => TypeError anonymous is not a function

      var anonymous = function () {
        console.log('anonymous function expression');
      };
    }
    ```

  <a name="hoisting--named-expresions"></a><a name="14.3"></a>
  - [14.3](#hoisting--named-expresions) 具名函数表达式提升变量名字而不是函数名字或函数体.

    ```javascript
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      superPower(); // => ReferenceError superPower is not defined

      var named = function superPower() {
        console.log('Flying');
      };
    }

    // the same is true when the function name
    // is the same as the variable name.
    function example() {
      console.log(named); // => undefined

      named(); // => TypeError named is not a function

      var named = function named() {
        console.log('named');
      }
    }
    ```

  <a name="hoisting--declarations"></a><a name="14.4"></a>
  - [14.4](#hoisting--declarations) 函数声明会提升函数名和函数体.

    ```javascript
    function example() {
      superPower(); // => Flying

      function superPower() {
        console.log('Flying');
      }
    }
    ```

  - 更多信息请参考 [JavaScript Scoping & Hoisting](http://www.adequatelygood.com/2010/2/JavaScript-Scoping-and-Hoisting/) by [Ben Cherry](http://www.adequatelygood.com/).

**[⬆ back to top](#table-of-contents)**


## 比较运算符 & 相等

  <a name="comparison--eqeqeq"></a><a name="15.1"></a>
  - [15.1](#comparison--eqeqeq) 使用 `===` 和 `!==`而非 `==` 和 `!=`. eslint: [`eqeqeq`](http://eslint.org/docs/rules/eqeqeq.html)

  <a name="comparison--if"></a><a name="15.2"></a>
  - [15.2](#comparison--if) 条件声明例如 `if` 会用 `ToBoolean` 这个抽象方法将表达式转成布尔值并遵循如下规则:

    + **Objects** evaluate to **true**
    + **Undefined** evaluates to **false**
    + **Null** evaluates to **false**
    + **Booleans** evaluate to **the value of the boolean**
    + **Numbers** evaluate to **false** if **+0, -0, or NaN**, otherwise **true**
    + **Strings** evaluate to **false** if an empty string `''`, otherwise **true**

    ```javascript
    if ([0] && []) {
      // true
      // an array (even an empty one) is an object, objects will evaluate to true
    }
    ```

  <a name="comparison--shortcuts"></a><a name="15.3"></a>
  - [15.3](#comparison--shortcuts) 使用简写.

    ```javascript
    // bad
    if (name !== '') {
      // ...stuff...
    }

    // good
    if (name) {
      // ...stuff...
    }

    // bad
    if (collection.length > 0) {
      // ...stuff...
    }

    // good
    if (collection.length) {
      // ...stuff...
    }
    ```

  <a name="comparison--moreinfo"></a><a name="15.4"></a>
  - [15.4](#comparison--moreinfo) 更多信息请见 [Truth Equality and JavaScript](http://javascriptweblog.wordpress.com/2011/02/07/truth-equality-and-javascript/#more-2108) by Angus Croll.

  <a name="comparison--switch-blocks"></a><a name="15.5"></a>
  - [15.5](#comparison--switch-blocks) 在用 `case` 和 `default` 语句创建的包含有如下词语(如 `let`, `const`, `function` 和 `class`)的代码块中使用花括号 .

  > 原因：这些词语在整个`switch`中可见但是仅仅在当执行到`case`时被赋值的时候初始化.当多条`case`语句试图定义同一个变量时会导致问题.

  eslint rules: [`no-case-declarations`](http://eslint.org/docs/rules/no-case-declarations.html).

    ```javascript
    // bad
    switch (foo) {
      case 1:
        let x = 1;
        break;
      case 2:
        const y = 2;
        break;
      case 3:
        function f() {}
        break;
      default:
        class C {}
    }

    // good
    switch (foo) {
      case 1: {
        let x = 1;
        break;
      }
      case 2: {
        const y = 2;
        break;
      }
      case 3: {
        function f() {}
        break;
      }
      case 4:
        bar();
        break;
      default: {
        class C {}
      }
    }
    ```

  <a name="comparison--nested-ternaries"></a><a name="15.6"></a>
  - [15.6](#comparison--nested-ternaries) 通常三元运算不应该被嵌套应该只是单行表达式.

    eslint rules: [`no-nested-ternary`](http://eslint.org/docs/rules/no-nested-ternary.html).

    ```javascript
    // bad
    const foo = maybe1 > maybe2
      ? "bar"
      : value1 > value2 ? "baz" : null;

    // better
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2
      ? 'bar'
      : maybeNull;

    // best
    const maybeNull = value1 > value2 ? 'baz' : null;

    const foo = maybe1 > maybe2 ? 'bar' : maybeNull;
    ```

  <a name="comparison--unneeded-ternary"></a><a name="15.7"></a>
  - [15.7](#comparison--unneeded-ternary) 避免不必要的三元表达式.

    eslint rules: [`no-unneeded-ternary`](http://eslint.org/docs/rules/no-unneeded-ternary.html).

    ```javascript
    // bad
    const foo = a ? a : b;
    const bar = c ? true : false;
    const baz = c ? false : true;

    // good
    const foo = a || b;
    const bar = !!c;
    const baz = !c;
    ```

**[⬆ back to top](#table-of-contents)**


##（代码）块

  <a name="blocks--braces"></a><a name="16.1"></a>
  - [16.1](#blocks--braces) 多行代码块使用花括号.

    ```javascript
    // bad
    if (test)
      return false;

    // good
    if (test) return false;

    // good
    if (test) {
      return false;
    }

    // bad
    function foo() { return false; }

    // good
    function bar() {
      return false;
    }
    ```

  <a name="blocks--cuddled-elses"></a><a name="16.2"></a>
  - [16.2](#blocks--cuddled-elses) 如果使用`if`和`else`多行代码块，把`else`放到和`if`块的闭合括号同一行. eslint: [`brace-style`](http://eslint.org/docs/rules/brace-style.html) jscs:  [`disallowNewlineBeforeBlockStatements`](http://jscs.info/rule/disallowNewlineBeforeBlockStatements)

    ```javascript
    // bad
    if (test) {
      thing1();
      thing2();
    }
    else {
      thing3();
    }

    // good
    if (test) {
      thing1();
      thing2();
    } else {
      thing3();
    }
    ```


**[⬆ back to top](#table-of-contents)**


## 注释

  <a name="comments--multiline"></a><a name="17.1"></a>
  - [17.1](#comments--multiline) 多行注释使用 `/** ... */` . 包含描述，指定所有参数的类型和值和返回值.

    ```javascript
    // bad
    // make() returns a new element
    // based on the passed in tag name
    //
    // @param {String} tag
    // @return {Element} element
    function make(tag) {

      // ...stuff...

      return element;
    }

    // good
    /**
     * make() returns a new element
     * based on the passed in tag name
     *
     * @param {String} tag
     * @return {Element} element
     */
    function make(tag) {

      // ...stuff...

      return element;
    }
    ```

  <a name="comments--singleline"></a><a name="17.2"></a>
  - [17.2](#comments--singleline) 单行注释使用 `//`. 在要注释部分的上边一行放置注释. 如果不是块的第一行则在注释前留一个空行.

    ```javascript
    // bad
    const active = true;  // is current tab

    // good
    // is current tab
    const active = true;

    // bad
    function getType() {
      console.log('fetching type...');
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // good
    function getType() {
      console.log('fetching type...');

      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }

    // also good
    function getType() {
      // set the default type to 'no type'
      const type = this._type || 'no type';

      return type;
    }
    ```

  <a name="comments--actionitems"></a><a name="17.3"></a>
  - [17.3](#comments--actionitems) 如果想表明这个问题以后会再次被查看或者给出了这个问题的解决方案，在注释前加上前缀`FIXME`或者`TODO`会有利于很快理解问题. 这些不同于一般的注释因为它们是可操作的. 操作是 `FIXME: -- need to figure this out` 或 `TODO: -- need to implement`.

  <a name="comments--fixme"></a><a name="17.4"></a>
  - [17.4](#comments--fixme) 使用 `// FIXME:` 来标注问题.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // FIXME: shouldn't use a global here
        total = 0;
      }
    }
    ```

  <a name="comments--todo"></a><a name="17.5"></a>
  - [17.5](#comments--todo) 使用 `// TODO:` 来标明问题的解决方案.

    ```javascript
    class Calculator extends Abacus {
      constructor() {
        super();

        // TODO: total should be configurable by an options param
        this.total = 0;
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Whitespace

  <a name="whitespace--spaces"></a><a name="18.1"></a>
  - [18.1](#whitespace--spaces) Use soft tabs set to 2 spaces. eslint: [`indent`](http://eslint.org/docs/rules/indent.html) jscs: [`validateIndentation`](http://jscs.info/rule/validateIndentation)

    ```javascript
    // bad
    function foo() {
    ∙∙∙∙const name;
    }

    // bad
    function bar() {
    ∙const name;
    }

    // good
    function baz() {
    ∙∙const name;
    }
    ```

  <a name="whitespace--before-blocks"></a><a name="18.2"></a>
  - [18.2](#whitespace--before-blocks) Place 1 space before the leading brace. eslint: [`space-before-blocks`](http://eslint.org/docs/rules/space-before-blocks.html) jscs: [`requireSpaceBeforeBlockStatements`](http://jscs.info/rule/requireSpaceBeforeBlockStatements)

    ```javascript
    // bad
    function test(){
      console.log('test');
    }

    // good
    function test() {
      console.log('test');
    }

    // bad
    dog.set('attr',{
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });

    // good
    dog.set('attr', {
      age: '1 year',
      breed: 'Bernese Mountain Dog',
    });
    ```

  <a name="whitespace--around-keywords"></a><a name="18.3"></a>
  - [18.3](#whitespace--around-keywords) Place 1 space before the opening parenthesis in control statements (`if`, `while` etc.). Place no space between the argument list and the function name in function calls and declarations. eslint: [`keyword-spacing`](http://eslint.org/docs/rules/keyword-spacing.html) jscs: [`requireSpaceAfterKeywords`](http://jscs.info/rule/requireSpaceAfterKeywords)

    ```javascript
    // bad
    if(isJedi) {
      fight ();
    }

    // good
    if (isJedi) {
      fight();
    }

    // bad
    function fight () {
      console.log ('Swooosh!');
    }

    // good
    function fight() {
      console.log('Swooosh!');
    }
    ```

  <a name="whitespace--infix-ops"></a><a name="18.4"></a>
  - [18.4](#whitespace--infix-ops) Set off operators with spaces. eslint: [`space-infix-ops`](http://eslint.org/docs/rules/space-infix-ops.html) jscs: [`requireSpaceBeforeBinaryOperators`](http://jscs.info/rule/requireSpaceBeforeBinaryOperators), [`requireSpaceAfterBinaryOperators`](http://jscs.info/rule/requireSpaceAfterBinaryOperators)

    ```javascript
    // bad
    const x=y+5;

    // good
    const x = y + 5;
    ```

  <a name="whitespace--newline-at-end"></a><a name="18.5"></a>
  - [18.5](#whitespace--newline-at-end) End files with a single newline character. eslint: [`eol-last`](https://github.com/eslint/eslint/blob/master/docs/rules/eol-last.md)

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);
    ```

    ```javascript
    // bad
    (function (global) {
      // ...stuff...
    })(this);↵
    ↵
    ```

    ```javascript
    // good
    (function (global) {
      // ...stuff...
    })(this);↵
    ```

  <a name="whitespace--chains"></a><a name="18.6"></a>
  - [18.6](#whitespace--chains) Use indentation when making long method chains (more than 2 method chains). Use a leading dot, which
    emphasizes that the line is a method call, not a new statement. eslint: [`newline-per-chained-call`](http://eslint.org/docs/rules/newline-per-chained-call) [`no-whitespace-before-property`](http://eslint.org/docs/rules/no-whitespace-before-property)

    ```javascript
    // bad
    $('#items').find('.selected').highlight().end().find('.open').updateCount();

    // bad
    $('#items').
      find('.selected').
        highlight().
        end().
      find('.open').
        updateCount();

    // good
    $('#items')
      .find('.selected')
        .highlight()
        .end()
      .find('.open')
        .updateCount();

    // bad
    const leds = stage.selectAll('.led').data(data).enter().append('svg:svg').classed('led', true)
        .attr('width', (radius + margin) * 2).append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led')
        .data(data)
      .enter().append('svg:svg')
        .classed('led', true)
        .attr('width', (radius + margin) * 2)
      .append('svg:g')
        .attr('transform', 'translate(' + (radius + margin) + ',' + (radius + margin) + ')')
        .call(tron.led);

    // good
    const leds = stage.selectAll('.led').data(data);
    ```

  <a name="whitespace--after-blocks"></a><a name="18.7"></a>
  - [18.7](#whitespace--after-blocks) Leave a blank line after blocks and before the next statement. jscs: [`requirePaddingNewLinesAfterBlocks`](http://jscs.info/rule/requirePaddingNewLinesAfterBlocks)

    ```javascript
    // bad
    if (foo) {
      return bar;
    }
    return baz;

    // good
    if (foo) {
      return bar;
    }

    return baz;

    // bad
    const obj = {
      foo() {
      },
      bar() {
      },
    };
    return obj;

    // good
    const obj = {
      foo() {
      },

      bar() {
      },
    };

    return obj;

    // bad
    const arr = [
      function foo() {
      },
      function bar() {
      },
    ];
    return arr;

    // good
    const arr = [
      function foo() {
      },

      function bar() {
      },
    ];

    return arr;
    ```

  <a name="whitespace--padded-blocks"></a><a name="18.8"></a>
  - [18.8](#whitespace--padded-blocks) Do not pad your blocks with blank lines. eslint: [`padded-blocks`](http://eslint.org/docs/rules/padded-blocks.html) jscs:  [`disallowPaddingNewlinesInBlocks`](http://jscs.info/rule/disallowPaddingNewlinesInBlocks)

    ```javascript
    // bad
    function bar() {

      console.log(foo);

    }

    // also bad
    if (baz) {

      console.log(qux);
    } else {
      console.log(foo);

    }

    // good
    function bar() {
      console.log(foo);
    }

    // good
    if (baz) {
      console.log(qux);
    } else {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-parens"></a><a name="18.9"></a>
  - [18.9](#whitespace--in-parens) Do not add spaces inside parentheses. eslint: [`space-in-parens`](http://eslint.org/docs/rules/space-in-parens.html) jscs: [`disallowSpacesInsideParentheses`](http://jscs.info/rule/disallowSpacesInsideParentheses)

    ```javascript
    // bad
    function bar( foo ) {
      return foo;
    }

    // good
    function bar(foo) {
      return foo;
    }

    // bad
    if ( foo ) {
      console.log(foo);
    }

    // good
    if (foo) {
      console.log(foo);
    }
    ```

  <a name="whitespace--in-brackets"></a><a name="18.10"></a>
  - [18.10](#whitespace--in-brackets) Do not add spaces inside brackets. eslint: [`array-bracket-spacing`](http://eslint.org/docs/rules/array-bracket-spacing.html) jscs: [`disallowSpacesInsideArrayBrackets`](http://jscs.info/rule/disallowSpacesInsideArrayBrackets)

    ```javascript
    // bad
    const foo = [ 1, 2, 3 ];
    console.log(foo[ 0 ]);

    // good
    const foo = [1, 2, 3];
    console.log(foo[0]);
    ```

  <a name="whitespace--in-braces"></a><a name="18.11"></a>
  - [18.11](#whitespace--in-braces) Add spaces inside curly braces. eslint: [`object-curly-spacing`](http://eslint.org/docs/rules/object-curly-spacing.html) jscs: [`requireSpacesInsideObjectBrackets`](http://jscs.info/rule/requireSpacesInsideObjectBrackets)

    ```javascript
    // bad
    const foo = {clark: 'kent'};

    // good
    const foo = { clark: 'kent' };
    ```

  <a name="whitespace--max-len"></a><a name="18.12"></a>
  - [18.12](#whitespace--max-len) Avoid having lines of code that are longer than 100 characters (including whitespace). Note: per [above](#strings--line-length), long strings are exempt from this rule, and should not be broken up. eslint: [`max-len`](http://eslint.org/docs/rules/max-len.html) jscs: [`maximumLineLength`](http://jscs.info/rule/maximumLineLength)

    > Why? This ensures readability and maintainability.

    ```javascript
    // bad
    const foo = jsonData && jsonData.foo && jsonData.foo.bar && jsonData.foo.bar.baz && jsonData.foo.bar.baz.quux && jsonData.foo.bar.baz.quux.xyzzy;

    // bad
    $.ajax({ method: 'POST', url: 'https://airbnb.com/', data: { name: 'John' } }).done(() => console.log('Congratulations!')).fail(() => console.log('You have failed this city.'));

    // good
    const foo = jsonData
      && jsonData.foo
      && jsonData.foo.bar
      && jsonData.foo.bar.baz
      && jsonData.foo.bar.baz.quux
      && jsonData.foo.bar.baz.quux.xyzzy;

    // good
    $.ajax({
      method: 'POST',
      url: 'https://airbnb.com/',
      data: { name: 'John' },
    })
      .done(() => console.log('Congratulations!'))
      .fail(() => console.log('You have failed this city.'));
    ```

**[⬆ back to top](#table-of-contents)**

## Commas

<a name="commas--leading-trailing"></a><a name="19.1"></a>
  - [19.1](#commas--leading-trailing) Leading commas: **Nope.** eslint: [`comma-style`](http://eslint.org/docs/rules/comma-style.html) jscs: [`requireCommaBeforeLineBreak`](http://jscs.info/rule/requireCommaBeforeLineBreak)

    ```javascript
    // bad
    const story = [
        once
      , upon
      , aTime
    ];

    // good
    const story = [
      once,
      upon,
      aTime,
    ];

    // bad
    const hero = {
        firstName: 'Ada'
      , lastName: 'Lovelace'
      , birthYear: 1815
      , superPower: 'computers'
    };

    // good
    const hero = {
      firstName: 'Ada',
      lastName: 'Lovelace',
      birthYear: 1815,
      superPower: 'computers',
    };
    ```

  <a name="commas--dangling"></a><a name="19.2"></a>
  - [19.2](#commas--dangling) Additional trailing comma: **Yup.** eslint: [`comma-dangle`](http://eslint.org/docs/rules/comma-dangle.html) jscs: [`requireTrailingComma`](http://jscs.info/rule/requireTrailingComma)

    > Why? This leads to cleaner git diffs. Also, transpilers like Babel will remove the additional trailing comma in the transpiled code which means you don't have to worry about the [trailing comma problem](es5/README.md#commas) in legacy browsers.

    ```javascript
    // bad - git diff without trailing comma
    const hero = {
         firstName: 'Florence',
    -    lastName: 'Nightingale'
    +    lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing']
    };

    // good - git diff with trailing comma
    const hero = {
         firstName: 'Florence',
         lastName: 'Nightingale',
    +    inventorOf: ['coxcomb chart', 'modern nursing'],
    };

    // bad
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully'
    };

    const heroes = [
      'Batman',
      'Superman'
    ];

    // good
    const hero = {
      firstName: 'Dana',
      lastName: 'Scully',
    };

    const heroes = [
      'Batman',
      'Superman',
    ];
    ```

**[⬆ back to top](#table-of-contents)**


## Semicolons

  <a name="semicolons--required"></a><a name="20.1"></a>
  - [20.1](#20.1) **Yup.** eslint: [`semi`](http://eslint.org/docs/rules/semi.html) jscs: [`requireSemicolons`](http://jscs.info/rule/requireSemicolons)

    ```javascript
    // bad
    (function () {
      const name = 'Skywalker'
      return name
    })()

    // good
    (function () {
      const name = 'Skywalker';
      return name;
    }());

    // good, but legacy (guards against the function becoming an argument when two files with IIFEs are concatenated)
    ;(() => {
      const name = 'Skywalker';
      return name;
    }());
    ```

    [Read more](http://stackoverflow.com/questions/7365172/semicolon-before-self-invoking-function/7365214%237365214).

**[⬆ back to top](#table-of-contents)**


## Type Casting & Coercion

  <a name="coercion--explicit"></a><a name="21.1"></a>
  - [21.1](#coercion--explicit) Perform type coercion at the beginning of the statement.

  <a name="coercion--strings"></a><a name="21.2"></a>
  - [21.2](#coercion--strings)  Strings:

    ```javascript
    // => this.reviewScore = 9;

    // bad
    const totalScore = this.reviewScore + ''; // invokes this.reviewScore.valueOf()

    // bad
    const totalScore = this.reviewScore.toString(); // isn't guaranteed to return a string

    // good
    const totalScore = String(this.reviewScore);
    ```

  <a name="coercion--numbers"></a><a name="21.3"></a>
  - [21.3](#coercion--numbers) Numbers: Use `Number` for type casting and `parseInt` always with a radix for parsing strings. eslint: [`radix`](http://eslint.org/docs/rules/radix)

    ```javascript
    const inputValue = '4';

    // bad
    const val = new Number(inputValue);

    // bad
    const val = +inputValue;

    // bad
    const val = inputValue >> 0;

    // bad
    const val = parseInt(inputValue);

    // good
    const val = Number(inputValue);

    // good
    const val = parseInt(inputValue, 10);
    ```

  <a name="coercion--comment-deviations"></a><a name="21.4"></a>
  - [21.4](#coercion--comment-deviations) If for whatever reason you are doing something wild and `parseInt` is your bottleneck and need to use Bitshift for [performance reasons](http://jsperf.com/coercion-vs-casting/3), leave a comment explaining why and what you're doing.

    ```javascript
    // good
    /**
     * parseInt was the reason my code was slow.
     * Bitshifting the String to coerce it to a
     * Number made it a lot faster.
     */
    const val = inputValue >> 0;
    ```

  <a name="coercion--bitwise"></a><a name="21.5"></a>
  - [21.5](#coercion--bitwise) **Note:** Be careful when using bitshift operations. Numbers are represented as [64-bit values](http://es5.github.io/#x4.3.19), but bitshift operations always return a 32-bit integer ([source](http://es5.github.io/#x11.7)). Bitshift can lead to unexpected behavior for integer values larger than 32 bits. [Discussion](https://github.com/airbnb/javascript/issues/109). Largest signed 32-bit Int is 2,147,483,647:

    ```javascript
    2147483647 >> 0 //=> 2147483647
    2147483648 >> 0 //=> -2147483648
    2147483649 >> 0 //=> -2147483647
    ```

  <a name="coercion--booleans"></a><a name="21.6"></a>
  - [21.6](#coercion--booleans) Booleans:

    ```javascript
    const age = 0;

    // bad
    const hasAge = new Boolean(age);

    // good
    const hasAge = Boolean(age);

    // best
    const hasAge = !!age;
    ```

**[⬆ back to top](#table-of-contents)**


## Naming Conventions

  <a name="naming--descriptive"></a><a name="22.1"></a>
  - [22.1](#naming--descriptive) Avoid single letter names. Be descriptive with your naming. eslint: [`id-length`](http://eslint.org/docs/rules/id-length)

    ```javascript
    // bad
    function q() {
      // ...stuff...
    }

    // good
    function query() {
      // ..stuff..
    }
    ```

  <a name="naming--camelCase"></a><a name="22.2"></a>
  - [22.2](#naming--camelCase) Use camelCase when naming objects, functions, and instances. eslint: [`camelcase`](http://eslint.org/docs/rules/camelcase.html) jscs: [`requireCamelCaseOrUpperCaseIdentifiers`](http://jscs.info/rule/requireCamelCaseOrUpperCaseIdentifiers)

    ```javascript
    // bad
    const OBJEcttsssss = {};
    const this_is_my_object = {};
    function c() {}

    // good
    const thisIsMyObject = {};
    function thisIsMyFunction() {}
    ```

  <a name="naming--PascalCase"></a><a name="22.3"></a>
  - [22.3](#naming--PascalCase) Use PascalCase only when naming constructors or classes. eslint: [`new-cap`](http://eslint.org/docs/rules/new-cap.html) jscs: [`requireCapitalizedConstructors`](http://jscs.info/rule/requireCapitalizedConstructors)

    ```javascript
    // bad
    function user(options) {
      this.name = options.name;
    }

    const bad = new user({
      name: 'nope',
    });

    // good
    class User {
      constructor(options) {
        this.name = options.name;
      }
    }

    const good = new User({
      name: 'yup',
    });
    ```

  <a name="naming--leading-underscore"></a><a name="22.4"></a>
  - [22.4](#naming--leading-underscore) Do not use trailing or leading underscores. eslint: [`no-underscore-dangle`](http://eslint.org/docs/rules/no-underscore-dangle.html) jscs: [`disallowDanglingUnderscores`](http://jscs.info/rule/disallowDanglingUnderscores)

    > Why? JavaScript does not have the concept of privacy in terms of properties or methods. Although a leading underscore is a common convention to mean “private”, in fact, these properties are fully public, and as such, are part of your public API contract. This convention might lead developers to wrongly think that a change won't count as breaking, or that tests aren't needed. tl;dr: if you want something to be “private”, it must not be observably present.

    ```javascript
    // bad
    this.__firstName__ = 'Panda';
    this.firstName_ = 'Panda';
    this._firstName = 'Panda';

    // good
    this.firstName = 'Panda';
    ```

  <a name="naming--self-this"></a><a name="22.5"></a>
  - [22.5](#naming--self-this) Don't save references to `this`. Use arrow functions or [Function#bind](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Function/bind). jscs: [`disallowNodeTypes`](http://jscs.info/rule/disallowNodeTypes)

    ```javascript
    // bad
    function foo() {
      const self = this;
      return function () {
        console.log(self);
      };
    }

    // bad
    function foo() {
      const that = this;
      return function () {
        console.log(that);
      };
    }

    // good
    function foo() {
      return () => {
        console.log(this);
      };
    }
    ```

  <a name="naming--filename-matches-export"></a><a name="22.6"></a>
  - [22.6](#naming--filename-matches-export) A base filename should exactly match the name of its default export.

    ```javascript
    // file 1 contents
    class CheckBox {
      // ...
    }
    export default CheckBox;

    // file 2 contents
    export default function fortyTwo() { return 42; }

    // file 3 contents
    export default function insideDirectory() {}

    // in some other file
    // bad
    import CheckBox from './checkBox'; // PascalCase import/export, camelCase filename
    import FortyTwo from './FortyTwo'; // PascalCase import/filename, camelCase export
    import InsideDirectory from './InsideDirectory'; // PascalCase import/filename, camelCase export

    // bad
    import CheckBox from './check_box'; // PascalCase import/export, snake_case filename
    import forty_two from './forty_two'; // snake_case import/filename, camelCase export
    import inside_directory from './inside_directory'; // snake_case import, camelCase export
    import index from './inside_directory/index'; // requiring the index file explicitly
    import insideDirectory from './insideDirectory/index'; // requiring the index file explicitly

    // good
    import CheckBox from './CheckBox'; // PascalCase export/import/filename
    import fortyTwo from './fortyTwo'; // camelCase export/import/filename
    import insideDirectory from './insideDirectory'; // camelCase export/import/directory name/implicit "index"
    // ^ supports both insideDirectory.js and insideDirectory/index.js
    ```

  <a name="naming--camelCase-default-export"></a><a name="22.7"></a>
  - [22.7](#naming--camelCase-default-export) Use camelCase when you export-default a function. Your filename should be identical to your function's name.

    ```javascript
    function makeStyleGuide() {
    }

    export default makeStyleGuide;
    ```

  <a name="naming--PascalCase-singleton"></a><a name="22.8"></a>
  - [22.8](#naming--PascalCase-singleton) Use PascalCase when you export a constructor / class / singleton / function library / bare object.

    ```javascript
    const AirbnbStyleGuide = {
      es6: {
      }
    };

    export default AirbnbStyleGuide;
    ```


**[⬆ back to top](#table-of-contents)**


## Accessors

  <a name="accessors--not-required"></a><a name="23.1"></a>
  - [23.1](#accessors--not-required) Accessor functions for properties are not required.

  <a name="accessors--no-getters-setters"></a><a name="23.2"></a>
  - [23.2](#accessors--no-getters-setters) Do not use JavaScript getters/setters as they cause unexpected side effects and are harder to test, maintain, and reason about. Instead, if you do make accessor functions, use getVal() and setVal('hello').

    ```javascript
    // bad
    class Dragon {
      get age() {
        // ...
      }

      set age(value) {
        // ...
      }
    }

    // good
    class Dragon {
      getAge() {
        // ...
      }

      setAge(value) {
        // ...
      }
    }
    ```

  <a name="accessors--boolean-prefix"></a><a name="23.3"></a>
  - [23.3](#accessors--boolean-prefix) If the property/method is a `boolean`, use `isVal()` or `hasVal()`.

    ```javascript
    // bad
    if (!dragon.age()) {
      return false;
    }

    // good
    if (!dragon.hasAge()) {
      return false;
    }
    ```

  <a name="accessors--consistent"></a><a name="23.4"></a>
  - [23.4](#accessors--consistent) It's okay to create get() and set() functions, but be consistent.

    ```javascript
    class Jedi {
      constructor(options = {}) {
        const lightsaber = options.lightsaber || 'blue';
        this.set('lightsaber', lightsaber);
      }

      set(key, val) {
        this[key] = val;
      }

      get(key) {
        return this[key];
      }
    }
    ```

**[⬆ back to top](#table-of-contents)**


## Events

  <a name="events--hash"></a><a name="24.1"></a>
  - [24.1](#events--hash) When attaching data payloads to events (whether DOM events or something more proprietary like Backbone events), pass a hash instead of a raw value. This allows a subsequent contributor to add more data to the event payload without finding and updating every handler for the event. For example, instead of:

    ```javascript
    // bad
    $(this).trigger('listingUpdated', listing.id);

    ...

    $(this).on('listingUpdated', (e, listingId) => {
      // do something with listingId
    });
    ```

    prefer:

    ```javascript
    // good
    $(this).trigger('listingUpdated', { listingId: listing.id });

    ...

    $(this).on('listingUpdated', (e, data) => {
      // do something with data.listingId
    });
    ```

  **[⬆ back to top](#table-of-contents)**


## jQuery

  <a name="jquery--dollar-prefix"></a><a name="25.1"></a>
  - [25.1](#jquery--dollar-prefix) Prefix jQuery object variables with a `$`. jscs: [`requireDollarBeforejQueryAssignment`](http://jscs.info/rule/requireDollarBeforejQueryAssignment)

    ```javascript
    // bad
    const sidebar = $('.sidebar');

    // good
    const $sidebar = $('.sidebar');

    // good
    const $sidebarBtn = $('.sidebar-btn');
    ```

  <a name="jquery--cache"></a><a name="25.2"></a>
  - [25.2](#jquery--cache) Cache jQuery lookups.

    ```javascript
    // bad
    function setSidebar() {
      $('.sidebar').hide();

      // ...stuff...

      $('.sidebar').css({
        'background-color': 'pink'
      });
    }

    // good
    function setSidebar() {
      const $sidebar = $('.sidebar');
      $sidebar.hide();

      // ...stuff...

      $sidebar.css({
        'background-color': 'pink'
      });
    }
    ```

  <a name="jquery--queries"></a><a name="25.3"></a>
  - [25.3](#jquery--queries) For DOM queries use Cascading `$('.sidebar ul')` or parent > child `$('.sidebar > ul')`. [jsPerf](http://jsperf.com/jquery-find-vs-context-sel/16)

  <a name="jquery--find"></a><a name="25.4"></a>
  - [25.4](#jquery--find) Use `find` with scoped jQuery object queries.

    ```javascript
    // bad
    $('ul', '.sidebar').hide();

    // bad
    $('.sidebar').find('ul').hide();

    // good
    $('.sidebar ul').hide();

    // good
    $('.sidebar > ul').hide();

    // good
    $sidebar.find('ul').hide();
    ```

**[⬆ back to top](#table-of-contents)**


## ECMAScript 5 Compatibility

  <a name="es5-compat--kangax"></a><a name="26.1"></a>
  - [26.1](#es5-compat--kangax) Refer to [Kangax](https://twitter.com/kangax/)'s ES5 [compatibility table](http://kangax.github.io/es5-compat-table/).

**[⬆ back to top](#table-of-contents)**

## ECMAScript 6 Styles

  <a name="es6-styles"></a><a name="27.1"></a>
  - [27.1](#es6-styles) This is a collection of links to the various ES6 features.

1. [Arrow Functions](#arrow-functions)
1. [Classes](#constructors)
1. [Object Shorthand](#es6-object-shorthand)
1. [Object Concise](#es6-object-concise)
1. [Object Computed Properties](#es6-computed-properties)
1. [Template Strings](#es6-template-literals)
1. [Destructuring](#destructuring)
1. [Default Parameters](#es6-default-parameters)
1. [Rest](#es6-rest)
1. [Array Spreads](#es6-array-spreads)
1. [Let and Const](#references)
1. [Iterators and Generators](#iterators-and-generators)
1. [Modules](#modules)

**[⬆ back to top](#table-of-contents)**

## Testing

  <a name="testing--yup"></a><a name="28.1"></a>
  - [28.1](#testing--yup) **Yup.**

    ```javascript
    function foo() {
      return true;
    }
    ```

  <a name="testing--for-real"></a><a name="28.2"></a>
  - [28.2](#testing--for-real) **No, but seriously**:
   - Whichever testing framework you use, you should be writing tests!
   - Strive to write many small pure functions, and minimize where mutations occur.
   - Be cautious about stubs and mocks - they can make your tests more brittle.
   - We primarily use [`mocha`](https://www.npmjs.com/package/mocha) at Airbnb. [`tape`](https://www.npmjs.com/package/tape) is also used occasionally for small, separate modules.
   - 100% test coverage is a good goal to strive for, even if it's not always practical to reach it.
   - Whenever you fix a bug, _write a regression test_. A bug fixed without a regression test is almost certainly going to break again in the future.

**[⬆ back to top](#table-of-contents)**


## Performance

  - [On Layout & Web Performance](http://www.kellegous.com/j/2013/01/26/layout-performance/)
  - [String vs Array Concat](http://jsperf.com/string-vs-array-concat/2)
  - [Try/Catch Cost In a Loop](http://jsperf.com/try-catch-in-loop-cost)
  - [Bang Function](http://jsperf.com/bang-function)
  - [jQuery Find vs Context, Selector](http://jsperf.com/jquery-find-vs-context-sel/13)
  - [innerHTML vs textContent for script text](http://jsperf.com/innerhtml-vs-textcontent-for-script-text)
  - [Long String Concatenation](http://jsperf.com/ya-string-concat)
  - [Are Javascript functions like `map()`, `reduce()`, and `filter()` optimized for traversing arrays?](https://www.quora.com/JavaScript-programming-language-Are-Javascript-functions-like-map-reduce-and-filter-already-optimized-for-traversing-array/answer/Quildreen-Motta)
  - Loading...

**[⬆ back to top](#table-of-contents)**


## Resources

**Learning ES6**

  - [Draft ECMA 2015 (ES6) Spec](https://people.mozilla.org/~jorendorff/es6-draft.html)
  - [ExploringJS](http://exploringjs.com/)
  - [ES6 Compatibility Table](https://kangax.github.io/compat-table/es6/)
  - [Comprehensive Overview of ES6 Features](http://es6-features.org/)

**Read This**

  - [Standard ECMA-262](http://www.ecma-international.org/ecma-262/6.0/index.html)

**Tools**

  - Code Style Linters
    + [ESlint](http://eslint.org/) - [Airbnb Style .eslintrc](https://github.com/airbnb/javascript/blob/master/linters/.eslintrc)
    + [JSHint](http://jshint.com/) - [Airbnb Style .jshintrc](https://github.com/airbnb/javascript/blob/master/linters/.jshintrc)
    + [JSCS](https://github.com/jscs-dev/node-jscs) - [Airbnb Style Preset](https://github.com/jscs-dev/node-jscs/blob/master/presets/airbnb.json)

**Other Style Guides**

  - [Google JavaScript Style Guide](https://google.github.io/styleguide/javascriptguide.xml)
  - [jQuery Core Style Guidelines](http://contribute.jquery.org/style-guide/js/)
  - [Principles of Writing Consistent, Idiomatic JavaScript](https://github.com/rwaldron/idiomatic.js)

**Other Styles**

  - [Naming this in nested functions](https://gist.github.com/cjohansen/4135065) - Christian Johansen
  - [Conditional Callbacks](https://github.com/airbnb/javascript/issues/52) - Ross Allen
  - [Popular JavaScript Coding Conventions on Github](http://sideeffect.kr/popularconvention/#javascript) - JeongHoon Byun
  - [Multiple var statements in JavaScript, not superfluous](http://benalman.com/news/2012/05/multiple-var-statements-javascript/) - Ben Alman

**Further Reading**

  - [Understanding JavaScript Closures](http://javascriptweblog.wordpress.com/2010/10/25/understanding-javascript-closures/) - Angus Croll
  - [Basic JavaScript for the impatient programmer](http://www.2ality.com/2013/06/basic-javascript.html) - Dr. Axel Rauschmayer
  - [You Might Not Need jQuery](http://youmightnotneedjquery.com/) - Zack Bloom & Adam Schwartz
  - [ES6 Features](https://github.com/lukehoban/es6features) - Luke Hoban
  - [Frontend Guidelines](https://github.com/bendc/frontend-guidelines) - Benjamin De Cock

**Books**

  - [JavaScript: The Good Parts](http://www.amazon.com/JavaScript-Good-Parts-Douglas-Crockford/dp/0596517742) - Douglas Crockford
  - [JavaScript Patterns](http://www.amazon.com/JavaScript-Patterns-Stoyan-Stefanov/dp/0596806752) - Stoyan Stefanov
  - [Pro JavaScript Design Patterns](http://www.amazon.com/JavaScript-Design-Patterns-Recipes-Problem-Solution/dp/159059908X)  - Ross Harmes and Dustin Diaz
  - [High Performance Web Sites: Essential Knowledge for Front-End Engineers](http://www.amazon.com/High-Performance-Web-Sites-Essential/dp/0596529309) - Steve Souders
  - [Maintainable JavaScript](http://www.amazon.com/Maintainable-JavaScript-Nicholas-C-Zakas/dp/1449327680) - Nicholas C. Zakas
  - [JavaScript Web Applications](http://www.amazon.com/JavaScript-Web-Applications-Alex-MacCaw/dp/144930351X) - Alex MacCaw
  - [Pro JavaScript Techniques](http://www.amazon.com/Pro-JavaScript-Techniques-John-Resig/dp/1590597273) - John Resig
  - [Smashing Node.js: JavaScript Everywhere](http://www.amazon.com/Smashing-Node-js-JavaScript-Everywhere-Magazine/dp/1119962595) - Guillermo Rauch
  - [Secrets of the JavaScript Ninja](http://www.amazon.com/Secrets-JavaScript-Ninja-John-Resig/dp/193398869X) - John Resig and Bear Bibeault
  - [Human JavaScript](http://humanjavascript.com/) - Henrik Joreteg
  - [Superhero.js](http://superherojs.com/) - Kim Joar Bekkelund, Mads Mobæk, & Olav Bjorkoy
  - [JSBooks](http://jsbooks.revolunet.com/) - Julien Bouquillon
  - [Third Party JavaScript](https://www.manning.com/books/third-party-javascript) - Ben Vinegar and Anton Kovalyov
  - [Effective JavaScript: 68 Specific Ways to Harness the Power of JavaScript](http://amzn.com/0321812182) - David Herman
  - [Eloquent JavaScript](http://eloquentjavascript.net/) - Marijn Haverbeke
  - [You Don't Know JS: ES6 & Beyond](http://shop.oreilly.com/product/0636920033769.do) - Kyle Simpson

**Blogs**

  - [DailyJS](http://dailyjs.com/)
  - [JavaScript Weekly](http://javascriptweekly.com/)
  - [JavaScript, JavaScript...](http://javascriptweblog.wordpress.com/)
  - [Bocoup Weblog](https://bocoup.com/weblog)
  - [Adequately Good](http://www.adequatelygood.com/)
  - [NCZOnline](https://www.nczonline.net/)
  - [Perfection Kills](http://perfectionkills.com/)
  - [Ben Alman](http://benalman.com/)
  - [Dmitry Baranovskiy](http://dmitry.baranovskiy.com/)
  - [Dustin Diaz](http://dustindiaz.com/)
  - [nettuts](http://code.tutsplus.com/?s=javascript)

**Podcasts**

  - [JavaScript Jabber](https://devchat.tv/js-jabber/)


**[⬆ back to top](#table-of-contents)**

## In the Wild

  This is a list of organizations that are using this style guide. Send us a pull request and we'll add you to the list.

  - **4Catalyzer**: [4Catalyzer/javascript](https://github.com/4Catalyzer/javascript)
  - **Aan Zee**: [AanZee/javascript](https://github.com/AanZee/javascript)
  - **Adult Swim**: [adult-swim/javascript](https://github.com/adult-swim/javascript)
  - **Airbnb**: [airbnb/javascript](https://github.com/airbnb/javascript)
  - **Apartmint**: [apartmint/javascript](https://github.com/apartmint/javascript)
  - **Ascribe**: [ascribe/javascript](https://github.com/ascribe/javascript)
  - **Avalara**: [avalara/javascript](https://github.com/avalara/javascript)
  - **Avant**: [avantcredit/javascript](https://github.com/avantcredit/javascript)
  - **Billabong**: [billabong/javascript](https://github.com/billabong/javascript)
  - **Bisk**: [bisk/javascript](https://github.com/Bisk/javascript/)
  - **Blendle**: [blendle/javascript](https://github.com/blendle/javascript)
  - **Brainshark**: [brainshark/javascript](https://github.com/brainshark/javascript)
  - **Chartboost**: [ChartBoost/javascript-style-guide](https://github.com/ChartBoost/javascript-style-guide)
  - **ComparaOnline**: [comparaonline/javascript](https://github.com/comparaonline/javascript-style-guide)
  - **Compass Learning**: [compasslearning/javascript-style-guide](https://github.com/compasslearning/javascript-style-guide)
  - **DailyMotion**: [dailymotion/javascript](https://github.com/dailymotion/javascript)
  - **DoSomething**: [DoSomething/eslint-config](https://github.com/DoSomething/eslint-config)
  - **Digitpaint** [digitpaint/javascript](https://github.com/digitpaint/javascript)
  - **Ecosia**: [ecosia/javascript](https://github.com/ecosia/javascript)
  - **Evernote**: [evernote/javascript-style-guide](https://github.com/evernote/javascript-style-guide)
  - **Evolution Gaming**: [evolution-gaming/javascript](https://github.com/evolution-gaming/javascript)
  - **EvozonJs**: [evozonjs/javascript](https://github.com/evozonjs/javascript)
  - **ExactTarget**: [ExactTarget/javascript](https://github.com/ExactTarget/javascript)
  - **Expensify** [Expensify/Style-Guide](https://github.com/Expensify/Style-Guide/blob/master/javascript.md)
  - **Flexberry**: [Flexberry/javascript-style-guide](https://github.com/Flexberry/javascript-style-guide)
  - **Gawker Media**: [gawkermedia/javascript](https://github.com/gawkermedia/javascript)
  - **General Electric**: [GeneralElectric/javascript](https://github.com/GeneralElectric/javascript)
  - **GoodData**: [gooddata/gdc-js-style](https://github.com/gooddata/gdc-js-style)
  - **Grooveshark**: [grooveshark/javascript](https://github.com/grooveshark/javascript)
  - **How About We**: [howaboutwe/javascript](https://github.com/howaboutwe/javascript-style-guide)
  - **Huballin**: [huballin/javascript](https://github.com/huballin/javascript)
  - **HubSpot**: [HubSpot/javascript](https://github.com/HubSpot/javascript)
  - **Hyper**: [hyperoslo/javascript-playbook](https://github.com/hyperoslo/javascript-playbook/blob/master/style.md)
  - **InfoJobs**: [InfoJobs/JavaScript-Style-Guide](https://github.com/InfoJobs/JavaScript-Style-Guide)
  - **Intent Media**: [intentmedia/javascript](https://github.com/intentmedia/javascript)
  - **Jam3**: [Jam3/Javascript-Code-Conventions](https://github.com/Jam3/Javascript-Code-Conventions)
  - **JeopardyBot**: [kesne/jeopardy-bot](https://github.com/kesne/jeopardy-bot/blob/master/STYLEGUIDE.md)
  - **JSSolutions**: [JSSolutions/javascript](https://github.com/JSSolutions/javascript)
  - **KickorStick**: [kickorstick/javascript](https://github.com/kickorstick/javascript)
  - **Kinetica Solutions**: [kinetica/javascript](https://github.com/kinetica/Javascript-style-guide)
  - **M2GEN**: [M2GEN/javascript](https://github.com/M2GEN/javascript)
  - **Mighty Spring**: [mightyspring/javascript](https://github.com/mightyspring/javascript)
  - **MinnPost**: [MinnPost/javascript](https://github.com/MinnPost/javascript)
  - **MitocGroup**: [MitocGroup/javascript](https://github.com/MitocGroup/javascript)
  - **ModCloth**: [modcloth/javascript](https://github.com/modcloth/javascript)
  - **Money Advice Service**: [moneyadviceservice/javascript](https://github.com/moneyadviceservice/javascript)
  - **Muber**: [muber/javascript](https://github.com/muber/javascript)
  - **National Geographic**: [natgeo/javascript](https://github.com/natgeo/javascript)
  - **National Park Service**: [nationalparkservice/javascript](https://github.com/nationalparkservice/javascript)
  - **Nimbl3**: [nimbl3/javascript](https://github.com/nimbl3/javascript)
  - **Orion Health**: [orionhealth/javascript](https://github.com/orionhealth/javascript)
  - **OutBoxSoft**: [OutBoxSoft/javascript](https://github.com/OutBoxSoft/javascript)
  - **Peerby**: [Peerby/javascript](https://github.com/Peerby/javascript)
  - **Razorfish**: [razorfish/javascript-style-guide](https://github.com/razorfish/javascript-style-guide)
  - **reddit**: [reddit/styleguide/javascript](https://github.com/reddit/styleguide/tree/master/javascript)
  - **React**: [/facebook/react/blob/master/CONTRIBUTING.md#style-guide](https://github.com/facebook/react/blob/master/CONTRIBUTING.md#style-guide)
  - **REI**: [reidev/js-style-guide](https://github.com/rei/code-style-guides/blob/master/docs/javascript.md)
  - **Ripple**: [ripple/javascript-style-guide](https://github.com/ripple/javascript-style-guide)
  - **SeekingAlpha**: [seekingalpha/javascript-style-guide](https://github.com/seekingalpha/javascript-style-guide)
  - **Shutterfly**: [shutterfly/javascript](https://github.com/shutterfly/javascript)
  - **Springload**: [springload/javascript](https://github.com/springload/javascript)
  - **StratoDem Analytics**: [stratodem/javascript](https://github.com/stratodem/javascript)
  - **SteelKiwi Development**: [steelkiwi/javascript](https://github.com/steelkiwi/javascript)
  - **StudentSphere**: [studentsphere/javascript](https://github.com/studentsphere/guide-javascript)
  - **SysGarage**: [sysgarage/javascript-style-guide](https://github.com/sysgarage/javascript-style-guide)
  - **Syzygy Warsaw**: [syzygypl/javascript](https://github.com/syzygypl/javascript)
  - **Target**: [target/javascript](https://github.com/target/javascript)
  - **TheLadders**: [TheLadders/javascript](https://github.com/TheLadders/javascript)
  - **The Nerdery**: [thenerdery/javascript-standards](https://github.com/thenerdery/javascript-standards)
  - **T4R Technology**: [T4R-Technology/javascript](https://github.com/T4R-Technology/javascript)
  - **VoxFeed**: [VoxFeed/javascript-style-guide](https://github.com/VoxFeed/javascript-style-guide)
  - **WeBox Studio**: [weboxstudio/javascript](https://github.com/weboxstudio/javascript)
  - **Weggo**: [Weggo/javascript](https://github.com/Weggo/javascript)
  - **Zillow**: [zillow/javascript](https://github.com/zillow/javascript)
  - **ZocDoc**: [ZocDoc/javascript](https://github.com/ZocDoc/javascript)

**[⬆ back to top](#table-of-contents)**

## Translation

  This style guide is also available in other languages:

  - ![br](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Brazil.png) **Brazilian Portuguese**: [armoucar/javascript-style-guide](https://github.com/armoucar/javascript-style-guide)
  - ![bg](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Bulgaria.png) **Bulgarian**: [borislavvv/javascript](https://github.com/borislavvv/javascript)
  - ![ca](https://raw.githubusercontent.com/fpmweb/javascript-style-guide/master/img/catala.png) **Catalan**: [fpmweb/javascript-style-guide](https://github.com/fpmweb/javascript-style-guide)
  - ![cn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/China.png) **Chinese (Simplified)**: [sivan/javascript-style-guide](https://github.com/sivan/javascript-style-guide)
  - ![tw](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Taiwan.png) **Chinese (Traditional)**: [jigsawye/javascript](https://github.com/jigsawye/javascript)
  - ![fr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/France.png) **French**: [nmussy/javascript-style-guide](https://github.com/nmussy/javascript-style-guide)
  - ![de](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Germany.png) **German**: [timofurrer/javascript-style-guide](https://github.com/timofurrer/javascript-style-guide)
  - ![it](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Italy.png) **Italian**: [sinkswim/javascript-style-guide](https://github.com/sinkswim/javascript-style-guide)
  - ![jp](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Japan.png) **Japanese**: [mitsuruog/javascript-style-guide](https://github.com/mitsuruog/javascript-style-guide)
  - ![kr](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/South-Korea.png) **Korean**: [tipjs/javascript-style-guide](https://github.com/tipjs/javascript-style-guide)
  - ![pl](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Poland.png) **Polish**: [mjurczyk/javascript](https://github.com/mjurczyk/javascript)
  - ![ru](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Russia.png) **Russian**: [uprock/javascript](https://github.com/uprock/javascript)
  - ![es](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Spain.png) **Spanish**: [paolocarrasco/javascript-style-guide](https://github.com/paolocarrasco/javascript-style-guide)
  - ![th](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Thailand.png) **Thai**: [lvarayut/javascript-style-guide](https://github.com/lvarayut/javascript-style-guide)
  - ![vn](https://raw.githubusercontent.com/gosquared/flags/master/flags/flags/shiny/24/Vietnam.png) **Vietnam**: [giangpii/javascript-style-guide](https://github.com/giangpii/javascript-style-guide)

## The JavaScript Style Guide Guide

  - [Reference](https://github.com/airbnb/javascript/wiki/The-JavaScript-Style-Guide-Guide)

## Chat With Us About JavaScript

  - Find us on [gitter](https://gitter.im/airbnb/javascript).

## Contributors

  - [View Contributors](https://github.com/airbnb/javascript/graphs/contributors)


## License

(The MIT License)

Copyright (c) 2014-2016 Airbnb

Permission is hereby granted, free of charge, to any person obtaining
a copy of this software and associated documentation files (the
'Software'), to deal in the Software without restriction, including
without limitation the rights to use, copy, modify, merge, publish,
distribute, sublicense, and/or sell copies of the Software, and to
permit persons to whom the Software is furnished to do so, subject to
the following conditions:

The above copyright notice and this permission notice shall be
included in all copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED 'AS IS', WITHOUT WARRANTY OF ANY KIND,
EXPRESS OR IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF
MERCHANTABILITY, FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT.
IN NO EVENT SHALL THE AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY
CLAIM, DAMAGES OR OTHER LIABILITY, WHETHER IN AN ACTION OF CONTRACT,
TORT OR OTHERWISE, ARISING FROM, OUT OF OR IN CONNECTION WITH THE
SOFTWARE OR THE USE OR OTHER DEALINGS IN THE SOFTWARE.

**[⬆ back to top](#table-of-contents)**

## Amendments

We encourage you to fork this guide and change the rules to fit your team's style guide. Below, you may list some amendments to the style guide. This allows you to periodically update your style guide without having to deal with merge conflicts.

# };



