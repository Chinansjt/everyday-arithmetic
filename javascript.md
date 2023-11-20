# 笔记

**这里记录我在学习过程中记的笔记，方便我日后复习巩固**

# 1、JS 数组

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

# 2、JS 对象

## 类型检测

**_解释_**：js 类型检测指是判断一个变量的类型及构造方式，分为两种类型：基础类型和引用类型；其中基础类型包括 String、Number、Boolean、undefine、null、symbol，引用类型包括 Object、Function、Array。

1. **typeof**：一般用于检测基础类型，检测 Object、null、array 都会返回 'object'，检测 Function 会返回‘function’，
2. **instanceof**：用于检测引用类型，检测是否属于原型链上。
3. **contractor**：每个对象都有一个 constructor 属性，指向该对象的构造函数
4. **Object.prototype.toString.call()**：更精确的判断变量的类型，可以精确判断是哪一种类型。

## 对象创建

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

## 继承

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

# 3、迭代器和生成器

## 迭代器

**_解释_**：迭代器去一个对象，用于定义可以迭代的协议。对象是无法迭代的，因此可以手动定义一个迭代器，用 for of 迭代。

## 生成器

**_解释_**：生成器是 es6 的一个函数，和普通函数不同，生成器里面的代码不会一下子执行完，可以控制暂停和恢复执行。改函数返回一个生成器对象。

# 4、设计模式

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

# 5、CSS

### BFC

**_解释_**：BFC（块级格式化上下文），指的是在文档流中，独立出来一个渲染块，bfc 里面的元素布局不影响外面的布局

会创建 BFC 的元素有

> - float 为 left、right
> - overflow 不为 visible
> - display 为 flex、inline-block、table-cell、grid 等
> - position 为 absolute、flexed
> - 根元素

BFC 的布局规则是

> - BFC 内的元素一块一块的垂直折叠
> - BFC 内的两个相邻的块级元素会发生 margin 重叠
> - BFC 内浮动的元素高度也会被计算（用于清除浮动）
> - BFC 里面的元素布局不会影响到外面的元素的布局

BFC 的作用

> - 消除两个相邻的元素 margin 重叠问题，把其中一个元素添加一个新的 BFC 里面
> - 浮动元素会脱离父元素的文档流，此时可以给父元素创建一个 BFC
> - 解决文字环绕浮动元素的问题。
> - 用于实现多栏布局，用浮动实现侧边栏固定

清除浮动可以给父容器添加 BFC，还可以使用 clearfix 定义一个伪类元素 clear：both

# 6、Vue 源码

## 1. 执行 new Vue 后发生了什么

> 执行 new Vue 后，会调用 Vue 这个构造函数并创建一个实例。在这个过程中，先将用户传入的属性和内部的默认属性进行合并。紧接着 Vue 执行了一系列的初始化工作，如初始化生命周期、事件系统和 render，调用**beforeCreate**钩子。接着，初始化 data、props、computer、watch 和 method 等属性，调用**created**钩子。如果传入有 el 属性则自动执行$mount 进行挂载操作，即编译模版并转成渲染函数，生成虚拟 dom，最后挂载到真实的 dom 上。如果没有 el 属性，就需要手动进行挂载。

## 2. Vue 实例挂载的整个过程

> 通过 new Vue 创建实例后, 若传入的 options 含有 el 属性，Vue 将自动开始挂载。此过程首先是模板编译：当存在 template 时，它被转化为渲染函数；否则，el 指定的外部 HTML 被用作模板。这个渲染函数中利用**createElement**生成虚拟 DOM，但是如果 option 中已经定义了 render 函数，那么就不会走这个编译模版的过程，而是直接调用**createElement**生成虚拟 DOM。接着，Vue 将虚拟 DOM 与真实 DOM 对比。首次挂载时，直接渲染；后续更新时，通过 diff 算法找出差异，并高效地更新真实 DOM。

## 3. Vue 组件创建的过程

>

# 7、浏览器

## 1. 从浏览器输入 URL 到页面呈现出内容，这个过程发生了什么

> - 地址解析：当输入 url 后，浏览器会先尝试在缓存中查找对应的 IP 地址，如果缓存没有就访问本地的操作系统，如果还没有就会访问远程的 DNS 系统进行地址解析，拿到对应的 IP 地址。
> - 建立 TCP 连接：当拿到 IP 地址时，浏览器会和服务器建立连接即 TCP/IP 连接，进行 3 次握手后便可连接成功，如果是 HTTPS 请求还要建立 SSL/TLS 加密连接。
> - 发送 HTTP 请求：浏览器和服务器建立连接之后就可以发生 HTTP 请求给到服务器。
> - 服务器返回响应请求：服务器收到请求后，会处理相应的逻辑，返回符合请求要求的数据，通常会有 HTML、CSS、JavaScript 等内容
> - 浏览器解析响应请求：浏览器解析响应数据，首先解析 HTML 生成一棵 DOM 树，与此同时 CSS 也在并行的解析生成 CSS OM 树，最后将解析出来的 DOM 树和 CSS OM 树进行合并得到一棵渲染树。如果在这个过程中遇到 Javascript 文件，那么就会阻塞 HTML 文档的解析，此时可以给 js 文件定义一个 defer 属性标识 js 文件要等 html 解析完成才会加载执行，还可以设置 async 属性，但是该属性只是异步加载 js 文件，在加载成功后，如果 html 还没解析完成还是会阻塞解析。
> - 浏览器渲染出来：当合并成 render 树的时候，浏览器会从根节点递归，在屏幕上标识每一个节点的位置、大小等，进行布局和重绘。一旦布局确定，浏览器就会进行渲染，将每一个节点绘制成实际像素。

