# CP372 Data Analytics and Business Intelligence
## Member
นางสาวอรนลิน มิ่งมิตร 66102010155  
นางสาวธนัญญา หินทุม 66102010241  
นางสาวเบญญาภา ปลอดเอี่ยม 66102010242  

## Scenario: The Hotel Business Owner's Dilemma Project
Problem 1 : Revenue Stagnation 
---  
<img width="1920" height="1080" alt="Project canvas" src="https://github.com/user-attachments/assets/0a6686b0-7108-43ee-accd-34a6c11a6206" />

---  

Problem : Revenue Stagnation 
---
โรงแรม The Azure Stay มีอัตราการเข้าพักอยู่ในระดับที่น่าพอใจ มีลูกค้าเข้าพักอย่างสม่ำเสมอ แต่รายได้ต่อห้องพักที่เปิดขาย (RevPAR) กลับอยู่ในระดับต่ำและไม่เติบโตสาเหตุหลัก จากการตั้งราคาห้องพักและการจัดสรรห้องพักที่ยังไม่เหมาะสม เช่น การขายห้องพักในราคาที่ต่ำเกินไปในบางช่วงเวลา หรือการจัดห้องพักไม่ตรงกับประเภทลูกค้า ทำให้โรงแรมไม่สามารถสร้างรายได้สูงสุดจากห้องพักที่มีอยู่ และส่งผลให้รายได้โดยรวมไม่เพิ่มขึ้น ทำให้เกิดเป็นภาวะรายได้หยุดนิ่ง (Revenue Stagnation) แม้จะมีลูกค้าเข้ามาใช้บริการ ซึ่งต้องการทราบปัจจัยที่สามารถนำมาปรับปรุงคุณภาพและวางแผนแก้ไขปัญหาของโรงแรม  

SMART Objectives
---
* Specific: วิเคราะห์ปัจจัยที่ส่งผลต่อ RevPAR โดยพิจารณาจากราคา (ADR), อัตราการเข้าพัก (OCC) และพฤติกรรมการจอง  
* Measurable: เพิ่มค่า RevPAR และ ADR จากข้อมูลย้อนหลัง  
* Achievable: ใช้ข้อมูลจากระบบ PMS ที่มีอยู่  
* Relevant: สนับสนุนการตัดสินใจด้าน Revenue Management  
* Time-bound: วิเคราะห์ข้อมูลย้อนหลัง 6–12 เดือน  
* เป้าหมาย: เพื่อช่วยผู้บริหารและทีม Revenue Management สามารถกำหนดกลยุทธ์การตั้งราคาและการขายห้องพักได้อย่างเหมาะสมในเวลาที่เหมาะสม ส่งผลให้โรงแรมสามารถเพิ่มรายได้รวมโดยไม่จำเป็นต้องเพิ่มจำนวนห้องพัก 

---

Questions 
---
* แต่ละ Booking Channel สร้างรายได้และกำไรสุทธิต่อห้อง (Net RevPAR) แตกต่างกัน
* Room Type และ Segment แบบใดที่สร้าง RevPAR สูงที่สุด
* RevPAR ในวันธรรมดา (Weekday) และวันหยุดสุดสัปดาห์ (Weekend) แตกต่างกันหรือไม่?
* ฤดูกาล (Season) ส่งผลต่อ RevPAR และ Occupancy อย่างไร?
* ห้องประเภท Suite มีอัตราการเข้าพักต่ำกว่าห้องประเภทอื่นในวันธรรมดาหรือไม่? 
---

Hypothesis 
---
* H1: Booking Channel แต่ละประเภทให้กำไรสุทธิ (Net RevPAR) แตกต่างกันอย่างมีนัยสำคัญ โดย Direct Channel ให้ Net RevPAR สูงกว่า OTA หลังหักค่าคอมมิชชัน
* H2: Room Type และ Customer Segment บางกลุ่มมีค่า RevPAR สูงกว่าค่าเฉลี่ยของโรงแรมอย่างมีนัยสำคัญ 
* H3: การปรับราคาตามวันในสัปดาห์และฤดูกาล ส่งผลให้ RevPAR เพิ่มขึ้นอย่างมีนัยสำคัญ โดยไม่ลด Occupancy 
* H4: Room Type (เช่น Suite) มีความต้องการต่ำในวันธรรมดา และยังไม่มี pricing/promotion ที่เหมาะสมเพื่อกระตุ้น demand 
---

