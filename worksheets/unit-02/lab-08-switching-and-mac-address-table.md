# ใบงานหน่วยที่ 2 แล็บที่ 8

## การทำงานของ Switch และ MAC Address Table

| รายการ | รายละเอียด |
|---|---|
| รหัสใบงาน | CPT-U02-L08 |
| ระดับ | ผู้เริ่มต้น–พื้นฐาน |
| ระยะเวลาโดยประมาณ | 180 นาที |
| รูปแบบการทำงาน | รายบุคคล หรือกลุ่มละไม่เกิน 2 คน |
| แล็บที่ควรทำก่อน | หน่วยที่ 1 แล็บที่ 3–4 และหน่วยที่ 2 แล็บที่ 7 |
| ไฟล์ที่ต้องส่ง | `CPT-U02-L08-ชื่อผู้เรียน.pkt` และใบงานที่บันทึกผลแล้ว |

---

## 1. เรื่องของใบงาน

ใบงานนี้เป็นการศึกษาวิธีทำงานของ Layer 2 Switch โดยเน้นกระบวนการเรียนรู้ Source MAC Address การบันทึกข้อมูลลง MAC Address Table และการตัดสินใจส่งต่อ Ethernet Frame

ผู้เรียนจะสร้าง LAN ที่มี PC 4 เครื่อง เชื่อมกับ Switch 1 ตัว จากนั้นล้าง Dynamic MAC Address Table สร้าง Traffic ทีละขั้น ตรวจสอบว่าพอร์ตใดสัมพันธ์กับ MAC Address ใด และใช้ Simulation Mode เปรียบเทียบการส่งต่อแบบ **Flooding** กับ **Known Unicast Forwarding**

นอกจากนี้ ผู้เรียนจะกำหนด Management IP ให้ Switch ผ่าน Cisco IOS CLI และทดลองย้าย PC ไปยังพอร์ตใหม่เพื่อสังเกตการอัปเดต MAC Address Table

## 2. เป้าหมายการเรียนรู้

เมื่อทำใบงานเสร็จ ผู้เรียนควรสามารถทำสิ่งต่อไปนี้ได้

1. อธิบายหน้าที่ของ Layer 2 Switch ได้
2. อธิบายส่วนประกอบของ Ethernet Frame ที่ Switch ใช้ตัดสินใจได้
3. อธิบายว่า Switch เรียนรู้จาก Source MAC Address ได้
4. อ่านคอลัมน์ VLAN, MAC Address, Type และ Port ใน MAC Address Table ได้
5. ใช้ `show mac address-table` และคำสั่งกรองข้อมูลได้
6. ล้าง Dynamic MAC Address Table เพื่อเริ่มการทดลองใหม่ได้
7. อธิบาย Broadcast, Unknown Unicast และ Known Unicast ได้
8. ใช้ Simulation Mode สังเกต Flooding และ Forwarding ได้
9. เชื่อมโยง MAC Address ของ PC กับพอร์ตจริงบน Switch ได้
10. อธิบายผลเมื่ออุปกรณ์ถูกย้ายไปพอร์ตใหม่ได้
11. กำหนดและทดสอบ Management IP ของ Switch ได้
12. แก้ไขปัญหาจากพอร์ต สาย IP และข้อมูลใน MAC Table ได้

## 3. ความรู้พื้นฐานก่อนเริ่ม

ผู้เรียนควรสามารถ

- สร้าง LAN และกำหนด IPv4 แบบ Static ได้
- ใช้คำสั่ง `ping`, `arp -a` และ `arp -d` ได้
- ใช้ Simulation Mode, Event Filters และ Capture/Forward ได้
- เชื่อมต่อ Console และใช้งาน Cisco IOS CLI ได้
- ใช้คำสั่ง `enable`, `configure terminal`, `show` และ `copy` ได้

## 4. แนวคิดพื้นฐาน

### 4.1 Switch ทำงานที่ชั้นใด

Switch ทั่วไปในแล็บนี้ทำงานหลักที่ **OSI Layer 2 หรือ Data Link Layer** โดยพิจารณา Source MAC Address และ Destination MAC Address ใน Ethernet Frame

### 4.2 MAC Address Table คืออะไร

MAC Address Table เป็นตารางที่ Switch ใช้เก็บความสัมพันธ์ระหว่าง

```text
VLAN + MAC Address + Interface
```

ตัวอย่างรูปแบบผลลัพธ์

```text
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    00d0.ba12.3456    DYNAMIC     Fa0/1
```

ค่า MAC จริงในไฟล์ของผู้เรียนจะแตกต่างจากตัวอย่าง

### 4.3 Switch เรียนรู้ MAC อย่างไร

เมื่อ Frame เข้ามาที่พอร์ต Switch จะอ่าน **Source MAC Address** และบันทึกว่า MAC นั้นอยู่หลังพอร์ตขาเข้า

```text
Frame จาก PC1 เข้าที่ Fa0/1
Source MAC = MAC ของ PC1

Switch เรียนรู้:
MAC ของ PC1 → Fa0/1
```

> Switch เรียนรู้ตำแหน่งจาก Source MAC ไม่ใช่ Destination MAC

### 4.4 การส่งต่อ Frame

