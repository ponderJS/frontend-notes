## 1、变量声明 let const

> 相同点：
* 不存在变量提升，先声明后使用；
* 局部变量/块级作用域，且绑定当前作用域；
* 当前作用域内不允许重复声明，包括用var声明的变量；


```
块级作用域：用 {} 扩起来的内容为块级作用域，let、const 声明的变量仅在此作用域内有效；尽量避免在块级作用域内用函数声明式方法定义变量，此时的函数声明处理方式类似于var方式，会发生变量提升，但可以采用表达式的方式声明函数。
```
> 不同点：
* const 声明的为只读常量，声明的同时必须赋值，而且值不可改变；

```
const声明本质上不是变量的值不得改动，而是变量指向的那个内存地址不得改动；
```

> 总结：ES5声明变量的方法有两种：var function，ES6新增了let const import class。

## 2、解构赋值

> 定义：从数组和对象中提取值，对变量进行赋值。
* 只要某种数据结构具有 Iterator 接口，都可以采用数组形式的解构赋值，包括Set结构；
* 允许指定默认值，匹配的赋值对象只有严格等于undefined时默认值才会生效，默认值也可引用解构赋值的其他变量，但该变量必须已经声明；
* 惰性求值；
* 作为函数的参数也能解构赋值，同时函数的参数可指定默认值；

## 3、字符串扩展

>  通过{}扩展Unicode码点范围：原码点范围在\u0000~\uFFFF之间，超出这个范围的字符，必须用双字节的形式表示，导致这些单个字符串长度为2。

* codePointAt方法能正确返回32位的 UTF-16 字符的码点，识别范围超过charCodeAt方法

```
获得字符串编码  'str'.chartCodeAt(index)     'str'.codePointAt(index)
编码转字符串    String.fromCharCode('')      String.fromCodePoint('')
```

* for...of循环能正确识别32位的UTF-16字符的码点

## 4、正则

* 字符串可以使用正则的方法match()、replace()、search()和split()；
* 添加正则修饰符 u，用来处理大于\uFFFF的 Unicode 字符
* 添加正则修饰符 y，作用与g修饰符类似，也是全局匹配，但是必须从上一次匹配成功的下一个位置(起始)位置开始

```
注意标志符为全局 g
String.prototype.match 返回一个数组，数组的第一项是进行匹配完整的字符串，之后的项是与圆括号组合后捕获的结果。如果没有匹配到，返回null
RegExp.prototype.exec  返回一个数组，「并更新正则表达式对象的属性，下次从上个节点」，完全匹配成功的文本作为第一项，只用正则括号里的匹配项填充到数组后面
```

## 5、数组

> 空位处理：[,,,,]

