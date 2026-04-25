# JS: Call Stack vs Event Loop

- Call Stack
  - เก็บ function calls ที่กำลัง execute
  - js รัน synchronous code ทีละคำสั่ง
  - ถ้า function ใช้เวลานานจะ block คำสั่งอื่นๆ
- Event Loop
  - คอยเช็คว่า Call Stack ว่างหรือยัง
  - รัน 1 Marcrotask แบบ FIFO (First In First Out)
    - Macrotask เช่น sync code (code แรกที่สามารถรันได้เลยถือเป็น macrotask และเป็นเหตุผลที่ทำใมการ setTimeout หรือ setInterval รันทีหลังแม้จะเป็น Macrotask), setTimeout, setInterval, I/O และ UI events
  - ถ้า stack ว่างจะนำ callback จาก Microtasks queue มารันบน Call Stack แบบ drain (รันจนกว่าจะหมด queue)
    - Microtasks เช่น Promise, then, catch, finally และ async function
  - Rerender UI (ถ้ามี)
  - วน loop

# JS: Memory

- Stack memory
  - LIFO (Last In First Out)
  - รวดเร็ว
  - Live แค่ใน scope เมื่อออกจาก scope จะคืน memory
  - ใช้เก็บ primitive value และ reference ที่ชี้ไปยัง object ใน Heap memory
- Heap memory
  - ช้ากว่า stack
  - ใช้เก็บ object เช่น object, array, function
  - GC (Gabage Collector) จะคืน memory ของ object ใดๆเมื่อไม่มี reference ใดๆจาก Stack memory ที่ชี้มายัง object

# JS: Hoisting

- JS ก่อน execute จะ move declaration เช่น var, let, const และ function ทั้งหมดไปด้านบนของ scope (ไม่ได้ย้ายไปจริงๆ)
  - var จะ initial value เป็น undefined
  - function จะถูก declare ทั้ง body
    - ทำให้ฟังก์ชันเมื่อประกาศที่ส่วนไหน๘อง scope ก็ถูกเรียกใช้งานได้ เช่นเรียกใช้ฟังก์ชัน ก่อนประกาศฟังก์ชัน
  - let และ const จะไม่มีมีการ initial value
    - จะ initial value เมื่อถึงบรรทัดที่ประกาศตัวแปรนั้นๆ
    - ในช่วงตั้งแต่ต้น scope จนถึงตำแหน่งที่มีการเรียกใช้ตัวแปรที่ declare ผ่าน let หรือ const จะเรียกว่า TDZ (Temporal Dead Zone) ในช่วง TDZ จะเกิด ReferenceError (เป็นการออกแบบเพื่อให้มั่นใจได้ว่าตัวแปรจะถูกเรียกใช้หลังจาก initial value)

# JS: Bundler

- เป็นเครื่องเมื่อที่รวมไฟล์ js หลายๆไฟล์เข้าด้วยกัน โดยอาจจะมีการทำ minimize เพื่อลดขนาด script และ tree shaking เพื่อลด code ที่ไม่ได้ใช้งานออกจาก bundles ตัวอย่าง Bundler เช่น Webpack, Vite และ Turbo pack
- Single Bundle vs Multiple Bundles
  - Single Bundle
    - รวมเป็นไฟล์เดียว
    - ไฟล์จะค่อนข้างใหญ่
    - ติดตั้งง่าย
    - เหมาะกับแอพขนาดเล็ก
    - เนื่องจากไฟล์ใหญ่ทำให้โหลดช้า
    - ต้องโหลด code ทั้งหมดทั้งที่ใช้ และไม่ได้ใช้ ก่อนจะใช้งานได้
    - ใช้งานร่วมกับ cache ยาก โดยเมื่อการแก้ไข code บางส่วนต้องทำการอัปเดต bundle และ invalidate cache
  - Multiple Bundles
    - แยกเป็นไฟล์เล็กๆ ย่อยๆ
    - cache ง่าย
    - โหลดไว
    - โหลดเฉพาะ code ที่ต้องการได้
    - ทำ lazy load ได้
    - จัดการยาก
    - config ซับซ้อนกว่า
    - หากกำหนดให้มีหลาย bundles อาจจะมี overhead เยอะ (request -> load -> execute)
      - ปัจจุบัน HTTP/2 และ HTTP/3 สามารถ reuse connection โหลด website content ได้โดยไม่ต้องเปิด connection ใหม่ทำให้ overhead ในส่วนของ HTTP 3-way handshake ลดลงไป

