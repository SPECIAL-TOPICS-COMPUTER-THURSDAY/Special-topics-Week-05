# 1.Docker Commands: คำสั่ง docker-compose up -d และ docker-compose exec esp32-dev bash ทำอะไร ?

## docker-compose up -d สร้างและรัน Docker Containers ทั้งหมดที่อยู่ในไฟล์ docker-compose exec esp32-dev bash เข้าสู่ Bash Shell ภายใน Docker Container ที่ชื่อ esp32-dev เพื่อให้คุณสามารถรันคำสั่งต่างๆ ภายใน Container นั้นได้เสมือนคุณเข้าไปอยู่ใน Container โดยตรง

# 2. ESP-IDF Tools: เครื่องมือไหนจาก Lab4 ที่จะใช้ในการ build โปรแกรม ESP32?

## idf.py เป็นเครื่องมือหลักจาก ESP-IDF ที่ใช้ในการจัดการโปรเจกต์ เช่น การ build, flash, monitor และอื่นๆ สำหรับการพัฒนาโปรแกรม ESP32 ครับ

# 3.New Tools: เครื่องมือใหม่ที่ติดตั้ง (tree, htop) ใช้ทำอะไร?

## Tree ใช้สำหรับแสดงโครงสร้างของ Directories และ Files ในรูปแบบของต้นไม้ ทำให้เห็นลำดับชั้นของ Folder ได้ชัดเจน htop ใช้สำหรับแสดงข้อมูลการใช้งาน CPU, Memory และ Process ต่างๆ แบบ Real-time ในรูปแบบ Interactive ที่ใช้งานง่ายกว่า top

# 4.Architecture Focus: การศึกษา ESP32 architecture แตกต่างจากการทำ arithmetic ใน Lab4 อย่างไร ?

## การศึกษา ESP32 architecture เน้นทำความเข้าใจโครงสร้างฮาร์ดแวร์, หน่วยความจำ, CPU cores, Peripherals ต่างๆ และวิธีการที่ซอฟต์แวร์โต้ตอบกับฮาร์ดแวร์โดยตรง (Low-level detail) การทำ arithmetic ใน Lab4 เน้นไปที่การเขียนโปรแกรมเพื่อคำนวณทางคณิตศาสตร์ โดยอาจไม่ได้เจาะลึกถึงการทำงานของฮาร์ดแวร์ที่รองรับการคำนวณนั้นๆ มากเท่าการศึกษา Architecture ครับ

# รูปภาพ
![Image_analysis_memory](/pic/Analysis_memory.png)
![Image_analysis_cache](/pic/Analysis_cache.png)
![Image_analysis_dual_core](/pic/Analysis_dual_core.png)