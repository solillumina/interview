# Big O: rules

การลดรูป Big O notation การทำงานซ้อนกันใช้การคูณ การทำงานต่อจากกันใช้การบวก

- Drop constants (ไม่นับตัวคูณ และค่าคงที่)
  O(2n) -> O(n)
- Drop smaller terms (การทำงานต่อเนื่อง นับแค่ Big O มากที่สุด)
  O(n² + n) -> O(n²)
- Sequential loops add (การทำงานต่อเนื่อง นับแค่ Big O มากที่สุด)
  O(n) + O(n) -> O(n)
- Nested loops multiply (loop ซ้อน loop เอามาคูณกัน)
  O(n) \* O(n) -> O(n²)
- Different inputs use different names (ขนาดที่ต่างกันใช้ notation ที่ต่างกัน)
  O(n + m), not always O(n)
- Recursion space includes call stack (recursive ต้องนับ call stack ด้วย)

# Big O: Time Complexity

ดูจากจำนวนรอบที่ code ทำงานตามขนาด input

- O(1)
  ไม่ขึ้นกับ n หรือเป็นค่าคงที่
  ```js
  return arr[0];
  ```
- O(n)
  loop ตาม input 1 รอบ

  ```js
  for (let i = 0; i < n; i++) {}
  ```

- O(n²)
  nested loop ที่แต่ละ loop วิ่งตาม n

  ```js
  for (let i = 0; i < n; i++) {
    for (let j = 0; j < n; j++) {}
  }
  ```

- O(log n)
  แต่ละรอบลดปัญหาลงครึ่งหนึ่ง เช่น binary search
  ```js
  while (left <= right) {
    mid = Math.floor((left + right) / 2);
  }
  ```
- O(sqrt(n))
  loop ถึงรากที่สองของ n
  ```js
  for (let i = 1; i <= Math.sqrt(n); i++) {}
  ```

# Big O: Space Complexity

ดู memory เพิ่มเติมที่สร้างตาม input

- O(1)
  ใช้ตัวแปรไม่กี่ตัว
  ```js
  let sum = 0;
  let max = arr[0];
  ```
- O(n)
  สร้าง array/map/set ที่โตตาม input

  ```js
  const seen = new Set();
  for (const x of arr) {
    seen.add(x);
  }
  ```

- O(n²)
  สร้าง matrix/table ขนาด n x
  ```js
  const dp = Array.from({ length: n }, () => Array(n).fill(0));
  ```
