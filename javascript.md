# 笔记

**这里记录我在学习过程中记的笔记，方便我日后复习巩固**

# JS

## 1.数组

- **push()**：在数组的尾部插入一个或多个元素，返回插入后数组的长度。
- **pop()**：在数组的尾部删除一个元素，返回被删除的元素。
- **shift()**：在数组的头部删除一个元素，返回被删除的元素。
- **unshift()**：在数组的头部插入一个或多个元素，返回插入后数组的长度
- **concat()**：组合数组，返回一个新数组。
- **join()**：拼接数组。
- **fill()**：填充数组。
- **forEach()**：对数组执行一个 for 循环，无返回操作。
- **map()**：对数组执行一次遍历，返回在执行方法过程中返回的元素集。
- **filter()**：对数组执行过滤操作，返回过滤条件中为 true 的元素。
- **some()**：对数组执行遍历操作，当存在至少一项是满足特定条件的时候，返回 true
- **every()**：对数组执行遍历操作，当所有项都满足特定条件的时候，返回 true
- **find()**：从数组头部执行遍历操作，当发现第一项满足特定条件的时候，返回这个元素。
- **findIndex()**：从数组头部执行遍历操作，当发现第一项满足特定条件的时候，返回这个的索引
- **isArray()**：判断是否是数组。
- **splice()**：对数组进行添加、删除、替换操作，返回被删除的元素。
- **slice()**：对数组进行截取操作，返回截取的元素。
- **includes()**：判断数组是否包含给定的项。
- **indexOf()**：返回数组第一个匹配到的索引。
- **lastIndexOf()**：返回数组最后一个匹配到的索引。
- **sort()**：对数组进行排序，默认情况下按字母顺序进行排序，但是如果传入一个方法，可以根据方法返回的值继续排序。返回 1 排在后面，返回-1 排在前面，返回 0 排序不变。
- **reverse()**：对数组进行翻转操作。
- **reduce()**：对数组从左到右进行累加操作，接受两个参数，第一个是一个回调方法，第二个可选，表示累加的初始值，不传默认数组第一项。
- **reduceRight()**：对数组从右到左进行累加操作。
  
  > 其中会改变原数组的方法有 push、pop、shift、unshift、splice、reverse、sort。

## 2.对象

### 对象创建

**_解释_**：对象的创建指的是在 js 中创建一个对象的过程，有工厂模式、构造函数模式、原型模式、Class 四种模式

1. **工厂模式**：

   ```javascript
   function createObj(name, age) {
     const obj = {};
     obj.name = name;
     obj.age = age;
     return obj;
   }
   let newObj = createObj("Ning", "26");
   ```

2. **构造函数模式**: 构造函数模式函数首字母大写，用 new 调用。该方法的缺点是构造函数里的方法无法得到重用，每创建一个构造函数实例就会定义一次方法，浪费内存。解决这个方法是可以将方法提到构造函数外面，这样即做到复用，但当方法特别多的时候就不方便管理了。

   ```javascript
   function CreateObj(name, age) {
     this.name = name;
     this.age = age;
     this.sayName = function () {
       console.log(this.name, this.age);
     };
   }
   let newObj = new CreateObj("Ning", "26");
   ```

3. **原型模式**：使用原型模式可以轻松的创建一个对象，在原型模式上，里面的方法和属性在实例上是共享的，当创建多个属性时，可以节省内存。但缺点是原型上所有方法和属性都是共享的，当修改了原型的方法或属性，就会对所有实例造成影响。

   ```javascript
   function CreateObj(name, age) {
     this.name = name;
     this.age = age;
   }
   CreateObj.prototype.sayName = function () {
     console.log(this.name);
   };

   let newObj = new CreateObj("name", 26);
   ```

4. **Class 模式**：Class 类构造函数是 ES6 的一个语法糖，本质上他是使用原型的模式来创建的

   ```javascript
   class CreateObj {
     constructor(name, age) {
       this.name = name;
       this.age = age;
     }
     sayName() {
       console.log(this.name);
     }
   }

   let newObj = new CreateObj("name", 26);
   ```

### 继承（7种）

**_解释_**：JS 继承是一种机制，允许我们用一个对象继承另一个对象的属性和方法，这使得可以共用现有代码。同时允许我们在子类中添加或修改继承到的方法和属性。

1. **原型链继承**：在原型链中，子对象的 prototype 指向父类的 prototype，父类的 prototype 指向祖父的 prototype，以此类推！原型链的最高级指向 Object.prototype，值为 null。而原型链继承就是运用了这个机制，子类通过 prototype 继承父类的属性和方法，所有实例共享原型链上的属性和方法。

   > 优点：通过原型链继承，所有属性共享原型链上的方法和属性

   > 缺点：由于是共享原型链的属性和方法，一但修改原型链的属性或方法，会对实例造成影响

   ```javascript
   // 原型链继承
   function Parent() {
     this.name = "ning";
     this.nickName = ["ning1", "ning2"];
   }

   Parent.prototype.sayHi = function () {
     console.log(`Hi ${this.name}`);
   };

   function Child(age) {
     this.age = age;
   }

   Child.prototype = new Parent(); // 原型链继承
   let instance1 = new Child(2);
   instance1.sayHi(); // Hi ning

   //缺点就是共享原型链的方法，会造成属性污染
   let instance2 = new Child(3);
   instance2.nickName.push("ning3");

   console.log(instance1.nickName); // ['ning1', 'ning2', 'ning3']
   ```

2. **构造函数继承**：子类通过构造函数调用父类的构造函数，以继承父类的属性

   > 优点：不涉及原型链，只涉及继承，从而不会存在状态共享问题；可以传递参数给父类，更灵活；

   ```javascript
   function Parent(name) {
     this.name = name;
     this.nickName = ["ning1", "ning2"];
   }
   function Child(name, age) {
     this.age = age;
     Parent.call(this, name);
   }
   let instance1 = new Child("ning", 2);
   let instance2 = new Child("yu", 3);
   instance1.nickName.push("ning3");
   //实例之间的属性不会污染
   console.log(instance1.nickName); //['ning1', 'ning2', 'ning3']
   console.log(instance2.nickName); //['ning1', 'ning2']
   ```

   > 缺点：由于不使用原型链，使用无法继承原型链上的属性和方法；只能继承父类原型的属性，不能继承父类原型的方法；每个实例都保存父类实例函数的副本，无法实现共用，影响性能！

   ```javascript
   function Parent(name) {
     this.name = name;
     this.nickName = ["ning1", "ning2"];
   }
   Parent.prototype.sayHi = function () {
     console.log(`Hi ${this.name}`);
   };
   function Child(name, age) {
     this.age = age;
     Parent.call(this, name);
   }
   let instance1 = new Child("ning", 2);
   //无法继承父对象的方法
   instance1.sayHi(); //TypeError: instance1.sayHi is not a function
   ```

3. **组合式继承**：解决构造函数继承和原型链继承存在的问题，结合这两种继承的优点。在子类的构造函数调用父类的构造函数，并且让子类继承父类的原型链。

   > 优点：结合构造函数和原型链继承的优点，既能继承父对象的属性又能继承原型链的属性，同时还不会存在属性污染的问题。

   > 缺点：父类构造函数被调用两次，存在性能问题；

   ```javascript
   function Parent(name) {
     this.name = name;
     this.nickName = ["ning1", "ning2"];
   }
   Parent.prototype.sayHi = function () {
     console.log(`Hi ${this.name}`);
   };
   function Child(name, age) {
     this.age = age;
     Parent.call(this, name); //第一次调用Parent
   }
   Child.prototype = new Parent(); //第二次调用Parent
   Child.prototype.constructor = Child;

   let instance1 = new Child("ning", 2);
   let instance2 = new Child("yu", 3);
   instance1.nickName.push("ning3");
   //实例之间的属性不会污染
   console.log(instance1.nickName); //['ning1', 'ning2', 'ning3']
   console.log(instance2.nickName); //['ning1', 'ning2']
   //可以继承原型的方法
   instance1.sayH1(); //Hi ning
   console.log(instance1 instanceof Parent); // true
   ```

4. **原型继承**： 通过一个空对象为中介，直接将传入的对象做为这个空对象的 prototype，然后 new 返回这个空对象

   > 优点：可以继承多个对象的属性和方法，支持多重继承；相对比较简单，有现成的方法 Object.create()可以实现

   > 缺点：子对象共享父对象的属性，可能存在篡改属性；并且无法传递参数给父对象的构造函数

   ```javascript
   function object(obj) {
     function F() {}
     F.prototype = obj;
     return new F();
   }
   let parent = {
     name: "ning",
     nickName: ["ning1", "ning2"],
     sayHi: function () {
       console.log(`hi! ${this.name}`);
     },
   };
   let child1 = object(parent); //等同于Object.create()方法
   let child2 = object(parent);
   child1.sayHi(); // hi! ning
   //可以串改父对象的属性
   child1.name = "yu";
   child1.sayHi(); //hi! yu
   //但共享的不受影响
   child2.sayHi(); //hi！ning
   ```

5. **寄生式继承**：在原型继承的基础上修改，在一个方法中封装继承的过程，达到增强对象的目的，然后返回它。

   > 优点：可以在不修改原有对象的情况下向其添加属性和方法。

   > 缺点：不能使用 instanceof 方法，比较难追本溯源；

   ```javascript
   function inheritObj(obj) {
     let newObj = Object.create(obj);
     //根据需求增强对象
     newObj.sayName = function () {
       console.log(`my name is ${this.name}`);
     };
     return newObj;
   }
   let parent = {
     name: "ning",
     nickName: ["ning1", "ning2"],
     sayHi: function () {
       console.log(`hi! ${this.name}`);
     },
   };
   let child1 = inheritObj(parent);
   //调用继承自父对象的方法
   child1.sayHi(); //h1 ning
   //调用新添加的方法
   child1.sayName(); //my name is ning
   ```

6. **寄生组合式继承**：和组合式继承类似，结合了构造函数和寄生式继承的优点。核心思想是在创建子类构造函数时，不调用父类构造函数的实例，而是使用原型链继承，然后在寄生式继承的基础上，将父类的原型复制给子类的原型。这就避免了调用两次父类构造函数。寄生组合式继承是当下库使用最多的方法，结合了其他继承，将其取其精华，去其糟粕！

   > 优点：避免了调用两次父了构造函数，提升了性能；保留了原型链，可以使用 instanceof 追本溯源；
   > 缺点：相对其他继承来说比较复杂；

   ```javascript
   function inheritPrototype(child, parent) {
     let prototype = Object.create(parent.prototype); //使用原型继承父类的原型
     parent.constructor = child; //重置子类构造函数的引用
     child.prototype = prototype; //子类原型设置为父类原型的副本
   }

   function Parent(name) {
     this.name = name;
     this.nickName = ["ning1", "ning2"];
   }
   Parent.prototype.sayHi = function () {
     console.log(`Hi ${this.name}`);
   };
   function Child(name, age) {
     this.age = age;
     Parent.call(this, name); //只调用了一次父类构造函数，提升了性能
   }
   inheritPrototype(Child, Parent);
   Child.prototype.sayNme = function () {
     console.log(`my name is ${this.name}`);
   };

   let instance1 = new Child("ning", 2);
   let instance2 = new Child("yu", 3);
   instance1.nickName.push("ning3");
   //修改原型属性，实例间不受影响
   console.log(instance1.nickName); //['ning1', 'ning2', 'ning3']
   console.log(instance2.nickName); //['ning1', 'ning2']
   //调用父类的方法
   instance1.sayHi(); //Hi ning
   //调用子类的方法
   instance1.sayNme(); //my name is ning
   //可以追本溯源
   console.log(instance1 instanceof Parent); //true
   ```

7. **Class 类继承**：运用 es6 的类继承，可以更简单直观的定义继承关系

   ```javascript
   // ES6 类继承
   class Parent {
     constructor(name) {
       this.name = name;
     }
     sayHi() {
       console.log(`Hi ${this.name}`);
     }
   }
   //使用extends关键字继承
   class Child extends Parent {
     constructor(name, age) {
       super(name);
       this.age = age;
     }
     sayName() {
       console.log(`my name is ${this.name}`);
     }
   }
   let instance1 = new Child("ning", 2);
   //调用父类的方法
   instance1.sayHi(); //Hi ning
   //调用子类的方法
   instance1.sayName(); //my name is ning
   ```

   **总结**：JavaScript 中的继承通常可以分为两大类：原型类继承和原型链类继承。原型类继承包括原型继承、寄生继承和寄生组合继承，而原型链类继承包括原型链继承、构造函数继承和组合继承。这两大类继承方式各自解决了不同的问题。

> **组合继承**：结合了原型链继承和构造函数继承的特点，克服了原型链继承中的属性污染问题，以及构造函数继承中无法共享父类原型上的方法的问题。

> **寄生组合继承**：寄生组合继承将组合继承进一步改进，解决了子对象修改父对象时对其他子对象产生影响的问题，同时避免了原型链中无法获取原型链上的属性和参数，以及不能传递参数给父对象的问题。寄生组合继承结合了各种继承方式的优点，是目前大多数库使用的高效继承方法。

## 3.迭代器和生成器

- 迭代器：迭代器提供一个遍历集合的统一接口，他是一个特殊的对象。迭代器通常包含一个next方法，该方法返回两个值。如果一个对象想要被for of遍历，需要在内部实现一个迭代器
```javascript
const array = [1,2,3]
const iterator = array[Symbol.iterator]() //创建一个迭代器
const result = iterator.next() //提供了一个统一的接口来遍历这个集合
```

- 生成器：生成器是 es6 的一个函数，和普通函数不同，生成器里面的代码不会一下子执行完，可以控制暂停和恢复执行。该函数返回一个生成器对象。
```javascript
function* generator() {
  yield 1;
  yield 2;
}
const numberGenerator = generator() //创建一个生成器，不会立即执行
const result = numberGenerator.next()  //执行生成器，直到遇见yield，就返回yield的值
```

## 4.IIFE表达式
IIFE在js中定义并且立即执行的函数，主要用于创建私有作用域，可以有效的封装和隔离代码，避免变量污染全局命名空间
```javascript
(function () {
//这里的作用域是私有的，不会污染到全局的命名空间，在函数完毕之后，这里的作用域会被摧毁
})()
```

## 5.函数柯里化
函数柯里化是指将一个接收多个参数的函数转为每次只接收一个参数的嵌套函数表现形式。柯里化的主要目的是将多参数的函数转为一个参数序列，这样可以逐个的使用这些参数
```javascript
//柯里化的简单实现
function add(a){
  return function(b) {
    return function(c) {
      return a + b + c
    }
  }
}
add(1)(2)(3)

//柯里化的高级实现
function curriedAdd(fun) {
  return function curried(...args1) {
    //当传入的参数数量等于或多于原函数时，则直接调用
    if(fun.length >= args.length) {
      return fun.apply(this, args1)
    } else {
      //如果少于则是柯里化调用，返回一个参数进行接收剩余参数
      return function(...args2) {
         return curried(this, args1.concat(args2))
      }
    }
  }
}

const curriedFun = curriedAdd(fun)
curriedFun(1,2)(3) //可以自由组合入参数量
```

