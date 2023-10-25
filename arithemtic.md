## 1、翻转字符串

**解法思路**：首先使用 split 将字符串分割成数组，在使用数组的 reverse 方法将字符串反转，最后用 join 将字符串拼接回字符串

```javascript
//代码实现
function reverseString(string) {
  if (!string) return "";
  const reverseStr = string.split("").reverse().join("");
  return reverseStr;
}
```

## 2、两数之和

### 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回它们的索引（索引从 0 开始）。

**解法思路**：创建一个 hash map 键值对象，通过计算 target 和其中一个的差值，判断 map 中有没有对应的差值，如果有就直接返回，没有就将相减的值存到 map 中，方便下次遍历

```javascript
//代码实现
function twosSum(nums, target) {
  let numsHashMap = new Map();
  for (let i = 0; i < nums.length; i++) {
    const surplus = target - nums[i];
    //判断map中是否已经存在对应的键值
    if (numsHashMap.has(surplus)) {
      return [numsHashMap.get(surplus), i];
    }
    //map中还没保存相减的键值，保存到map中下次遍历时，有就直接返回，存储的key为nums的值，而不是nums的索引
    numsHashMap.set(nums[i], i);
  }
  return null;
}
```

## 3、合并两个数组（有序）

### 给定两个有序整数数组 list1 和 list2， list2 合并到 list1 中，使得 list1 成为一个有序数组。

**解法思路**：使用 while 循环，取 list1 和 list2，的第一项进行比较，小的 push 到一个新的数组中，当 list1 或 list2 的其中一项循环结束后，将另一项合并到新数组中，可以使时间复杂度为 O(m + n)

```javascript
//代码实现
function masterTowsLists(list1, list2) {
  const masterList = [];
  let i = 0;
  let j = 0;
  while (i < list1.length && j < list2.length) {
    if (list1[i] < list2[j]) {
      masterList.push(list1[i]);
      i++;
    } else {
      masterList.push(list2[j]);
      j++;
    }
  }
  //将剩余项合并到新数组中
  while (i < list1.length) {
    masterList.push(list1[i]);
    i++;
  }
  while (j < list2.length) {
    masterList.push(list2[j]);
    j++;
  }
  return masterList;
}
```

## 4、续上第三题（合并两个数组），加上额外的限制条件，当传入的两个数组是无序时，合并两个数组

**解法思路**：将传入的两个无序数组合并为一个有序数组，其实可以看作为数组排序，有很多种方法可以实现，下面就用快速排序来实现。

```javascript
//代码实现
function quickSort(list) {
  if (list.length <= 1) {
    return list;
  }
  let pivot = list[0];
  let left = [];
  let right = [];
  //i从1开始算，因为0已经取值为基数pivot，如果从0开始累加会陷入递归死循环爆栈
  for (let i = 1; i < list.length; i++) {
    if (pivot < list[i]) {
      right.push(list[i]);
    } else {
      left.push(list[i]);
    }
  }
  return quickSort(left).concat(pivot, quickSort(right));
}
function masterTwosLists(list1, list2) {
  const masterList = list1.concat(list2);
  if (masterList.length === 0) {
    return [];
  }
  const list = quickSort(masterList);
  return list;
}
```

## 5、回文数

### 给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

**解法思路**：将数组转为字符串，使用头尾双指针比较正序和倒叙的字符串是否完全相同，当头指针大于尾指针时，循环中止。

```javascript
//代码实现
function isPalindrome(x) {
  // 负数不可能是回文数，直接返回 false
  if (x < 0) {
    return false;
  }

  // 将整数转化为字符串
  const str = x.toString();

  // 使用双指针法比较正序和倒序字符串是否相同
  let left = 0;
  let right = str.length - 1;

  while (left < right) {
    if (str[left] !== str[right]) {
      return false;
    }
    left++;
    right--;
  }

  return true;
}
```

## 6、最长公共前缀

### 编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。

**解法思路**：数组中的任意一项字符串作为基数比较，遍历数组中的每项一项和基数字符串进行比较，判断每一项的值是否包含基数，如不包含依次递减基数的长度，每次都更新基数的值

```javascript
//代码实现
function longestCommonPrefix(strs) {
  if (strs.length === 0) {
    return "";
  }
  //取一项作为基数
  let prefix = strs[0];

  for (let i = 1; i < strs.length; i++) {
    while (strs[i].indexOf(prefix) !== 0) {
      prefix = prefix.slice(0, prefix.length - 1);
      if (prefix === "") {
        return "";
      }
    }
  }

  return prefix;
}
```