| สถานการณ์ | การทำงานของ Switch |
|---|---|
| Destination MAC อยู่ใน Table และอยู่คนละพอร์ต | ส่งออกเฉพาะพอร์ตที่บันทึกไว้ หรือ Known Unicast Forwarding |
| Destination MAC ไม่อยู่ใน Table | Flood ออกพอร์ตอื่นใน VLAN เดียวกัน ยกเว้นพอร์ตขาเข้า |
| Destination MAC เป็น Broadcast | Flood ออกพอร์ตอื่นใน VLAN เดียวกัน |
| Destination MAC อยู่พอร์ตเดียวกับขาเข้า | กรอง Frame ไม่ส่งกลับออกพอร์ตเดิม |

### 4.5 Unknown Unicast และ Broadcast ต่างกันอย่างไร

- **Broadcast** มี Destination MAC เป็น `FFFF.FFFF.FFFF` และตั้งใจส่งถึงทุก Host ใน Broadcast Domain
- **Unknown Unicast** มี Destination MAC ของ Host เพียงตัวเดียว แต่ Switch ยังไม่รู้ว่า MAC นั้นอยู่พอร์ตใด จึงต้อง Flood ชั่วคราว

### 4.6 Dynamic MAC และ Aging

รายการที่ Switch เรียนรู้จาก Traffic จะมี Type เป็น Dynamic และจะถูกลบเมื่อไม่มี Traffic จาก MAC นั้นตามเวลาที่กำหนด ค่าเริ่มต้นที่พบได้ทั่วไปคือประมาณ 300 วินาที แต่ค่าจริงขึ้นอยู่กับอุปกรณ์และ Configuration

แล็บนี้ไม่รอให้ Aging Timer หมด แต่ใช้คำสั่งล้าง Dynamic Table เพื่อเริ่มสถานการณ์ใหม่ทันที

### 4.7 ARP Table กับ MAC Address Table

| ตาราง | อยู่ที่ใด | เก็บข้อมูลอะไร |
|---|---|---|
| ARP Table/Cache | PC, Router หรืออุปกรณ์ Layer 3 | IPv4 Address ↔ MAC Address |
| MAC Address Table | Switch | MAC Address ↔ Switch Port ภายใน VLAN |

ตารางทั้งสองมีหน้าที่ต่างกันและสามารถถูกล้างแยกจากกันได้

## 5. อุปกรณ์และซอฟต์แวร์ที่ใช้

### 5.1 ซอฟต์แวร์

- โปรแกรม Cisco Packet Tracer จำนวน 1 ชุด
- โฟลเดอร์สำหรับบันทึกไฟล์งานของผู้เรียน

### 5.2 อุปกรณ์จำลองใน Packet Tracer

| ลำดับ | ประเภทอุปกรณ์ | รุ่นที่แนะนำ | จำนวน | ชื่อที่กำหนด |
|---:|---|---|---:|---|
| 1 | Switch | Cisco 2960 | 1 | `SW1` |
| 2 | PC | PC-PT | 1 | `ADMIN-PC` |
| 3 | PC | PC-PT | 3 | `PC1`, `PC2`, `PC3` |

ADMIN-PC ใช้ทั้งพอร์ต RS-232 สำหรับ Console และ FastEthernet0 สำหรับ Management Traffic

## 6. แผนผังเครือข่าย

```text
PC1 Fa0 -------------- Fa0/1  
                               \
PC2 Fa0 -------------- Fa0/2   \
                                  SW1
PC3 Fa0 -------------- Fa0/3   /
                               /
ADMIN-PC Fa0 --------- Fa0/24
         RS-232 ------ Console
```

สาย Ethernet ทุกเส้นเป็น Copper Straight-Through และสายบริหารเป็น Console Cable

## 7. ตารางกำหนด IP Address

เครือข่ายที่ใช้คือ `192.168.80.0/24`

| อุปกรณ์ | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| SW1 | VLAN 1 | `192.168.80.2` | `255.255.255.0` | ไม่กำหนด |
| ADMIN-PC | FastEthernet0 | `192.168.80.10` | `255.255.255.0` | ไม่กำหนด |
| PC1 | FastEthernet0 | `192.168.80.11` | `255.255.255.0` | ไม่กำหนด |
| PC2 | FastEthernet0 | `192.168.80.12` | `255.255.255.0` | ไม่กำหนด |
| PC3 | FastEthernet0 | `192.168.80.13` | `255.255.255.0` | ไม่กำหนด |

แล็บนี้ไม่มี Router อุปกรณ์ทั้งหมดอยู่ใน Network เดียวกัน จึงไม่ต้องกำหนด Default Gateway

## 8. ตารางการเชื่อมต่อและสายสัญญาณ

| ลำดับ | อุปกรณ์ต้นทาง | พอร์ตต้นทาง | อุปกรณ์ปลายทาง | พอร์ตปลายทาง | สายที่ใช้ |
|---:|---|---|---|---|---|
| 1 | PC1 | FastEthernet0 | SW1 | FastEthernet0/1 | Copper Straight-Through |
| 2 | PC2 | FastEthernet0 | SW1 | FastEthernet0/2 | Copper Straight-Through |
| 3 | PC3 | FastEthernet0 | SW1 | FastEthernet0/3 | Copper Straight-Through |
| 4 | ADMIN-PC | FastEthernet0 | SW1 | FastEthernet0/24 | Copper Straight-Through |
| 5 | ADMIN-PC | RS-232 | SW1 | Console | Console Cable |

