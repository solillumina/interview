# Idempotency

Idempotency คือคุณสมบัติที่ทำ operation เดิมซ้ำกี่ครั้ง ผลลัพธ์สุดท้ายยังเหมือนกับทำครั้งเดียว

แนวคิดนี้สำคัญมากใน distributed system, API, payment, message queue และ retry logic เพราะ network หรือ service อาจ timeout ได้ แม้ request จริงอาจถูก process สำเร็จไปแล้ว

## Example

ถ้า client ส่ง request จ่ายเงิน แล้วเกิด timeout client อาจ retry request เดิมอีกครั้ง

ถ้า API ไม่ idempotent:

- retry แล้วอาจตัดเงินซ้ำ
- สร้าง order ซ้ำ
- ส่ง email ซ้ำ
- publish event ซ้ำ

ถ้า API idempotent:

- retry request เดิมจะได้ผลลัพธ์เดิม
- ไม่เกิด side effect ซ้ำ
- ปลอดภัยกับ retry มากขึ้น

## HTTP Methods

บาง HTTP method มี idempotency ตาม design

### GET

`GET` ควรเป็น idempotent เพราะเป็นการอ่านข้อมูล ทำซ้ำกี่ครั้งก็ไม่ควรเปลี่ยน state ของระบบ

### PUT

`PUT` มักเป็น idempotent เพราะเป็นการ replace resource ด้วย state เดิม

ตัวอย่าง:

```http
PUT /users/1
{
  "name": "Alice"
}
```