# JS: Map vs WeakMap and Set vs WeakSet

- Map
  - เก็บข้อมูลแบบ key-value
  - Key เป็น value ชนิดใดก็ได้ เช่น string, number, object
  - Loop data ได้
  - เก็บข้อมูลเป็นลำดับ
  - มีขนาดข้อมูลที่ชัดเจน
  - key และ value จะยังถูกอ้างอิงอยู่ตราบใดที่ map object ยังถูกใช้งาน (สามารถเกิด memory leaks ได้ถ้าใช้ไม่ถูกต้อง)
- WeakMap
  - เก็บข้อมูลแบบ key-value
  - Key ต้องเป็น object เท่านั้น
  - Key อาจจะโดน GC คืน memory เมื่อไหร่ก็ได้ (weak reference)
  - Loop data ไม่ได้
  - ไม่ขนาดของข้อมูลที่แน่นอนเนื่องจาก Key อาจจะถูก GC คืน memory เมื่อไหร่ก็ได้
- Set
  - เก็บค่าที่ไม่ซ้ำกัน
  - Value เป็นชนิดใดก็ได้
  - Loop data ได้
  - เก็บข้อมูลเป็นลำดับ
  - มีขนาดข้อมูลที่ชัดเจน
- WeakSet
  - เก็บ object ที่ไม่ซ้ำกัน
  - Value ต้องเป็น object เท่านั้น
  - Value อาจจะโดน GC คืน memory เมื่อไหร่ก็ได้ (weak reference)
  - Loop data ไม่ได้
  - ไม่ขนาดของข้อมูลที่แน่นอนเนื่องจาก Key อาจจะถูก GC คืน memory เมื่อไหร่ก็ได้

# JS: Cookie vs Session vs LocalStorage

- Cookie
  - เก็บข้อมูลขนาดเล็กเป็น string แบบ key-value
  - Browser จะส่ง cookie ที่ตรงกับ domain/path ไปกับ HTTP request อัตโนมัติ เมื่อกำหนดให้ส่ง credential ไปด้วยที่ requester
  - ใช้ได้ข้าม tabs/windows ภายใต้ origin/domain ที่กำหนด
  - กำหนดอายุได้ด้วย `Max-Age`
  - ป้องกันการอ่านจาก JS ได้ด้วย `HttpOnly`
  - เพิ่มความปลอดภัยได้ด้วย `Secure` และ `SameSite`
- Session
  - เป็นข้อมูลฝั่ง server ที่ผูกกับ user/session หนึ่ง ๆ
  - มักทำงานร่วมกับ cookie โดยเก็บ `sessionId` ไว้ใน cookie
  - เมื่อ server ล่ม session จะหายไปตาม server
  - สามารถเก็บ session ไว้ใน database หรือ cache เพื่อแชร์ session ระหว่าง node หรือป้องกัน session หายไปเมื่อ service ล่ม และสามารถทำให้ scale ได้
- LocalStorage
  - เก็บข้อมูลใน browser เป็น key-value string
  - ข้อมูลอยู่ถาวรจนกว่าจะถูกลบ
  - ใช้ร่วมกันได้ทุก tabs/windows ของ origin เดียวกัน
  - ไม่ถูกส่งไปกับ HTTP request อัตโนมัติ
  - JS อ่านได้จึงไม่ควรเก็บ sensitive data

# JS: ES6 ECMAScript2015

- Arrow Function (=>)
  ```js
  const sum = (a, b) => a + b;
  ```
- Default Parameters
  ```js
  function sum(a, b = 10) {
    return a + b;
  }
  ```
- Spread Operator
  ```js
  function fn(...args) {}
  ```
- Rest Parameters (...args)
  ```js
  const { id, ...rest } = user;
  ```
- Destructure Object
  ```js
  const { id } = user;
  ```
- Template Literals
  ```js
  `Hello ${username}`;
  ```
- Tagged Template Literals
  ```js
  function sql(strings, values) {}
  sqlHello`${username}`;
  ```
