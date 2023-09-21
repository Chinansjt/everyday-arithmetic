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

## 给定一个整数数组 nums 和一个目标值 target，请你在该数组中找出和为目标值的那两个整数，并返回它们的索引（索引从 0 开始）。

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

## 给定两个有序整数数组 list1 和 list2， list2 合并到 list1 中，使得 list1 成为一个有序数组。

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

## 给你一个整数 x ，如果 x 是一个回文整数，返回 true ；否则，返回 false 。回文数是指正序（从左向右）和倒序（从右向左）读都是一样的整数。

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

## 编写一个函数来查找字符串数组中的最长公共前缀。如果不存在公共前缀，返回空字符串 ""。

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

## 给定一个只包括 '('，')'，'{'，'}'，'['，']' 的字符串 s ，判断字符串是否有效。

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
  //当遍历完整个字符串，栈是空是，则构成有效括号
  return bracket.length === 0;
}
```

## 8、删除有序数组中的重复项

## 给你一个 非严格递增排列 的数组 nums ，请你 原地 删除重复出现的元素，使每个元素 只出现一次 ，返回删除后数组的新长度。元素的 相对顺序 应该保持 一致 。然后返回 nums 中唯一元素的个数。

**解法思路**：删除数组中重复的元素，但又要保持相对顺序不变，可以使用双指针的方法。定义一个指针，从1开始，因为第一个元素一定是唯一的，然后依次遍历数组，当遍历的元素和指针不相等时，则为不重复数组，将元素移到以指针下标的位置中（往前移），然后指针累加。依次重复上述步骤，当遍历完数组，指针的大小就是不重复项元素的长度

```javascript
//代码实现
function removeDuplicates(nums) {
  if(nums.length === 0) {
    return 0
  }
  let uniqueCount = 1 //指针的长度
  for(let i = 1; i < nums.length; i++) {
    //将当前项与前一项相比，不相等就是不重复的
    if(nums[i] !== nums[i-1]) {
      //往前移
      nums[uniqueCount] = nums[i]
      uniqueCount++
    }
  }
  return uniqueCount
}
```
