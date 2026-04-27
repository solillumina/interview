# SQL: Limit Offset vs Cursor pagination

- Limit Offset Pagination
  - ใช้ `LIMIT` เพื่อกำหนดจำนวน rows ที่ต้องการ
  - ใช้ `OFFSET` เพื่อข้าม rows ก่อนหน้า
  - เขียนง่าย และเหมาะกับ pagination แบบต้องการตำแหน่งของ page number ที่แน่นอน
  - รองรับการ jump ไป page ใดก็ได้
  - เมื่อมีจำนวนข้อมูลเยอะขึ้นจะช้าลง
  - การ `OFFSET` สูงจะช้าลง เพราะ database ต้อง scan/skip rows ก่อนหน้า
  - ถ้ามี insert/delete ระหว่างเปลี่ยนหน้า อาจเกิดข้อมูลซ้ำหรือข้ามข้อมูลได้
- Cursor Pagination
  - ใช้ค่าของ row ล่าสุดที่เห็น (last seen) เป็น cursor ที่บอกตำแหน่งล่าสุด ใช้ในการ query page ถัดไป
  - performance ดีกว่าเมื่อข้อมูลเยอะ เพราะใช้ index ได้ดีและไม่ต้อง scan/skip rows แต่เป็นการ filter row ใน page ก่อนหน้าออกไปเลย
  - stable กว่าเมื่อมี insert/delete ระหว่าง pagination
  - เหมาะกับ infinite scroll, feed, chat, API list
  - jump ไป page number
  - ต้องใช้ `WHERE` กับ unique row data เพื่อป้องกันข้อมูลซ้ำ โดยปกติจะใช้ค่า `created_at, id` และต้องมี order ที่แน่นอนและไม่ซ้ำ เช่น `ORDER BY created_at DESC, id DESC`
    ```sql
    WHERE (created_at, id) < (:last_created_at, :last_id) -- Tuple search
    ORDER BY created_at DESC, id DESC
    LIMIT 10
    -- ===Equivalent to
    -- WHERE created_at < :last_created_at
    --   OR (
    --     created_at = :last_created_at
    --     AND id < :last_id
    --   )
    ```
  - กรณี mixed order ASC และ DESC จะใช้ tuple search ไม่ได้
    ```sql
    WHERE score < :last_score
      OR (
        score = :last_score
        AND id > :last_id
      )
    ORDER BY score DESC, id ASC
    LIMIT 10;
    ```