## 9. ตารางบันทึก MAC Address ของอุปกรณ์

เปิด Config ของ PC แต่ละเครื่อง ดู MAC Address ที่ FastEthernet0 และบันทึกค่าจริง

| อุปกรณ์ | IP Address | MAC Address ที่พบ | พอร์ต SW1 ที่ควรเรียนรู้ |
|---|---|---|---|
| ADMIN-PC | `192.168.80.10` | ................................ | Fa0/24 |
| PC1 | `192.168.80.11` | ................................ | Fa0/1 |
| PC2 | `192.168.80.12` | ................................ | Fa0/2 |
| PC3 | `192.168.80.13` | ................................ | Fa0/3 |

> Packet Tracer สร้าง MAC Address ให้แต่ละอุปกรณ์ ห้ามใช้ค่าตัวอย่างหรือคัดลอกจากไฟล์ของผู้อื่น

## 10. ขั้นตอนการปฏิบัติงาน

### ตอนที่ 1: สร้างและบันทึกไฟล์งาน

1. เปิด Cisco Packet Tracer
2. เลือก File > New
3. บันทึกเป็น `CPT-U02-L08-ชื่อผู้เรียน.pkt`
4. วาง SW1, ADMIN-PC, PC1, PC2 และ PC3
5. ตั้ง Display Name ให้ตรงตามตาราง
6. ต่อสายตามข้อ 8
7. รอให้ลิงก์เป็นสีเขียว
8. กด `Ctrl+S`

### ตอนที่ 2: กำหนด IPv4 ให้ PC

เปิด **Desktop > IP Configuration** ของ PC แต่ละเครื่อง เลือก Static แล้วกำหนดค่าตามข้อ 7

| อุปกรณ์ | IP ที่กำหนด | Mask ที่กำหนด | ตรวจแล้ว |
|---|---|---|:---:|
| ADMIN-PC | `192.168.80.10` | `255.255.255.0` | ☐ |
| PC1 | `192.168.80.11` | `255.255.255.0` | ☐ |
| PC2 | `192.168.80.12` | `255.255.255.0` | ☐ |
| PC3 | `192.168.80.13` | `255.255.255.0` | ☐ |

เว้น Default Gateway และ DNS Server ว่าง

### ตอนที่ 3: เปิด Console และกำหนดค่าพื้นฐาน SW1

1. เปิด **ADMIN-PC > Desktop > Terminal**
2. ใช้ค่า Terminal เริ่มต้น
3. หากพบ Initial Configuration Dialog ให้ตอบ `no`
4. กด Enter จนเห็น `Switch>`
5. พิมพ์คำสั่งทีละบรรทัด

```text
enable
configure terminal
hostname SW1
no ip domain-lookup
enable secret Enable123
username admin secret Admin123
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
```

### ตอนที่ 4: กำหนด Management IP ของ SW1

ที่ `SW1(config)#` พิมพ์

```text
interface vlan 1
 description MANAGEMENT_SVI
 ip address 192.168.80.2 255.255.255.0
 no shutdown
exit
```

แล็บนี้ไม่มี Router จึงไม่ต้องกำหนด `ip default-gateway`

### ตอนที่ 5: กำหนดพอร์ตและ Description

ที่ `SW1(config)#` พิมพ์

```text
interface fastEthernet 0/1
 description PC1
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/2
 description PC2
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/3
 description PC3
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/24
 description ADMIN_PC
 switchport mode access
 no shutdown
end
```

ตรวจสอบ

```text
show interfaces description
show ip interface brief
```

สิ่งที่คาดหวัง

- Fa0/1, Fa0/2, Fa0/3 และ Fa0/24 เป็น up/up
- VLAN 1 ใช้ `192.168.80.2` และเป็น up/up

### ตอนที่ 6: บันทึก SW1

```text
copy running-config startup-config
```

กด Enter ยอมรับ Destination Filename แล้วตรวจ

```text
show startup-config
```

### ตอนที่ 7: ทดสอบ Management IP

ที่ ADMIN-PC เปิด Command Prompt แล้วพิมพ์

```text
ipconfig
ping 192.168.80.2
```

ผลที่คาดหวังคือ ADMIN-PC ได้รับ Reply จาก Management IP ของ SW1

### ตอนที่ 8: บันทึก MAC Address จริง

1. เปิด Config ของ ADMIN-PC, PC1, PC2 และ PC3
2. ดู MAC Address ของ FastEthernet0
3. บันทึกในตารางข้อ 9
4. ตรวจรูปแบบ MAC ให้ครบ 12 Hexadecimal Digits

ตัวอย่างรูปแบบที่อาจพบ

```text
00D0.BA12.3456
```

หรือ

```text
00:D0:BA:12:34:56
```

### ตอนที่ 9: ตรวจ MAC Address Table ก่อนล้าง

ที่ Console ของ SW1 พิมพ์

```text
show mac address-table
```

