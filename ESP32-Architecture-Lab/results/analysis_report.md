### แบบฟอร์มส่งงาน

**ข้อมูลนักศึกษา:**
- ชื่อ: นาย ภูมิ อัครธรรม
- รหัสนักศึกษา:66030270
- วันที่ทำการทดลอง: 31/7/2568

**Checklist การทดลอง:**
- [ /] Environment setup สำเร็จ (ต่อเนื่องจากสัปดาห์ที่ 4)
- [ /] Memory architecture analysis เสร็จสมบูรณ์
- [ /] Cache performance testing เสร็จสมบูรณ์
- [ /] Dual-core analysis เสร็จสมบูรณ์
- [ /] รายงานผลการทดลองครบถ้วน



### การบันทึกผลการทดลอง 

**Table 2.1: Memory Address Analysis**

| Memory Section | Variable/Function | Address (ที่แสดงออกมา) | Memory Type |
|----------------|-------------------|----------------------|-------------|
| Stack | stack_var | 0x 0x3ffb550| SRAM |
| Global SRAM | sram_buffer | 0x 0x3ffb1ac | SRAM |
| Flash | flash_string | 0x 0x4f407048 | Flash |
| Heap | heap_ptr | 0x 0x3ffb5264 | SRAM |

**Table 2.2: Memory Usage Summary**

| Memory Type | Free Size (bytes) | Total Size (bytes) |
|-------------|-------------------|--------------------|
| Internal SRAM | 380096 | 520,192 |
| Flash Memory | _________ | varies |
| DMA Memory | 	383906 | varies |

### คำถามวิเคราะห์ (ง่าย)

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน
ตอบ SRAM (Static Random-Access Memory): ใช้สำหรับเก็บข้อมูลที่โปรแกรมใช้งานในขณะรัน (Runtime) เช่นตัวแปรต่างๆ ที่สร้างขึ้นมาใหม่ (Heap allocation) หรือตัวแปรใน Stack ตัวอย่างในผลลัพธ์คือ sram_buffer และ stack_var
Flash Memory: ใช้สำหรับเก็บข้อมูลโปรแกรม (Firmware) และข้อมูลแบบถาวรที่ยังคงอยู่แม้จะปิดเครื่องไปแล้ว ตัวอย่างในผลลัพธ์คือ flash_string

2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน
ตอบ Internal SRAM: จากผลลัพธ์ stack_var (Stack) และ sram_buffer (Global SRAM) มี Address อยู่ในช่วง 0x3ffb... และ 0x3ffb5264 ซึ่งเป็นช่วง Address ของ Internal SRAM
Flash Memory: จากผลลัพธ์ flash_string มี Address อยู่ในช่วง 0x4f40... ซึ่งเป็นช่วง Address ของ Flash Memory

3. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร
ตอบ Internal SRAM:
Free heap size: 383906 bytes
Largest free block: 172932 bytes
Total Size: 520,192 bytes (จากตารางที่คุณให้มา)
ใช้ไปแล้ว: 520,192 - 383906 = 136286 bytes
Flash Memory:
ข้อมูลที่ให้มาไม่ได้ระบุขนาด Free Size หรือ Total Size ของ Flash Memory
DMA capable memory:
Free heap size: 383906 bytes (ค่านี้อาจจะซ้ำกับ heap ปกติ ขึ้นอยู่กับการตั้งค่า)

### การบันทึกผลการทดลอง

**Table 3.1: Cache Performance Results**

| Test Type | Memory Type | Time (μs) | Ratio vs Sequential |
|-----------|-------------|-----------|-------------------|
| Sequential | Internal SRAM | 5258 | 1.00x |
| Random | Internal SRAM | 6158 | 1.17x |
| Sequential | External Memory | 22846 | 1.00x |
| Random | External Memory | 28845 | 1.26x |

**Table 3.2: Stride Access Performance**

| Stride Size | Time (μs) | Ratio vs Stride 1 |
|-------------|-----------|------------------|
| 1 | 5472 | 1.00x |
| 2 | 2713 | 0.50x |
| 4 | 1719 | 0.31x |
| 8 | 788 | 0.14x |
| 16 | 401 | 0.09x |

### คำถามวิเคราะห์

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access
ตอบ Sequential access เร็วกว่า random access เพราะอาศัยหลักการทำงานของ cache
เมื่อข้อมูลถูกอ่านแบบเรียงลำดับ (sequential) CPU จะสามารถดึงข้อมูลชุดถัดไปที่อยู่ติดกันจากหน่วยความจำหลัก (main memory) มาเก็บไว้ใน cache ได้ล่วงหน้า (prefetching)
เมื่อถึงคราวที่ CPU ต้องการข้อมูลถัดไป ข้อมูลนั้นจะพร้อมใช้งานอยู่ใน cache แล้ว ทำให้ไม่ต้องเสียเวลาไปดึงจากหน่วยความจำหลักที่ช้ากว่า
ในทางกลับกัน random access การเข้าถึงข้อมูลแบบสุ่ม ทำให้ข้อมูลที่ CPU ดึงมาเก็บไว้ใน cache ล่วงหน้าไม่มีประโยชน์ เพราะข้อมูลที่ต้องการต่อไปอาจจะไม่ได้อยู่ติดกัน ทำให้เกิด cache miss บ่อยครั้ง CPU จึงต้องเสียเวลาไปดึงข้อมูลจากหน่วยความจำหลักในแต่ละครั้ง ส่งผลให้ประสิทธิภาพต่ำกว่า