## 7、有效的括号

### 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

**解法思路**：遍历传入的数组，利用栈的先进后出的特效，依次将左括号 push 到栈中，当遇到非左括号时，将栈中最后进入的项弹出来，如果和当前遍历的形成括号，那么就继续执行，否则不构成有效括号

```javascript
//代码实现
function isBracket(s) {
  if (s.length === 0) {
    return false;
  }
  const bracket = [];
  const bracketMap = { "(": ")", "{": "}", "[": "]" }; //建立括号的映射值
  for (let i = 0; i < s.length; i++) {
    //判断字符串项是否是括号映射的键（即是否是左括号）
    if (s[i] in bracketMap) {
      bracket.push(s[i]);
    } else {
      //如果栈中为空则没有左括号就先有右括号，不是有效括号
      //将栈中的最后一项弹出，和当前字符串项比较，构成一个括号就不做操作，不构成就不是有效括号
      if (bracket.length === 0 || bracketMap[bracket.pop()] !== s[i]) {
        return false;
      }
    }
  }
  //当遍历完整个字符串，栈是空栈，则构成有效括号
  return bracket.length === 0;
}
```

## 8、删除有序数组中的重复项

### 给你一个 非严格递增排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

**解法思路**：删除数组中重复的元素，但又要保持相对顺序不变，可以使用双指针的方法。定义一个指针，从 1 开始，因为第一个元素一定是唯一的，然后依次遍历数组，当遍历的元素和指针不相等时，则为不重复数组，将元素移到以指针下标的位置中（往前移），然后指针累加。依次重复上述步骤，当遍历完数组，指针的大小就是不重复项元素的长度

```javascript
//代码实现
function removeDuplicates(nums) {
  if (nums.length === 0) {
    return 0;
  }
  let uniqueCount = 1; //指针的长度
  for (let i = 1; i < nums.length; i++) {
    //将当前项与前一项相比，不相等就是不重复的
    if (nums[i] !== nums[i - 1]) {
      //往前移
      nums[uniqueCount] = nums[i];
      uniqueCount++;
    }
  }
  return uniqueCount;
}
```

## 9、搜索插入位置

### 给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。请必须使用时间复杂度为 O(log n) 的算法。

**解法思路**：当看到复杂度为 O(log N)时，可以下意识的想到二分查找法来解决

```javascript
//代码实现
function searchInsert(nums, target) {
  let left = 0;
  let right = nums.length - 1;
  while (left <= right) {
    const mid = Math.floor((left + right) / 2);
    if (nums[mid] === target) {
      return mid; // 找到目标，返回索引
    } else if (nums[mid] < target) {
      left = mid + 1; // 目标值在右半部分
    } else {
      right = mid - 1; // 目标值在左半部分
    }
  }
  return left; // 目标值不存在，返回插入位置
}
```

## 10、最后一个单词的长度

### 给你一个字符串 s，由若干单词组成，单词前后用一些空格字符隔开。返回字符串中 最后一个 单词的长度。

**解法思路**：设定一个是否是单词尾部的标识，字符串尾部开始循环，当第一次遇到非 '' 时，将表示设置为 true, 然后将单词长度累加，直到遇到下一个 ''。

```javascript
//代码实现
function lastWorkLength(s) {
  let sLen = 0;
  let foundLastWord = false;
  for (let i = s.length; i >= 0; i--) {
    if (s[i] !== "") {
      //遇到非空格的字符串，长度 + 1，并且表示已经找到末尾单词
      sLen++;
      foundLastWord = true;
    } else if (foundLastWord) {
      //当已经找到末尾单词，再遇到空字符串，则表示单词已经找到，终止循环
      break;
    }
  }
  return sLen;
}
```

## 11、加一

### 给定一个由 整数 组成的 非空 数组所表示的非负整数，在该数的基础上加一。最高位数字存放在数组的首位， 数组中每个元素只存储单个数字。

**解法思路**：定义一个表示进位的值，初始化为 1，从数组最后一位开始遍历，每次加上进位的值，然后更新进位的值，当没有进位时直接返回数组，如果遍历完了数组，则表示该数组全部为 0，你们在头部添加一个进位 1，然后返回数组