บันทึกจำนวนรายการ Dynamic ที่พบก่อนล้าง

```text
จำนวน Dynamic Entries ก่อนล้าง: ........ รายการ
```

เหตุผลที่อาจมีรายการอยู่แล้วคือ PC เคยส่ง Frame เช่น ARP หรือ Ping ระหว่างการกำหนดค่าและทดสอบ

### ตอนที่ 10: ล้าง Dynamic MAC Address Table

ที่ `SW1#` พิมพ์

```text
clear mac address-table dynamic
```

ตรวจสอบทันที

```text
show mac address-table dynamic
```

ผลที่คาดหวังคือไม่มี Dynamic Entry หรือมีน้อยกว่าก่อนล้างอย่างชัดเจน

> หาก IOS จำลองไม่ยอมรับ Syntax ให้ใช้ `clear mac address-table ?` เพื่อดูตัวเลือก หรือทดลองรูปแบบ `clear mac-address-table dynamic` ที่พบใน IOS บางรุ่น

### ตอนที่ 11: เรียนรู้ MAC ของ PC1 และ PC2

1. ที่ PC1 เปิด Command Prompt
2. พิมพ์

```text
ping 192.168.80.12
```

3. ทำซ้ำหากครั้งแรกสูญหายหนึ่ง Packet
4. กลับ Console ของ SW1 แล้วพิมพ์

```text
show mac address-table dynamic
```

5. บันทึกผลที่สัมพันธ์กับ PC1 และ PC2

| MAC Address | Type | VLAN | Port | ตรงกับอุปกรณ์ |
|---|---|---:|---|---|
| ................................ | Dynamic | ........ | ........ | PC1 / PC2 |
| ................................ | Dynamic | ........ | ........ | PC1 / PC2 |

สิ่งที่ควรพบ

- MAC ของ PC1 อยู่ Fa0/1
- MAC ของ PC2 อยู่ Fa0/2

### ตอนที่ 12: เรียนรู้ MAC ของ PC3 และ ADMIN-PC

ที่ PC1 พิมพ์

```text
ping 192.168.80.13
ping 192.168.80.10
```

จากนั้นที่ SW1 พิมพ์

```text
show mac address-table dynamic
```

บันทึกตารางหลังสร้าง Traffic ครบทุก PC

| อุปกรณ์ | MAC Address | VLAN | Type | Port ที่พบ | ตรงตาม Topology หรือไม่ |
|---|---|---:|---|---|---|
| PC1 | ................................ | ........ | ........ | ........ | ตรง / ไม่ตรง |
| PC2 | ................................ | ........ | ........ | ........ | ตรง / ไม่ตรง |
| PC3 | ................................ | ........ | ........ | ........ | ตรง / ไม่ตรง |
| ADMIN-PC | ................................ | ........ | ........ | ........ | ตรง / ไม่ตรง |

### ตอนที่ 13: กรองข้อมูลตามพอร์ต

ที่ SW1 พิมพ์

```text
show mac address-table interface fastEthernet 0/1
```

บันทึก MAC ที่พบ

....................................................................................................

จากนั้นทดลอง

```text
show mac address-table interface fastEthernet 0/2
show mac address-table interface fastEthernet 0/3
show mac address-table interface fastEthernet 0/24
```

> หาก IOS รุ่นนั้นไม่รองรับรูปแบบกรอง Interface ให้ใช้ `show mac address-table ?` ดู Syntax ที่รองรับ แล้วใช้ `show mac address-table dynamic` เปรียบเทียบด้วยตนเอง

### ตอนที่ 14: ทดลองย้าย PC3 ไปพอร์ตใหม่

1. บันทึกว่า MAC ของ PC3 อยู่ Fa0/3
2. ใช้ Delete ลบเฉพาะสายระหว่าง PC3 และ SW1
3. ต่อ PC3 FastEthernet0 ไป SW1 FastEthernet0/10 ด้วย Copper Straight-Through
4. รอให้ลิงก์เป็นสีเขียว
5. ที่ PC3 Ping PC1

```text
ping 192.168.80.11
```

6. ที่ SW1 พิมพ์

```text
show mac address-table dynamic
```

7. บันทึกผล

| รายการ | ก่อนย้าย | หลังย้าย |
|---|---|---|
| MAC ของ PC3 | ................................ | ................................ |
| Port ที่ SW1 บันทึก | Fa0/3 | ................................ |

คำอธิบาย: เมื่อ Switch ได้รับ Frame ใหม่จาก Source MAC ของ PC3 ที่ Fa0/10 จะอัปเดตตำแหน่งของ MAC นั้นเป็นพอร์ตใหม่

### ตอนที่ 15: คืน PC3 ไปพอร์ตเดิม

1. ลบสาย PC3–Fa0/10
2. ต่อ PC3 กลับไป SW1 Fa0/3
3. รอให้ลิงก์ทำงาน
4. ที่ PC3 Ping PC1
5. ตรวจ MAC Table ว่า MAC ของ PC3 กลับมา Fa0/3

### ตอนที่ 16: เตรียมการทดลอง Unknown Unicast

เป้าหมายคือทำให้ PC1 รู้ MAC ของ PC2 แต่ SW1 ไม่รู้ว่าปัจจุบัน MAC ของ PC2 อยู่พอร์ตใด

