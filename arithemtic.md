## 1、翻转字符串

**解法**：首先使用 split 将字符串分割成数组，在使用数组的 reverse 方法将字符串反转，最后用 join 将字符串拼接回字符串

```javascript
function reverseString(string) {
  if (!string) return "";
  const reverseStr = string.split("").reverse().join("");
  return reverseStr;
}
```