```javascript
//代码实现
function plusOne(digits) {
  let carry = 1; //初始化进位为1
  for (let i = digits.length - 1; i >= 0; i--) {
    const sum = digits[i] + carry; //当前的位数加上进位，默认首次是加1
    digits[i] = sum / 10; // 取余数作为当前的值
    carry = Math.floor(sum / 10); //更新进位，如果值为1表示还有进位，还需要继续加
    if (carry === 0) {
      //没有进位了直接返回数组
      return digits;
    }
  }
  //遍历完后，数组的值全为0，那么在数组首位添加一个进位1，
  digits.unshift(carry);
  return digits;
}
```

## 12、二进制求和

### 给你两个二进制字符串 a 和 b ，以二进制字符串的形式返回它们的和。

**解法思路**：定义一个进位，从字符串末尾遍历两个字符串，将当前位和进位相加，最后更新进位。这里主要的思想是，当两个字符串其中一个已经遍历完时，用 0 来代替，而当前位只和进位相加

```javascript
//代码实现
function addBinary(a, b) {
  let carry = 0; //定义一个进位
  let results = ""; //表示结果的字符串
  let i = a.length - 1;
  let j = b.length - 1;

  //当只要没有遍历完两个字符串，或存在进位，则继续循环
  while (i >= 0 || j >= 0 || carry > 0) {
    //如果两个字符串不相等，用0来代替当前相加的位
    const digitA = i >= 0 ? parseInt(a[i]) : 0;
    const digitB = j >= 0 ? parseInt(b[j]) : 0;

    const sum = digitA + digitB + carry; //相加
    carry = Math.floor(sum / 2); //更新进位
    results = (sum % 2) + results;

    i--;
    j--;
  }
  return results;
}
```

## 13、 x 的平方根

### 给你一个非负整数 x ，计算并返回 x 的 算术平方根 。由于返回类型是整数，结果只保留 整数部分 ，小数部分将被 舍去 。

**解法思路**：使用二分法查找，根据公式 m = (x - (b - a)/2) 计算中间值，不断的逼近正确的平方根值

```javascript
//代码实现
function mySqrt(x) {
  if (x < 2) return;

  let a = 1;
  let b = Math.floor(x / 2);
  let result = 0;
  while (a <= b) {
    const mid = Mth.floor(a - (b - a) / 2); //计算中间值
    const sqrt = mid * mid; //计算平方值

    if (sqrt === x) {
      return mid;
    } else if (sqrt < x) {
      a = mid + 1;
      result = mid;
    } else {
      b = mid - 1;
    }
  }
  return result;
}
```

## 14、手动实现一个 trim 方法

### 实现一个方法传入一个字符串，将首尾非字符串的字符给去掉

**解题思路**：定义首尾两个指针，遍历字符串，直到遇到字符串为止，然后根据指针的下标截取正确的字符串

```javascript
//代码实现
function handleWriteTrim(s) {
  if(s.length < 1) }{
    return s
  }
  let left = 0
  let right = s.length - 1

  //从前往后匹配，当遇到非空字符串停止
  while(left < right && /\s/.test(s[left])) {
    left++
  }
  //从后往前匹配，当遇到非空字符串停止
  while(left < right && /\s/.test(s[right])) {
    right--
  }

  const result = s.substring(left, right - 1)
  return result
}
```

## 15、大数相加

### 在 JS 中，一般的数字相加都可以用 + 号完成，但是当数字大到一定程度的时候，超出了 JS 的表示范围，那么用+相加就会超出数字的表示范围，因此可以用字符串代替，运用简单的加法得出结果。

**解题思路**：定义两个指针和进位值，从字符串的尾部开始遍历，依次相加，当存在进位时，把进位加上。

```javascript
//代码实现
function addString(num1, num2) {
  let i = num1.length - 1;
  let j = num2.length - 1;
  let carry = 0;
  let result = "";
  while (i >= 0 || j >= 0 || carry >= 1) {
    const digit1 = num1[i] ? parseInt(num1[i]) : 0;
    const digit2 = num2[j] ? parseInt(num2[j]) : 0;
    const sum = digit1 + digit2 + carry;
    carry = Math.floor(sum / 10);
    result = Math.floor(sum % 10) + result;
    i--;
    j--;
  }
  return result;
}
```

## 16、扁平化数组

