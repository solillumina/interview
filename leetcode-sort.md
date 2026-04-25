# Merge Sort

- Time: `O(n log n)`
- Space: `O(n)`
- Idea: แบ่ง array เป็นครึ่ง ๆ แล้ว merge กลับแบบ sorted

```js
const mergeSort = (arr) => {
  if (arr.length <= 1) return arr;

  const mid = Math.floor(arr.length / 2);
  const left = mergeSort(arr.slice(0, mid));
  const right = mergeSort(arr.slice(mid));

  return merge(left, right);
};

const merge = (left, right) => {
  const result = [];
  let i = 0;
  let j = 0;

  while (i < left.length && j < right.length) {
    if (left[i] <= right[j]) {
      result.push(left[i]);
      i++;
    } else {
      result.push(right[j]);
      j++;
    }
  }

  return result.concat(left.slice(i), right.slice(j));
};
```

# Quick Sort

- Average Time: `O(n log n)`
- Worst Time: `O(n^2)`
- Space: `O(n)` สำหรับ version ที่สร้าง array ใหม่
- Idea: เลือก pivot แล้วแบ่งค่าที่น้อยกว่า/มากกว่า pivot จากนั้น sort ซ้ำ

```js
const quickSort = (arr) => {
  if (arr.length <= 1) return arr;

  const pivot = arr[arr.length - 1];
  const left = [];
  const right = [];

  for (let i = 0; i < arr.length - 1; i++) {
    if (arr[i] <= pivot) left.push(arr[i]);
    else right.push(arr[i]);
  }

  return [...quickSort(left), pivot, ...quickSort(right)];
};
```

# Bubble Sort

- Time: `O(n^2)`
- Space: `O(1)` ถ้าไม่นับ copy array
- Idea: เทียบคู่ติดกันแล้ว swap ให้ค่ามากค่อย ๆ ลอยไปท้าย array

```js
const bubbleSort = (arr) => {
  const result = [...arr];

  for (let i = 0; i < result.length - 1; i++) {
    for (let j = 0; j < result.length - 1 - i; j++) {
      if (result[j] > result[j + 1]) {
        const temp = result[j];
        result[j] = result[j + 1];
        result[j + 1] = temp;
      }
    }
  }

  return result;
};
```

# Selection Sort

- Time: `O(n^2)`
- Space: `O(1)` ถ้าไม่นับ copy array
- Idea: หา minimum ของส่วนที่ยังไม่ sorted แล้วเอามาวางตำแหน่งแรกของส่วนนั้น

```js
const selectionSort = (arr) => {
  const result = [...arr];

  for (let i = 0; i < result.length - 1; i++) {
    let minIndex = i;

    for (let j = i + 1; j < result.length; j++) {
      if (result[j] < result[minIndex]) minIndex = j;
    }

    const temp = result[i];
    result[i] = result[minIndex];
    result[minIndex] = temp;
  }

  return result;
};
```