1. อยู่ใน Realtime Mode
2. ที่ PC1 Ping PC2 ให้สำเร็จ

```text
ping 192.168.80.12
```

3. ที่ PC1 ตรวจ ARP Cache

```text
arp -a
```

4. ยืนยันว่ามี IP `192.168.80.12` และ MAC ของ PC2
5. อย่าล้าง ARP Cache ของ PC1
6. ที่ Console ของ SW1 ล้างเฉพาะ Dynamic MAC Table

```text
clear mac address-table dynamic
show mac address-table dynamic
```

7. ตรวจว่า MAC ของ PC2 ไม่อยู่ใน Dynamic Table ของ SW1

สถานะที่ต้องได้ก่อนทดลอง

| ตาราง | มีข้อมูลของ PC2 หรือไม่ |
|---|---|
| ARP Cache ของ PC1 | มี |
| MAC Address Table ของ SW1 | ไม่มี |

### ตอนที่ 17: สังเกต Unknown Unicast ใน Simulation Mode

1. เปลี่ยนเป็น **Simulation Mode**
2. เปิด Edit Filters และเลือกเฉพาะ ARP กับ ICMP
3. ล้าง Event List เก่า
4. ใช้ **Add Simple PDU** จาก PC1 ไป PC2
5. กด Capture/Forward ทีละขั้น
6. เปิด PDU Details ของ Frame ที่เข้าสู่ SW1
7. ตรวจ Destination MAC ว่าเป็น MAC ของ PC2 ไม่ใช่ Broadcast
8. สังเกตการส่งออกจาก SW1

บันทึกสิ่งที่พบ

- พอร์ตขาเข้า: ................................
- พอร์ตที่ SW1 ส่งออก: .............................................................................
- อุปกรณ์ที่ได้รับสำเนา Frame: ...................................................................
- อุปกรณ์ใดรับข้อมูลต่อ: ............................................................................
- อุปกรณ์ใดทิ้ง Frame เพราะ Destination MAC ไม่ตรง: ..............................................

คำอธิบายที่คาดหวัง: SW1 ยังไม่รู้ Destination MAC ของ PC2 จึง Flood Unknown Unicast ออกพอร์ตอื่นใน VLAN 1 ยกเว้นพอร์ตขาเข้า แต่มีเพียง PC2 ที่รับ Frame ไปประมวลผลต่อ

> หากเห็น ARP ก่อน ICMP แสดงว่า ARP Cache ของ PC1 ไม่มีรายการ PC2 ให้กลับ Realtime, Ping ให้สำเร็จ, ตรวจ `arp -a`, ล้างเฉพาะ MAC Table ของ SW1 แล้วเริ่มใหม่

### ตอนที่ 18: สังเกต Known Unicast

หลังการทดลองครั้งแรก SW1 ควรเรียนรู้ Source MAC ของ PC2 จาก Frame ตอบกลับ

1. ปล่อยให้ Simulation Scenario แรกทำงานจนจบ
2. ตรวจที่ SW1 ว่า MAC ของ PC2 อยู่ Fa0/2
3. อย่าล้าง MAC Table
4. เริ่ม Scenario ใหม่หรือล้างเฉพาะ Event List
5. Add Simple PDU จาก PC1 ไป PC2 อีกครั้ง
6. กด Capture/Forward
7. บันทึกพอร์ตที่ SW1 ส่ง Frame ออก

```text
Known Unicast ถูกส่งออกพอร์ต: ........................
```

ผลที่คาดหวังคือ SW1 ส่ง Frame เฉพาะ Fa0/2 ไม่ Flood ไป PC3 หรือ ADMIN-PC

### ตอนที่ 19: เปรียบเทียบผล

| รายการ | Unknown Unicast | Known Unicast |
|---|---|---|
| Destination MAC อยู่ใน Table หรือไม่ | ไม่อยู่ | อยู่ |
| วิธีส่งต่อของ SW1 | ................................ | ................................ |
| จำนวนพอร์ตขาออกโดยทั่วไป | ................................ | ................................ |
| Host อื่นได้รับสำเนาหรือไม่ | ................................ | ................................ |

### ตอนที่ 20: บันทึกและตรวจขั้นสุดท้าย

1. กลับ Realtime Mode
2. ตรวจ PC3 เชื่อมกลับ Fa0/3
3. Ping จาก PC1 ไป PC2, PC3 และ ADMIN-PC
4. Ping Management IP `192.168.80.2`
5. ตรวจ MAC Table ให้สัมพันธ์กับพอร์ตจริง
6. บันทึก Startup Configuration ของ SW1 อีกครั้ง

```text
copy running-config startup-config
```

7. กด `Ctrl+S` บันทึกไฟล์ Packet Tracer
8. ปิดและเปิดไฟล์ใหม่
9. ทดสอบ Ping และ MAC Learning อีกครั้ง

## 11. ชุดคำสั่งกำหนดค่า SW1 ฉบับรวม

ใช้ตรวจเทียบหลังจากทำตามขั้นตอนแล้ว

