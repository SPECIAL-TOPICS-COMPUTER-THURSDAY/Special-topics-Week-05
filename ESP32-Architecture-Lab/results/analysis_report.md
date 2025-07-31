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

1. **Memory Types**: SRAM และ Flash Memory ใช้เก็บข้อมูลประเภทไหน?
2. **Address Ranges**: ตัวแปรแต่ละประเภทอยู่ใน address range ไหน?
3. **Memory Usage**: ESP32 มี memory ทั้งหมดเท่าไร และใช้ไปเท่าไร?

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

1. **Cache Efficiency**: ทำไม sequential access เร็วกว่า random access?
2. **Memory Hierarchy**: ความแตกต่างระหว่าง internal SRAM และ external memory คืออะไร?
3. **Stride Patterns**: stride size ส่งผลต่อ performance อย่างไร?