Data Dictionary
---
**Table: fact_bookings**    
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **booking_id** | Nominal | รหัสการจอง | RES-20000 ถึง RES-25033 |
| **guest_id** | Nominal | รหัสลูกค้า | G-1008 ถึง G-9469 |
| **booking_date** | Interval | วันที่ทำรายการจอง | 2024-01-02 ถึง 2026-06-28 |
| **check_in_date** | Interval | วันที่เข้าพัก | ต้องไม่ก่อนวันจอง (BLT >= 0) |
| **check_out_date** | Interval | วันที่คืนห้อง | ต้องหลังวันเช็คอิน (LOS >= 1) |
| **room_type_id** | Nominal | รหัสประเภทห้อง | RT_FAM, RT_DLX, RT_SUIT, RT_EXEC, RT_STD |
| **rate_code_id** | Nominal | รหัสเรทราคา | RC_WHL, RC_LONG, RC_PKG, RC_BAR, RC_CORP, RC_GOV, RC_PROMO |
| **channel_id** | Nominal | รหัสช่องทางการจอง | CH_GDS, CH_CORP, CH_PHN, CH_BOOK, CH_DIR, CH_EXP, CH_WALK, CH_WHL |
| **segment_id** | Nominal | รหัสกลุ่มลูกค้า | SEG_CORP, SEG_LEIS, SEG_GRP, SEG_GOV, SEG_PKG |
| **status** | Nominal | สถานะการจอง | Checked-Out, Cancelled, Confirmed, No-Show |
| **total_room_revenue** | Ratio | รายได้ค่าห้องพัก | 0.00 - 84,000.00 (Cancelled = 0) |
| **number_of_rooms** | Ratio | จำนวนห้องที่จอง | 1 - 12 |
| **adults_count** | Ratio | จำนวนผู้ใหญ่ | 0 - 15 |
| **children_count** | Ratio | จำนวนเด็ก | 0 - 9 (ค่าว่างถูกแทนด้วย 0) |
| **total_guests** | Ratio | จำนวนผู้เข้าพักทั้งหมด | 0 – 24 |
| **LOS** | Ratio | จำนวนคืนที่เข้าพัก | 1 – 14 |
| **BLT** | Ratio | ระยะเวลาจองล่วงหน้า | 0 – 120 |
| **commission_cost** | Ratio | ค่าคอมมิชชั่น | 0 – 14280 |
| **net_room_revenue** | Ratio | รายได้สุทธิ | 0 - 77000 |
| **room_sold** | Ratio | จำนวน room-night ที่ขาย | 1 - 154 |  

**Table: dim_rate_codes**  
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **rate_code_id** | Nominal | รหัสเรทราคา | RC_CORP, RC_BAR, RC_PROMO, RC_PKG, RC_GOV, RC_WHL, RC_LONG  |
| **rate_name** | Nominal | ชื่อเรทราคา | Corporate Flat Rate, Best Available Rate, Package Deal, Government Rate, Promotional Rate, Wholesale Rate, Long Stay Rate (7+ nights) |
| **description** | Nominal | ความหมายแต่ละชื่อเรทราคา | Includes Breakfast & Wifi, Room Only, Includes Breakfast & Dinner, Room Only + ID Required, Non-Refundable, Includes Breakfast, Includes Wifi |
| **is_commissionable** | Nominal | การจ่ายคอมมิชชั่น | True, False |