## 6. Map、Set和WeakMap、WeakSet
是es6的新增数据结构，用`new Map()`等创建一个方法，可以执行增删查的操作，获取长度等
- Map是保存键值对的集合，key可以是任意类型的值
- Set是保存唯一值的集合，value可以是任意类型，但是必须是唯一的
- WeakMap和WeakSet都是前两者的弱引用，即当原始的引用被清除后，保存在Weak里的值也会被清除掉，有利于垃圾回收机制回收，是不可遍历的 

Set可用于数组去重`[...new Set(array)]`

## 7. 数组去重方法（4种）
```javascript
const array = [1,2,2,3,1,3]
//1.用 Set 
const uniqueArray1 = [...new Set(array)]
//2、用fitter
const uniqueArray2 = array.fitter((item, index) => array.indexof(item) === index)
//3、用Map
const arrMap = new Map()
array.forEach((item) => arrMap.set(item, true))
const uniqueArray3 = Array.from(arrMap.keys())
//4、先排序后去重
```

## 8. 数据类型检测（5种）

- typeOf：只能准确检测基础类型，null、数组和对象检测都是`object`，不能判断基础内建对象
- instanceof: 检测是否属于原型链上的属性， 如`[] instanceof Array`
- constructor：检测是否是该类型的构造函数，`([]).constructor === Array`
- Object.toString.call()：准确无误的判断数据类型，`Object.prototype.toString.call([]) === '[object Array]'`
- Array.isArray()：只能判断数组

## 9. Promise
用于函数的异步编程，解决传统方法的回掉地狱问题

- 实例方法有`promise.then(),promise.catch(),promise.finally()`

**静态方法**
- Promise.resolve()：返回一个Promise对象，视为成功的结果
- Promise.reject()：返回一个Promise对象，视为失败的结果
- Promise.all()：接收一个Promise对象集合，全部执行完成返回执行结果的数组，如果有一项失败，则返回对应的失败
- Promise.arrSettled()：等待所有的Promise都执行完成，返回对应的结果集合，不管是成功还是失败
- Promise.race()：接收Promise对象集合，返回最先执行完成的结果，不管是成功还是失败
- Promise.any()：接收Promise对象集合，当其中的一个Promise执行成功，返回执行成功的结果，如果都失败，返回失败的集合

## 10. async/await
```javascript
async function fetchData() {
    try {
        // 等待异步操作（例如网络请求）完成
        const response = await fetch('https://example.com/data');
        const data = await response.json();
        return data; // 返回的是一个解决的 Promise
    } catch (error) {
        // 处理异步操作中的错误
        console.error('Error fetching data:', error);
        throw error; // 抛出异常将返回一个拒绝的 Promise
    }
}

// 使用 async 函数
fetchData().then(data => {
    console.log('Fetched data:', data);
}).catch(error => {
    console.error('Error:', error);
});
```

## 11. Class

- super只能用在派生的子类中，用来调用父类的构造函数以及调用父类的方法，子类的this只能在调用super之后才能使用。
- static定义静态方法，使得Class不需要被new调用可以直接使用其静态方法。传统的构造函数静态方法在Function.static直接定义。

## 12. Proxy和Reflect

- Proxy是一个代理对象，用于对一个对象进行监听他的操作`let proxy = new Proxy(target, handle)`
- Reflect是一个内置属性，不能当作构造函数调用，只有静态方法。Reflect的静态方法于Object的方法类似，每一个reflect方法都有一个proxy的方法与之对应。

## 13. ES新特性
- 展开操作符和解构
- `Promise.any(),Promise.allSettled(),Promise.finally()`
- `Array.prototype.flat(),Array.prototype.flatMap()`扁平化和映射
- import动态导入
- `Object.values(), Object.entries()`
- 逻辑运算符：`||=`相当于 `a = a || b`如果a是真值则将a设置为b, `&&=, ??=`

## 14. 模块化
JavaScript模块化是指将大型的代码库分解成可重用、可维护的小块，这有助于代码组织、依赖管理和团队协作。模块化允许我们封装细节，只暴露必要的API，从而提高了代码的可维护性和可扩展性。

目前广泛应用的模块化解决方案有
- CommonJS: CommonJS广泛用于Node.js环境，它支持同步加载模块，非常适合服务端开发。但由于其同步的特性，直接在浏览器端使用可能会导致性能问题。
- AMD和RequireJS：“AMD是为浏览器端设计的异步模块加载方案，通过RequireJS实现。它允许在运行时按需加载模块，适合大型的浏览器应用，但可能会导致回调地狱。
- UMD：“UMD是一种兼容CommonJS和AMD的模块定义方法，使得模块可以跨环境使用。虽然灵活，但随着ES模块的普及，UMD的必要性有所降低。
- ES Modules：ES模块是JavaScript官方的模块化标准，支持静态导入和导出，允许编译时优化，如树摇（Tree Shaking）。现代浏览器和最新版本的Node.js都原生支持ES模块，它是推荐的模块化方案。

CommonJS的同步加载：适用于服务器环境，因为模块加载完成后代码才继续执行，但在浏览器环境中可能导致性能问题，因为它会阻塞代码执行直到模块被下载和解析。
ES Modules的静态结构：指的是ESM必须放在文件的顶层中，不可以放在函数或者条件判断中加载。但最新的es新特性已经支持动态加载。

## 15. setTimeOut和requestAnimationFrame
两者都是js用来延迟代码执行的解决方案
- setTimeOut是web api的一部分，主要是用在指定的时间后执行回调，但是具体的执行时间会受其他因素的影响，不适于频繁执行
- requestAnimationFrame是浏览器 Api的一部分，主要是用于优化动画加载的效果，延迟执行的时间根据浏览器刷新的频率而定，在浏览器下一次执行重绘的时候执行回调。

## 16. 浮点数表示
在IEEE 754标准中，一个浮点数由三个部分组成：符号位、指数位和尾数（或称有效数字）位。这种表示方法并不总是能够精确表示像0.1、0.2这样的小数。这些值在转换为二进制时会变成无限循环小数，因此必须在某处截断，这导致了精度的损失。
# 设计模式

## 工厂模型

**_解释_**：提供一个创建对象的接口，允许子类决定实例化哪一个类。工厂模型类似于开发一个超级产品，而开发这个产品的过程中，拆分成多种不同的生产线，我们把这些生产线就称为工厂模式的子类型，当需要构建一个新产品时，把这些生产线拿出来组装即可

## 原型模式

**_解释_**：通过克隆现有对象来创建新对象。原型模式更贴近于 js 编程思想，在 js 中，每个对象都有一个原型对象而这个原型对象指向父类的原型对象，当我们需要扩展这个对象时，可以通过原型去扩展。

## 单例模型

**_解释_**：确保一个类只有一个实例，并提供一个全局的访问点来访问这个实例。单例模型指的是一个对象只有一个实例对象，当我们多次 new 一个构造函数时，其结果都指向第一个 new 的对象，并且提供全局统一的方法去访问它，Vuex 就运用了单例模型的思想。

```javascript
//代码实现
class SingleInstance {
  constructor() {
    //查看是否已经存在实例，有则直接返回
    if (SingleInstance.instance) return SingleInstance.instance;
    this.value = Math.random();
    //创建一个实例
    SingleInstance.instance = this;
    return this;
  }
}
```

## 适配器模型

**_解释_**：当一个接口返回的参数不是我们想要的参数时，我们可以封装一个方法去适配这个接口的返回参数，以达到我们想要的目的，这就是适配器模型；可以理解为电子产品中的转换器，如将 use-b 转换为 type-c 接口。

## 装饰器模型

**_解释_**：指在不改变原有对象的情况下，再对接口进行封装扩展，使其能满足更复杂的需求。

## 代理模型

**_解释_**：代码模型指是通过一个中间商对一个事件进行操作，如 Proxy 操作，A 访问 B，不能直接访问，而是通过访问 C 才能访问到 B

## 观察者模型

**_解释_**：观察者模型定义了一个一对多的依赖关系，让多个观察者同时监听一个一个对象，当这个对象发生状态更新时，可以通知这些观察者，使它们能够及时更新。通常存在一个被称为“主题”的对象，它直接维护其观察者的列表，并在状态改变时直接通知它们。

```javascript
//代码实现
function Observer() {
  this.upData = function (message) {
    console.log(`this message is ${message}`);
  };
}

function Subject() {
  this.observers = [];

  this.addObserver = function (fun) {
    this.observers.push(fun);
  };

  this.removeObserver = function (fun) {
    this.observers.splice(index, 1);
  };

  this.notify = function (data) {
    if (!this.observers.length) return;
    for (let item of this.observers) {
      item.upData(data);
    }
  };
}
//构建观察者
let instance1 = new Observer();
let instance2 = new Observer();

//构建主题对象
let subject = new Subject();

//添加到依赖中
subject.addObserver(instance1);
subject.addObserver(instance2);

//通知观察者们
subject.notify("1");
```

## 发布-订阅模型

**_解释_**：和观察者模型类似，区别在于当对象发生变化时，观察者模型是主动去通知众多观察者更新状态；而发布-订阅模型是当对象发生变化不主动通知，而是发步到一个中转平台上，而观察者们去订阅这个平台的消息，当发生变更时就会通知到观察者及时更新状态。通常有一个中间层，即事件频道或消息代理。发布者发布消息到一个频道，而订阅者订阅那个频道来接收消息。发布者和订阅者通常不直接知道彼此的存在。

```javascript
//代码实现
class PubSub {
  constructor() {
    this.subscribers = {};
  }

  subscribe(event, callBack) {
    if (!this.subscribers[event]) this.subscribers[event] = [];

    this.subscribers[event].push(callBack);
  }

  unsubscribe(event, callBack) {
    if (!this.subscribers[event]) return;

    let index = this.subscribers[event].indexOf(callBack);
    if (index > -1) {
      this.subscribers[event].splice(index, 1);
    }
  }

  publish(event, data) {
    if (!this.subscribers[event] || !this.subscribers[event].length) return;

    for (let item of this.subscribers[event]) {
      item(data);
    }
  }
}

const fun1 = (data) => {
  console.log(`this fun1 is ${data}`);
};
const fun2 = (data) => {
  console.log(`this fun2 is ${data}`);
};
//构建发布/订阅对象
const subscribers = new PubSub();
//订阅event频道
subscribers.subscribe("event", fun1);
subscribers.subscribe("event", fun2);

//发布通知
subscribers.publish("event", "6");
```

# CSS

## 1.盒模型

CSS盒模型有两种，一种为标准盒模型，一种为怪异盒模型（IE）盒模型

- 标准盒模型：`box-sizing：content-box;`默认值
- 怪异盒模型：`box-sizing：border-box;`宽高度不包括`margin`

## 2.CSS选择器（11种）

- 类选择器、标签选择器、ID选择器
- 属性选择器 `input[type="text"]`
- 后代选择器`div p`，div后面的所有p标签起作用
- 子类选择器`div > p`，div后面的子元素是p标签的起作用
- 相邻兄弟选择器`div + p`，跟div同一级别并且是后面的第一个p标签起作用
- 通用兄弟选择器`div ~ p`，跟div同一级别后面的所有p标签起作用
- 伪类选择器`div:hover`，用于给div定于特定的状态，如`input:focus`
- 伪元素选择器`div::before`，用于在div的前面加入一个元素。
- 组合选择器`div, .class, #id`

> 伪类选择器主要用于描述一个元素的状态的改变，而伪元素选择器更多用来给一个元素的前后插入一个样式

## 3.CSS优先级（6个）

CSS的优先级排序根据每种的选择器的特异性值相加，最高的值就是最终的显示结果
!important > 内联样式(1000)  > ID选择器(100) > 类选择器、伪类、属性选择器(10) > 标签选择器、伪元素选择器(1) > 其他的选择器

比如 `.class #id div p`这个的特异性值 10 + 100 + 1 + 1 = 111

## 4.BFC

BFC（块级格式化上下文），指的是在文档流中，独立出来一个渲染块，bfc 里面的元素布局不影响外面的布局

会创建 BFC 的元素有

- float 为 left、right
- overflow 不为 visible
- display 为 flex、inline-block、table-cell、grid 等
- position 为 absolute、flexed
- 根元素

BFC 的布局规则是

- BFC 内的元素一块一块的垂直折叠
- BFC 内的两个相邻的块级元素会发生 margin 重叠
- BFC 内浮动的元素高度也会被计算（用于清除浮动）
- BFC 里面的元素布局不会影响到外面的元素的布局

BFC 的作用

- 消除两个相邻的元素 margin 重叠问题，把其中一个元素添加一个新的 BFC 里面
- 浮动元素会脱离父元素的文档流，此时可以给父元素创建一个 BFC
- 解决文字环绕浮动元素的问题。
- 用于实现多栏布局，用浮动实现侧边栏固定
  
清除浮动可以给父容器添加 BFC，还可以使用 clearfix 定义一个伪类元素 clear：both

## 5.CSS3

- 引入了更多的选择器，如属性选择器、伪类选择器、伪元素选择器
- 盒模型控制`box-sizing`
- 新增了文本效果，如文字阴影`text-shadow`、文字下沉`::first-letter`
- 多背景支持，`background`设置多个背景，`border-image`设置边框为背景图片，`border-radius`设置圆角，`box-shadow`设置盒子阴影效果。
- 2D/3D转换，`transformation`控制元素平移、转换、放大等效果
- 动画，`keyframes`控制关键字帧，`animation`动画效果
- 过渡，`transition`控制元素的过度效果
- flex和grid布局
- colors可以设置RGBA，控制透明度等
- 媒体查询，`@media screen and(min-width: 600px) { .class {}}`当屏幕至少为600px时，应用`.class`这个类。

## 6.CSS样式隔离

CSS样式隔离的目的是确保CSS样式只应用于它们应该影响的特定元素或者组件，避免全局的意外冲突。通常我们在定义样式的名字时，遵守一定的命名约定，从而降低冲突。在vue中，可以给style定义一个`scoped`属性。

## 7.层叠上下文

层叠上下文用于控制，元素在当前层的展示位置，`z-index`的值越大，显示的层级就更高，将优先展现在屏幕上。层叠上下文只能在同一个父元素下的子元素比较才起作用，比如

```html
<h1>
  <h2><h3></h3></h2>
  <h4></h4>
</h1>
```

当h1创建了层叠上下文，那么h3可以和h4相对于h1比较层级。

创建层叠上下文的方法有

- 根元素
- 设置position的元素
- opacity，transform，filter等不是默认值
- flex、grid的直接子元素

## 7. div居中(6种)

div居中可以分为块元素和行内元素居中

- 使用flex实现水平、垂直居中
- 使用gird实现水平、垂直居中`display: gird; place-items: center`
- 使用 position 实现水平、垂直居中
- 使用 `margin: 0 auto` 实现水平居中，
- 对于行内元素使用`line-heigh`实现垂直居中，使用`text-align`实现水平居中。

## 8. float

浮动可用于环绕效果，当设置了浮动的元素会脱离文档流，而非浮动的元素则会环绕它的周围。