```text
enable
configure terminal
hostname SW1
no ip domain-lookup
enable secret Enable123
username admin secret Admin123
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
interface vlan 1
 description MANAGEMENT_SVI
 ip address 192.168.80.2 255.255.255.0
 no shutdown
exit
interface fastEthernet 0/1
 description PC1
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/2
 description PC2
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/3
 description PC3
 switchport mode access
 no shutdown
exit
interface fastEthernet 0/24
 description ADMIN_PC
 switchport mode access
 no shutdown
end
copy running-config startup-config
```

## 12. คำสั่งตรวจสอบที่ใช้ในแล็บ

### 12.1 Cisco IOS บน SW1

| คำสั่ง | หน้าที่ |
|---|---|
| `show mac address-table` | แสดง MAC Address Table ทั้งหมด |
| `show mac address-table dynamic` | แสดงเฉพาะ Dynamic Entries |
| `show mac address-table interface fastEthernet 0/1` | กรองรายการตาม Interface หาก IOS รองรับ |
| `clear mac address-table dynamic` | ล้าง Dynamic Entries |
| `show interfaces description` | ตรวจ Port Status และ Description |
| `show ip interface brief` | ตรวจ Management IP และ Interface Status |
| `show running-config` | ตรวจ Configuration ปัจจุบัน |
| `copy running-config startup-config` | บันทึก Configuration |

### 12.2 Command Prompt บน PC

| คำสั่ง | หน้าที่ |
|---|---|
| `ipconfig` | ตรวจ IP Address ของ PC |
| `ping <destination-ip>` | สร้าง Traffic และทดสอบการเชื่อมต่อ |
| `arp -a` | ตรวจความสัมพันธ์ IP–MAC ใน ARP Cache |
| `arp -d` | ล้าง Dynamic ARP Cache ตาม Syntax ที่รุ่นรองรับ |

## 13. แบบฝึกอ่าน MAC Address Table

พิจารณาตารางสมมติ

```text
Vlan    Mac Address       Type        Ports
----    -----------       --------    -----
   1    0001.1111.1111    DYNAMIC     Fa0/1
   1    0002.2222.2222    DYNAMIC     Fa0/2
   1    0003.3333.3333    DYNAMIC     Fa0/3
```

ตอบคำถาม

1. Frame จาก Source MAC `0002.2222.2222` เข้าทางพอร์ตใดตามตาราง

   คำตอบ: ............................................................................................

2. หาก Frame เข้าทาง Fa0/1 และ Destination MAC เป็น `0003.3333.3333` Switch ส่งออกพอร์ตใด

   คำตอบ: ............................................................................................

3. หาก Destination MAC เป็น `0004.4444.4444` ซึ่งไม่มีในตาราง Switch ทำอย่างไร

   คำตอบ: ............................................................................................

4. หาก Frame เข้าทาง Fa0/1 และ Destination MAC ก็อยู่ Fa0/1 Switch ทำอย่างไร

   คำตอบ: ............................................................................................

## 14. การทดสอบการทำงานขั้นสุดท้าย

| ลำดับ | รายการทดสอบ | เกณฑ์ที่ผ่าน | ผ่าน | ไม่ผ่าน |
|---:|---|---|:---:|:---:|
| 1 | Topology | PC 4 เครื่องเชื่อม SW1 ตามตาราง และ Console ถูกต้อง | ☐ | ☐ |
| 2 | IP Address | ทุก PC ใช้ `192.168.80.0/24` และ IP ไม่ซ้ำ | ☐ | ☐ |
| 3 | Management IP | SW1 VLAN 1 เป็น `192.168.80.2/24`, up/up | ☐ | ☐ |
| 4 | Ping | PC1 Ping PC2, PC3, ADMIN-PC และ SW1 ผ่าน | ☐ | ☐ |
| 5 | MAC Recording | บันทึก MAC จริงของ PC ครบ 4 เครื่อง | ☐ | ☐ |
| 6 | Clear Dynamic | ล้างและตรวจ Dynamic Table ได้ | ☐ | ☐ |
| 7 | MAC Learning | MAC แต่ละเครื่องสัมพันธ์กับพอร์ตถูกต้อง | ☐ | ☐ |
| 8 | Port Move | สังเกต PC3 ย้าย Fa0/3→Fa0/10 และคืนกลับได้ | ☐ | ☐ |
| 9 | Unknown Unicast | อธิบาย Flooding เมื่อไม่พบ Destination MAC ได้ | ☐ | ☐ |
| 10 | Known Unicast | อธิบาย Forwarding ออกพอร์ตเดียวได้ | ☐ | ☐ |
| 11 | Persistence | Startup Config และไฟล์ `.pkt` สมบูรณ์ | ☐ | ☐ |

### ผลการทดสอบโดยรวม

- [ ] ผ่านครบทุกข้อ
- [ ] ผ่านบางข้อและแก้ไขเรียบร้อยแล้ว
- [ ] ยังมีปัญหาและต้องการความช่วยเหลือ

## 15. แนวทางแก้ไขปัญหาเบื้องต้น

