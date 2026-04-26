# TS: Class vs Interface

- `class`
  - มีอยู่จริงตอน runtime
  - ใช้สร้าง object ได้ด้วย `new`
  - มี constructor, properties และ methods ได้
  - ใช้ inheritance ได้ผ่าน `extends`
  - ใช้ตรวจ runtime ได้ด้วย `instanceof`
  - ใน JavaScript มี `class` จริง
- `interface`
  - ไม่มีใน JavaScript runtime
  - เป็น feature ของ TypeScript
  - ใช้กำหนด shape/type ของ object
  - compile แล้วหายไป ไม่เหลือเป็น code ตอน runtime
  - ใช้ new ไม่ได้
  - ใช้ instanceof ไม่ได้
  - เหมาะกับการบอก contract ว่า object ควรมี field/method อะไรบ้าง