清除浮动用于解决浮动元素导致的高度塌陷问题，可以用一个伪元素定义一个`clear: both`来清除浮动，为其父元素定义`overflow: hidden`，或者设置`flex`

## 9. flex布局

- `flex: 1 0 200px`表示`flex-grow: 1; flex-shrink: 0; flex-basis: 200px`的简写
- 当 Flex 容器空间充足时，`flex-grow` 决定了项目如何分配多余的空间，默认值 0 ，不会增长。
- 当 Flex 容器空间紧张时，`flex-shrink` 决定了项目如何减少它们的尺寸以适应容器，默认值为 1 会缩小。
- `align-self`: 允许单个Flex项目有不同于其他项目的对齐方式。

## 10. 移动端适配

- 使用相对单位，如em、rem、vh、vw等
- 使用媒体查询
- 使用 Flex 进行弹性布局
- 使用流式布局，不使用相对单位使用百分比单位

## 11. em、rem、rpx的原理

3者都是普遍用于移动端适配

- em单位大小相对于直接父元素的字体大小来决定
- rem单位相对于跟元素的字体大小来决定，通过设定根元素的字体大小，当后面使用rem时，都会以根元素字体大小来定义
- rpx是微信小程序的单位，用于适配不同的屏幕大小。在微信小程序中，屏幕被分为750份，在宽度750px的屏幕中，1rpx就等于1px。微信小程序回按照这一比例自动的适配不同的屏幕大小

## 12. CSS性能优化

养成良好的编程习惯，可以很大程度上避免一些问题

- 减少重绘和重排
- 优化选择器，避免使用标签选择器，推荐使用类选择器。避免过度的嵌套选择器，这样会增加浏览器匹配的负担，同时应尽量避免使用*通配符选择器，这样会对页面上的每一个元素都进行适配。
- 减少CSS的体积，使用压缩工具将CSS进行压缩。
- 使用外联样式表，这样有利于浏览器缓存，并且把外联样式提前，放到head标签中，会跟页面并行加载
- 使用css模块化，按需加载
- 合理的使用png、jpg、svg、webp格式的图片，以优化图片的加载时间。
- 使用link加载样式，避免使用@import。link加载是跟页面并行加载的，js可以动态的获取和修改，是所有浏览器支持的。而@import是从一个CSS文件导入另一个CSS文件专门用于加载CSS文件，会导致渲染延迟，并且不支持js操作样式。
- 尽量使用flex布局，通常更高效。
- 合并css文件，如果页面加载10个css文件,每个文件1k，那么也要比只加载一个100k的css文件慢。
- 不要在ID选择器前面进行嵌套，ID本来就是唯一的而且权限值大，嵌套完全是浪费性能。
- 建立公共样式类，把相同样式提取出来作为公共类使用。
- 巧妙运用css的继承机制，如果父节点定义了，子节点就无需定义。
- 拆分出公共css文件，对于比较大的项目可以将大部分页面的公共结构样式提取出来放到单独css文件里，这样一次下载 后就放到缓存里，当然这种做法会增加请求，具体做法应以实际情况而定。
- 不用css表达式，表达式只是让你的代码显得更加酷炫，但是对性能的浪费可能是超乎你想象的。
- 少用css rest，可能会觉得重置样式是规范，但是其实其中有很多操作是不必要不友好的，有需求有兴趣，可以选择normolize.css。
- cssSprite，合成所有icon图片，用宽高加上background-position的背景图方式显现icon图，这样很实用，减少了http请求。

## 13. 移动端兼容问题汇总

- 屏幕适配大小：使用rem或媒体查询等适应不同大小的屏幕
- 触摸事件：在移动端中使用触摸屏操作，保障需要点击触摸的元素足够大，才能确保有效的点击。
- 在meta设置`viewport`来控制布局在移动端上的表现
- 在不同的移动端浏览器对js和css的实现可能不一致，因此使用新特性应该先看好兼容性。
- 在移动端中尽量使用flex布局，可以解决不同浏览器之间的布局不一致问题

## 14. 图片格式

- PNG：提供无损压缩支持透明度，适用于需要高质量的背景图或透明的场景使用，但是文件大小通常也更大
- JPG：支持高压缩率，适用于色彩丰富的图片，文件大小通常都更小，加载快，不正常透明图，有损压缩
- SVG：矢量图形，xml格式，不需要发送请求，可以无限缩放而不失真，支持透明，可以压缩，适用于icon小图标，文件小，但是不适合复杂的图像
- GIF：支持简单的动画和透明度，但是色彩有限，不适合复杂或高质量的图像
- WebP：谷歌的现代图片格式，提供无损和有损压缩，使用于优化加载时间的网页，特别是移动端，文件体积小，支持透明和动画，但是是新的格式在旧的浏览器可能不兼容。

## 15. preload和prefetch

- preload用于优化当前页面的资源加载，即在当前的页面中哪些资源是可能需要的应该提前加载
- prefetch用于优化未来导航页面的加载，即在未来导航到的页面中哪些资源是可能需要的应该提前加载，浏览器可以在空闲时间就加载资源而不必等到需要的时候再加载

使用方式：`<link rel="preload" href="main.js" as="script" />`

## 16. src和href

- src用于用于嵌入内容，当元素需要保护其他资源时使用，会阻塞浏览器渲染
- href用于建立连接，用于连接另外一个页面，不会阻塞浏览器渲染

## 17. meta标签

meta标签定义在head标签内，用于定义浏览器的元数据。提供了关于 HTML 文档的信息，如字符集、控制视口行为、页面描述、关键词、作者、缓存行为等

- 指定字符集：`<meta charset="utf-8"/>`
- 控制视口行为：`<meta name="viewport" content="with=device-width, initial-scale=1" />`
- 页面描述：`<meta name="description" content="这是一个页面" />`
- 设置关键字：`<meta name="keywords" content="关键字1，关键字二" />`
- 设置作者：`<meta name="auth" content="ning" />`
- 用于缓存：`<meta http-equiv="Catch-Control" content="no-store" />` 
- 兼容性模式：`<meta http-equiv="X-UA-Compatible" content="IE=edge" />` 用于指定 Internet Explorer 使用最新的渲染引擎

`http-equiv`属性主要用于html文档控制http响应头的行为。

## 18. HTML5 

- 语意化标签，header、footer、section、nav标签等
- localStorage、sessionStorage
- 表单的类型，`input type="email, date"`等
- 对图形图像的支持，Canvas、SVG，还加了video、audio音频的支持
- web worker，web socket的支持
- 拖放API、geolocation、history、postMessage等

## 19. CSS绘制三角形

- border：用border配置4个边的属性，设置其中一个的颜色，其他设置为transparent。
```CSS
//实现一个倒三角
.triangle {
  width: 0;
  height: 0;
  border-top: 100px solid red;
  border-left: 100px solid transparent;
  border-right: 100px solid transparent;
  border-bottom: 100px solid transparent
}
```
- clip-path：clip-path可以剪切各种自定义的图像，通过函数`clip-path: polygon()`设置多边形、通过`circle`设置圆形、`ellipse`设置椭圆， `inset`设置矩形
- 通过背景渐变`background: linear-gradient(45deg, deeppink, deeppink 50%, transparent 50%, transparent 100%)`
- transform: rotate 配合 overflow: hidden 绘制三角形



# 6、Vue 源码

## 1. 执行 new Vue 后发生了什么

> 执行 new Vue 后，会调用 Vue 这个构造函数并创建一个实例。在这个过程中，先将用户传入的属性和内部的默认属性进行合并。紧接着 Vue 执行了一系列的初始化工作，如初始化生命周期、事件系统和 render，调用**beforeCreate**钩子。接着，初始化 data、props、computer、watch 和 method 等属性，调用**created**钩子。如果传入有 el 属性则自动执行$mount 进行挂载操作，即编译模版并转成渲染函数，生成虚拟 dom，最后挂载到真实的 dom 上。如果没有 el 属性，就需要手动进行挂载。

## 2. Vue 实例挂载的整个过程

> 通过 new Vue 创建实例后, 若传入的 options 含有 el 属性，Vue 将自动开始挂载。此过程首先是模板编译：当存在 template 时，它被转化为渲染函数；否则，el 指定的外部 HTML 被用作模板。这个渲染函数中利用**createElement**生成虚拟 DOM，但是如果 option 中已经定义了 render 函数，那么就不会走这个编译模版的过程，而是直接调用**createElement**生成虚拟 DOM。接着，Vue 将虚拟 DOM 与真实 DOM 对比。首次挂载时，直接渲染；后续更新时，通过 diff 算法找出差异，并高效地更新真实 DOM。

## 3. Vue 组件创建的过程

>

# 7、浏览器

## 1. Chrome架构
现在的Chrome架构是多进程架构，分别由浏览器主进程、渲染进程、网络进程、GPU进程、以及可能存在的插件进程。采用多进程架构主要是解决浏览器单进程架构的不稳定、不流畅、不安全等问题。

多进程架构
- 浏览器主进程：负责浏览器进程间的IPC通信，管理浏览器的资源、子进程的交互管理、用户的交互等
- 渲染进程：在安全沙箱中执行，负责从网络下载下来的文件解析和执行，如HTML、JS、CSS等，默认情况下每个Tab对应一个渲染进程
- 网络进程：主要对网络资源的加载
- GPU进程：负责页面的绘制等
- 插件进程：浏览器的插件执行进程，由于插件经常会崩溃，因此可以把插件的执行作为一个单独的进程来管理，对浏览器的其他进程没有影响。

单进程的问题
- 不稳定：采用单进程架构，浏览器中的每个资源以线程为单位执行，那么当其中一个线程崩溃后，整个浏览器都会崩溃停止执行。
- 不流畅：当线程给一个资源执行时，如果这个线程太耗时就会阻塞其他模块的执行
- 不安全：浏览器从网络下载的资源在一个进程中执行，进程里的线程共享同一种资源，如果存在恶意代码，就会访问到页面的系统资源，而这些都是不允许的，会存在安全隐患。

多线程架构本质上解决了单线程的问题，提升了浏览器的稳定性、流畅性、安全性，但是多进程架构同时也会使浏览器的系统变得复杂许多，并且多进程还可能会造成过多的内存占用。

## 2. 从浏览器输入 URL 到页面呈现出内容，这个过程发生了什么

1. 用户输入URL，浏览器会根据用户输入的信息判断是搜索内容还是网址，如果是搜索内容，就将搜索内容和默认搜索引擎合成新的URL；如果用户输入的内容符合URL规则，浏览器就会根据URL协议，在这段内容上加上网络协议合成合法的URL
2. 用户输入完内容，按下回车键或者搜索键，浏览器导航栏显示loading状态，但是页面还是呈现前一个页面，这是因为新页面的响应数据还没有获得
3. 浏览器进程构建请求行信息和请求头的内容，会通过进程间通信（IPC）将URL请求发送给网络进程，如 GET /index.html HTTP1.1
4. 网络进程获取到URL，先去本地缓存中查找是否有缓存文件，如果有，拦截请求，直接返回200；否则，进入网络请求过程
5. 网络进程首先查找本地的DNS缓存服务，如果先前已经访问过并且缓存有这个DNS数据，就会直接返回缓存数据，否则，发起请求获取DNS信息，根据DNS信息将域名解析出IP和端口号，如果没有端口号，http默认80，https默认443。
6. Chrome 有个机制，同一个域名同时最多只能建立 6 个TCP 连接，如果在同一个域名下同时有 10 个请求发生，那么其中 4 个请求会进入排队等待状态，直到进行中的请求完成。如果当前请求数量少于6个，会直接建立TCP连接。
7. 在TCP三次握手建立连接后，如果是https请求，还需要建立TLS连接。http请求加上TCP头部字段如源端口号、目的端口号和用于校验数据完整性的序号等，向下传输到网络层
8. 网络层在数据包上加上IP头部字段如源IP地址和目的IP地址，继续向下传输到底层
9. 在最底层物理链路层通过物理网络传输给目的服务器主机
10. 目的服务器主机接收到数据包，解析出IP头部，识别出数据部分，将解开的数据包向上传输到传输层
11. 传输层获取到数据包，解析出TCP头部，识别端口，将解开的数据包进行验证或排序，得到完整数据后向上传输到应用层
12. 当应用层处理HTTP请求时，它首先解析请求头和请求体。如果需要进行重定向，服务器会返回HTTP状态码301或302。这时，响应头中会包含一个Location字段，指明重定向的URL。浏览器接收到这些信息后，会根据状态码和Location字段自动进行重定向，发起新的请求。如果不需要重定向，服务器会检查请求头中关于缓存的字段，例如`If-None-Match`。这个字段通常包含一个之前响应中的ETag值，服务器用它来判断请求的资源自上次请求以来是否有更新。如果资源未更新，服务器会返回304状态码，这表示之前的缓存仍然有效，服务器不会发送新的数据。如果还包含`if-modified-since`字段还会匹配这个字段，跟服务器的`last- modified`字段匹配
13. 如果资源有更新，或者请求头中没有相关的缓存信息，服务器会返回新的数据和200状态码。同时，如果服务器希望浏览器缓存这些新数据，它会在响应头中添加一个Cache-Control字段，例如Cache-Control: max-age=2000，这里的max-age=2000指示浏览器缓存数据的最大年龄为2000秒。然后响应数据在目的服务器中进行封装，逐级的向下传输回请求端的网络进程中。
14. 数据传输完成，TCP四次挥手断开连接。如果，浏览器或者服务器在HTTP头部加上`Connection:Keep-Alive`，TCP就一直保持连接。保持TCP连接可以省下下次需要建立连接的时间，提升资源加载速度，这个只在1.1以上版本支持
15. 网络进程将获取到的数据包进行解析，根据响应头中的`Content-type`来判断响应数据的类型，如果是字节流类型，就将该请求交给下载管理器，该导航流程结束，不再进行；如果是`text/html`类型，就通知浏览器进程获取到响应数据准备渲染
16. 浏览器进程收网络进程的通知后，根据当前请求页面是否是和当前tab页面是同一个站点（根域名和协议一样就被认为是同一个站点），如果是，就复用之前网页的渲染进程，否则，新创建一个单独的渲染进程
17. 浏览器主进程会发出“提交文档”的消息给对应的渲染进程，渲染进程收到消息后，会和网络进程建立传输数据的“管道”，文档数据从网络进程传输到渲染进程后，渲染进程会返回“确认提交”的消息给浏览器进程
18. 浏览器进程收到“确认提交”的消息后，会更新浏览器的页面状态，包括了安全状态、地址栏的 URL、前进后退的历史状态，并更新web页面，此时的web页面是空白页
19. 渲染进程对文档进行页面解析和子资源加载，通过HTML解析器将HTML转为浏览器可以理解的DOM树。与此同时并行的计算CSS样式，生成`styleSheets`，生成styleSheets后转化样式表里的属性值，使其标准化，如颜色属性同一转为rgb的值，不同格式的font-size转化成统一的单位，然后计算DOM树中每一个节点的样式，根据CSS继承和层叠的规则，算出DOM树中每一个节点的样式，生成ComputedStyle
20. 渲染进程生成了DOM树和ComputedStyle后，还不能直接渲染，因为还没有算出可见元素在屏幕的几何位置，因此进入布局阶段。在布局阶段，渲染进程基于DOM树和ComputedStyle生成布局树（Layout Tree）。布局树仅包含需要显示的节点，并计算每个可见元素的具体位置和尺寸，然后这些信息被保存回布局树。
21. 得到布局树后，渲染引擎还需要为特定的节点生成专用的图层（根据z-index或需要裁剪的元素会被分为一个单独的层，并不是布局树的每个节点都包含一个图层，如果一个节点没有对应的层，那么这个节点就从属于父节点的图层），并生成一棵对应的图层树，因为浏览器的页面实际上被分成了很多图层，这些图层叠加后合成了最终的页面。
22. 为每个图层生成绘制列表，并将其提交到合成线程。
23. 合成线程将图层分成图块，并在光栅化线程池中将图块转换成位图。因为如果在一个很长的页面中，将所有的图层都显示出来是很浪费性能并且是没有必要的，所以只需要将可视区域的内容显示出来即可，这就是分块的原因。
24. 合成线程发送绘制图块命令 DrawQuad 给浏览器进程。
25. 浏览器进程根据 DrawQuad 消息生成页面，并显示到屏幕上

