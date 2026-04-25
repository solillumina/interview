# Fraction Recuring

```js
fractionToDecimal(a, b) {
  if (a === 0) return "0";

  let result = "";

  if ((a < 0) !== (b < 0)) // Find correct sign
    result += "-";

  let numerator = Math.abs(a);
  let denominator = Math.abs(b);
  result += Math.floor(numerator / denominator);
  let remainder = numerator % denominator;

  if (remainder === 0) return result; // หารได้เศษ 0

  result += ".";
  const seen = new Map();

  while (remainder !== 0) {
    if (seen.has(remainder)) {
      const index = seen.get(remainder);
      result = result.slice(0, index) + "(" + result.slice(index) + ")";

      break;
    }

    seen.set(remainder, result.length);

    remainder = remainder * 10;
    result = result + Math.floor(remainder / denominator);
    remainder = remainder % denominator;
  }

  return result;
}
```

# Longest palindrome

```js
longestPalindrome(s){
  let start = 0;
  let end = 0;

  const expand = (left, right) => { // * ขยายซ้ายขวาพร้อมกัน
    while (left >= 0 && right < s.length && s[left] === s[right]) {
      left--;
      right++;
    }

    return right - left - 1;
  };

  for (let i = 0; i < s.length; i++) {
    const odd = expand(i, i); // กรณี palindrome แบบมีตัวอักษรตรงกลางตัวเดี่ยวเช่น aba
    const even = expand(i, i + 1); // กรณี palindrome แบบมีตัวอักษรตรงกลางแบบคู่เช่น abba
    const len = Math.max(odd, even);

    if (len > end - start + 1) {
      start = i - Math.floor((len - 1) / 2); // แปลง len เป็น start ให้รองรับทั้งกรณี odd และ even
      end = i + Math.floor(len / 2);
    }
  }

  return s.slice(start, end + 1); // end + 1 เพราะต้องการ character ตรง end index ด้วย
};
```

# Floyd's Tortoise and Hare

กรณี array และ array มี cycle (value เป็นค่า index)

```js
findDup(nums){
  let slow = nums[0]
  let fast = nums[0]

  do{ // Detect cycle
    slow = nums[slow]
    fast = nums[nums[fast]]
  }while(slow !== fast)

  slow = nums[0] // reset slow
  while(slow !== fast){ // loop ค้นหาค่าซ้ำ
    slow = nums[slow]
    fast = nums[fast]
  }

  return slow
}
```