| อาการ | สาเหตุที่เป็นไปได้ | วิธีตรวจสอบหรือแก้ไข |
|---|---|---|
| MAC Table ว่างหลัง Ping | Ping ไม่สำเร็จ ลิงก์ไม่ทำงาน หรือเพิ่งล้าง Table | ตรวจ IP/สาย แล้ว Ping ใหม่ก่อน Show |
| MAC อยู่ผิดพอร์ต | ต่อสายไม่ตรงตาราง หรือเพิ่งย้าย PC | ตรวจ Physical Connection และสร้าง Traffic จาก PC นั้น |
| ไม่พบ MAC ของ PC3 หลังย้าย | PC3 ยังไม่ได้ส่ง Frame จากพอร์ตใหม่ | Ping จาก PC3 เพื่อให้ SW1 เรียนรู้ Source MAC |
| พบ MAC เดียวมากกว่าหนึ่งครั้งชั่วคราว | Table/Simulation ยังไม่อัปเดตหรือ Scenario เก่า | สร้าง Traffic ใหม่และตรวจ Dynamic Table อีกครั้ง |
| VLAN 1 down/down | ไม่มี Access Port ใน VLAN 1 ที่ Up | ตรวจ Fa0/1–3, Fa0/24 และสาย |
| Ping SW1 ไม่ได้ | SVI IP/Mask ผิดหรือ VLAN 1 ไม่ Up | ตรวจ `show ip interface brief` |
| ไม่เห็น Unknown Unicast | SW1 ยังมี MAC ของ PC2 | ล้างเฉพาะ Dynamic MAC Table และยืนยันว่า PC1 ยังมี ARP Entry |
| เห็น ARP ในการทดลอง Unknown Unicast | PC1 ไม่มี ARP Entry ของ PC2 | Ping ก่อน ตรวจ `arp -a` แล้วล้างเฉพาะ Switch Table |
| Simulation Flood ไม่ชัด | Filter หรือ Event List มี Traffic อื่น | เลือก ARP/ICMP และเริ่ม Scenario ใหม่ |
| Clear Command ไม่ทำงาน | IOS รุ่นนั้นใช้ Syntax ต่างกัน | ใช้ `clear mac address-table ?` หรือ Hyphen Syntax ตาม Help |
| Ping ครั้งแรกสูญหายหนึ่ง Packet | ARP หรือ MAC Learning กำลังเกิดขึ้น | Ping ซ้ำและใช้เป็นข้อมูลในการวิเคราะห์ |

## 16. แบบบันทึกผลการปฏิบัติงาน

| รายการ | ข้อมูลของผู้เรียน |
|---|---|
| ชื่อ–นามสกุล | ............................................................ |
| ชั้น/กลุ่ม | ............................................................ |
| วันที่ทำแล็บ | ............................................................ |
| ชื่อไฟล์ที่ส่ง | ............................................................ |
| เวลาที่ใช้จริง | ............................................................ |

### สิ่งที่สังเกตได้จาก MAC Learning

....................................................................................................

....................................................................................................

### ความแตกต่างระหว่าง Unknown และ Known Unicast

....................................................................................................

....................................................................................................

### ปัญหาที่พบและวิธีแก้ไข

....................................................................................................

....................................................................................................

## 17. คำถามท้ายแล็บ

1. Switch ใช้ Address ใดในการสร้าง MAC Address Table

   คำตอบ: ............................................................................................

2. Switch เรียนรู้จาก Source MAC หรือ Destination MAC เพราะเหตุใด

   คำตอบ: ............................................................................................

3. คอลัมน์ Port ใน MAC Address Table หมายถึงอะไร

   คำตอบ: ............................................................................................

4. Switch ทำอย่างไรเมื่อ Destination MAC ไม่มีใน Table

   คำตอบ: ............................................................................................

5. Known Unicast แตกต่างจาก Unknown Unicast อย่างไร

   คำตอบ: ............................................................................................

6. เพราะเหตุใด MAC ของ PC3 จึงเปลี่ยนจาก Fa0/3 เป็น Fa0/10 หลังย้ายสายและ Ping

   คำตอบ: ............................................................................................

7. ARP Table และ MAC Address Table เก็บข้อมูลต่างกันอย่างไร

   คำตอบ: ............................................................................................

8. เหตุใดแล็บนี้จึงต้องให้ PC1 มี ARP Entry ของ PC2 แต่ล้าง MAC Table ของ SW1 เพื่อสร้าง Unknown Unicast

   คำตอบ: ............................................................................................

9. เหตุใด Host ที่ได้รับ Flooded Unknown Unicast แต่ Destination MAC ไม่ตรงจึงทิ้ง Frame

   คำตอบ: ............................................................................................

10. Management IP จำเป็นต่อการส่งต่อ Frame ปกติของ Layer 2 Switch หรือไม่

    คำตอบ: ...........................................................................................

## 18. สรุปผลการเรียนรู้โดยผู้เรียน

หลังทำแล็บนี้ ฉันสามารถ

- [ ] อธิบาย Source MAC Learning ได้
- [ ] อ่าน MAC Address Table ได้
- [ ] ใช้คำสั่ง Show และ Clear MAC Table ได้
- [ ] เชื่อม MAC Address กับพอร์ตจริงได้
- [ ] อธิบาย Broadcast และ Unknown Unicast Flooding ได้
- [ ] อธิบาย Known Unicast Forwarding ได้
- [ ] สังเกตการเปลี่ยนพอร์ตของ MAC หลังย้ายอุปกรณ์ได้
- [ ] แยก ARP Cache และ MAC Address Table ได้
- [ ] กำหนดและทดสอบ Management IP ได้