## 3. 变量提升
由于JS代码的执行顺序是**先编译再执行**，变量提升是指在JavaScript代码的编译阶段中，JS引擎会把var定义的变量的声明部分和函数的声明部分提到代码的“开头”，存到变量环境中，而变量的值默认会被赋值为默认值`undefined`。因此我们可以在变量或函数未声明之前使用它，而不会报错。在代码执行阶段，JavaScript 引擎会从变量环境中去查找自定义的变量和函数。函数提升要比变量提升的优先级要高一些，且不会被变量声明覆盖，但是会被变量赋值之后覆盖。

正常访问函数和变量都会经过一个创建、初始化、赋值3个阶段
- var定义的变量，创建、初始化会被提升，所以在未定义的var变量之前访问其值是`undefined`
- let和const定义的变量，只有创建会被提升，因此在访问未定义的变量之前访问会报错，这就是暂时性死区
- 函数的创建、初始化、赋值都会被提升


## 4. 执行上下文
JS的执行上下文有三种：全局执行上下文、函数执行上下文、eval函数执行上下文

每一个执行上下文都包含了当前函数的变量环境和词法环境

- 在JS的开始执行阶段，会创建一个全局上下文，并将其压入栈低
- 每当执行过程中，遇到一个函数JS引擎都会给这个函数创建一个函数执行上下文，并将其压入栈顶。
- 如果在一个函数 A 中调用了另外一个函数 B，那么 JavaScript 引擎会为 B 函数创建执行上下文，并将 B 函数的执行上下文压入栈顶。
- 当一个函数执行完成后就会从栈中弹出，进行执行下一个上下文，直到全局上下文执行结束。

## 5. 调用栈溢出
调用栈溢出指在计算机中，函数调用的层级超过了指定的层级导致调用栈的容量不够然后溢出。在计算机中一个函数被调用就会被压到栈顶，当函数执行完成并返回时就从栈顶弹出。一般太深层的函数嵌套和无限的递归会导致栈溢出。

## 6. 作用域
作用域是指变量和函数的可见范围，即作用域控制着变量和函数的可见性和生命周期。在ES6之前JS只有全局作用域、函数作用域，ES6之后为了解决使用var来定义变量存在的变量提升的问题，引入let、const关键字来实现块级作用域。

- 在函数执行上下文中，包含了变量环境和词法环境，其中var和函数的声明在编译阶段会存放到变量环境中，而使用let和const定义的变量被保存到词法环境中。
- 在词法环境内部，维护了一个小型栈结构，栈底是函数最外层的变量，进入一个块级作用域块后，就会把该作用域块内部的变量压到栈顶；当作用域执行完成之后，该作用域的信息就会从栈顶弹出。
- 当访问一个变量时，会先从词法环境的栈顶开始查找，一直查到变量环境。

## 7. 作用域链
作用域链是指在访问一个变量时，查找这个变量的顺序。当前作用域下没有这个变量时，会向上层作用域查找直到全局作用域。而作用域链是由词法作用域来决定的，词法作用域是代码编译阶段就决定好的，和函数是怎么调用的没有关系。

## 8. 栈空间、堆空间、代码空间

- 栈空间是一种先进后出的数据结构，用于存储局部变量、函数调用的返回地址等，提供快速的访问能力，并且会自动分配地址和回收地址
- 堆空间是指用于动态内存分配的内存区域，用于存储应用类型，通常访问比栈慢，需要手动分配和回收内存，如果使用不当会造成堆内存泄漏。对象、数组、函数实际上是存储在堆中，然后返回堆的指针并将指针存在栈中。
- 代码段指的是 js 的代码，以二进制的形式存储在计算机中。.js 文件的代码都是

## 9. 垃圾回收机制
Chrome的V8引擎采用了分代回收的机制，分为新生代和老生代。

- 新生代：存储生命周期较短的对象。这个区域相对较小，但是回收频率高。新生代内存使用了Scavenge算法，采用复制的方式进行垃圾回收。当对象在新生代中存活足够长的时间后，它们会被移到老生代中。
- 老生代：存储生命周期较长或内存很大的对象。老生代的大小远大于新生代，但垃圾回收频率较低。老生代使用Mark-Sweep（标记-清除）和Mark-Compact（标记-整理）算法进行垃圾回收。
  
- Scavenge算法将新生代区域分为两部分为对象区域和空闲区域，当对象区域存储满时，开始执行回收操作。对存活的对象进行标记并且把对象复制到空闲区域中，然后清空对象区域，并执行反转操作，即将原本的空闲区域变为对象区域，原对象区域清空后变成空闲区域
- Mark-Sweep：从根内存开始变量，标记可以访问的对象，然后清理未被访问到的对象，会使内存不连续，产生内存碎片
- Mark-Compact：解决标记-清除算法存在的内存碎片问题，当执行完清理操作时，会将存活内存向一端移动，形成连续的内存。
- 在垃圾清理的过程中，如果老生代内存过大，执行清理操作可能会占据很长时间，导致js执行陷入全停顿状态，因此V8引擎又采用了增量标记算法，将垃圾回收的过程分成多个小片段穿插在js的执行过程中，避免垃圾回收长时间占用浏览器主线程


## 10. JavaScript 执行过程

当浏览器解析 HTML 文档并遇到 script 标签时，它将下载相应的 JS 源代码。一旦 JavaScript 引擎获得源代码，它首先进行**词法分析**，这一步将源代码分解为多个标记，如“const”、“square”、“=”、“(n)”、“=>”等。紧接着，**语法分析**阶段开始，将这些标记转换为一个**抽象语法树** (AST)，这是一个计算机可以理解的代码结构表示。在代码真正执行前，引擎进行了一个关键的**预编译**步骤。在这一阶段，它识别所有的变量和函数声明，并为它们在内存中分配空间。这也是变量提升和函数提升现象产生的地方。随后，为了提高效率，现代 JS 引擎使用**即时编译** (JIT) 技术。这意味着代码被边解释边编译，AST 首先被转换为字节码，然后进一步转换为机器码。最终，这些机器码在 JavaScript 引擎中执行，激活所有的函数调用、赋值和其他程序操作。这个流程确保了 JavaScript 代码的高效、流畅的执行

## 11. 消息队列和事件循环
在JavaScript中，事件循环是一个核心机制，用于在单线程环境下处理异步事件。它依赖于调用栈来执行同步任务和消息队列来管理异步任务。消息队列中的任务被分为宏任务和微任务。宏任务包括诸如setTimeout、setInterval、I/O等，而微任务则包括Promise和MutationObserver的回调。

当执行环境执行同步代码时，这些代码在调用栈中运行。一旦调用栈为空，事件循环会检查消息队列。如果存在宏任务，它会执行一个宏任务，在执行宏任务的过程中，如果遇到微任务，会把微任务添加到微任务队列，在执行完当前宏任务之后并不会马上执行下一个宏任务，而是执行微任务队列的任务。完成这些后，浏览器可能会进行UI渲染。然后事件循环将移至下一个宏任务，并重复此过程。

## 12. 宏任务和微任务
消息队列存放宏任务，微任务有单独的微任务队列，宏任务在执行的过程中发现微任务就会把微任务存到微任务队列中，宏任务执行完毕后就会执行微任务队列的任务。



## 6. 浏览器的分层和合成机制

> 在浏览器渲染过程中，它采用了**分层**策略，将不同种类的元素分别渲染到独立的图层。这样做的好处在于，当某一层的元素发生变化时，只需重绘该特定层而无需重绘整个页面，从而提高渲染效率。而**合成**则是指浏览器在最终呈现页面时，会将这些独立的图层按照特定的顺序和规则组合成一个完整的场景。但关键在于，当某一层的内容有所变动，浏览器只需重新渲染该层并将其与其他层进行合成，而不是重新渲染整体，进一步优化了性能。

## 7. 跨标签页通信

> - localStorage/sessionStorage：在将数据存储到 localStorage/sessionStorage 中，在共享的页面监听 message 的变化，或者取出存储的数据。
> - BroadcastChannel()：一个构造函数，允许我们创建一个实例，在这个实例中 postMessage 一个数据，然后在另一个页面中获取。
> - ShardWord：Web Workers 中的一种，可以被不同的页面发送或共享数据
> - Cookie：存储在请求头中，通过服务器进行通信。
> - Window.postMessage：从一个窗口发送消息到另一个窗口，无论这个窗口是否同源，可以用于跨域通信。

## 8. 常见的内存泄漏有哪些

> - 未清除的定时器
> - 事件监听函数结束后没有清除
> - 定义太多全局变量，而且占的内存都巨大
> - 闭包引用了大量的数据，而且在使用后没有手动消除

## 9. 前端常见的攻击手段

- 跨站脚本攻击(xss)：攻击者向目标网站注入一段可执行的恶意代码，使代码可以在目标网站上执行，从而达到一定的目的。常见的攻击类型有存储型 XSS、反射性 XSS、DOM 型 XSS。
  - 存储型 XSS 就是攻击者向目标服务器提交一份恶意的代码，存入到目标服务器的数据库中。当用户请求目标服务器的相关数据时，就会返回到用户的浏览器中执行，从而达到攻击目的。存储型 XSS 危害性更广，因为攻击者提交恶意代码到服务器，如何有关目标服务器的用户都有可能受害。
  - 反射性 XSS 相对与存储型的危害就没有那么广，反射型是指攻击者构建一个包含恶意代码的 URL，诱导用户点击这个 URL，然后恶意代码就会以参数的形式反射回用户的页面然后执行。如 url?xss=<script>xss</script>，在页面中就是<div>{{xss}}</div>。
  - dom 型 XSS 是不牵涉到页面 Web 服务器的。具体来说就是黑客可以劫持用户的html页面，然后修改页面的数据。

  > **针对 XSS 的防范有**：对用户的输入和输出进行严格过滤或转译，不允许输入和输出原样的返回到客户端中。使用 CSP 机制限制浏览器的不同源的代码执行，在响应头设置`Content-Security-Polity`的值来定义加载的源。一般攻击者都是为了拿到用户的 cookie 指，在 cookie 字段设置为`httpOnly`，限制浏览器通过 js 访问 cookie 的值。同时避免用户的输入直接显示到页面的 dom 中，如使用`v-html`或`innerHtml`。

  CSP是通过响应头来实现，主要的功能包括限制加载其他域下的资源文件、禁止向第三方域提交数据、防止内联脚本的执行、提供一个报告机制

- 跨站请求伪造(CSRF)：在目标用户已经登录了站点的情况下，诱导用户点击一个恶意连接，然后黑客伪装成用户向目标服务区发起请求，达到攻击目的。

  > **防范的措施**可以在请求的过程中生成一个 Token，在发送请求时，服务端验证这个 Token 是可信的，再对请求进行处理，否者拒绝处理。又或者设置 cookie 的`SameSide`属性，限制跨站请求时，不能使用 cookie。服务器通过origin请求头验证请求的真实性，拒绝不明来源的请求

- 点击劫持：点击劫持使用的是视觉欺骗，诱导用户在不知情的情况下点击不安全的按钮，以达到特定的目的，即将危险的点击操作伪装成安全的，用户以为是安全的其实是不安全的。
- 中间人攻击：发生在网络传输的过程，攻击者在两个通信方中间进行拦截、截取或更换他们的通信信息。常见的攻击类型有 ARP 欺骗、DNS 劫持、SSL/TLS 劫持、Wi-Fi 欺骗等。

- DNS 劫持：在用户向指定 URL 发送请求时，在这个过程中会查询 URL 对应的 IP 地址。此时攻击者就会串改或劫持用户的请求，将请求的目标网站 IP 地址重定向到攻击者给定的 IP 地址，然后与用户进行通信。
- 开放重定向：web 上的安全漏洞，允许用户从一个受信任的链接重定向到外部的链接。这种攻击的存在通常是由于网站没有正确验证重定向参数的结果，使得攻击者可以自由指定重定向目标

## 10. 安全沙箱
安全沙箱指的是将不受信任的代码与系统代码隔离开来，让系统的代码在安全沙箱中执行不会收到其他代码的影响，安全沙箱主要有以下特点：
- 隔离环境，将系统代码和外部下载的代码隔离开，只能访问沙箱允许访问的资源
- 限制网络的访问和操作数据，在安全沙箱中的代码执行不能随意的访问网络和系统文件数据
- 跨域限制，在沙箱中收同源策略的影响会存在跨域，除非设置了CORS
  
在浏览器中，渲染进程放在了安全沙箱中执行，因此，即便是下载的资源存在安全隐患，通过安全沙箱也会将渲染进程和系统的资源隔离出来不受影响。


# Webpack

## 1. webpack 概述以及工作原理

**概述**：Webpack 是一个现代 JavaScript 应用的模块打包器，它将各种资源视为模块。Webpack 通过分析一个或多个指定的入口文件，递归地构建出项目的依赖图，然后将这些依赖合并成一个或多个 bundle 文件，以便于在浏览器中执行。

**工作原理**

- Entry 入口文件：Webpack 首先读取`webpack.config.js`中的配置，特别是 entry 属性来确定一个或多个入口文件。Webpack 从这些入口文件开始解析应用程序。
- 生成依赖图：Webpack 递归地处理入口文件及其依赖的模块或库。这些依赖通过相应的 loader 进行转换，最终形成一个完整的依赖关系图。
- 生成 Chunk：基于依赖图，Webpack 生成 chunk。每个 chunk 通常对应一个输出文件。
- Output 输出生成 Bundle：Webpack 根据配置将 chunk 合并成一个或多个 bundle 文件。这些文件是最终要在浏览器中加载的静态资源。
- Plugin 的工作时间点：Plugins 在 Webpack 的整个编译过程中介入，可执行一系列复杂操作，包括但不限于优化、资源管理和环境变量的注入。

## 2. 资源管理

Webpack 将所有类型的资源视为模块。它天然处理 JavaScript，但对于其他类型的文件，如 CSS、图片或 HTML，Webpack 使用各种 loader 将它们转换成有效的模块。这些转换后的模块随后被添加到依赖图中。此外，plugins 提供了进一步处理这些资源的能力，如优化、压缩等。