**Table: dim_channels**  
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **channel_id** | Nominal | รหัสช่องทาง | CH_DIR, CH_EXP, CH_WALK, CH_BOOK, CH_GDS, CH_PHN, CH_WHL, CH_CORP |
| **channel_name** | Nominal | ชื่อช่องทาง | Expedia, Booking.com, Walk-In, Direct Website, GDS / Travel Agent, Phone Reservation, Wholesaler, Corporate Portal |
| **channel_type** | Nominal | ประเภทของช่องทางการจอง | OTA (Online Travel Agent), Direct, GDS, Wholesaler |
| **commission_rate** | Ratio | อัตราคอมมิชชั่น | 0.00 - 0.2 (เช่น 0.15 สำหรับ Expedia) |    

**Table: dim_room_inventory**
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **date** | Interval | วันที่ | 2024-01-01 ถึง 2026-06-30 |
| **total_capacity** | Ratio | ความจุห้องทั้งหมด | 100 (ค่าคงที่ตามขนาดโรงแรม) |
| **rooms_out_of_order** | Ratio | ห้องที่ปิดซ่อม | 0 - 4 |
| **rooms_available_for_sale** | Ratio | ห้องที่พร้อมขาย | 96 - 100 |  

**Table: dim_calendar**
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **date_key** | Interval | วันที่ที่เป็นตัวระบุข้อมูลในแต่ละแถว | 2024-01-01 ถึง 2026-06-30 |
| **day_name** | Nominal | ชื่อวัน | Monday, Tuesday, ..., Sunday |
| **is_weekend** | Nominal | วันหยุดสุดสัปดาห์ | True (Sat/Sun), False (Mon-Fri) |
| **is_holiday** | Nominal | วันหยุดนักขัตฤกษ์ | True, False |
| **season** | Ordinal | ฤดูกาล | High Season, Shoulder Season, Low Season |  

**Table: dim_roomtype**
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **room_type_id** | Nominal | รหัสประเภทห้อง | RT_STD, RT_DLX, RT_FAM, RT_EXEC, RT_SUIT |
| **room_type_name** | Nominal | ชื่อประเภทห้องพัก | Standard Room, Deluxe Room, Family Room, Executive Room, Suite Room |
| **base_price** | Ratio | ราคาพื้นฐานของห้องพัก | 350, 550, 800, 1100, 1200 |  

**Table: dim_segments**
| Attribute | Type | Description | Valid Range / Example |
| :--- | :--- | :--- | :--- |
| **segment_id** | Nominal | รหัสกลุ่มลูกค้า | SEG_CORP, SEG_LEIS, SEG_GRP, SEG_GOV, SEG_PKG |
| **segment_name** | Nominal | ชื่อกลุ่มลูกค้า | Corporate, Leisure, Group, Government, Package |

---

Data Preparation
---
* สร้างชุดข้อมูล (Dataset) ที่จำเป็น : สร้างตารางข้อมูล: fact_bookings, dim_room_inventory เป็นต้น
* ตรวจสอบความถูกต้องข้อมูล : เช็คข้อมูลในแต่ละตารางให้ถูกต้องและสอดคล้องกัน
* แก้ข้อมูลให้ถูกต้อง : จัดรูปแบบวันที่, Replace Value ให้เริ่มด้วยพิมพ์ใหญ่ใน Column Status
---