### 请你编写一个函数，它接收一个 多维数组 arr 和它的深度 n ，并返回该数组的 扁平化 后的结果。多维数组 是一种包含整数或其他 多维数组 的递归数据结构。

**解题思路**：扁平化数组有很多种方法，最简单的方法是使用 Array 的内置 flat 函数进行扁平化数组，flat 接受两个参数，第一个参数是需要扁平化的数组，第二个参数是需要扁平化的深度，并返回已经扁平化的数组。另一种方法是使用递归的方法，在递归中循环遍历传入的数组，判断当前项是否是数组，如果是数组并且深度不大于指定深度则继续递归调用。否则将当前项 push 到结果集中

```javascript
//代码实现
function flat(arr, n) {
  const result = [];

  function flatArray(arrs, deep) {
    for (let i = 0; i < arrs.length; i++) {
      if (Array.isArray(arrs[i]) && deep < n) {
        flatArray(arrs[i], deep + 1);
      } else {
        result.push(arrs[i]);
      }
    }
  }
  flatArray(arr, 0);
  return result;
}
```

## 17、千分位分割符

### 给你一个整数 n，请你每隔三位添加点（即 "." 符号）作为千位分隔符，并将结果以字符串格式返回。

**解题思路**：定义一个计数器，然后将整数 n 转为字符串再遍历它，每次遍历都将计算器加一，当计算器达到 3 是，表明已经是千分位了，将 . push 到结果集中，在将计算器重置为 0

```javascript
//代码实现
function thousandSeparator(n) {
  const str = n.toString();
  const result = [];

  for (let i = str.length - 1, count = 0; i > 0; i--) {
    if (count === 3) {
      result.unshift(".");
      count = 1;
    } else {
      count++;
    }
    result.unshift(str[i]);
  }

  return result.join("");
}
```

## 18、质数（素数）

### 给一个整数 n，找出这个整数内的所有质数

**解题思路**：使用暴力解法，定义一个方法 isPrimes 判断一个数是否是质数，然后循环 n，在 n 的循环中调用 isPrimes 方法，当返回 true 时则质数的数量+1

```javascript
//代码实现
function countPrimes(n) {
  let count = 0;
  //判断是否是质数
  function isPrimes(num) {
    for (let i = 2; i < Math.sqrt(num); i++) {
      if (num % i === 0) {
        return false;
      }
    }
    return true;
  }

  for (let i = 2; i < n; i++) {
    if (isPrimes(i)) {
      count++;
    }
  }
  return count;
}
```

## 19、实现防抖和节流函数

### 防抖函数：在给定的时间内，连续触发多次都会往后延迟，比如说你 1 秒内触发了 1 次，在 0.8 秒后又触发了一次，那么在第二次触发后就会往后延迟一秒钟执行

### 节流函数：在给定的时间内不管触发多少次都会按第一次来的触发的来执行

**解题思路**：节流函数：利用时间搓来实现，定义一个上一次执行时间的标识，在每次执行时，获取当前时间，当两次间隔大于给定的间隔时则执行操作；防抖函数：利用定时器实现，定义一个定时器，当定时器已经存在时，清楚定时器，重新定时，当后续没有操作时，定时器的时间到了，就会执行操作了。

**适用场景**：节流函数适用于表单的提交，滚动加载等场景；防抖函数适用于 input 输入，浏览器的 resize、scroll 等操作；

```javascript
//代码实现
//防抖函数实现
function debounce(fun, delay) {
  let timer;
  return function (...args) {
    if (timer) clearTimerOut(timer);

    timer = setTimeOut(() => {
      fun.apply(this, args);
    });
  };
}

//节流函数实现
function throttle(fun, delay) {
  let lastRunTime = 0;
  return function (...args) {
    let now = Date.now();
    if (now - lastRunTime >= delay) {
      fun.apply(this, arg);
      lastRunTime = now;
    }
  };
}
```

## 20、实现一个 shallowClone、deepClone

### 深拷贝是指当我们复制一个引用类型时，我们修改被复制或复制出来的对象时，另一个对象不会被修改，我们复制引用类型时，是在堆内存只能开辟一个新内存了存储新对象，修正引用类型时，就可以彼此不受影响。一般来说，我们复制一个引用类型，我们只是复制该对象的一个指针，已复制的对象是指向被复制对象的存储地址。