## 3. 输出管理

经过一系列的处理后，Webpack 根据`webpack.config.js`中定义的 output 选项将资源打包成一个或多个 bundle。这些 bundle 文件是最终要在浏览器中加载的静态资源，它们包含了所有必要的代码和资源。

## 4. loaders

loader是webpack的核心功能之一，loader作为资源的加载器。loader不仅限于文件转换，还可以包括代码检查、格式化等功能。webpack本质上只能处理js文件，但是通过loaders，webpack可以处理各种类型的资源，比如css、img等资源，类似于翻译官一样，loader把不同的资源转换成webpack可以识别的资源，便于webpack处理。loader是逆向执行的管道操作，在use属性中，可以作为一个数组定义，loader从最后一项加载，最后一项的处理输出作为前一项的处理输入。如`use: ['style-loader', 'css-loader', 'sass-lader']`，webpack首先处理sass-lader，将sass转换为css，然后css-loader再根据sass-loader的输出结果转换为js文件，以此类推，直到处理成最种webpack可以识别的格式。

常见的loader有

- style-loader：将css添加到dom中
- css-loader：处理css，将css转换成js模块
- vue-style-loader：处理vue的样式，将vue的css添加到dom中
- sass-loader：处理sass文件的代码，将sass转换成css
- babel-loader：babel转换器，处理将es6+的语法转换成es5语法，兼容旧的浏览器。
- cache-loader：将loader的结果存入缓存中，一般用于很耗性能的loader

## 5. plugins

plugins是webpack的核心功能之一，通过plugin我们可以在合适的时机对webpack的编译过程介入，以完成更广泛的目的。

常见的插件有

- HtmlWebpackPlugin：主动生成html入口文件，文件包含打包资源的内容。
- MissCssExtractPlugin：用于分离css样式为一个独立的文件。
- HotModuleReplacementPlugin：热更新插件
- VueLoaderPlugin：加载处理vue的插件
- DefinePlugin：定义全局变量的插件
- ESlintPlugin：用于集成ESlint

**plugin的工作机制**

plugin在webpack构建的整个过程中都会执行，每个时间点都对应着不同的hook，用来控制构建的过程。首先webpack在开始构建时，会访问plugins属性，plugin是一个class类，通过 new 调用plugin，会调用plugin实例的apply方法，webpack会给apply传入一个compiler属性，compiler在整个webpack构建过程中只生成一次，通过在apply内部调用compiler属性的hooks，可以对应webpack不同的构建阶段。其中webpack通过Tapable机制控制整个过程，Tapable将webpack的所有插件串联起来，plugin通过加入Tapable机制可以改变webpack的运作。Tapable是一个工具库，提供tap、asyncTap、tapPromise方法，每一个hooks都包含着这3个方法，可以控制hook的同步或异步执行。其中compiler又包含了compilation hook，在编译阶段运行，与compiler不同，compilation在每次构建的过程中都会创建一个新的。compilation 又存在不同的hooks。整个构建的过程都围绕了complier和compilation展开。

Webpack插件的执行机制是基于事件驱动的，这一机制主要通过Webpack内部的一个名为Tapable的库实现。在这个框架中，插件可以监听一系列的事件钩子（Hooks），这些钩子分布在Webpack的构建流程的不同阶段。

首先，Webpack的插件是一个类，这个类必须实现一个名为apply的方法。当Webpack启动时，它会遍历配置中的所有插件，对每个插件实例调用其apply方法。在这个方法内部，插件通过提供给apply方法的compiler对象注册它需要监听的钩子。

这里的compiler对象代表了Webpack的整个构建环境，负责整个构建流程。它包含了Webpack的所有配置信息，以及一系列可以被插件利用的生命周期钩子。插件通过这些钩子注册回调函数，以便在特定时刻执行特定任务。

  ```javascript
  class MyPlugin {
    constructor(options) {
      this.options = options
    }
    apply(compiler) {
      compiler.hooks.initialize.tap('MyPlugin', ()=> {
        console.log('在webpack初始化阶段执行。。。')
      }),
      compiler.hooks.beforeCompile.tapAsync('MyPlugin', (params, callBack) => {
        console.log('在webpack开始编译阶段执行。。。')
        callBack()
      }),
      compiler.hooks.compilation.tap('MyPlugin', (compilation) => {
        console.log('在compilation已经创建了阶段执行。。。')
        compilation.hooks.builtModule.tap('MyPlugin', (params) => {
          console.log('在compilation开始编译阶段执行。。。')
        })
      }),
      compiler.hooks.emit.tapAsync('MyPlugin', (compilation, callBack)=> {
        console.log('在资源已经编译完成，并且在输入output之前阶段执行。。。')
        compilation.assets[this.options.fileName] = {
          source: () => {
            return '这里可以将数据写入到打包目录下的this.options.fileName文件'
          }
        }
      }),
      compiler.hooks.done.tap('MyPlugin', (stats)=> {
        console.log('在webpack编译完全部资源后触发。。。')
      })
    }
  }
  ```

## 6. code splitting

代码切分是 webpack 的优化技术，通过将一个大的 bundle 文件切成小的 chunk，这些 chunk 根据实际需求动态的价值，减少首次加载的时间。webpack 有 3 种方法实现 code splitting

- entry 入口文件：通过配置 entry 属性为多入口打包，可以手动将每个入口切分成不同的 bundle 文件，减少总 bundle 的大小

  ```javascript
  entry: {
    index: '.src/index.js',
    main: './src/main.js'
  }
  ```

- 动态加载：使用 import 实现动态加载，webpack 将每个使用 import 的模块切成独立的 chunk。

  ```javascript
  import("./src/index.js").then((indexModule) => {
    //使用模块的逻辑
  });
  ```

- SplitChunksPlugin：通过 webpack 自带的插件，将重复的依赖提取到一个共享 chunk 中，避免依赖在多个模块使用时，每个模块都进行打包。

  ```javascript
  optimization: {
    splitChunks: {
      chunk: "all";
    }
  }
  ```

  **将代码进行切分的好处是，可以减少首次加载时间，按需的加载代码。**

## 7. Module 和 Chunk

module 和 chunk 都是 webpack 的核心概念，在 webpack 中，把一切资源都认为是模块，如 js 文件、css、img 等，这些模块是 webpack 的基本构成单元。而 chunk 则是 webpack 构建过程中的输出单元，由不同的模块组成的，使用代码拆分可以将一个 bundle 拆成多个 chunk，同时又可以用多个 chunk 组合成一个 bundle 输出。总的来说，模块是 Webpack 的基本工作单元，负责具体资源的处理，而 chunk 则是模块在编译过程中的组合体，最终决定了输出文件的结构和性能优化。

## 8. 文件指纹

用于版本控制或优化缓存，会在文件名中注入一个 hash 值，每当内容发生改变就会生成一个新的 hash 值，有助于浏览器识别。

- 项目指纹 Hash：`[name].[hash].js`当项目的内容发生变化，hash 值就会变化
- Chunkhash：`[name].[chunckhash].js`chunk 发生改变，chunkhash 就会变化
- Contenthash：`[name].[contenthash].js`文件内容发生改变，contenthash 就会变化

## 9. 打包 Library

  指的是可以将webpack打包成类似于npm包的形式，供其他地方使用，可以将webpack的library打包发布到npm中。

  实现一个库可以在output中配置

  ```javascript
  output: {
    library: {
      name: 'myLibrary',
    }
  }
  ```

  同时，如果在库的实现过程中，引入了其他的库，默认情况下打包会把它一起打包，但是可以使用externals排除掉指定的库，从而不打包它。

  ```javascript
  module.exports = {
    externals: {
      lodash: 'lodash', //排除打包lodash依赖
    }
  }
  ```

## 10. HRM

  热更新是webpack核心功能之一，在4.0以上的版本已经默认开启，用于在开发过程中，当代码被修改后可以自动的更新修改的页面，而不需要手动刷新浏览器，极大的提高开发效率，还可以保留当前页面的状态。
  
**实现原理**：当我们启动Webpack的开发服务器，它会起一个本地服务，这个服务会实时监控我们的代码文件。一旦发现文件变动，它只会重新编译那些发生变化的部分，而不是整个项目。编译完毕后，Webpack会生成一个包含了更新信息的manifest文件。接着，通过一个WebSocket连接，Webpack把这个manifest发送到我们的浏览器端。浏览器拿到这些信息后，就会去请求那些更新过的模块代码。然后，系统会自动把旧模块替换成新的。如果我们需要，也可以用accept方法来手动管理这些更新过程中的状态转移。万一更新过程出了问题，Webpack会尝试一些错误处理手段，比如刷新整个页面。这样一来，我们就能在开发过程中即时看到变化，又不会丢失当前的应用状态。

## 11. Tree Shaking

  webpack里的一个重要概念，用于移除在项目中未被引用的代码，仅支持es模块，因为es模块（import和export）是静态的，而commonJS则是动态的。当移除的文件代码有副作用时，可以在package.json中设置sideEffects属性，将包含有副作用的代码跳过，从而webpack不会去检查它。

## 12. Shimming

   Shimming是一个高级功能，可以为webpack项目定义一些全局变量，以解决在模块化构建过程中存在的兼容性问题，Shimming可以在不修改源代码的情况下为模块提供一个全局变量。比如我们需要在A、B、C三个文件访问同一个变量，默认情况下由于模块化的划分，是没有的，但是我们可以通过Shimming定义一个变量，供这3个文件都可以访问到。

- ProvidePlugin：使用插件为全局定义一个变量，比如为全局引入一个jQuery变量。

   ```javascript
   plugins: [
    new webpack.providePlugin({
      $: 'jquery', //引入了jquery，并且在每一个模块中都可以用 $ 访问到。
    })
   ]
   ```

- import-loader：提供更细粒度的Shimming，比如在指定文件下访问window

   ```javascript
    rules: [{
      test: require.resolve('./src/index.js'),
      use: 'import-loader?this=>window' ///为index.js提供一个访问this的变量
    }]
   ```

- export-loader：在文件中，没有使用export将模块导出，那么可以使用这个加载器将模块导出。

  ```javascript
    // 在index中定义两个变量，但是没有用模块化导出
    var a = 'a'
    var person = {
      son: 'b'
    }

    //使用加载器导出
    rules: [{
      test: require.resolve('./src/index.js'),
      use: 'export-loader?a, son=person' ///为index.js提供一个访问this的变量
    }]

    //在其他文件可以import导入index.js
    import {a, son} from './src/index.js'

  ```

## 13. Source-Map

Source Map是一个保存着映射信息的文件，它保存着源代码和被打包编译之后代码的对应关系。因此即使源代码已经被压缩和转换，通过source map仍能准确的找到源代码的位置，方便我们调试。

在webpack的配置中配置devtool属性，开启source-map功能，根据配置的属性，会生成一个或多个.map文件，这些文件保存着源代码的行列信息。当在浏览器打开源代码调试功能时，浏览器就会调用.map文件的信息找出源代码的映射，从而显示源代码，而不显示压缩的代码。

## 14.  MF

MF指的是模块联邦，是webpack5引入的新功能，运行不同的js应用或组件共享代码，跟微前端的概念类似。使用mf可以在应用中加载其他的应用的代码，即便它们是部署在不同的项目中。允许应用在运行时共享代码，而不是在构建时。这意味着应用可以共享实时的、动态的代码。

ModuleFederationPlugin插件实现共享功能

  ```javascript
  //在主应用中使用remotes加载远程暴露出来的组件
  plugins: [
    new ModuleFederationPlugin({
      remotes: {
        app: 'app@http://localhost:8080/main.js'
      }
    })
  ]

  //在远处应用使用exposes暴露组件出去
  plugins: [
    new ModuleFederationPlugin({
      name: 'app',
      fileName: 'main.js',
      exposes: {
        './MyCommon': './src/common.js'
      }
    })
  ]

  //在主应用使用组件
  import('./app/MyCommon')
  ```

## 15. runtime 和 manifest
  
runtime可以理解为是一块代码，由webpack在编译的过程中生成，里面包含了程序在运行过程中执行的代码。主要负责程序里所有模块的连接、执行和交互，runtime知道在程序的执行过程中如何去加载模块，如何让模块间有序的工作。

manifest可以理解为是一个保存着各种模块的映射表，通过这张表即使在再复杂的程序中都能准确无误的找到目标模块。

在webpack打包出来的程序执行时，runtime也会跟着执行，当在执行过程中程序需要加载一个依赖的模块时，runtime就会通过manifest去找到这个模块，确定目标模块的位置，然后按需加载。

## 16.  webpack 优化手段

- exclude、include：在Babel中，并非所有的js文件都需要转换。使用exclude把不需要转换的排除文件掉，使用include把需要转换的包含进来，推荐使用include。

- cache-loader：将耗性能的loader结果缓存起来，下次构建时可以读取缓存而不需要重新构建它

```javascript
use: ['cache-loader', 'Babel-loader'], //放在需要缓存的loader前面
```

- thread-loader：开启多个进程池，将转换的插件用在独立的进程中继续，使用方法跟cache-loader一样，放在需要开辟新进程loader的前面。

- TerserWebpackPlugin开启多进程构建和缓存

- HardSourceWebpackPlugin：提升二次构建的时间，第一次构建时会将模块的依赖关系存到缓存中，当下次构建时，只要源文件不发生变化，就会从缓存中使用这些依赖。

- resolve：正确的使用module.resolve属性，可以极大的提高编程时导入模块的效率

```javascript
resolve: {
  alias: {
    @util: path.resolve(__dirname, 'src/util') // 定义路径的别名
  },
  extension: ['.js', 'vue'], //匹配导入的文件名，导入时可以不指定文件名，从左到右匹配，因此最常用的应该放到最前面，减少尝试次数。
  mainFiles: ['index'], //当导入一个目录时，可以不指定文件，默认导入目录的index文件
}
```

- optimization：配置optimization可以优化打包处理的结果。配置`optimization.splitChunks`抽离依赖中的公共代码为一个chunk，减少打包的体积。配置`optimization.runtimeChunk`可以将运行时的manifest抽离成一个或多个文件。

- tree-shaking

- Babel7优化

- 使用webpack-merge将`webpack.config.js`文件进行环境区分

## 17. 优化HMR的更新速度
在项目的多次迭代过程中，项目会变得非常庞大，导致了webpack的启动时间和热更新时间变得非常的缓慢，所以我做了一些优化来加快hrm的更新时间

- 使用代码切分，将整个项目切分成很多的chunk文件输出，在热更新时只需要更新相应的chunk，这样有效的减少hrm更新时间
- 优化loaders的配置，有些loader在处理大量的文件时会变得非常慢，这样就会拖累到热更新的时间，比如说像Babel、Sass这种，配置include或者exclude属性，然后对babel启动缓存
- 高效的配置source map，因为生成复杂的source map会降低hrm的更新时间
- 优化webpack的路径解析，因为过多的路径或者是别名会增加解析的负担，还会拖累hrm的时间
- 使用Dll插件将一些第三方库打包成单独的chunk，这样主应用构建时就不会重复的构建这些库。
  配置 Dll插件要单独建立一个dll文件，然后通过webpack构建出来，再在项目的配置文件中加入这个构建出来的文件。