Hypothesis 1 :  Booking Channel แต่ละประเภทให้กำไรสุทธิ (Net RevPAR) แตกต่างกันอย่างมีนัยสำคัญ โดย Direct Channel ให้ Net RevPAR สูงกว่า OTA หลังหักค่าคอมมิชชัน     
---
**กราฟ :** เปรียบเทียบรายได้เฉลี่ยสุทธิและกำไรสุทธิต่อห้อง (Net RevPAR) ของ Booking channal ต่างๆ  
**เหตุผลการใช้** : Bar Chart (กราฟแท่ง) เพื่อเปรียบเทียบข้อมูลแบบหมวดหมู่ (Categorical Data) ระหว่าง Channel ต่างๆ 
ซึ่งกราฟแท่งช่วยให้เห็นความแตกต่างระหว่าง Average Revenue และ Net RevPAR ของแต่ละช่องทางได้อย่างชัดเจนที่สุด  
โดยพบว่า  
* Direct: เป็นช่องทางที่ทำกำไรดีที่สุด เนื่องจากไม่มีค่าคอมมิชชัน ทำให้ Net RevPAR สูงสุด → ควรเพิ่มสัดส่วนเพื่อ maximize กำไร  
* OTA: ทำรายได้และกำไรได้ดีรองลงมา แม้มีค่าคอมฯ แต่ยังสำคัญในการเข้าถึงลูกค้าใหม่ → ใช้เป็นช่องทาง “ขยายตลาด”  
* GDS: แม้ตั้งราคาขายได้สูง แต่กำไรสุทธิต่ำ → สะท้อนต้นทุนแฝงสูง ควร “ทบทวนความคุ้มค่า”  
* Wholesaler: รายได้และกำไรต่ำสุด เน้นขายจำนวนมากมากกว่ากำไร → เหมาะใช้ระบายห้องช่วง Demand ต่ำ     

<img width="1340" height="581" alt="image" src="https://github.com/user-attachments/assets/67c4886b-d41a-4ae2-842d-a598218da685" />  

---

Hypothesis 2 : Room Type และ Customer Segment บางกลุ่มมีค่า RevPAR สูงกว่าค่าเฉลี่ยของโรงแรมอย่างมีนัยสำคัญ  
---
**กราฟ :** เปรียบเทียบ Room Type และ Segment ที่สร้าง RevPAR ได้สูงที่สุด    
**เหตุผลการใช้ :** Bar Chart (กราฟแท่ง) เพราะต้องการเปรียบเทียบตัวแปรแบบหมวดหมู่ 2 แกนพร้อมกัน คือ 
Room Type และ Segment ทำให้เห็นภาพทันทีว่าแท่งไหน (กลุ่มไหน) คือตัวทำเงิน และแท่งไหนคือจุดบอด  
โดยพบว่า   
* ห้อง Standard คือรายได้หลัก (Key Driver): ทำ RevPAR สูงสุดทิ้งห่างห้องอื่น โดยมีแรงหนุนหลักจากลูกค้ากลุ่มแพ็กเกจและหน่วยงานรัฐ  
* กลุ่มแพ็กเกจ (PKG) ทำกำไรสูงสุด: เป็นกลุ่มที่สร้างรายได้ดีที่สุดในภาพรวม สะท้อนความสำเร็จของกลยุทธ์การขายแบบมัดรวม (Bundling)  
* กลุ่มหน่วยงานรัฐ (GOV) ครองตลาดห้อง Suite: เป็นฐานลูกค้าหลักที่สร้างผลตอบแทนสูงสุดให้กับห้องพักระดับสวีท  
* กลุ่มหมู่คณะ (GRP) ผลตอบแทนต่ำสุด: เนื่องจากมักได้เรทราคาพิเศษ จึงควรควบคุมโควตาห้องพัก (Inventory) อย่างระมัดระวังเพื่อไม่ให้เสียโอกาสขายราคาเต็ม  
* ห้อง Family มีความเสี่ยงกระจุกตัว: รายได้พึ่งพากลุ่มแพ็กเกจเพียงกลุ่มเดียว ควรทำการตลาดดึงดูดกลุ่มอื่น (เช่น Leisure หรือ Corporate) เพิ่มเติมเพื่อกระจายความเสี่ยง  

<img width="1007" height="598" alt="image" src="https://github.com/user-attachments/assets/f2031683-4611-499f-a5f5-ba7cf3591b36" />

---