**解题思路**：实现一个深拷贝有很多中方法，一般用两种方法：1、使用 JSON 来将对象转为字符串再序列化这个字符串,但是这个方法没办法复制方法和正则，复制方法会返回 null，而复制正则会返回 null；2、是使用递归执行深拷贝；

```javascript
//代码实现
//实现shallowClone，有很多中方式，直接赋值、使用Object.assign()、使用展开运输符...等
let a = {name: 'ning', regExp: /\s/, siyName: function() {
  console.log(this.name)
}}

let b = a; //使用=
let c = Object.assign({}, a); //使用Object.assign()
let d = {...a}; //使用...

//实现deepClone，用JSON。缺点是无法拷贝方法和正则表达式，会返回null和{}
function jsonDeepClone(obj) {
  return JSON.parse(JSON.stringify(obj))
}

//实现deepClone，用递归，最简单的版本
function recursionDeepClone1(obj) {
  if(typeOf obj === 'object') {
    let newObj = Array.isArray(obj) : [] : {}
    for(const key in obj) {
      newObj[key] = recursionDeepClone1(obj[key])
    }
    return newObj
  } else {
    return obj
  }
}

// 但我们观察这个代码，是实现了深拷贝功能，但是如果存在循环引用的值就会让递归爆栈从而造成死循环状态，造成这个原因主要是因为拷贝的对象间接或直接的引用了自己，如：
a.newA = a // 这个在a的属性里又存在一个属性跟a一样

//解决这个问题我们可以使用map，用 key-value的方式存储循环引用的值，如果在内存中已经存在了这个对象，则直接取出来返回，如果没有则将值存入map中

function recursionDeepClone2(obj, map = new Map()) {
  if(typeOf obj === 'object') {
    let newObj = Array.isArray(obj) ? [] : {}
    if(map.get(obj)) {
      return map.get(obj)
    }
    map.set(obj, newObj)
    for(const key in obj) {
      newObj[key] = recursionDeepClone2(obj[key], map)
    }
    return newObj
  } else {
    return obj
  }
}

//在这里我们使用map的来解决问题，但是这样又会存在一个问题，因为map是强引用的值，当我们需要拷贝的对象过于庞大的时候，就会消耗很大的内存，当我们不使用这个引用的时候还会存在于我们的内存中，如

//这里我们创建了一个mapObj对象，当我们set了map后，将该值设为null，理论上我们需要在内存中清除这个maoObj,在mapTag中也应该消除，而map是强引用的，使用我们无法达到这个目的，从而让垃圾回收机制自动回收内存。
let mapObj = { name: 'ning' };
let mapTag = new Map()
mapTag.set(mapObj, 'myName')
mapObj = null
console.log(mapTag) // 会打印maoObj相关的内容

//使用weakMap就可以解决这个问题
let mapObjW = { name: 'ning' };
let mapTagW = new WeakMap()
mapTagW.set(mapObjW, 'myName')
mapObjW = null
console.log(mapTagW) // 内存已经释放，对应空值

//因此我们可以改map为weakMap

function recursionDeepClone3(obj, map = new WeakMap()) {
  //代码实现
}

//在上述中，因为很好的处理的深拷贝功能，但是还是会存在着其他问题，比如如果拷贝的对象存在Date、RegEx等对象，因此我们需要兼容这些对象

//最终代码，虽然还是无法很完善的兼容所有问题，但是已经具体的实现了这个深拷贝功能
function recursionDeepClone4(obj, map = new WeakMap()) {
  if (obj === null) return obj;
  if(obj instanceof Date) return new Date(obj)
  if(obj instanceof RegEx) return new RegEx(obj)
  if(typeof obj === 'object') {
    let newObj = Array.isArray(obj) ? [] : {}
    if(map.get(obj)) {
      return map.get(obj)
    }
    map.set(obj, newObj)
    for(const key in obj) {
      newObj[key] = recursionDeepClone4(obj[key], map)
    }
    return newObj
  } else {
    return obj
  }
}
```

## 21、手写实现 Promise.all()和 Promise.race()

### Promise.all()和 Promise.race() 是 promise 异步编程的一个方法，接受一个可迭代的数组，数组的项是一个 promise 的实例；二者的区别在于，all 是必须等数组里的 promise 实例必须执行完并且都是 resolve 状态才会执行，返回的值是一个数组，对应每一项的 resolve 值，当遇到任意一项是 reject 时，则停止执行，返回 reject 的值；而 race 则是当其中的实例有一个最先完成的状态，不管是 resolve 还是 reject 都会返回其中的值。

