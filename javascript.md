# 笔记

### 这里记录我在学习过程中记的笔记，方便我日后复习巩固

# 1、JS 数组
* **push()**：在数组的尾部插入一个或多个元素，返回插入后数组的长度。
* **pop()**：在数组的尾部删除一个元素，返回被删除的元素。
* **shift()**：在数组的头部删除一个元素，返回被删除的元素。
* **unshift()**：在数组的头部插入一个或多个元素，返回插入后数组的长度
* **concat()**：组合数组，返回一个新数组。
* **join()**：拼接数组。
* **fill()**：填充数组。
* **forEach()**：对数组执行一个for循环，无返回操作。
* **map()**：对数组执行一次遍历，返回在执行方法过程中返回的元素集。
* **filter()**：对数组执行过滤操作，返回过滤条件中为true的元素。
* **some()**：对数组执行遍历操作，当存在至少一项是满足特定条件的时候，返回true
* **every()**：对数组执行遍历操作，当所有项都满足特定条件的时候，返回true
* **find()**：从数组头部执行遍历操作，当发现第一项满足特定条件的时候，返回这个元素。
* **findIndex()**：从数组头部执行遍历操作，当发现第一项满足特定条件的时候，返回这个的索引
* **isArray()**：判断是否是数组。
* **splice()**：对数组进行添加、删除、替换操作，返回被删除的元素。
* **slice()**：对数组进行截取操作，返回截取的元素。
* **includes()**：判断数组是否包含给定的项。
* **indexOf()**：返回数组第一个匹配到的索引。
* **lastIndexOf()**：返回数组最后一个匹配到的索引。
* **sort()**：对数组进行排序，默认情况下按字母顺序进行排序，但是如果传入一个方法，可以根据方法返回的值继续排序。返回1排在后面，返回-1排在前面，返回0排序不变。
* **reverse()**：对数组进行翻转操作。
* **reduce()**：对数组从左到右进行累加操作，接受两个参数，第一个是一个回调方法，第二个可选，表示累加的初始值，不传默认数组第一项。
* **reduceRight()**：对数组从右到左进行累加操作。
>其中会改变原数组的方法有push、pop、shift、unshift、splice、reverse、sort。
# 2、JS 对象

## 类型检测
**_解释_**：js类型检测指是判断一个变量的类型及构造方式，分为两种类型：基础类型和引用类型；其中基础类型包括String、Number、Boolean、undefine、null、symbol，引用类型包括Object、Function、Array。

1. **typeof**：一般用于检测基础类型，检测Object、null、array都会返回 'object'，检测Function会返回‘function’，
2. **instanceof**：用于检测引用类型，检测是否属于原型链上。
3. **contractor**：每个对象都有一个constructor属性，指向该对象的构造函数
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
   > 缺点：由于不使用原型链，使用无法继承原型链上的属性和方法；只能继承父类原型的属性，不能继承父类实例的方法；每个实例都保存父类实例函数的副本，无法实现共用，影响性能！
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
   //单共享的不受影响
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
            this.age = age
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
**总结**：JavaScript中的继承通常可以分为两大类：原型类继承和原型链类继承。原型类继承包括原型继承、寄生继承和寄生组合继承，而原型链类继承包括原型链继承、构造函数继承和组合继承。这两大类继承方式各自解决了不同的问题。

>**组合继承**：结合了原型链继承和构造函数继承的特点，克服了原型链继承中的属性污染问题，以及构造函数继承中无法共享父类原型上的方法的问题。

>**寄生组合继承**：寄生组合继承将组合继承进一步改进，解决了子对象修改父对象时对其他子对象产生影响的问题，同时避免了原型链中无法获取原型链上的属性和参数，以及不能传递参数给父对象的问题。寄生组合继承结合了各种继承方式的优点，是目前大多数库使用的高效继承方法。

# 3、迭代器和生成器

## 迭代器
**_解释_**：迭代器去一个对象，用于定义可以迭代的协议。对象是无法迭代的，因此可以手动定义一个迭代器，用for of迭代。

## 生成器
**_解释_**：生成器是es6的一个函数，和普通函数不同，生成器里面的代码不会一下子执行完，可以控制暂停和恢复执行。改函数返回一个生成器对象。

# 4、设计模式

## 工厂模型
**_解释_**：提供一个创建对象的接口，允许子类决定实例化哪一个类。工厂模型类似于开发一个超级产品，而开发这个产品的过程中，拆分成多种不同的生产线，我们把这些生产线就称为工厂模式的子类型，当需要构建一个新产品时，把这些生产线拿出来组装即可

## 原型模式
**_解释_**：通过克隆现有对象来创建新对象。原型模式更贴近于js编程思想，在js中，每个对象都有一个原型对象而这个原型对象指向父类的原型对象，当我们需要扩展这个对象时，可以通过原型去扩展。

## 单例模型
**_解释_**：确保一个类只有一个实例，并提供一个全局的访问点来访问这个实例。单例模型指的是一个对象只有一个示例对象，当我们多次new一个构造函数时，其结果都指向第一个new的对象，并且提供全局统一的方法去访问它，Vuex就运用了单例模型的思想。

## 适配器模型
**_解释_**：当一个接口返回的参数不是我们想要的参数时，我们可以封装一个方法去适配这个接口的返回参数，以达到我们想要的目的，这就是适配器模型；可以理解为电子产品中的转换器，如将use-b转换为type-c接口。

## 装饰器模型
**_解释_**：指在不改变原有对象的情况下，再对接口进行封装扩展，使其能满足更复杂的需求。

## 代理模型
**_解释_**：代码模型指是通过一个中间商对一个事件进行操作，如Proxy操作，A访问B，不能直接访问，而是通过访问C才能访问到B

## 观察者模型
**_解释_**：观察者模型定义了一个一对多的依赖关系，让多个观察者同时监听一个一个对象，当这个对象发生状态更新时，可以通知这些观察者，使它们能够及时更新。通常存在一个被称为“主题”的对象，它直接维护其观察者的列表，并在状态改变时直接通知它们。

## 发布-订阅模型
**_解释_**：和观察者模型类似，区别在于当对象发生变化时，观察者模型是主动去通知众多观察者更新状态；而发布-订阅模型是当对象发生变化不主动通知，而是发步到一个中转平台上，而观察者们去订阅这个平台的消息，当发生变更时就会通知到观察者及时更新状态。通常有一个中间层，即事件频道或消息代理。发布者发布消息到一个频道，而订阅者订阅那个频道来接收消息。发布者和订阅者通常不直接知道彼此的存在。