## NPM

### 1. 执行 npm install 背后发生了什么

> npm install 本质是执行为项目安装依赖的过程。首先，npm 会检索项目下的 package.json 文件下的依赖，如果是首次安装那么会直接访问 npm 仓库，非首次安装就会查看 node_modules 目录，了解哪些需要安装或者哪些包需要更新，然后在查询 npm 仓库；由于每个包可能依赖其他的包，那么 npm 就会创建一个依赖树，确保有些包不会重复安装；之后 npm 开始下载所需要的包，再解压到 node_modules 目录下。npm 会生成或更新 package-lock.json 文件，这个文件包含的是下载的包的精确版本信息，确保了其他人下载的跟你下载的一样。此时，npm install 已经执行完成，后台会输出这些安装过程的日记。但是如果 node_modules 存在了但 package 不存在的包，那么 npm 会把这些包删除掉。

# Vite

## 1. 概述

Vite是一个更现代的前端构建工具，相比于其他的js构建工具，vite具有更快的启动、更快的HRM更新。

## 2. 工作原理

Vite的工作原理在生产和开发模式有所不同

- 在开发模式中，vite会创建一个本地的服务器，在这个服务器中，并不会像其他构造工具一样会对工程进行一次编译，而是利用了浏览器对原生ES模块的支持，当应用请求一个模块时，请求会直接去Vite服务器获取。在获取的过程中，vite会实时的对模块进行转译，vite仅处理当前请求的模块，也就是说，除了当前请求的模块以外，其他的模块还是源文件，并不会对这些模块进行构建，因此大大的提高了应用的启动速度。vite还支持快速的HRM，当修改的文件发生变化时，vite只更新当前模块，而不是整个页面，因此可以做到秒响应修改。vite使用ESBuild进行预构建，也就是说对于第三方的库，vite首次启动时会对它进行预构建，下次使用时则速度更快。

- 在生产模式下，因为 Rollup 在优化和代码分割方面提供了更成熟的功能，适合生产环境，vite使用Rollup进行打包。Rollup打包过程包括代码分割、动态导入以及针对应用的特定优化如tree-shaking、压缩等。此外，vite还支持懒加载、预渲染和服务端渲染等高级应用。

## 3. vite打包过程

在生产环境下，Vite 仅在执行 vite build 命令时进行打包。此时，Vite 会读取 vite.config.js 文件中的配置，并以配置的入口文件为起点开始打包流程。作为核心打包工具，Vite 使用 Rollup 来执行各种操作，包括代码转换和资源引入等。得益于 Vite 在首次启动时对 node_modules 下的第三方模块进行的预构建，打包过程中无需重新构建这些依赖，从而节省了大量时间。在处理应用代码的同时，Vite 也会优化静态资源，并最终生成用于部署的静态文件。

## 4. vite插件机制

vite插件采用Rollup插件机制，并在其基础上做了一些扩展。

```javascript
//一个简单的vite插件
export default function myPlugin(options = {}) {
  return {
    name: 'my-plugin',
    startBuild() {
      console.log('开始构建前执行')
    },
    transform(code, id) {
      console.log('在处理和转换代码模块阶段执行')
    }
  }
}
```

## 5. 预构建

vite在开发模式下，并不需要对应用进行全体的编译，而是使用原生ESM根据需要按需加载的模块，从而大大的提高了工程启动的时间。但是由于第三方库并不是完全兼容原生ESM，因此需要预先对第三方库进行预构建。

- 为什么需要预构建？

  > 许多npm包并不是使用ESM模块，而是使用CommonJS或者AMD模块，浏览器并不能识别它们，预构建把这些非ESM模块转成ESM模块，供浏览器直接使用。

  > 在npm包中，有些库会有很多模块，当使用import导入时，就会发送一个http请求。当模块众多时，会发送多个http请求，因此vite会将这些模块编译成一个模块，发送一个请求即可。

  > 当npm存在node代码时，vite会编译转换可以在浏览器执行的代码。

- 工作原理

  > vite在启动时会检测`import`语句，确定哪些第三方库需要预构建，vite采用esbuild进行预构建，将非ESM格式的代码转为ESM格式代码，优化依赖加载，将依赖处理成少量的捆绑包，减少http的请求，并且把结果进行缓存，下次启动时，如依赖没有修改则可以直接取缓存的结果，加快启动速度。

- 预构建的优点

  > 对依赖进行兼容，优化开发服务器的性能，减少http的请求，更快的热更新速度。同时可以提高optimizeDeps.include 或 optimizeDeps.exclude来自定义转换的行为。

## 6. vite-plugin-vue

`vite-plugin-vue` 是 Vite 的官方插件，专门用于处理 Vue 单文件组件（.vue 文件）。这个插件扩展了 Vite 的能力，使其能够理解和编译 Vue 的单文件组件。

## 7. vite生产模式下打包的目的

工程打包的主要目的是为了压缩代码、树摇、代码拆分、缓存优化、最小化第三方库的体积

- 压缩代码：移除不必要的空格、注释，缩短变量名等，以减小文件体积。
- 树摇（Tree Shaking）：移除未使用的代码，减少最终包的体积。
- 代码拆分（Code Splitting）：将代码分割成不同的块，优化加载速度。
- 缓存优化：通过添加哈希值到文件名，优化浏览器缓存。
- 资源优化：优化图片和其他静态资源的大小和加载方式。
- 最小化第三方依赖体积：确保第三方库以最优方式包含。

## 总结：Webpack和Vite的区别，各自的优缺点

Webpack和Vite都是现代前端工程中广泛使用的构建工具，但是它们在构建方式和优化性能方面都有一些不同。

- 在构建方面

  > Webpack是一个模块打包工具，它通过一个或多个入口文件，通过对于loader转换的结果递归的构建一个依赖图，然后将这些依赖打包成少数几个bundle文件，在开发模式和生产模式下都会对工程进行构建和打包的过程。

  > Vite是一个更现代的前端构建工具，Vite 在开发模式下采用原生 ES 模块导入，从而实现按需加载，避免了整个应用的构建。

- 各自优点

  > Webpack是一个成熟稳定的项目，社区庞大并提供了丰富的loaders和plugins，并且可以通过webpack的配置文件进行更贴合实际工程的配置，以满足不同场景的工程，灵活性更高。

  > Vite由于是按需加载，开发模式下启动更快，并且热更新也比Webpack快很多。vite的配置通常更简单，提供了很多开箱即用的功能，对于新人更加的友好。

- 各自的缺点

  > Webpack每次启动都会对整个工程进行构建，对于大型的应用或者随着工程的逐渐变大，开发模式下构建的速度可能会变得非常慢，热更新的速度也会有所降低。webpack复杂的配置对于新人可能不太友好。

  > Vite虽然在快速的发展，但是社区相比于webpack来说还不是那么的丰富和成熟，在对于更老的或非标准的npm包的兼容可能会存在一些问题

# Babel

## 1. 工作原理

Babel通过解析、转换、生成这三个步骤将现代的JS代码转为向后兼容的代码。这个过程通过词法分析和语法分析产生 AST，然后通过插件修改这个 AST，最后将 AST 转换回 JavaScript 代码。

**解析**：Babel将源代码字符串进行**词法分析**，将代码分解成一个个Tokens，这些Tokens是代码的最小单位，然后通过**语法分析**根据JS的规则将Tokens转换成一个AST树。

**转换**：接着就是最关键的步骤，Babel会将这棵AST通过Babel的插件进行各种自定义的操作，这些插件可以对AST的各个节点进行删除、修改、替换等操作，最后将生成一棵新的AST树。

**生成**：将新的AST再转换成原始的代码结构，功能没有修改，但是语法已经进行了修改。

## 2. 基础配置

Babel的基础配置通常包括preset和plugins这两个配置。

- preset是一个高效配置Babel插件的方法，可以理解为它是一组插件的集合，Babel的一个插件处理一个功能，而preset就是一组插件的集合。有了preset可以大大简化Babel的配置，从而不需要一个一个的引入插件。常用的插件有preset-env、vue/cli-plugin-babel/preset。

- plugin是Babel实现代码转换的具体结构，一个插件对应一个转换功能，比如`babel/plugin-transform-arrow-functions`用于转换箭头函数。

# TCP

## 1. TCP 概述

TCP 是传输层的关建协议，它是可靠的、面向连接的、基于字节流的全双工协议。首先，它可靠：确保数据按顺序到达，并在丢失时重新发送。其次，它是面向连接的，在发送数据之前必须通过三次握手来建立通讯。另外，TCP 是基于字节流的，这意味着它可以灵活地传输任何大小的数据。而且不仅客户端可以启动连接，服务器也能做同样的事，是一个全双工的协议。

## 2. 首部字段

tcp 首部字段包含了多种信息，这些信息用于指导 TCP 数据段的发送和接收，确保数据传输的可靠性。

- 源端口号、目标端口号
- 序列号(SYN)：解决数据包的乱序、重复问题，根据序列号组装出正确的数据再传递给上层应用。确保数据的可靠、有效的传输，并使得 TCP 可以处理掉包、重传和乱序数据包的问题。
- 确认号(ACK)：确认传递的数据包已经到达，表示比 ACK 小的数据序列号已经全部接收完毕。
- 数据偏移(Data Offset)
- 保留(Reversed)：为未来的添加或修改保留位置
- 控制位(Flags)：常用的有 RST、SYN、PSH、FIN
- 窗口大小(Window Size)：表示当前能够接收的数据量
- 校验和(Checksum)：用于检查 TCP 首部和数据的正确性和完整性，当收到校验有差错的报文，不会进行确认，而是直接丢弃它
- 紧急指针(Urgent Pointer)
- 选项和填充(Options and Padding)：可选的，用于控制参数，如 MSS、SAC、时间戳等

## 3. 分层

一般来说，分层有两种分层，一种为 OSI 七层模型（物理层、链路层、网络层、传输层、会话层、表示层、应用层），另一种为 TCP/IP 层模型（应用层、传输层、网络层、链路层）

TCP/IP 分层

- 应用层
  应用层包含了所有与网络应用相关的协议，如 HTTP、FTP、SMDP、DNS 等；主要为应用提供通信服务。
- 传输层
  传输层为两台计算机上的应用提供了端到端的逻辑通信服务，确保数据可以从源主机传输到目的主机，传输层主要关注如何进行通信；主要协议有 TCP、UDP 协议。
- 网络层
  网络层为两台主机之间负责数据包的路由和转发，确定数据如何从发送者传输到接收者，将传输层产生的数据段封装成分组数据包发送到目的主机，并提供路由选择的能力；主要协议有 IP、IGMP(组播协议)。
- 网络接口层

  网络接口层提供主机连接到物理网络所需要的硬件和相关的协议，主要为负责帧的传播、物理寻址等；蓝牙、以太网、Wi-Fi 都属于这一层的。

  分层的好处本质是让复杂的问题简单化。通过各层的独立，限制了依赖范围，各层都有独立的接口，各层的工作不影响其他层的；灵活性更好，促进了标准化。

## 4. MTU 和 MSS

- MTU 工作在链路层，限定最大的数据帧的传输单元。由于以太网传输的大小是有限制的，使用最大的 MTU 不能超过数据传输链路的路径 MTU。

- MSS 工作在传输层，限制了传输层给网络层数据的最大段的大小。由于 MTU 的限制，发送方会把传输层给的数据段进行切片，而 MSS 的作用就是将数据主动进行切片然后递交给网络层。IP 数据包长度超过传输链路的 MTU 时要进行分片，而 TCP 层为了 IP 层不用分片，就主动进行分片再传输。 MSS = MTU - IP 首部 - TCP 首部

## 5. 三次握手和四次挥手

- 三次握手

  客户端发送一个初始的序列号 SYN 给服务器，此时客户端进入 SYN_SENT 状态
  
  服务端收到客户端的 SYN 后，回复一个 ACK 确认号和自己的初始序列号 SYN，此时服务端进入 SYN_RCVD 状态
  
  客户端收到服务端的 ACK 和 SYN 后，会对这个 SYN 进行确认，然后发送给服务器。此时客户端进入 ESTABLISHED 状态，服务端收到 ack 后随即进入 ESTABLISHED 状态，握手完成。

  自连接

  在一台主机里，一个进程监听一个端口进行连接请求，然后请求的主机刚好又是自己，就自己连接上了自己。

- 四次挥手

  客户端发送一个 FIN 序列号给服务器表示主动请求断开连接，此时客户端的状态为 FIN_WAIT-1
  
  服务器收到客户端的 FIN，先对这个 FIN 进行确认，此时服务器进入 CLOSE_WAIT 状态
  
  然后稍等一个定时器后服务器再回复一个 FIN 给客户端，此时服务器进入 LACK_WAIT 状态
  
  客户端收到服务器的 ACK 后进入 FIN_WAIT-2 状态，接着收到 FIN 后，对这个 FIN 进行确认就进入 
  
  WAIT_TIME 状态，最后等待 2 个 MSL 计时器后进入 CLOSED 状态，服务器收到确认号进入 CLOSED，连接关闭。

  同时关闭

  在挥手的时候，由于 TCP 是全双工的，两方同时发送一个 FIN 给彼此表示请求关闭，进入了 FIN_WAIT-1 状态，那么在对端收到 FIN 后，都进入了 CLOSING 状态，此时都会发送一个 ACK 给对端，进入 WAIT_TIME 状态，等待两个 MSL 就进入了 CLOSED 状态。这个过程中，双方同时请求关闭，都没有数据发送，可以跳过 FIN_WAIT-2 状态，3 次挥手就可以成功。

  SYN 和 FIN 都需要消耗一个序列号，因为只要需要 ack 确认的都需要消耗序列号。

  为什么需要四次挥手，三次可以吗？

  理论上是可以的，但是这个要确保服务器没有数据再传给客户端，因为客户端发送 FIN 时，表示客户端没有请求再发送，然后进入半关闭状态，但是服务端收到客户端的 FIN 后，是可以发送数据给客户端的，但是如果是 3 次挥手，同时回复 ACK+FIN，那么就无法再发送数据就进入半关闭状态了，所以要先发送一个 ack 给客户端确认，以免客户端收不到进行重传，然后等一个计时器时间延长确认，如果没有数据发送了，就发送 FIN 进入半关闭状态。

  可以 4 次握手吗？

  理论上也是可以的，即把 ack 和 syn 分开来传输，但是这完全没有必要，因为在连接之初，并没有数据传输，而是进行同步双方的序列号，所以没必要分开传输消耗性能。

## 6. TCP 首部时间戳

时间戳不是首部的一个固定字段，而是在 option 上的一个可选字段，由双方共同开启才会生效。主要为了计算 RTT 和解决 PAWS 问题，其中发送方在 options 中带 Tval 标识，而接收方收到后取发送方的 Tval 为自己的 Tscr 值，然后自己再递增生成一个 Tval 值发送给发送方。PAWS 指解决序列号回绕问题，在高速的网络传输中，大量的数据传输可能会造成序列号重复的问题，导致不知道当前接受的数据是新的还是旧的，而有了时间戳就很好的解决了这个问题。