Hypothesis 3 : การปรับราคาตามวันในสัปดาห์และฤดูกาล ส่งผลให้ RevPAR เพิ่มขึ้นอย่างมีนัยสำคัญ โดยไม่ลด Occupancy   
---
**กราฟ :** เปรียบเทียบ RevPAR ของ weekday/weekend  
**เหตุผลการใช้ :** Bar Chart (กราฟแท่ง) ใช้เพื่อเปรียบเทียบ 2 หมวดหมู่ที่ตัดขาดกันชัดเจน แสดงรายได้ของ weekday และ weekend ทำให้เห็นข้อมูลและเปรียบเทียบได้ชัดว่า weekday/weekend ที่มี RevPAR สูง   
โดยพบว่า  
* วันธรรมดาสร้างรายได้หลัก: RevPAR วันธรรมดาสูงกว่าวันหยุดกว่า 3 เท่า (281.17 vs 89.31 บาท) สะท้อนชัดเจนว่าฐานลูกค้าหลักคือกลุ่มองค์กรและหน่วยงานรัฐ (Business Hotel)  
* กลยุทธ์กระตุ้นวันหยุด (Weekend Strategy): ควรเร่งจัดโปรโมชันดึงดูดกลุ่มนักท่องเที่ยวและครอบครัว (เช่น Staycation) เพื่ออุดช่องโหว่รายได้ช่วงสุดสัปดาห์    
* บริหารราคาเพื่อเพิ่มกำไร (Yield Management): ช่วงวันธรรมดาที่มี Demand สูง ควรปรับราคาขึ้นและควบคุมโควตาส่วนลด เพื่อดัน RevPAR ให้เต็มศักยภาพ

<img width="1134" height="589" alt="image" src="https://github.com/user-attachments/assets/d289630b-e34b-4cef-9ad9-7d983b79a23d" />  

---  

**กราฟ :** เปรียบเทียบประสิทธิภาพของ RevPAR และ Occupancy ในแต่ละ Season 
**เหตุผลการใช้ :** Combo Chart - Bar + Line ผสมกราฟแท่ง (RevPAR) และ กราฟเส้น (Line Chart สำหรับ Occupancy %) เหตุผลที่ใช้กราฟเส้นเพราะตัวเลขอัตราการเข้าพักเป็นข้อมูลที่มีความต่อเนื่อง (Continuous trend) เปลี่ยนแปลงไปตามกาลเวลา (ฤดูกาล) การทาบเส้น Occ บนแท่งรายได้ ทำให้เห็นประสิทธิภาพการตั้งราคาในแต่ละช่วงเวลาได้อย่างเหมาะสม  
โดยพบว่า  
* High Season ยอดเยี่ยมสุด: ทำยอด RevPAR และอัตราเข้าพัก (OCC ทะลุ 30%) สูงสุดตามเป้าหมาย  
* Shoulder Season แข็งแกร่ง: รักษาระดับรายได้ใกล้เคียง High Season ช่วยพยุงโมเมนตัมธุรกิจได้อย่างดี  
* Low Season ซบเซาหนัก: OCC ดิ่งลงเหลือเพียง ~10% ต้องเร่งอัดโปรโมชันและเจาะกลุ่มสัมมนา (MICE) เพื่อพยุงรายได้


<img width="934" height="385" alt="image" src="https://github.com/user-attachments/assets/586aa69f-5805-49a7-b1a9-6293108ef6e6" />



---

Hypothesis 4 : Room Type มี RevPAR ต่ำในวันธรรมดา และยังขาด pricing/promotion ที่เหมาะสม ส่งผลให้ RevPAR โดยรวมของโรงแรมลดลง  
---
**กราฟ :** เปรียบเทียบ Room Type แต่ละประเภท โดยแยกเป็น Weekday/Weekend   
**เหตุผลการใช้ :** Bar Chart (กราฟแท่ง) เพื่อเจาะลึกเปรียบเทียบ Room Type แต่ละประเภท โดยแยกเป็น Weekday/Weekend 
แถบสีจะช่วยให้เห็นความเหลื่อมล้ำที่ชัดเจน  
โดยพบว่า  
* วันธรรมดาสร้างรายได้สูงสุดในทุกประเภทห้องพัก: แสดงถึงภาพลักษณ์ Business Hotel ที่มีฐานลูกค้าหลักเป็นกลุ่มผู้เดินทางเพื่อธุรกิจ    
* ห้อง Standard คือกลไกรายได้หลัก (Top Performer): ทำยอด RevPAR ในวันธรรมดาสูงสุดอย่างโดดเด่นเมื่อเทียบกับห้องประเภทอื่น  
* ห้อง Executive ทำผลงานเป็นอันดับสอง: สอดรับกับความต้องการของกลุ่มลูกค้าองค์กรและระดับผู้บริหารที่เดินทางมาปฏิบัติงาน  

