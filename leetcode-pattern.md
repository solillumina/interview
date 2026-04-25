# Backtrack

- เลือก choice หนึ่ง
- เพิ่ม choice เข้า current state/path
- recurse เพื่อไปต่อ
- undo choice เพื่อย้อนกลับมาลอง choice อื่น

```js
backtrack(path, choices) => {
  if (isComplete(path)) {
    result.push([...path]);
    return;
  }

  for (const choice of choices) {
    if (!isValid(choice, path)) continue;

    path.push(choice); // Add choice
    backtrack(path, choices); // Recursive process new choices
    path.pop(); // Undo choice if necessary
  }
};

```

# Binary Search

- ค้นหาแบบตัดตัวเลือกทีละครึ่งนึงโดยข้อมูลต้อง sort แล้ว
- มี pointer ซ้าย `left` และขวา `right`
- หา `mid`
- ถ้าเจอ target ให้ return index
- ถ้า `nums[mid] < target` แปลว่า target อยู่ฝั่งขวา
- ถ้า `nums[mid] > target` แปลว่า target อยู่ฝั่งซ้าย

```js
binarySearch(nums, target){
  let left = 0;
  let right = nums.length - 1;

  while (left <= right) {
    const mid = Math.floor((left + right) / 2); // หา mid

    if (nums[mid] === target) return mid;
    if (nums[mid] < target) left = mid + 1; // พบว่าข้อมูลที่ต้องการค้นหาควรจะอยู่ครึ่งขวา
    else right = mid - 1; // พบว่าข้อมูลที่ต้องการค้นหาควรจะอยู่ครึ่งซ้าย
  }

  return -1;
};
```

# Monotonic Stack

ใช้ stack ที่เพื่อประมวลผลล่วงหน้า และต้องย้อนกลับมาประมวลผลก่อนหน้า

- loop array ทีละตัว
- while stack ยังไม่ empty และ value ปัจจุบันทำให้ order พัง ให้ pop
- ใช้ค่าที่ pop เพื่อคำนวณ answer
- push index ปัจจุบันเข้า stack

```js
monotonicStack(nums){
  const stack = [];

  for (let i = 0; i < nums.length; i++) {
    while (stack.length > 0 && nums[i] > nums[stack[stack.length - 1]]) {
      const index = stack.pop();
      // ...process...
    }

    stack.push(i);
  }
};
```

# Two Pointers

ใช้ pointer 2 ตัวเพื่อประมวลผลแบบ scan โดยการเลื่อน index ทั้งซ้าย และขวา

```js
twoPointer(nums, target){
  let right = nums.length - 1;

  while (left < right) {
    if (condition(nums, left, right)) left++;
    else right--;
  }
};
```

# DFS (Depth-First Search)

ใช้ traverse graph/tree

- เริ่มจาก node หนึ่ง
- mark ว่า visited
- ไป children ต่อทีละตัวแบบลึกที่สุดก่อน
- ถ้าไปต่อไม่ได้ ค่อยย้อนกลับ

```js
dfs(node){
  if (seen(node)) return;

  visit(node);

  for (const childNode of node.children) {
    dfs(childNode);
  }
};
```

# DP (Dynamic Programing)

ใช้กับโจทย์ที่สามารถแตกเป็น subproblems ซ้ำๆ เพื่อหาคำตอบที่เป็นไปได้ทั้งหมดก่อนจะเลือกคำตอบที่ดีที่สุด

- นิยาม state: `dp[i]` หมายถึงอะไร
- กำหนด base case
- เขียน transition formula
- เติมค่า dp ตามลำดับที่ถูกต้อง
- return answer

```js
dp(n){
  // ใช้ n เมื่อ state เท่ากับ index เริ่มจาก index 0 และใช้ผลลัพธ์อยู่ที่ index ได้เลย
  // ใช้ n + 1 เมื่อใช้ n state และต้องการ state n + 1 เพื่อแก้ปัญหา
  // อาจจะต้องใช้ multi dimension array ในการแก้ปัญหา
  const dp = Array(n + 1).fill(0);
  dp[0] = baseValue()

  for(let i = 1; i <= n; i++){
    dp[i] = relation(dp[i], dp[i - 1])
    // ...other relation etc...
  }

  return dp[n - 1]
}
```