## 7. 半连接和全连接队列

半连接队列（SYN 队列）和全连接队列（Accept 队列）是服务器的概念，用于帮助服务器处理传入的 TCP 连接请求。半连接队列，指在服务器接收到 SYN 并且回复 ACK 和 SYN 时后，也就是第二次握手完成后，服务器会将连接放到半连接的队列中；全连接队列，指在服务器完成 3 次握手之后，连接被放到全连接队列中，然后等待应用 accept 取走使用。当这两个队列满了的时候，服务器会对新的请求丢弃或者延迟处理。

## 8. SYN flood 攻击

当半连接队列被填满时，服务器会丢弃新的连接请求。那么问题就来了，如果攻击者模拟大量的发送方，向接收方发送大量的 SYN，表示要建立请求，而接收方并不知道这个 SYN 的真实性，就会回复 ACK 和 SYN 把请求转移到半连接队列，很快队列就会填满，而真正的用户想访问接收方尝试建立连接，但此时队列已经满了，使用没办法成功建立请求，这就是 SYN flood 请求。解决的方法有扩大半连接队列、限制 SYN 的接收率、使用防 DDos 服务、SYN cookie。而 SYN cookie 是最有效的方法，当半连接队列满时，使用特殊的算法计算出一个 cookie 值，放在回复的 ACK+SYN 中，当发送方收到后在回复的 ACK 中带上这个 cookie，接收方收到后会校验这个 cookie 的合法性，然后再给这个连接分配资源，本质上就是只有通过 SYN cookie 建立的连接成功后才会分配资源，减少了服务器资源的使用。当启用 SYN cookies 时，服务器只有在三次握手成功完成后才为连接分配资源。这与传统的 TCP 握手行为不同，其中服务器在收到 SYN 请求并发送 SYN+ACK 响应后，即使连接尚未完全建立，也会为其分配资源。

## 9. 快速打开

快速打开指在已经建立了一次 TCP 握手的情况下，在下一次连接建立的过程中，可以在发送 SYN 的同时携带数据，这样可以减少一个 RTT 往返时间，加快数据传输的效率。正常情况下，两个端每次发送数据之前都需要先建立连接，而不能直接在 SYN 携带数据，而有了快速打开之后，在发送方法首次和接收方建立连接时，options 有一个 TCP Fast Open 字段标识当前连接为了快速打开，而接收方接收到 SYN 后，生成一个 Cookie，连同 ack 和 syn 一起返回给发送方，发送方接收后返回一个 ack 然后把 cookie 存在本地。当下一次需要和接收方连接时，在 options 中带上这个 cookie，再携带数据一起发送，接收方接收后验证 cookie 的真实性，确认无误后即可以返回 ack+syn+发送方需要的数据给发送方，如果 cookie 不正确则走正常连接流程。这样就可以减少一个 RTT 往返时间，更快一步的拿到数据。

## 10. 超时重传、快速重传、SACK

超时重传指在发送端发送了数据之后，在等待接收端返回的过程中会启用一个计时器，当达到规定的时间后，还没有收到 ack，就会再重新发送数据，确保了 TCP 的可靠性，重传间隔的时间遵循指数级退避。

快速重传指在数据传输的过程中，发送端发送一连串的数据给接收端，那么接收端就会对这些分段数据进行确认，在发送端收到 3 次或以上接收端传来的 ack 中，并没有包含前面已经发送的数据时，发送端就不会等超时再重新传数据，而是直接把缺失的数据传给接收端。

在快速重传的过程中，仔细观察就会发现如发送 0 - 1000， 1001 - 2000，2001 - 3000 的过程中，实际上接收端可能已经接收了 1、3 段数据了，但是 ack 只对第一段数据进行确定，而第三段并没有，那么发送端就不知道已经发送端 2、3 段数据需不需要重新发送。而有了 SACK 就可以解决这个问题，在返回的 ack 中带上 sack，表示之前发送的数据哪些已经接收到了，重传 sack 没有包含的数据就可以了。

## 11. 超时重传的时间计算(RTO)

在重传的过程中，需要遵循一定的时间间隔进行重传，如果间隔太久接收段会等太久，如果间隔时间太短，已经传输的数据可能并没有丢失而是因为宽度等问题迟迟没有到，这个时候又重新传，就会造成或加大网络的堵塞，那么就需要使用到 RTO。计算的方法有经典算法取 RTT 的平均时间，平滑的计算出重传的时间，主要是适用 RTT 波动较小的时候使用；标准算法，主要是适用 RTT 波动较大的时候使用。

## 12. 滑动窗口

滑动窗口是 TCP 的一个重要的概念，主要用于控制发送方发送的数据流量，有发送窗口和接收窗口。

> 发送窗口是在发送方维护的，用于控制发送给接收方的数据量。发送窗口的大小可以根据接收方的接收窗口动态调整。当发送方发送的数据量达到接收窗口的上限时，发送端就会达到“窗口已满”**(Window Full)**的状态，此时发送方在途的数据已经等于接收窗口大小。除非接收到 ACK 确认，否则发送方不会再发送更多数据。

> 接收窗口则是在接收方维护的，指示接收方目前能够接收的数据量。在高速的网络传输中，如果接收方接收数据的速度很快，但处理和移出缓存的速度不够迅速，会导致接收端的缓存填满。在这种情况下，接收端会通过发送窗口更新（Window Update）通知发送方其窗口大小，可能会通告一个较小的窗口大小，甚至是零窗口**Zero Window**，后者表示接收窗口已满，暂时无法接收新数据，发送窗口应当调整为 0。

> 为了应对零窗口情况，TCP 实现了零窗口探测**Zero Window Probe**机制。当发送方的窗口大小为 0 时，它会周期性地发送探测消息到接收方，以便确定接收方是否已准备好接收更多数据。当接收方的窗口不再是零时，它会响应探测，发送方随后可以恢复数据发送。

## 13. 拥塞控制

滑动窗口整体上是控制发送端和接收端的数据流量，控制的是自身，而拥塞控制则不同，把持着整个网络的传输数据流量，主要算法有慢启动、拥塞避免、快速重传、快速恢复。为了实现这些算法，每一个 TCP 连接都包含了拥塞窗口、慢启动阀值的概念。与接收窗口不同，拥塞窗口是控制自身在收到对端的 ack 确认之前，还能传输的最大 MSS 段数，发送窗口的大小等于拥塞窗口和接收窗口两者的最小值，也就是说发送窗口受接收端和网络传输快慢的限制，不能比两者都大，拥塞控制算法本质上是控制拥塞窗口大小的变化。

慢启动是指在 TCP 连接后，两端各自初始化自己的拥塞窗口大小，由一个固定的值开始，每确认一个 ack 就会时 cwnd+1，所以每往返一个 RTT 就会指数级的递增，以达到最适合网络传输的大小。

由于慢启动算法的指数级增长，拥塞窗口很快就会变得很大，如果不加以限制很容易造成网络的堵塞，于是有了拥塞避免算法，该算法规定，当 cwnd 增长到一定的阀值大小时，变为线性级增长放缓增长速度，每经过一个 RTT，cwnd+1 个 MSS 段大小，以避免网络的拥塞。

快速恢复算法是发生在快速重传之后，当发送端开始重传丢失的数据时，并不会遵循慢启动算法的方式，而是将 cwnd 的阀值减半，直接将 cwnd 的大小设置为阀值的大小，然后遵循线性增长的进行传输，加快网络的吞吐量。

## 14. Nagle 算法

Nagle 的设计是为了减少小数据包在网络中的传输，减少带宽的浪费。在网络传输中，每发出一个包都会包含 TCP 和 IP 头部信息，这些信息大小一般都为 40 字节，而如果在传输的过程中，太小的一个包都进行一个 tcp 传输，那么就会非常浪费带宽，而 Nagle 算法就是为了避免传输大量的小包而出现的。

Nagle 的工作原理

- 在首次传输数据中，不论大小，都会立即发送出去。
- 而在此之后，只有满足以下两个条件之一才会发送更多数据
  - 接收端已经对前面发送的所有数据都进行确认
  - 要发送的数据达到 MSS 段大小
- 在等待 ACK 确认的过程中，如果积累了很多小包，那么 Nagle 算法会将这些包合并到一个 MSS 段大小，然后再发送

## 15. 延迟确认

在传输过程中，接收端每收到一个数据包都需要对其进行 ACK 确认，而如果每次进行确认都只回复一个 ACK，那么就会造成很大的浪费。延迟确认允许接收端收到数据后，延迟一定的时间再进行确认，而不是马上就进行确认。这样就可以减少网络上的 ACK 确认包的数量，降低网络和主机的负载。

延迟确认工作原理

- 接收端收到一个 TCP 包，并不会立即的进行确认，而是开启一个定时器
- 在定时器到期之前，如果接收端收到了很多包，不会每个包都回复一个 ACK 确认，而是累积成起来，等定时器到期时，发送一个累积确认 ACK，确认这个 ACK 之前的所有包都已经收到了。
- 同样，在定时器到期之前接收端如果有数据要发送给发送端，那么 ACK 就可以搭上顺风车一起返回给发送端。

延迟确认是默认开启的功能，可以提高网络的效率，但是有些情况下也会给传输的过程造成一定的负担。如 Nagle 算法和延迟确认一起工作的话，就会出现严重的性能问题。试想一下，Nagle 算法是把多个包攒成一个一起发送，而延迟确认是收到包不马上确认，两边互等浪费时间。

## 16. keepalive

Keepalive 是检测网络连接是否仍然存在的机制，当一个 TCP 连接已经建立后，长期没有数据传输，Keepalive 机制就会发送一个探测信息，接收端必须响应这个信息，否者发送端会假设这个连接已经断开而关闭这个连接。

Keepalive 工作原理

- 在启用了 keepalive 的 TCP 连接成功后，如果在一定的时间内没有数据在连接上交换，发送端就会发送一个 Keepalive 探测包，这个探测包是为了探测对方是否还存在，不包含任何数据。

- 如果接收端收到这个探测包会进行确认一个 ACK，表示连接是活动的，并且继续保持连接状态

- 如果发送端长时间没有收到这个 ACK 确认，那么就会重复发送探测包，在重复了一定的次数后还没有收到确认包，就默认连接已经断开，然后关闭这个连接。

## 17. 队头阻塞

队头阻塞指的是在一系列的消息队列中，第一个消息因为某种原因延迟处理，而导致了后面的消息即使已经到达了但是还不能被及时处理。在 TCP 中，队头的阻塞是因为 TCP 的严格顺序而引起的，由于 TCP 保证包的顺序到底，因此即使后面的数据先到达，但是仍然不能够被处理。例如，有 A、B、C3 个包，B 和 C 先到达了，而 A 由于某种原因延迟或丢失，迟迟没有到达，接收端必须等待 A 的到达才会对这一组消息进行处理，结果 B 和 C 就被 A 给阻塞了。

# HTTP

## 1. HTTP 的报文结构

HTTP 的报文的结构由开始行、头部字段、一个空行和可选的消息体组成，分为请求报文和响应报文。请求报文由请求行、请求头、一个空行、请求体组成，而响应报文由状态行、响应头、一个空行、响应实体组成。

## 2. 请求方法

HTTP 的请求方法有 GET、POST、PUT、DELETE、OPTIONS、HARD 等

- GET 方法

  主要是获取指定的服务器资源。幂等（多次请求产生的数据都是一样的），不管请求多少次对服务器的数据都不会产生任何影响，请求数据包含在 URL 中，可缓存。

- POST 方法

  主要是向服务端提交数。非幂等（相同的多次请求会产生不同的效果），每次请求都会对服务器数据产生副作用，请求数据包含在请求体中，但不能缓存。

- PUT 方法

  将请求的内容放置或者修改内容到指定的 URI 中，可以理解为修改数据。幂等（相同的多次请求产生的效果都一样），不管请求多少次对服务器的影响都是有限的，请求数据包含在请求体中，不能被缓存。

- DELETE

  对指定的内容进行删除操作。幂等（相对的请求产生的效果都是一样的），不管请求多少次对服务器的影响都是有限的，数据包含在 url 中，不能被缓存。

- OPTIONS

  请求获取服务器允许的通信选项，一般是当 POST 的预请求使用，请求服务器能使用的请求方法以及可以使用的字段。幂等（多次请求，返回的响应都是一样的），用于查询不会产生副作用，没有请求数据，可以缓存

- HARD

  请求资源的标头信息，返回的和 GET 响应的类似，但没有实际数据。主要用于在下载一个大文件时，先通过 HARD 请求到实际请求到头部字段 content-length 的实际大小，再进行请求，和 option 类似。

## 3. GET 和 POST 请求的区别

GET 主要是用于获取数据，而 POST 主要用于数据的提交修改等。GET 请求的数据包含在 URL 中，由于 URL 长度受浏览器限制，所以请求的数据就有了限制；而 POST 请求的数据包含在请求体中，所以数据长度没有限制。由于 GET 请求的数据暴露在 URL 中，相对来说就没有 POST 请求安全。GET 请求可以被浏览器缓存记录，而 POST 则不行。GET 请求是幂等的，不管请求多少次返回的数据都是一样的，并且不会对数据产生副作用；而 POST 是非幂等的，每次请求的响应都不一样，并且会对数据产生副作用。

## 4. 状态码

状态码存在 HTTP 的响应状态行中，有 1XX、2XX、3XX、4XX、5XX

1XX：表示请求正在进行中

2XX：表示请求成功

- 200(OK)：表示正常请求成功
- 202(Accepted)：表示服务已经收到了请求，但是还没有处理
- 204(Not Content)：服务器成功处理了请求，但是没有返回数据
- 206(Partial Content)：表示请求的区间数据成功，要结合请求头的 Rang 字段使用

3XX：表示重定向，需要后续操作才能完成请求

- 301(Moved Permanently)：表示永久重定向，当前的请求已经被永久的重定向到响应头的 Location 的 URL 中，下次发起请求应该请求到 Location 中
- 302(Found)：表示临时重定向，当前的请求被临时的重定向到响应头的 Location 的 URL 中，但是下次请求还是可以正常的请求当前的 URL
- 303(See Other)：通常作为 PUT 和 POST 的返回结果
- 304(NOT Modified)：使用协商缓存返回到的数据，表示当前的请求返回数据命中了协商缓存，数据没有被修改过，使用缓存的数据。

4XX：表示客户端错误

- 400(Bad Request)：表示客户端发送给服务端的请求数据存在错误。
- 401(Unauthorized)：表示当前请求需要鉴权，鉴权成功后才能正常发起请求
- 402(Payment Required)：表示当前的请求需要付费后才能正常使用
- 403(Forbidden)：表示客户端的请求错误，服务端可以处理，但是拒绝处理。
- 404(Not Found)：表示请求的页面不存在，不能确定是永久丢失还是零时丢失
- 405(Method Not Allowed)：表示当前请求方法被服务器禁止
- 410(Gone)：表示请求的页面已经永久的丢失，跟 404 类似，但是确认为永久丢失

5XX：表示服务端错误

- 500(Internal Serve Error)：服务器出现了错误，尚不能处理
- 501(Not Implemented)：表示客户端请求的访问不被服务器支持
- 503(Service Unavailable)：表示当前服务不可用，服务器可能挂掉了

