# DB Principle: ACID

ACID คือคุณสมบัติหลักของ database transaction เพื่อให้ข้อมูลถูกต้องและน่าเชื่อถือ (Transaction ในที่นี้นับรวม query ปกติ)

- Atomicity
  - Transaction ต้องสำเร็จทั้งหมด หรือไม่สำเร็จเลย
  - ถ้ามี step ใด fail ต้อง rollback ทั้ง transaction

- Consistency
  - Transaction ต้องทำให้ข้อมูลอยู่ใน state ที่ถูกต้องตาม rules/constraints เสมอ
  - เช่น foreign key, unique constraint และอื่นๆ

- Isolation
  - Transaction หลายตัวที่รันพร้อมกันต้องไม่รบกวนกันจนเกิดข้อมูลผิด
  - ผลลัพธ์ควรเหมือนกับว่ารันทีละ transaction ตามลำดับ

- Durability
  - เมื่อ transaction commit แล้ว ข้อมูลต้องไม่หาย
  - แม้ server crash หรือ restart ข้อมูลที่ commit แล้วควรยังอยู่

# DB: Delete vs Truncate

- Delete
  - ใช้ `WHERE` เพื่อค้นหาข้อมูลที่จะต้องการลบได้
  - ถ้าไม่ใส่ `WHERE` จะลบทุก row แต่ยังเป็นการลบทีละ row
  - ใน PostgreSQL row จะถูก mark as deleted และ cleanup ภายหลังโดย `VACUUM` และใน DB engine อื่นๆอาจจะมีรายละเอียดการทำงานของการ delete ที่ต่างกัน
  - `VACUUM` ทำให้ได้ disk space คืน แต่ในบาง DB engine อาจจะทำให้มีพื้นที่ในการ reuse แทนที่การคืน disk space
- Truncate
  - ใช้ `WHERE` ไม่ได้
  - ลบข้อมูลทั้งหมดใน table
  - เร็วกว่า `DELETE` เพราะไม่ลบทีละ row
  - ได้ disk space คืนทันที
  - สามารถ reset identity/auto increment ได้ เช่น `TRUNCATE table_name RESTART IDENTITY`
  - rollback ได้ใน PostgreSQL ถ้าอยู่ใน transaction
