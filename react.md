# React: Dev vs Build

- Dev
  - ใช้ตอนพัฒนา
  - รัน development server เช่น Vite dev server หรือ Webpack dev server
  - มี HMR (Hot Module Replacement) ที่ทำการ Hot reload website
  - มี source map และ error overlay ช่วย debug จึงไม่ปลอดภัยหากมีผู้อื่นเข้าถึงได้
  - ไม่ได้ optimize ไฟล์ใหญ่กว่า และช้ากว่าแบบที่ build แล้ว
  - ข้อมูลอยู่ใน memory หาก server ล่มต้อง start ใหม่
- Build
  - ใช้สร้าง production static files เช่น html, JS และ CSS
  - Compile, bundle, minify และ optimize code
  - ทำ tree shaking เพื่อลบ code ที่ไม่ได้ใช้
  - แยกไฟล์/chunks และใส่ hash ใน filename เพื่อสนับสนุนการ cache
  - สามารถนำไฟล์ที่ build แล้วไป serve ด้วย static server หรือ CDN (Content Distributed Network)
    - website content แบบ CSR (Client Side Rendering) ที่ไม่ต้องอาศัย backend (SSR (Server Side Rendering), SSG (Server Side Generator)) จะถูก request ไป render ที่ client จึงทำให้สามารถ deploy บน CDN ได้โดยการชี้ IP มาที่ CDN