## 2. 调用栈溢出

> 调用栈溢出指在计算机中，函数调用的层级超过了指定的层级导致调用栈的容量不够然后溢出。在计算机中一个函数被调用就会被压到栈顶，当函数执行完成并返回时就从栈顶弹出。一般太深层的函数嵌套和无限的递归会导致栈溢出。

## 3. 栈空间、堆空间、代码空间

> - 栈空间是一种先进后出的数据结构，用于存储局部变量、函数调用的返回地址等，提供快速的访问能力，并且会自动分配地址和回收地址
> - 堆空间是指用于动态内存分配的内存区域，用于存储应用类型，通常访问比栈慢，需要手动分配和回收内存，如果使用不当会造成堆内存泄漏。对象、数组、函数实际上是存储在堆中，然后返回堆的指针并将指针存在栈中。
> - 代码段指的是 js 的代码，以二进制的形式存储在计算机中。.js 文件的代码都是

## 4. 垃圾回收机制

> - 引用计数：浏览器标记对象的引用情况，当引用的值为 0 时，垃圾回收机制就会自动回收。但是如果存在循环引用就不会生效。
> - 标记清除：用来解决引用计数存在的循环引用问题，浏览器会定期的从根对象开始逐步检查，标记可以访问到的对象，访问完毕时，没有被标记的对象被当成垃圾清除。
> - 手动清除：当我们声明一个对象时，不需要再使用的时候可以手动的清除，如使用闭包。也可以使用弱引用集合来断开引用，如 weakMap、weakSet
> - 内存限制：浏览器设置了一定的内存容量阀值，当到达内存的使用限制时，会频繁的调用垃圾回收机制。

## 5. JavaScript 执行过程

> 当浏览器解析 HTML 文档并遇到 script 标签时，它将下载相应的 JS 源代码。一旦 JavaScript 引擎获得源代码，它首先进行**词法分析**，这一步将源代码分解为多个标记，如 var、if 等。紧接着，**语法分析**阶段开始，将这些标记转换为一个**抽象语法树** (AST)，这是一个计算机可以理解的代码结构表示。在代码真正执行前，引擎进行了一个关键的**预编译**步骤。在这一阶段，它识别所有的变量和函数声明，并为它们在内存中分配空间。这也是变量提升和函数提升现象产生的地方。随后，为了提高效率，现代 JS 引擎使用**即时编译** (JIT) 技术。这意味着代码被边解释边编译，AST 首先被转换为字节码，然后进一步转换为机器码。最终，这些机器码在 JavaScript 引擎中执行，激活所有的函数调用、赋值和其他程序操作。这个流程确保了 JavaScript 代码的高效、流畅的执行

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
  - 反射性 XSS 相对与存储型的危害就没有那么广，反射型是指攻击者构建一个包含恶意代码的 URL，诱导用户点击这个 URL，将恶意代码提交到服务器，然后服务器再把恶意代码返回给用户的浏览器，然后执行，以达到攻击目的。
  - dom 型 XSS 是指攻击者伪造一个 URL，诱导用户点击执行，而 URL 中的恶意代码又会被浏览器动态的插入到 DOM 中，于是达到攻击目的。攻击者构造一个链接，如 `http://example.com/page#<script>alert('XSS')</script>`。用户访问这个 URL 时，页面的 JavaScript 代码从 URL 的片段（hash）读取数据并将其插入 DOM，导致脚本执行。

针对 XSS 的防范有：对用户的输入和输出进行严格过滤或转译，不允许输入和输出原样的返回到客户端中。使用 CSP 机制限制浏览器的不同源的代码执行，在响应头设置`Content-Security-Polity`的值。一般攻击者都是为了拿到用户的 cookie 指，在 cookie 字段设置为`httpOnly`，限制浏览器通过 js 访问 cookie 的值。同时避免用户的输入直接显示到页面的 dom 中，如使用`v-html`或`innerHtml`。

- 跨站请求伪造(CSRF)：这种攻击方式是用户已经在网站登录了的情况下，诱导用户执行一些操作，然后在用户不知情的情况下执行一个恶意操作，使用用户已经登录的状态伪装成用户，从而达到攻击目的。

**防范的措施**可以在请求的过程中生成一个 Token，在发送请求时，服务端验证这个 Token 是可信的，再对请求进行处理，否者拒绝处理。又或者设置 cookie 的`SameSide`属性，限制跨站请求时，不能使用 cookie

