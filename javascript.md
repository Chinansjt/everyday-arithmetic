# 笔记

### 这里记录我在学习过程中记的笔记，方便我日后复习巩固

# 1、JS 函数

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
   > 缺点：由于不使用原型链，使用无法继承原型链上的属性和方法；只能继承父类实例的属性，不能继承父类实例的方法；每个实例都保存父类实例函数的副本，无法实现共用，影响性能！
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
