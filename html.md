# CSS: link tag

- วางไว้ที่ส่วนไหนทำงานที่ส่วนนั้นโดย หยุดการ parse HTML
- กรณีวางก่อน HTML
  - โหลด CSS ก่อน parse HTML
  - หลัง parse HTML จะได้ content ใน style ที่กำหนด
- กรณีวางหลัง HTML
  - HTML parsed เสร็จก่อน
  - CSS ฒาทีหลังทำให้เกิดการ rerender ของ HTML โดย apply CSS เข้าไป

# JS: script tag

- กรณีวาง script ไว้ส่วนไหนของ html จะมีผลทำให้ html หยุดการ parse HTML และรอจนรัน script สำเร็จส่งผลให้ตำแหน่งของ script มีผลกับการ render หน้าเว็บ
- defer vs async
  - defer
    - โหลด JS ระหว่างที่ browser parse HTML (เมื่อ HTML อ่าน script tag ถึงเกิดการโหลด JS)
    - ทำงานเมื่อ parse HTML แล้ว
    - ทำงานตามลำดับแบบ FIFO (First In First Out)
  - async
    - โหลด JS ระหว่างที่ browser parse HTML (เมื่อ HTML อ่าน script tag ถึงเกิดการโหลด JS)
    - ทำงานทันที่เมื่อโหลดเสร็จ ระหว่างนี้จะหยุดการ parse HTML
    - ไม่รับประกันลำดับการทำงาน เนื่องจาก script ไหนโหลดเสร็จก่อนจะทำงานก่อน

# Render

HTML + CSS + JS
DOM + CSSOM → Render Tree → Style → Layout → Paint → Composite

- HTML parser -> DOM
- CSS parser -> CSSOM
- DOM + CSSOM -> Render Tree
  - รวม DOM node ที่ต้องแสดงผลกับ style ที่คำนวณแล้ว
  - node ที่ไม่แสดงผล เช่น `display: none` จะไม่อยู่ใน Render
- Render Tree -> Recalculate Style
  - คำนวณว่าแต่ละ element ได้ CSS property อะไร
  - เกิดเมื่อเปลี่ยน class, inline style, หรือ pseudo-class (`:hover`, `:focus`)
- Recalculate Style -> Layout/Reflow
  - คำนวณขนาดและตำแหน่งของ elements
  - คำนวณว่าแต่ละ element อยู่ตรงไหน กว้าง/สูงเท่าไร
  - เกิดใหม่ได้เมื่อแก้ layout-related style เช่น `width`, `height`, `margin`, `font-size`
  - `getBoundingClientRect()` อาจจะบังคับให้ browser reflow ถ้า rerender ยังไม่เสร็จมักจะเกิดเมื่อ loop update layout-related style และอ่านค่า bounding พร้อมกันไป
  - Window resize
  - Animation (เฉพาะ layout-related style)
  - Font load
  - Image load กรณีไม่ได้ระบุ width/height
- Layout/Reflow -> Paint
  - วาด pixels เช่น text, color, border, shadow
  - แปลง visual style เป็น pixels
  - เกิดใหม่ได้เมื่อแก้ style ที่กระทบหน้าตา เช่น `color`, `background`, `box-shadow`
- Paint -> Composite
  - รวม layers แล้วแสดงผลบน screen
  - รวม painted layers เป็นภาพสุดท้ายบนหน้าจอ
  - style เช่น `transform` และ `opacity` มัก composite ได้โดยไม่ต้อง layout/reflow