- 点击劫持：点击劫持使用的是视觉欺骗，诱导用户在不知情的情况下点击不安全的按钮，以达到特定的目的，即将危险的点击操作伪装成安全的，用户以为是安全的其实是不安全的。
- 中间人攻击：发生在网络传输的过程，攻击者在两个通信方中间进行拦截、截取或更换他们的通信信息。常见的攻击类型有 ARP 欺骗、DNS 劫持、SSL/TLS 劫持、Wi-Fi 欺骗等。

- DNS 劫持：在用户向指定 URL 发送请求时，在这个过程中会查询 URL 对应的 IP 地址。此时攻击者就会串改或劫持用户的请求，将请求的目标网站 IP 地址重定向到攻击者给定的 IP 地址，然后与用户进行通信。
- 开放重定向：web 上的安全漏洞，允许用户从一个受信任的链接重定向到外部的链接。这种攻击的存在通常是由于网站没有正确验证重定向参数的结果，使得攻击者可以自由指定重定向目标

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

loader是webpack的核心功能之一，loader作为资源的加载器。loader不仅限于文件转换，还可以包括代码检查、格式化等功能。webpack本质上只能处理js文件，但是提高loaders，webpack可以处理各种类型的资源，比如css、img等资源，类似于翻译官一样，loader把不同的资源转换成webpack可以识别的资源，便于webpack处理。loader是逆向执行的管道操作，在use属性中，可以作为一个数组定义，loader从最后一项加载，最后一项的处理输出作为前一项的处理输入。如`use: ['style-loader', 'css-loader', 'sass-lader']`，webpack首先处理sass-lader，将sass转换为css，然后css-loader再根据sass-loader的输出结果转换为js文件，以此类推，直到处理成最种webpack可以识别的格式。

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
   - VueLoaderPlugin：加载出来vue的插件
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

- entry 入口文件：通过配置 entry 属性为多入口，可以手动将每个入口切分成不同的 bundle 文件，减少总 bundle 的大小

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
   - export-loader：在文件中，没有显示的使用export将模块导出，那么可以使用这个加载器将模块导出。
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

## NPM

### 1. 执行 npm install 背后发生了什么

> npm install 本质是执行为项目安装依赖的过程。首先，npm 会检索项目下的 package.json 文件下的依赖，如果是首次安装那么会直接访问 npm 仓库，非首次安装就会查看 node_modules 目录，了解哪些需要安装或者哪些包需要更新，然后在查询 npm 仓库；由于每个包可能依赖其他的包，那么 npm 就会创建一个依赖树，确保有些包不会重复安装；之后 npm 开始下载所需要的包，再解压到 node_modules 目录下。npm 会生成或更新 package-lock.json 文件，这个文件包含的是下载的包的精确版本信息，确保了其他人下载的跟你下载的一样。此时，npm install 已经执行完成，后台会输出这些安装过程的日记。但是如果 node_modules 存在了但 package 不存在的包，那么 npm 会把这些包删除掉。

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
  服务端收到客户端的 SYN 后，恢复一个 ACK 确认号和自己的初始序列号 SYN，此时服务端进入 SYN_RCVD 状态
  客户端收到服务端的 ACK 和 SYN 后，会对这个 SYN 进行确认，然后发送给服务器。此时客户端进入 ESTABLISHED 状态，服务端收到 ack 后随即进入 ESTABLISHED 状态，握手完成。

  自连接

  在一台主机里，一个进程监听一个端口进行连接请求，然后请求的主机刚好又是自己，就自己连接上了自己。

- 四次挥手

  客户端发送一个 FIN 序列号给服务器表示主动请求断开连接，此时客户端的状态为 FIN_WAIT-1
  服务器收到客户端的 FIN，先对这个 FIN 进行确认，此时服务器进入 CLOSE_WAIT 状态
  然后稍等一个定时器后服务器再回复一个 FIN 给客户端，此时服务器进入 LACK_WAIT 状态
  客户端收到服务器的 ACK 后进入 FIN_WAIT-2 状态，接着收到 FIN 后，对这个 FIN 进行确认就进入 WAIT_TIME 状态，最后等待 2 个 MSL 计时器后进入 CLOSED 状态，服务器收到确认号进入 CLOSED，连接关闭。

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

- HTTP/2.0：2015 年正式推出，支持二进制帧的机制，将请求和响应分成多个传输帧。引入多路复用的概念，可以在同一个连接并行的传输，解决了 1.X 的对头阻塞问题。支持服务器推送，引入了头部压缩机制，减少了冗余头部信息量的传输，并且强制使用了 HTTPS 进行加密传输。

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

HTTPS 本质上是套了一层 TLS/SSL 壳的 HTTP 协议，不是一个新的协议结构，主要是为了使连接更安全，用于为数据的传输提供加密、身份验证和保持数据的完整性包含。HTTPS 在 HTTP 的的基础上加了一个 TLS 层负责数据的加密和解密，使数据传输可以基于 TLS 上传输，确认数据的安全性。

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