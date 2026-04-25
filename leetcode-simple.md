# Find mid

- Array 1D
  ```js
  floor((leftIndex + rightIndex) / 2);
  ```
- Array 2D
  - แปลง 2D to 1D แล้วหาแบบ 1D
- Linked list
  ```js
  mid = head;
  fast = head;
  while (fast && fast.next) {
    mid = mid.next; // Move ทีละ 1
    fast = fast.next.next; // Move ทีละ 2
  }
  ```

# Fibonacci with cache

```js
fib(n, memo={}){
  if(n in mem) return mem[n]
  if(n <= 1) return n

  memo[n] = fib(n-1, memo) + fib(n-2, memo)
  return memo[n]
}
```

# Factorial

```js
factorial(n){
  if(n === 0) return 1

  return n * factorial(n-1)
}
```

# Prime number

```js
isPrime(n){
  if(n <= 1) return false

  // Loop จนถึง root ของ n เนื่องจากคู่การคูณที่ทำให้ได้ค่า n จะมีค่าน้อยกว่า sqrt(n)
  // ดังนั้นสามารถใช้ค่าที่น้อยกว่า n เพื่อ reflex หาคู่ของมันที่ relate กับ n ได้
  for(let i = 1; i <= sqrt(n); i++){
    if(n % i === 0) return false
  }

  return true
}
```

# Power number

```js
isPower(base, v){
  const rest = log(v)/log(base) // ถอด log

  return rest === floor(rest)

  // or
  // if(v < 1) return false
  // while(v % base === 0){ // หารจนกว่าจะหารได้ไม่ลงตัว
  //   v = v/base
  // }
  // return v === 1 // ถ้าค่าสุดท้ายเป็น 1 คือหารได้ลงตัว
}
```

# Divisor count

```js
countDivisor(n){
  const c = 0

  for(let i = 1; i <= sqrt(n); i++){
    if(n % i !== 0) continue // หารไม่ลงตัวข้าม

    if(i * i === n){ // หารลงตัว และคู่ตัวคูณที่คูณกันแล้วได้ค่า n เป็นเลขเดียวกัน
      c += 1
    }else{ // หารลงตัว และคู่ตัวคูณที่คูณกันแล้วได้ค่า n เป็นคนละเลขกัน
      c += 2
    }
  }

  return c
}
```

# Perfect number

```js
isPerfect(n){
  let sum = 0

  for(let i = 1; i < n; i++){
    if(n % i === 0)
      sum = sum + i
  }

  return sum

  // or
  // const sum = 1
  // for(let i = 2; i <= sqrt(n); i++){
  //   if(n % i !== 0) continue // หารไม่ลงตัวข้าม
  //   if(i * i === n){
  //     sum = sum + i
  //   }else{
  //     sum = sum + i + n/i
  //   }
  // }
}
```

# GCD (Greatest Common Divisor)

หารร่วมมาก

```js
gcd(a, b){
  return b === 0 ? gcd(b, a % b)

  // or
  // while(b !== 0){
  //   const temp = a
  //   b = a % b
  //   a = temp
  // }
  // return a
}
```

# LCM (Least Common Multiple)

คูณร่วมน้อยโดย LCM(a, b) \* GCD(a, b) = a \* b

```js
  lcm(a, b) {
    return (a * b) / gcd(a, b);
  }
```

# Day of weeks

```js
dayOfWeek(d, m, y){
  const t = [0, 3, 2, 5, 0, 3, 5, 1, 4, 6, 2, 4];
  if (m < 3)
    y -= 1; // นับ Jan/Feb เป็นส่วนหนึ่งของปีก่อน เพื่อคำนวณ leap year ให้ถูกต้อง

  return (y + y/4 - y/100 + y/400 + t[m - 1] + d) % 7 // (y + y/4 - y/100 + y/400) คือการคำนวน leap year (ปีอธิกสุรทิน)
}
```

# Decimal to binary

```js
decimal2Binary(n){
  return n.toString(2)

  // or
  const bin = ''
  while(n > 0){
    const bit = n & 1 // หา bit ขวาสุด
    const bin = bin + bit
    n = n >> 1 // shift bit ทางขวา 1 bit
  }
  return bin
}
```

# Floor of Square root

```js
floorSqrt(n){
  let r = 1
  while(r * r < n>){
    r++
  }
  return r - 1
}
```

# nPr(n,r)

จับกลุ่มโดยลำดับมีความสำคัญ

```
n! * (n - r)!
```

# nCr(n,r)

จับกลุ่มโดยลำดับไม่มีความสำคัญ

```
n! * (r! * (n - r)!)
```
