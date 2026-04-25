# URL: Scheme

`https://api.example.com/users?id=123`

- protocol: `https`
- port: `443`
- origin: `https://api.example.com` (include port if exists)
- sub domain: `api`
- domain: `example.com`
- path: `/users`
- query: `?id=123`
- TLD: `.com`

# Methods for restful API

- GET: get data
- POST: create data
- PATCH: update data
- PUT: replace data
- DELETE: delete data
- OPTIONS: preflight เช็ค endpoint ว่ารองรับ method หรือ header อะไรบ้าง (CORS)

# HTTP 1.1, 2 and 3

- HTTP/1.1
  - เป็น protocol แบบ text-based
  - ใช้ TCP
  - รองรับ persistent connection ด้วย `keep-alive`
  - request/response บน connection เดียวกันมักต้องรอเป็นลำดับ
  - ถ้ามีหลาย request พร้อมกัน มักต้องเปิดหลาย TCP connections ใหม่
  - อาจเจอปัญหา TCP head-of-line blocking คือ TCP ต้องรอ packets ต้องโหลดสำเร็จทั้งหมดถึงจะ process ต่อไป
- HTTP/2
  - เป็น protocol แบบ binary
  - ใช้ TCP
  - รองรับ multiplexing คือส่งหลาย request/response พร้อมกันบน TCP connection เดียวกันได้
  - บีบอัด header ด้วย HPACK
  - ลดปัญหาความต้องการเปิดหลาย connection จาก HTTP/1.1
  - ยังเกิดปัญหา TCP head-of-line blocking
- HTTP/3
  - ใช้ QUIC บน UDP แทน TCP
  - รองรับ multiplexing เหมือน HTTP/2
  - ลดปัญหา TCP head-of-line blocking
  - setup connection เร็วขึ้น เพราะ QUIC รวม TLS ไว้ในตัว
  - เหมาะกับ network ที่ไม่เสถียร เช่น mobile network

# HTTP 3 ways handshake

ก่อน client/server จะส่งข้อมูลผ่าน TCP ได้ ต้องสร้าง connection ด้วย 3 ขั้นตอน

- handshake
  - #1 SYN
    - Client ส่ง packet ไปหา server เพื่อขอเริ่ม connection
    - เหมือนบอกว่า “ขอเชื่อมต่อ”
  - #2 SYN-ACK
    - Server ตอบกลับว่าได้รับคำขอแล้ว และพร้อมเชื่อมต่อ
    - เหมือนบอกว่า “รับทราบ และพร้อม”
  - #3 ACK
    - Client ตอบกลับเพื่อยืนยัน
    - หลังจากนี้ connection พร้อมใช้งาน

# TLS 1.2 vs TLS 1.3

TLS คือ protocol ที่ใช้ encrypt การสื่อสาร เช่น HTTPS

- TLS 1.2
  - handshake ใช้หลาย RTT (Round Trip Times) ทำให้ช้า
  - handshake
    - #1
      - Client ส่ง `ClientHello`
      - Server ตอบกลับ: `ServerHello`, `Certificate`, `ServerKeyExchange`, `ServerHelloDone`
    - #2
      - Client ส่ง: `ClientKeyExchange`, `ChangeCipherSpec`, `Finished`
      - Server ตอบกลับ: `ChangeCipherSpec`, `Finished`

- TLS 1.3
  - ออกแบบให้เร็วโดยลดขั้นตอน handshake และปลอดภัยกว่า TLS 1.2
  - handshake
    - #1
      - Client ส่ง key share ตั้งแต่ `ClientHello`
      - Server ตอบ key share กลับใน `ServerHello`