ES6明确将空位转为undefined，ES6新增的方法也正确处理，包括：entries()、keys()、values()、find()、findIndex()、copyWithin()、fill()、Array.from、扩展运算符、for...of循环;   
ES5原有的方法forEach(), filter(), reduce(), every() 和some(）都做忽略空位处理，map()保留空位，join()和toString()会将空位视为undefined，而undefined和null最后会被处理成空字符串；  
空位处理方法不同，所以要尽量避免出现空位。

## 6、数值

* ES6二进制用前缀0b或0B表示，八进制数值用前缀0o或0O表示。
* 新增规范了一些方法


## 7、对象

* 直接写入变量和函数，作为对象的属性和方法，注意，简洁写法的属性名总是字符串
```
    var hello='Hello';
    let obj ={
        hello,
        foo(){}
    }
```
* 使用属性名表达式
```
    let propKey = 'foo';
    let obj = {
        [propKey]: true,
        ['a' + 'bc']: 123
    };
```

> 简洁写法于属性表达式不可同时使用

* 相等判断 

Object.is() 与严格比较运算符（===）的行为基本一致，但两个例外：+0不等于-0；NaN等于自身。


## class

ES6引入了 Class（类）这个概念，作为对象的模板，通过class关键字，可以定义类。可以看作是一个语法糖，它的绝大部分功能，ES5都可以做到。

* 使用对象简写的方式

```
    class Foo(){
        constructor(){

        }
        method(){

        }
    }
```

* 通过类的内部所有定义的方法，都是不可枚举的。ES5方式在原型上定义的方法是可枚举的(constructor除外)
```
    var Point = function (x, y) {
    // ...
    };

    Point.prototype.toString = function() {
    // ...
    };

    Object.keys(Point.prototype)
    // ["toString"]
    Object.getOwnPropertyNames(Point.prototype)
    // ["constructor","toString"]
```
* 类的属性名，可以采用表达式
```
    let methodName = 'getArea';

    class Square {
        constructor(length) {
            // ...
        }

        [methodName]() {
            // ...
        }
    }
```
* 类和模块的内部，默认使用严格模式
* 类必须有constructor方法，如果没有显式定义，会默认添加一个空的constructor方法
* 类必须使用new调用，否则会报错
* 表达式方式定义类
```
    const MyClass = class Me {
        getClassName() {
            return Me.name;
        }
    };
```
> 这个类的名字是MyClass而不是Me，Me只在 Class 的内部代码可用，指代当前类。也可以省略Me，如下：
```
    const MyClass = class { /* ... */ };
```
* 立即执行类
```
    let person = new class {
        constructor(age) {
            this.age = age;
        }
        sayAge() {
            console.log(this.age);
        }
    }(18);
    person.sayAge(); // 18
```
* 和let、const一样，不允许重复定义，必须先声明后使用
* 提案：私有属性/方法
* this默认指向实例
* 静态方法，使用static关键字，静态方法不被实例继承，可以被子类继承和调用，也可以从super对象上调用
```
    class Foo(){
        static someMethod(){
            ...
        }
    }
    // 等同于
    class Foo(){}
    Foo.someMethod=function(){}
```
* 提案：静态属性，也使用static关键字。ES6明确规定，类内部只有静态方法，没有静态属性
```
    class Foo(){
        static someProp=1;

    }
    // 等同于
    class Foo(){}
    Foo.someProp=1;
```
* 提案：实例属性，直接写在类的定义中，不必非要写在constructor里
```
    class Foo(){
        state=1;
    }
    // 等同于
    class Foo(){
        constructor(){
            super();
            this.state=1;
        }
    }
```
* new.target，一般用在构造函数之中，返回new命令的那个构造函数
> 子类继承父类时，使用new.target会返回子类

## extend

Class通过extends关键字实现继承

* 子类必须在constructor方法中调用super方法
```
ES5的继承，实质是先创造子类的实例对象this，然后再将父类的方法添加到this上面（Parent.apply(this)）。ES6 的继承机制完全不同，实质是先创造父类的实例对象this（所以必须先调用super方法），然后再用子类的构造函数修改this。
```
* super这个关键字，既可以当作函数使用，也可以当作对象使用
> 作为函数时，super()只能用在子类的构造函数之中，用在其他地方就会报错
> super作为对象时，在普通方法中，指向父类的原型对象；在静态方法中，指向父类的静态方法
* this指向，在子类的普通方法中指向子类的实例，在子类的静态方法中指向子类本身
* 子类没有定义constructor方法，这个方法会被默认添加
* prototype & __proto__
> 大多数浏览器的 ES5 实现之中，每一个对象都有__proto__属性，指向对应的构造函数的prototype属性。
> Class作为构造函数的语法糖，同时有prototype属性和__proto__属性，因此同时存在两条继承链。1.子类的__proto__属性，表示构造函数的继承，总是指向父类；子类prototype属性的__proto__属性，表示方法的继承，总是指向父类的prototype属性。

## Symbol

ES6 引入了一种新的原始数据类型Symbol，表示独一无二的值。它是 JavaScript 语言的第七种数据类型，前六种是：undefined、null、布尔值（Boolean）、字符串（String）、数值（Number）、对象（Object）。

* Symbol值通过Symbol函数生成，Symbol值不是对象，不能添加属性，是一种类似于字符串的数据类型
```
Symbol函数可以接受一个字符串作为参数，表示对 Symbol 实例的描述，主要是为了在控制台显示，或者转为字符串时，比较容易区分
```
* Symbol函数前不能使用new命令，否则会报错
* Symbol值不能与其他类型的值进行运算，会报错
```
Symbol值可以显式的转为字符串
    let sym = Symbol('My symbol');
    String(sym) // 'Symbol(My symbol)'
    sym.toString() // 'Symbol(My symbol)' 

Symbol值可以转为布尔值(真)
    let sym = Symbol();
    Boolean(sym) // true
    !sym  // false
```
* Symbol值作为对象属性时不能用点运算符方式，必须放在方括号之中
* Symbol值作为属性名时，该属性还是公开属性，不是私有属性
```
Symbol值作为属性名不会出现在for...in、for...of循环中，也不会被Object.keys()、Object.getOwnPropertyNames()、JSON.stringify()返回。Object.getOwnPropertySymbols方法，可以获取指定对象的所有Symbol属性名，Reflect.ownKeys方法也可以返回所有类型的键名，包括Symbol属性名；
利用Symbol属性名不会被常规方法遍历到的特性，可以为对象定义一些非私有的、但又希望只用于内部的方法。
```

## Proxy

Proxy用于修改某些默认行为，等同于在语言层面做出修改，所以属于一种“元编程”（meta programming），即对编程语言进行编程。可以理解为在目标对象之前架设一层“拦截”，外界对该对象的访问，都必须先通过这层拦截，因此提供了一种机制，可以对外界的访问进行过滤和改写。Proxy 这个词的原意是代理，用在这里表示由它来“代理”某些操作，可以译为“代理器”。

```
    var proxy = new Proxy(target, handler);
```
## Reflect

Reflect对象与Proxy对象一样，也是ES6为了操作对象而提供的新API

* 将Object对象的一些明显属于语言内部的方法（比如Object.defineProperty），放到Reflect对象上
```
现阶段，某些方法同时在Object和Reflect对象上部署，未来的新方法将只部署在Reflect对象上。也就是说，从Reflect对象上可以拿到语言内部的方法
```
* 修改某些Object方法的返回结果，让其变得更合理
```
Object.defineProperty(obj, name, desc)在无法定义属性时，会抛出一个错误，而Reflect.defineProperty(obj, name, desc)则会返回false
```
* 让Object操作都变成函数行为
```
name in obj和delete obj[name]对应Reflect.has(obj, name)和Reflect.deleteProperty(obj, name)
```
* Reflect对象的方法与Proxy对象的方法一一对应
```
Proxy对象可以方便地调用对应的Reflect方法，完成默认行为，作为修改行为的基础。也就是说，不管Proxy怎么修改默认行为，总可以在Reflect上获取默认行为
```
* Reflect对象一共有 13 个静态方法
```
    Reflect.apply(target, thisArg, args)
    Reflect.construct(target, args)
    Reflect.get(target, name, receiver)
    Reflect.set(target, name, value, receiver)
    Reflect.defineProperty(target, name, desc)
    Reflect.deleteProperty(target, name)
    Reflect.has(target, name)
    Reflect.ownKeys(target)
    Reflect.isExtensible(target)
    Reflect.preventExtensions(target)
    Reflect.getOwnPropertyDescriptor(target, name)
    Reflect.getPrototypeOf(target)
    Reflect.setPrototypeOf(target, prototype)
```


