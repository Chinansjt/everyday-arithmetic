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

## 3、合并两个数组

## 给定两个有序整数数组 list1 和 list2， list2 合并到 list1 中，使得 list1 成为一个有序数组。

**解法思路**：使用 while 循环，取 list1 和 list2，的第一项进行比较，小的 push 到一个新的数组中，当 list1 或 list2 的其中一项循环结束后，将另一项合并到新数组中，可以使时间复杂度为O(m + n)

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
