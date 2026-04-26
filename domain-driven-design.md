# Domain-Driven Design

Domain-Driven Design หรือ DDD คือแนวทางออกแบบซอฟต์แวร์โดยให้ “Domain” หรือความรู้ทางธุรกิจเป็นศูนย์กลางของระบบ แทนที่จะเริ่มจาก database, framework, หรือ technical structure ก่อน

เป้าหมายหลักคือทำให้ code สะท้อนภาษาธุรกิจจริง เพื่อให้ developer และ domain expert คุยกันรู้เรื่อง และลดความคลาดเคลื่อนระหว่าง business requirement กับ implementation

## Core Concepts

### Domain

Domain คือขอบเขตของปัญหาทางธุรกิจที่ระบบต้องแก้ เช่น ระบบจองโรงแรม, ระบบชำระเงิน, ระบบจัดส่งสินค้า

### Model

Model คือ representation ของ domain ใน code ไม่ใช่แค่ data structure แต่รวมถึง rules, behavior, และ business logic

ตัวอย่าง:

- Order สามารถ `pay()`, `cancel()`, `ship()` ได้
- Order ที่จ่ายเงินแล้วอาจ cancel ไม่ได้
- Payment ต้องมีสถานะ เช่น pending, paid, failed

### Ubiquitous Language

Ubiquitous Language คือภาษากลางที่ business และ developer ใช้ร่วมกัน

เช่น ถ้าธุรกิจเรียกว่า `Order`, `Invoice`, `Shipment` ใน code ก็ควรใช้คำเดียวกัน ไม่ควรตั้งชื่อเป็นคำ technical ที่คน business ไม่เข้าใจ

### Entity

Entity คือ object ที่มี identity ของตัวเอง และยังถือว่าเป็นตัวเดิมแม้ข้อมูลภายในจะเปลี่ยน

ตัวอย่าง:

- User
- Order
- Product

Order เดิมอาจเปลี่ยน status จาก `pending` เป็น `paid` แต่ยังเป็น Order เดิม เพราะมี `orderId` เดียวกัน

### Value Object

Value Object คือ object ที่ไม่มี identity แยกของตัวเอง เปรียบเทียบกันด้วยค่าข้างใน

ตัวอย่าง:

- Money
- Address
- DateRange
- Email

ถ้า Money สองตัวมี currency และ amount เท่ากัน ถือว่าเป็นค่าเดียวกัน

### Aggregate

Aggregate คือกลุ่มของ Entity และ Value Object ที่ถูกจัดการเป็นหน่วยเดียวกัน โดยมี Aggregate Root เป็นตัวหลักที่คุมกฎ

ตัวอย่าง:

Order อาจเป็น Aggregate Root และมี OrderItem อยู่ข้างใน

ภายนอกควรแก้ไข OrderItem ผ่าน Order เท่านั้น เพื่อให้ Order คุม business rules ได้ เช่น ห้ามเพิ่ม item ถ้า order ถูก paid แล้ว

### Repository

Repository คือ abstraction สำหรับดึงหรือบันทึก Aggregate โดยซ่อนรายละเอียด database

ตัวอย่าง:

- `OrderRepository.findById(orderId)`
- `OrderRepository.save(order)`

Repository ไม่ควรเป็นที่ใส่ business logic หลัก ควรให้ domain object เป็นคนตัดสินใจ business behavior

### Domain Service

Domain Service ใช้เมื่อ business logic ไม่ได้เป็นของ Entity หรือ Value Object ตัวใดตัวหนึ่งโดยตรง

ตัวอย่าง:

- การคำนวณ shipping cost
- การตรวจสอบ fraud
- การโอนเงินระหว่าง account

### Bounded Context

Bounded Context คือขอบเขตที่ model และภาษาหนึ่ง ๆ มีความหมายชัดเจนภายใน context นั้น

คำเดียวกันอาจมีความหมายต่างกันในคนละ context

ตัวอย่างคำว่า `Customer`:

- ใน Sales context อาจหมายถึงคนที่สนใจซื้อ
- ใน Billing context อาจหมายถึงคนที่ต้องออก invoice
- ใน Support context อาจหมายถึงคนที่เปิด ticket

DDD แนะนำให้แยก model ตาม context แทนที่จะบังคับใช้ model เดียวทั้งระบบ

## Why DDD

DDD เหมาะกับระบบที่ business logic ซับซ้อน เพราะช่วยให้:

- code อ่านใกล้เคียงกับภาษาธุรกิจ
- business rules อยู่ในที่ที่เหมาะสม
- ลดปัญหา logic กระจายอยู่ใน controller/service/database
- แยกขอบเขตของระบบได้ชัดเจนขึ้น
- ทำให้ระบบ evolve ตามธุรกิจได้ง่ายขึ้น

## When Not To Use DDD

DDD อาจเกินความจำเป็นถ้าระบบเป็น CRUD ง่าย ๆ ไม่มี business rules ซับซ้อน เช่น admin dashboard ทั่วไปที่แค่เพิ่ม ลบ แก้ไขข้อมูล

ถ้า domain ไม่ซับซ้อน การใช้ DDD เต็มรูปแบบอาจทำให้ code หนักเกินไป