<img width="1003" height="625" alt="image" src="https://github.com/user-attachments/assets/1737ef9b-69ba-4ab3-8823-1ef9ac275a39" />


---

Findings and Insights
---
1. ด้านช่องทางการจอง (Channel Performance & Profitability)  
* ช่องทาง **Direct Booking** ให้อัตรากำไรสุทธิ (Net RevPAR) สูงสุดเนื่องจากไม่มีภาระต้นทุนค่าธรรมเนียมตัวแทนจำหน่าย (Commission Fees) รองลงมาคือ **OTA** ที่ยังรักษาระดับรายได้และกำไรได้อย่างมีประสิทธิภาพ ในขณะที่ **Wholesaler/GDS** แม้จะมียอดจอง (Occupancy) สูง แต่ถูกหักค่าคอมมิชชันสูงมากจนกระทบต่อกำไรโดยตรง  
* **Insight:** โรงแรมกำลังเผชิญภาวะ **"Profit Margin Erosion"** (กำไรถดถอยสวนทางกับยอดจอง) ตัวเลข Occupancy ที่สูงอาจเป็นภาพลวงตา เพราะพึ่งพาช่องทางที่เน้นปริมาณ (Volume) แต่มีต้นทุนสูงเกินไป ทำให้กำไรสุทธิรั่วไหล  

2. ด้านประเภทห้องและกลุ่มลูกค้า (Product Matching & Segment)  
* **"Sweet Spot"** หรือจุดที่ทำเงินสูงสุดคือ การขายห้อง **Standard ให้กลุ่มลูกค้า Package** ซึ่งสร้าง RevPAR สูงสุดอย่างมีนัยสำคัญ (฿46.36) ในขณะที่การนำห้อง Premium (Deluxe/Suite) ไปขายพ่วงให้กลุ่ม Group กลับสร้างรายได้ต่ำมาก  
* **Insight:** ห้อง Standard และลูกค้ากลุ่ม Package คือขุมทรัพย์หลัก การพยายามระบายห้อง Premium ด้วยการหั่นราคาให้กลุ่ม Group ถือเป็นการจับคู่ (Matching) ที่ผิดพลาดและสูญเสียโอกาสในการทำกำไรมหาศาล  

3. ด้านช่วงเวลาและฤดูกาล (Seasonality & Day of Week)  
* **RevPAR ในช่วงวันธรรมดาสูงกว่าวันหยุดสุดสัปดาห์ถึง 3 เท่า** ในทุกหมวดหมู่ห้องพัก (รายได้วันหยุดหายไป 68%) โดยโรงแรมรักษาระดับผลประกอบการได้ดีเยี่ยมในช่วง High และ Shoulder Season แต่ในช่วง **Low Season อัตราการเข้าพักหดตัวรุนแรงเหลือเพียงระดับ 10%**  
* **Insight:** โรงแรมมีฐานลูกค้าวันธรรมดาที่แข็งแกร่งและบริหารราคา (Yield) ช่วงรอยต่อฤดูกาลได้ยอดเยี่ยม แต่มีจุดอ่อนร้ายแรงในการดึงดูดลูกค้ากลุ่มท่องเที่ยว (Leisure) ในวันหยุด ซึ่งเป็นช่วงเวลาที่ควรสร้างรายได้ได้ดีกว่านี้  