```javascript
//代码实现
Promise.all = function (iterators) {
  return new Promise((resolve, reject) => {
    if (!iterators || iterators.length === 0) {
      resolve([]);
    }
    let result = [];
    let count = 0;
    for (let item of iterators) {
      Promise.resolve(item)
        .then((value) => {
          result.push(value);
          if (++count === iterators.length) {
            resolve(result);
          }
        })
        .catch((value) => {
          reject(vale);
        });
    }
  });
};

Promise.race = function (iterators) {
  return new Promise((resolve, reject) => {
    if (iterators.length === 0) {
      return;
    }
    for (let item of iterators) {
      Promise.resolve(item).then(
        (value) => {
          resolve(value);
        },
        (value) => {
          reject(value); //这里使用第二个参数调用reject是因为执行了当前这个promise实例，就能直接知道实例的状态，如果是放在catch中获取，就会造成要等待其他的实例执行完成才会执行，可能会造成结果不准确。
        }
      );
    }
  });
};

let proArray = [Promise.resolve(1), Promise.reject(2), Promise.resolve(3)];
Promise.all(proArray).then((res) => {
  console.log(res); // Uncaught (in promise) 2
});
Promise.all(proArray).then((res) => {
  console.log(res); // [1,2,3]
});
```
## 22、手写实现Promise
### 实现了最基础的版本，没有解决then回调地狱的问题

```javascript
//代码实现
class MyPromise {
  constructor(executer) {
    this.state = 'pending'
    this.value = null
    this.reason = null
    this.onResolves = []
    this.onRejects = []

    let resolve = (value) => {
      if(this.state === 'pending') {
        this.state = 'finished'
        this.value = value
        this.onResolves.foreach((fun) => fun(value))
      }
    }
    let reject = (value) => {
      if(this.state === 'pending') {
        this.state = 'rejected'
        this.reason = value
        this.onRejects.foreach(fun => fun(value))
      }
    }
    try {
      executer(resolve, reject)
    } catch(error) {
      reject(error)
    }
  }
  then(onResolveFun,onRejectFun) {
    if(typeof onResolveFun === 'function' && this.state === 'finished') {
      onResolveFun(this.value)
    }
    if(typeof onRejectFun === 'function' && this.state === 'rejected') {
      onRejectFun(this.reason)
    }
    if(this.state === 'pending') {
      if(typeof onResolveFun === 'function') {
        this.onResolves.push(onResolveFun)
      }
      if(typeof onRejectFun === 'function') {
        this.onRejects.push(onRejectFun)
      }
    }
  }
  catch() {
    return this.then(null, onRejectFun)
  }
}
```

## 23、手写实现 new、bind、call、apply

### new 是用于调用构造函数的，当用 new 调用的函数自动识别为构造函数。返回一个构造函数的实例。内部执行步骤如下：

> - 创建一个新的空对象
> - 给这个新对象的原型设置为传入构造函数的原型对象
> - 绑定新对象的 this 为构造函数的 this，并调用构造函数
> - 如果构造函数返回一个对象则返回构造函数的对象，如果返回的是非对象则返回新创建的对象；这一步是因为如果在构造函数内部显示的返回一个对象，则 new 中直接返回这个对象，否则就通过 new 创造一个新对象

```javascript
//代码实现
function myNew(Constructor, ...args) {
  let obj = {};
  obj.__proto__ = Constructor.prototype;
  let result = Constructor.apply(obj, args);

  return typeof result === "object" && typeof result !== "null" ? result : obj;
}
```

### bind 用于绑定 this 的指向，更改函数的上下文，执行 bind 后函数并不会立即执行，需要再次调用。

```javascript
//用法实例
let person = {
  name: "ning",
  sayName: function () {
    console.log(`my name is ${this.name}`);
  },
};
//这里调用sayName，里面的this是指向person的，因为this的指向是在调用时确定的，谁调用它this就指向谁。这里是person调用它，所以指向person。
person.sayName(); // my name is ning
//这用调用sayName，里面的this是指向window，因为这这里并不是属于person调用它，而是它以一个属性传递给setTimeout，等同于
// let fun = person.sayName, 然后调用fun，而调用fun是全局window调用的。
setTimeout(person.sayName, 1000); // my name is undefine
let bindSay = person.sayName.bind(person); //将this绑定为person
setTimeout(bindSay, 1000); // my name is ning
```