สิ่งสำคัญที่ได้เรียนรู้จากแล็บนี้คือ

....................................................................................................

....................................................................................................

สิ่งที่ต้องการฝึกเพิ่มเติมคือ

....................................................................................................

....................................................................................................

## 19. เกณฑ์การประเมิน

| หัวข้อประเมิน | คะแนนเต็ม |
|---|---:|
| Topology, IP และ Management SVI ถูกต้อง | 2 |
| บันทึกและอ่าน MAC Address Table ได้ | 2 |
| อธิบาย Source MAC Learning และ Port Move ได้ | 2 |
| สาธิต Unknown/Known Unicast ใน Simulation ได้ | 2 |
| Troubleshooting บันทึกผล ตอบคำถาม และสรุปผลได้ | 2 |
| **รวม** | **10** |

เกณฑ์ผ่านที่แนะนำ: **อย่างน้อย 7 คะแนนจาก 10 คะแนน** และผู้เรียนต้องสามารถระบุ MAC ของ PC1–PC3 กับพอร์ต Fa0/1–Fa0/3 ได้ถูกต้อง

## 20. งานที่ต้องส่ง

1. ไฟล์ Packet Tracer ชื่อ `CPT-U02-L08-ชื่อผู้เรียน.pkt`
2. ใบงานที่บันทึก MAC จริง ตาราง Dynamic MAC ผลย้ายพอร์ต ผล Unknown/Known Unicast คำถามท้ายแล็บ และสรุปผลครบถ้วน

---

## ภาคผนวกสำหรับผู้สอน: แนวคำตอบย่อ

### ตาราง MAC ที่คาดหวังหลังสร้าง Traffic ครบ

| อุปกรณ์ | Port ที่คาดหวัง | Type |
|---|---|---|
| PC1 | Fa0/1 | Dynamic |
| PC2 | Fa0/2 | Dynamic |
| PC3 | Fa0/3 | Dynamic |
| ADMIN-PC | Fa0/24 | Dynamic |

MAC Address จริงต้องตรวจจากไฟล์ของผู้เรียน

### เฉลยแบบฝึกอ่านตาราง

1. Fa0/2
2. Fa0/3
3. Flood ออกพอร์ตอื่นใน VLAN 1 ยกเว้นพอร์ตขาเข้า
4. Filter Frame และไม่ส่งกลับออกพอร์ตเดียวกับที่รับเข้ามา

### แนวคำตอบคำถามท้ายแล็บ

1. Source MAC Address พร้อม VLAN และพอร์ตขาเข้า
2. Source MAC เพราะพอร์ตขาเข้าคือตำแหน่งที่ Switch ยืนยันได้ว่า Source นั้นเข้ามาจากที่ใด
3. Interface ที่ Switch เรียนรู้ว่าใช้เข้าถึง MAC Address นั้น
4. Flood Frame ออกพอร์ตอื่นใน VLAN เดียวกัน ยกเว้นพอร์ตขาเข้า
5. Known Unicast มี Destination MAC อยู่ใน Table และส่งออกพอร์ตที่ระบุ ส่วน Unknown Unicast ไม่มี Entry จึงถูก Flood
6. Switch ได้รับ Frame ที่มี Source MAC เดิมจากพอร์ต Fa0/10 จึงอัปเดต Mapping
7. ARP เก็บ IP↔MAC ที่อุปกรณ์ Layer 3 ใช้ ส่วน MAC Table เก็บ MAC↔Switch Port แยกตาม VLAN
8. เพื่อให้ PC1 สร้าง Frame ที่มี Destination MAC ของ PC2 ได้ทันที แต่บังคับให้ SW1 พบว่า Destination MAC ยังไม่มีใน Table
9. NIC ตรวจ Destination MAC แล้วพบว่าไม่ใช่ของตนและไม่ใช่ Broadcast/Multicast ที่รับ จึงไม่ส่งขึ้นชั้นบน
10. ไม่จำเป็นต่อการส่งต่อ Frame Layer 2 ปกติ Management IP ใช้บริหารและตรวจสอบ Switch ผ่านเครือข่าย

### ข้อสังเกตสำหรับผู้สอน

- จำนวน Dynamic Entry อาจเปลี่ยนตาม Traffic และ Aging ควรตรวจ Mapping มากกว่าจำนวนตายตัว
- Simulation ของ Packet Tracer แต่ละรุ่นอาจแสดง Event ต่างกันเล็กน้อย ให้ประเมินหลักการ Flood/Forward และ Destination MAC
- ก่อนรับไฟล์ ตรวจว่า PC3 กลับมาต่อ Fa0/3 แล้ว
- Management SVI ไม่ใช่เงื่อนไขที่ Switch ต้องมีเพื่อ Forward Frame แต่ใช้เป็นส่วนฝึกการบริหารอุปกรณ์

### แล็บถัดไป

**หน่วยที่ 3 แล็บที่ 9: การใช้ Router เชื่อมต่อสองเครือข่าย LAN**