2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร
ตอบ Internal SRAM (Static Random-Access Memory) คือหน่วยความจำที่อยู่ภายในตัวชิป ESP32
External Memory คือหน่วยความจำภายนอกที่เชื่อมต่อกับชิป เช่น Flash Memory หรือ PSRAM
ความแตกต่างหลัก คือ ความเร็ว โดย Internal SRAM เร็วกว่า External Memory อย่างมาก จากผลการทดลองจะเห็นได้ว่า External Memory Access ช้ากว่า Internal SRAM Access ถึง 4.34 เท่า (External/Internal Speed Ratio: 4.34x) นอกจากนี้ External Memory อาจมีความจุมากกว่า แต่ก็แลกมาด้วยความเร็วที่ช้าลง

3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร
ตอบStride size คือระยะห่างของการเข้าถึงข้อมูลแต่ละครั้งในหน่วยความจำ
จากผลการทดลองจะเห็นว่า เมื่อ Stride size เพิ่มขึ้น ประสิทธิภาพจะดีขึ้น (เวลาที่ใช้ลดลง)
Stride 1 คือการเข้าถึงข้อมูลแบบเรียงลำดับปกติ ซึ่งมีประสิทธิภาพดีเนื่องจากใช้ cache ได้เต็มที่
แต่เมื่อ Stride size เพิ่มขึ้น (เช่น 2, 4, 8, 16) โปรแกรมจะข้ามข้อมูล ทำให้การเข้าถึงข้อมูลแต่ละครั้งไม่ได้อยู่ติดกัน
ผลที่ได้คือเวลาในการเข้าถึงข้อมูลลดลงอย่างมาก (เวลาที่ใช้ลดลง) เนื่องจากมีจำนวน Iteration ที่น้อยลงสำหรับการประมวลผลข้อมูลชุดเดิม ทำให้การทำงานของ CPU มีประสิทธิภาพมากขึ้น

### การบันทึกผลการทดลอง

**Table 4.1: Dual-Core Performance Summary**

| Metric | Core 0 (PRO_CPU) | Core 1 (APP_CPU) |
|--------|-------------------|-------------------|
| Total Iterations | 100 | 150 |
| Average Time per Iteration (μs) | 44 | 9615 |
| Total Execution Time (ms) | 5000 | 5942 |
| Task Completion Rate | 100% | 100% |

**Table 4.2: Inter-Core Communication**

| Metric | Value |
|--------|-------|
| Messages Sent | 10 |
| Messages Received | 10 |
| Average Latency (μs) | 14385.22|
| Queue Overflow Count | 0 |

### คำถามวิเคราะห์

1. **Core Specialization**: จากผลการทดลอง core ไหนเหมาะกับงานประเภทใด
ตอบ Core 0 เหมาะสำหรับงานที่ต้องการความเร็วในการประมวลผลสูงและใช้เวลาน้อยต่อ Iteration (เฉลี่ย 44 μs) เนื่องจากมีค่าเฉลี่ยเวลาต่อ Iteration ที่ต่ำมาก

Core 1 ใช้เวลาเฉลี่ยต่อ Iteration สูงกว่ามาก (9615 μs) และยังสามารถประมวลผลได้จำนวน Iteration ที่สูงกว่า (150 ครั้ง) จึงอาจเหมาะกับงานที่ต้องการจำนวนการทำงานที่มากและมีความซับซ้อนมากกว่า ซึ่งอาจใช้เวลาในการประมวลผลแต่ละครั้งนานกว่า


2. **Communication Overhead**: inter-core communication มี overhead เท่าไร
ตอบ จากข้อมูลความหน่วง (latency) ของการสื่อสารระหว่าง Core ที่บันทึกไว้ มีค่าเฉลี่ยประมาณ 14385.22 μs

3. **Load Balancing**: การกระจายงานระหว่าง cores มีประสิทธิภาพหรือไม่
ตอบ การกระจายงานระหว่าง Core ไม่มีประสิทธิภาพ ในแง่ของเวลาการประมวลผล เพราะ Core 1 ใช้เวลาต่อ Iteration นานกว่า Core 0 อย่างมากถึง 218 เท่า (9615 μs / 44 μs ≈ 218.5)

แม้ว่า Core 1 จะทำงานได้จำนวน Iteration ที่มากกว่า แต่ด้วยเวลาเฉลี่ยที่สูงมาก ทำให้เวลาการทำงานรวม (Total Execution Time) ของ Core 1 (5942 ms) ก็มากกว่า Core 0 (5000 ms) เช่นกัน

แสดงให้เห็นว่า Core 1 มีภาระงานที่หนักกว่ามากเมื่อเทียบกับ Core 0

# รูปผลการทดลองทั้งหมด 
![alt text](<สกรีนช็อต 2025-07-31 203408.png>)

![alt text](<สกรีนช็อต 2025-07-31 203451.png>)

![alt text](<สกรีนช็อต 2025-07-31 210048.png>) 

![alt text](<สกรีนช็อต 2025-07-31 210031.png>)

![alt text](<สกรีนช็อต 2025-07-31 210018.png>)

![alt text](<สกรีนช็อต 2025-07-31 210009.png>)


## ผมไม่่สามารถส่งไฟล์ build ได้ครับเลยลบออกแล้ว commit ส่งเฉพาะโฟลเดอร์
![alt text](<สกรีนช็อต 2025-07-31 203626.png>)