4. ด้านประสิทธิภาพห้องพักในช่วงวันหยุด (Weekend Premium Bottleneck)    
* ห้องทุกประเภททำผลงานได้ดีในวันธรรมดา แต่กราฟรายได้กลับ **"ดิ่งลงเหว" ในช่วงวันหยุดสุดสัปดาห์ (Weekend)** ซึ่งหักล้างสมมติฐานเดิมโดยสิ้นเชิง  
* **Insight:** ต้นตอของปัญหา **"Revenue Stagnation"** (รายได้รวมชะงัก) คือความล้มเหลวในการทำกำไรจากห้องกลุ่ม Premium ในวันเสาร์-อาทิตย์ โรงแรมไม่สามารถเจาะกลุ่มลูกค้าที่มีกำลังซื้อสูงในช่วงวันหยุดได้ตามศักยภาพที่มี  
---

Recommendations​ / Action & Impact
---
1. ด้านช่องทางการจอง (Channel Performance & Profitability)  
* Price Parity with Value Add: รักษาราคาหน้าเว็บให้เท่ากับ OTA  แต่ให้ "Instant Benefit" เฉพาะผู้ที่จอง Direct Booking เท่านั้น เช่น Free Breakfast, Early Check-in หรือ Food & Beverage Credit มูลค่า 500 บาท ซึ่งต้นทุนจริงของโรงแรมต่ำกว่าค่าคอมมิชชันที่เสียให้ OTA    
* Channel Allotment Control: ออกนโยบายจำกัดจำนวนห้องพักที่ปล่อยให้ Wholesaler/GDS ในช่วง High Season (ที่มีความต้องการสูง) เพื่อบังคับให้ Inventory ไหลมาสู่ช่องทาง Direct ที่กำไรสูงกว่าโดยธรรมชาติ    

2. ด้านประเภทห้องและกลุ่มลูกค้า (Product Matching & Segment)
* กระตุ้นขายห้อง Premium และปรับ Deluxe/Suite ด้วยการ Bundle/Upgrade: เลิกขายห้องประเภท Deluxe/Suite เปล่าๆ ในราคาลดพิเศษ แต่ให้ทำ "Experience Bundle" มาสร้างรายได้เพิ่มผ่านการ Bundle กับกลุ่ม Package (ที่เป็น Sweet Spot) แทน มาผูกกับบริการที่ต้นทุนต่ำแต่ Value สูงสำหรับลูกค้ากลุ่มนี้ ลูกค้าจะรู้สึกว่าเพิ่มเงินอีกนิดเดียวแต่ได้ความคุ้มค่า 
ช่วยให้ระบายห้อง Deluxe ได้ในราคาที่สูงกว่าการขาย Group Rate หลายเท่า  

3. ด้านช่วงเวลาและฤดูกาล (Seasonality & Day of Week)
* Weekend Leisure Pivot สร้างโครงสร้างราคาและแพ็กเกจแบบ "Leisure-Bundling" สำหรับวันเสาร์-อาทิตย์โดยเฉพาะ เพื่อดึงดูดกลุ่ม Staycation หรือครอบครัวในพื้นที่  
* High Season Yield Optimization: ทบทวนการตั้งราคาช่วง High Season ใหม่ โดยเน้นปรับ ADR ให้สูงขึ้นตาม Demand เพื่อดัน RevPAR ให้ทิ้งห่างจากช่วง Shoulder Season  

4. ด้านประสิทธิภาพห้องพักในช่วงวันหยุด (Weekend Premium Bottleneck)
* Premium Room Monetization for Weekend เปลี่ยนการขายห้อง Suite/Deluxe ในวันเสาร์-อาทิตย์ จากกลุ่ม B2B (Group/Wholesale) มาเน้นกลุ่ม B2C (Leisure/Couple/Family) ที่มีกำลังซื้อสูงกว่า  
* Product & Promotion: จัดแคมเปญ "Weekend Romantic Escape" หรือ "Family Weekend Fun" เน้นกลุ่ม Last-minute booking ผ่าน Social Media ในช่วงปลายสัปดาห์  



---

Documents link
---
Slide Presentation :  https://canva.link/lzxc4l2t0mitcv3  
VDO Presentation :  https://youtu.be/l4snI85z6ck

---