## 5. HTTP 各版本之间的优缺点

从 HTTP 首版确定到现在，已经迭代了 4 个版本，为 HTTP/0.9、HTTP/1.0、HTTP/1.1、HTTP/2.0

- HTTP/0.9：1991 年推出，只支持 GET 请求，不支持请求头和元数据，只能请求简单的 HTML 文件

- HTTP/1.0：1996 年推出，引入了请求头和响应头的概念，允许传输除了纯文本之外的其他类型数据，还引入了状态码、多字符集主持。但是不支持持久连接，每次请求/响应完成后，就会断开连接，下次请求要重新进行 TCP 连接。

- HTTP/1.1：1997 年推出，支持持久连接，引入管道化技术，允许同一个 TCP 连接中，按顺序发送多个请求而不需要等待响应。引入分块传输概念，将生成的数据分块的进行传输效率更快，还引入了更多的缓存策略，同时支持更多的请求方法，如 PUT、DELETE、OPTIONS 等。

- HTTP/2.0：2015 年正式推出，支持二进制帧的机制，将请求和响应分成多个传输帧。引入多路复用的概念，可以在同一个连接并行的传输，解决了 1.X 的对头阻塞问题。支持定义优先级的请求，支持服务器推送，引入了头部压缩机制，减少了冗余头部信息量的传输，并且强制使用了 HTTPS 进行加密传输。

- HTTP/3.0：不基于TCP进行传输，而是基于UDP进行传输。在UDP的传输之上建立一个QUIC（快速UDP互联网连接）协议，这个协议可以实现TCP的可靠传输、更快的握手、TLS、多路复用的数据流等。
  
HTTP/2.0的核心是实现了多路复用，它允许在同一TCP连接上同时发送多个请求和响应，而不必等待其他请求或响应完成。由于TCP 的慢启动、多条 TCP 连接竞争带宽和队头阻塞，会造成宽度的利用率低下。而多路复用就是通过一个域名就采用一个tcp长连接来传输数据，避免了多条tcp连接出现的慢启动问题。多路复用引入了二进制分帧层来实现，当数据经过二进制分帧层时，会讲数据切割成一个个带有ID编号的帧进行传输，在服务器收到这些帧后会将相同ID的二进制帧合并成一条完整的数据供上层使用。

HTTP/2 的一个核心特性是使用了多路复用技术，因此它可以通过一个 TCP 连接来发送多个 URL 请求。多路复用技术能充分利用带宽，最大限度规避了 TCP 的慢启动所带来的问题，同时还实现了头部压缩、服务器推送等功能，使得页面资源的传输速度得到了大幅提升。在 HTTP/1.1 时代，为了提升并行下载效率，浏览器为每个域名维护了 6 个 TCP 连接；而采用 HTTP/2 之后，浏览器只需要为每个域名维护 1 个 TCP 持久连接，同时还解决了 HTTP/1.1 队头阻塞的问题。

HTTP/2.0 的二进制帧就是将 1.X 的文本格式改为二进制的格式进行传输，在 2.0 中，客户端和服务端之间的传输采用了二进制帧的格式，这些帧是最小的传输单位，传输成为流，每个帧都包含了特定的数据信息。有了这个二进制帧的设计可以使 HTTP 支持多路复用，在一个连接中并行的传输数据，而不会相互影响，解决了队头的阻塞问题。同时还可以设置传输的优先级，规定哪些是需要优先传输的内容，并且支持头部压缩的功能。

头部压缩原理：在 1.X 版本中，每次 HTTP 传输都携带着头部信息，这些信息大多都是相同的，如果每次传输都带上就会带来很大的性能开销。于是在 2.0 版本中，采用了 HPACK 算法来对头部信息进行压缩处理，使之体积更小传输更快。

## 6. 跨域

跨域是受同源策略的影响，一个端无法对另一个不同源的端进行请求。这样做的目的是处于安全的问题考虑，防止当前源被其他恶意的请求造成影响，如 XSS、CSRF 等。但是如果想跨源访问另外一个地址，有以下 6 种解决方案。

解决的方法有使用 JSONP、postMessages、CORS、代理服务器、document.domain、web socket

- JSONP
  基于 HTML 的 script 标签的 src 属性不受同源策略的影响，动态的生成一个 script 标签，插入文档中，将要请求的 url 设置为 src 属性，在 url 的尾部拼接一个 callBack 回调方法，用于服务器的响应请求。但是由于是用 url 作为连接请求，所以只支持 GET 请求。

- CORS
  是 W3C 标准的跨域解决方案，由服务器在响应头设置`Access-Control-Allow-Origin`的值允许指定源可以请求，如果允许所有的源请求，可以将值设置为*，`Access-Control-Allow-Origin: *`。

- 代理服务器
  客户端和服务端传输数据存在同源策略的限制，但服务器和服务器之间请求则不受限制。在客户端中请求同一个源的服务器，然后再由这个源去请求指定的源的数据，做一个中间代理请求。

- postMessage
  postMessage 是 HTML5 的一个方法，支持同一个浏览器不同的窗口之间进行通信，如在 A 页面传输到 B 页面，在 B 页面监听 message 事件，以及可以和页面中嵌入到 iframe 通信，所以这个方法仅用于同一个浏览器内的不同源实现。

- document.domain
  document.domain 属性允许用来做两个相同的主域但是不同的子域的页面进行 JavaScript 通信。在两个相同主域但不同子域的情况下，原则是是不能互相访问对方的 dom 结构，但是通过双方共同设置`document.domain`属性为同一个域名，那么就可以访问对方的 dom 结构了。但是这样更容易引发安全问题，如 XSS 等。

- Web Socket
  根据 web socket 不受同源策略的限制，可以实现双工通信。

## 7. 首部字段

首部字段为客户端和服务端之间的传输提供了重要的信息，分为通用首部、请求首部、响应首部、实体首部 4 大类

- **通用首部字段**：可以在请求和响应头里使用，但是跟实体传输的数据没有太大的关联

  - Catch-Control：用于描述数据的缓存信息，用那种机制来实现缓存，是单向的。
  - Connection：表示当前连接会话完成之后保持的连接状态，在 1.1 版本中，默认为 keep-alive 持久连接
  - Transfer-Encoding：是一个逐级跳转的首部字段，跟 Accept-Encoding 类似，但是它不是相对于端到端的，而是级到级的。
  - Pragma：
  - Upgrade：表示提示服务器需要升级协议，如使用 web socket 协议传输，那么这个值就是 web socket
  - Data：报文创建的时间
  - Via：用于描述经过了哪些代理的服务器，每经过一个代理的服务器都会把代理服务器的信息记录到这个字段

- **请求首部字段**：在发送请求时使用，关于请求方的信息

  - Accept：告知服务器客户端允许处理的内容类型，用 MIME 格式表示
  - Accept-charset：告知服务器客户端允许处理的字符集类型
  - Accept-Language：告知服务器客户端允许处理的的语言
  - Accept-Encoding：告知服务器客户端允许处理的编码压缩类型，可选值为`gzip、compress、deflate、identity、*`
  - Authorization：提供服务端用于验证客户端身份的信息的凭据
  - Host：目标源服务器的主机名和端口号
  - If-Match：属于条件请求，配合`ETag`使用，当远端的实体标签与 ETag 这个值相同的话，将内容返回
  - If-None-Match：与 `If-Math` 相反，跟 ETag 值不同就返回
  - User-Agent：通常用来表示浏览器或其他代理的信息
  - Origin：表示请求的来源，一般在代理访问的时候使用，比如 A ->B —>C ，Origin 的值可以是 B 的地址
  - Referer：表示请求的源页面，指是那个根源页面发出来的请求
  - Range：表示请求指定范围的数据，如果有则以 206 状态码返回，如果没有就返回 416，如何服务器忽略这个字段就返回正常的 200

- **响应首部字段**：在响应请求时使用，更多时关于服务端响应的信息
- ETag：表示资源的版本信息，即表示当前请求的实体数据有没有被修改过，让缓存更加高效。分为强比较和弱比较，弱比较带 **w/** 前缀
- Location：表示应该访问这个字段的值的 url，配合 3XX 重定向使用
- Server：接收请求处理服务端的信息
- WWW-Authorization：配合 401 使用，表示请求方还没鉴权
- Content-Security-Policy：XSS 攻击的解决方案，这个头限制了当前浏览器可以加载哪些源的脚本。

- **实体首部字段**：关于请求和响应的实体数据的描述信息
  - Content-Encoding：实体的编码格式
  - Content-Language：实体的语言类型
  - Content-Length：实体的长度
  - Content-Type：实体的数据类型
  - Expires：实体数据的过期时间，表示在此时间之后数据会过期，但如果`Catch-Control`的值为`max-age`或`s-max-age`时会被覆盖
  - Last-Modified：上一次实体数据的修改时间

**Accept 内容协商**：请求首部的 Accept 系列属于内容协商的首部，搭配实体首部的 Content 系列首部使用，比如当请求头发送`Accept: html/text`时，其目的是为了告诉服务器我接受 **text/html** 格式类型的实体数据，那么服务端返回时，实体首部的就是`Content-type: html/text`，告诉客户端返回的是 **text/html** 格式类型的实体数据

## 8. HTTPS

HTTPS 本质上是套了一层 TLS/SSL 安全层的 HTTP 协议，不是一个新的协议结构，主要是为了使连接更安全，用于为数据的传输提供加密、身份验证和保持数据的完整性。HTTPS 在 HTTP 的的基础上加了一个 TLS 层负责数据的加密和解密，使数据传输可以基于 TLS 上传输，确认数据的安全性。

## 9. TLS

TLS 是 SSL 的升级版本，两者在设计上是相识的，但是 TLS 采用了更加先进的加密算法和安全措施，相比于 SSL 就要更安全一些。
当前最新的版本是 TLS1.3，该版本提供了更快的握手过程，减少了不必要的 RTT，同时改进了加密的算法，比之前的版本更安全可靠。

TLS 协议主要确保通过 HTTP 进行的数据传输得到加密保护，从而在数据传送过程中避免被监听，确保个人隐私安全。此外，TLS 通过其证书系统提供身份验证，使得服务端能够确认通信数据的来源，确保数据传输双方的身份明确。最后，TLS 通过完整性验证机制保障数据在传输过程中未被篡改，从而抵御潜在的中间人攻击，确保数据的完整性和真实性。

TLS 握手过程

- 客户端 Hello：客户端给服务端发送一个支持的 TLS 版本列表、加密方法和一个随机生成数
- 服务端 Hello：服务端收到后，会选出一个 TLS 的版本，然后再生成一个随机数再带上证书返回给客户端
- 客户端验证：客户端验证服务端发来的证书，确保是可信以及有效的
- 密钥交换：客户端使用服务端的证书获得的公钥加密生成一个预主密钥，并将预主密钥发送给服务端
- 会话密钥生成：服务端使用私钥解密预主密钥，双方都是根据预主密钥和随机数派生出一个会话密钥，这个会话密钥用于对数据的加密和解密。
- 结束握手：交换双方的**Finished**信息，确保双方的握手过程没有被窜改，然后确认握手完成

HTTPS 的数据传输过程

- 建立一个 TCP 连接
- 在 TCP 连接的基础上再进行 HTTPS 握手
- 数据传输，双方使用握手过程中生成的会话密钥来加密/解迷数据。发送方将数据切割成一个个 TLS 记录，每个记录都包含它的 MAC 码，再进行加密传输。接收方接收到后，用会话密钥解密数据，再检验 MAC 码，确保数据的完整性和真实性。
- 数据传输完成后，一方想要关闭连接，就会发送一个**close_notify**警告给对方，表示要关闭会话

## 10. 定长和不定长数据传输

定长和不定长数据传输是属于 HTTP 传输数据的概念，在 HTTP 中，定长数据传输是指当前发送的数据长度大小是已知的，在头部字段`Content-Length`中定义了传输数据的长度。而不定长数据传输的数据则是指数据的长度是未知的，由服务端动态的生成，不定长数据传输采用了**分块数据传输**的概念，把过长的数据切割成小块然后传输。

## 11. Post请求为什么有时候会发送两次请求

在使用CORS时，浏览器会多发送一个OPTIONS预检请求，请求服务器是否支持当前的请求。在发送可能会对服务器资源产生修改的请求、使用了自定义的请求头、POST发送非简单的Accept内容值、尝试修改和设置cookie这些操作中都会发送一个预测请求。发送的预测请求会在请求头带上`Origin,Access-Control-Request-Method,Access-Control-Request-Headers`字段，询问客户端支持哪些源、哪些请求方法和请问将携带的字段，随后服务端会返回`Access-Control-Allow-Origin,Access-Control-Allow-Method,Access-Control-Allow-Headers`，来响应支持的请求源、方法和字段。

## 12. Http和TCP的关系

TCP（传输控制协议）和HTTP（超文本传输协议）在都是网络通信中重要的协议。可以把TCP想象成为数据传输的可靠卡车司机，而HTTP则像是定义了货物（也就是数据）的包装和交付方式的规则。

具体来说，TCP位于传输层，它负责在两个网络节点之间建立稳定的、有序的、并且可靠的数据传输通道。它是确保数据能够正确送达目的地，处理数据的分包、重传、流量控制和拥塞控制的任务。简单来说，TCP就是确保我们的数据能够完整、顺序地到达对方手中的技术。

而HTTP位于应用层，它定义了客户端和服务器之间交换数据的格式和规则，主要用于网页数据的传输。HTTP决定了如何包装这些数据（请求和响应的结构），以及客户端和服务器如何进行对话（如请求方法、状态代码等）。这就像是我们在寄快递时填写的地址单和包裹内容清单，确保寄送的内容是我们想要的，并且对方也明白收到了什么。

所以，TCP和HTTP在网络通信中是紧密相连的。TCP为HTTP提供了可靠的传输基础，而HTTP则利用这个基础来进行高效的网络通信和数据交换。简而言之，TCP是数据传输的基础，而HTTP是在这个基础上实现具体应用的一种协议。

## 13. HTTPS一定安全吗
并不是绝对安全的，HTTPS是通过在HTTP的基础上新建一个TLS安全层继续数据的加密/解密传输，然而在这个过程中数据仍然可能被窃取。
- HTTPS通过CA证书来验证服务端的身份，那么如果颁发证书的机构受到攻击或不可信，这可能会导致安全隐患
- 服务器在配置TLS/SSL的过程中出现差错或者使用了弱密码来加密数据，很可能回导致黑客的攻击
- 客户端或服务器上如果存在软件漏洞，很可能会造成HTTPS连接受到攻击
- HTTPS通过安全层来加/解密数据，可以避免中间人攻击。但是尽管如此，如果黑客在用户的设备上安装恶意的证书并且以某种手段取得信任，那么黑客仍然可以使用恶意的证书来对用户传输的数据进行解密。
- SSL剥离攻击，黑客可以强行的将HTTPS降级为HTTP连接
- HTTPS保证数据的安全传输不被识别，但是并不能保证网站的数据是否安全或合法，因此仍可能会存在钓鱼或恶意网站