#### 手写实现步骤

> - 返回一个函数
> - 返回的函数可以有参数
> - 确保正确的 this 上下文
> - 考虑到使用 new 构造函数时的情况

```javascript
//代码实现
Function.prototype.myBind = function (context, ...args1) {
  let that = this; //保存内部的this
  //args2是在调用的时候可能会传入参数
  function Bound(...args2) {
    // 当是构造函数调用时，this 指向实例，that指向绑定的函数，结果为true，将绑定函数的this绑定为实例的this
    // 而当是作为普通函数调用时，this指向window，that指向绑定的函数，结果为false，将绑定函数的this绑定为传入的context上下文。
    let isNew = this instanceof that;
    return that.apply(isNew ? this : context, args1.concat(args2));
  }
  //Temp方法的主要作用就是重新创建一个新的、干净的原型复制给Bound，这样在原方法改变的情况下，调用的不好出现问题。
  //定义一个新方法，用于继承原型。这里，我们定义了一个临时的构造函数 Temp。它的目的是仅仅作为一个桥梁，将原始函数的原型链接到绑定函数的原型上，而不会调用原始函数。
  function Temp() {}
  // 这里，我们将 Temp 的原型设置为原始函数（也就是 bind 方法被调用的函数）的原型。this 在这里指的是我们想要绑定的原始函数。
  Temp.prototype = this.prototype;
  //我们创建了 Temp 的一个新实例，并将这个新实例作为绑定函数（Bound）的原型。这意味着，现在，通过绑定函数创建的任何新对象都将从原始函数的原型继承属性。
  Bound.prototype = new Temp();
  // 这种方法的好处是，我们没有直接创建原始函数的新实例作为绑定函数的原型，因为那样可能会有不必要的副作用（因为原始函数的构造函数代码会被执行）。通过使用空的 Temp 函数，我们只是创建了一个原型链，而不会执行任何额外的代码。
  return Bound;
};
```

### call和apply都是用于绑定this的指向，只是接收的参数不同。手写实现如下(apply一样实现，只是调用时传入的参数格式不一样)
> * 获取传入的上下文，如果为null或者undefine则指定为window全局对象
> * 用Symbol标识一个唯一的方法，目标是不会修改或者覆盖原本传入的上下文this对象
> * 然后执行该方法，并获取返回值。在这个内部实现中，`this`是指向调用它的函数，也就是说是等于调用它的函数，比如 `fun.call()`, 这里的`this`就是等于`fun`的，使用如果是`this()`其实可以视为`fun()`调用
> * 返回返回值

```javascript
//代码实现
Function.prototype.myCall = function(context, ...args) {
  if(typeof this !== 'function' ) {
    throw new TypeError('Called object is not a function');
  }
  context = context || window

  let funSymbol = Symbol('fun')
  context[funSymbol] = this

  let result = context[funSymbol](...args)
  delete context[funSymbol]
  return result
}
```

## 24、js控制并发数
```javascript
//代码实现
async function asyncPool(limit, arrayFun) {
  let resolve = []
  let executing = []
  for(let item of arrayFun) {
    const p = item().then((val) => {
      resolve.push(val)
      executing.splice(executing.indexOf(p), 1)
    })
    executing.push(p)
    //当执行的队列任务数超过并发控制数时，并发控制执行
    if(executing.length >= limit) {
      //最先执行完成的继续执行
      await Promise.race(executing)
    }
  }
  //等待所有的任务执行成功
  await Promise.all(executing)
  return resolve
}
```

## 25、js实现实现二叉树的层序遍历; 给定一个二叉树的根节点 root ，返回其节点值的 层序遍历 。 （即逐层地，从左到右访问所有节点）。


```javascript
//代码实现
function levelOrder(root) {
  if(!root) return []

  let queue = [root]
  let result = []

  while(queue.length > 0) {
    let currentNode = []
    let queueSize = queue.length
    for(let i = 0; i < queueSize; i++) {
      let node = queue.shift()
      currentNode.push(node.val)
      if(node.left) queue.push(node.left)
      if(node.right) queue.push(node.right)
    }
    result.push(currentNode)
  }
  return result
}
```
