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

# Power number

```js
IsPower(base, v){
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
IsPerfect(n){
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
