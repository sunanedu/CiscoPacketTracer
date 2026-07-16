# ใบงานหน่วยที่ 3 แล็บที่ 9

## การใช้ Router เชื่อมต่อสองเครือข่าย LAN

| รายการ | รายละเอียด |
|---|---|
| รหัสใบงาน | CPT-U03-L09 |
| ระดับ | พื้นฐาน |
| ระยะเวลาโดยประมาณ | 180 นาที |
| รูปแบบการทำงาน | รายบุคคล หรือกลุ่มละไม่เกิน 2 คน |
| แล็บที่ควรทำก่อน | หน่วยที่ 2 แล็บที่ 5–8 |
| ไฟล์ที่ต้องส่ง | `CPT-U03-L09-ชื่อผู้เรียน.pkt` และใบงานที่บันทึกผลแล้ว |

---

## 1. เรื่องของใบงาน

ใบงานนี้เป็นการใช้ Router เชื่อมต่อเครือข่าย LAN สองเครือข่าย ผู้เรียนจะกำหนด IPv4 Address ให้ Router Interface ผ่าน Cisco IOS CLI เปิดใช้งาน Interface ด้วย `no shutdown` และกำหนด Default Gateway ให้ PC ทุกเครื่อง

จากนั้นผู้เรียนจะตรวจสอบ Routing Table เพื่อดูเส้นทางแบบ Directly Connected และ Local ทดสอบการสื่อสารทั้งภายใน LAN เดียวกันและข้าม LAN รวมถึงใช้ Simulation Mode วิเคราะห์ว่า Router เปลี่ยน Ethernet Header อย่างไรเมื่อส่งต่อ Packet

กิจกรรม Troubleshooting จะให้ผู้เรียน Shutdown Interface ของ Router ชั่วคราว แล้วเปรียบเทียบผลต่อ Local-LAN, Inter-LAN และ Routing Table ก่อนคืนค่าที่ถูกต้อง

## 2. เป้าหมายการเรียนรู้

เมื่อทำใบงานเสร็จ ผู้เรียนควรสามารถทำสิ่งต่อไปนี้ได้

1. อธิบายเหตุผลที่ต้องใช้ Router เชื่อม Network ต่างกันได้
2. อธิบายว่า Router Interface แต่ละพอร์ตต้องอยู่คนละ IP Network ได้
3. กำหนด IPv4 Address และ Description ให้ Router Interface ผ่าน CLI ได้
4. เปิดและปิด Interface ด้วย `no shutdown` และ `shutdown` ได้
5. ใช้ `show ip interface brief` ตรวจสถานะ Interface ได้
6. อ่าน Connected Route และ Local Route จาก `show ip route` ได้
7. กำหนด Default Gateway ให้ PC ตาม LAN ที่เชื่อมอยู่ได้
8. ทดสอบ Same-LAN และ Inter-LAN ด้วย `ping` ได้
9. อธิบายว่าการสื่อสาร Same-LAN ไม่ต้องผ่าน Router ได้
10. อธิบายลำดับการส่ง Packet ไปยังต่าง Network ได้
11. อธิบายว่า IP Header และ Ethernet Header เปลี่ยนแปลงต่างกันอย่างไรเมื่อผ่าน Router ได้
12. วิเคราะห์ผลเมื่อ Router Interface อยู่ในสถานะ Administratively Down ได้

## 3. ความรู้พื้นฐานก่อนเริ่ม

ผู้เรียนควรสามารถ

- คำนวณ Network Address ของ `/24` ได้
- อธิบายหน้าที่ของ IP Address, Subnet Mask และ Default Gateway ได้
- ใช้ Console Cable และ Cisco IOS CLI ได้
- ใช้คำสั่ง `enable`, `configure terminal`, `interface`, `ip address`, `no shutdown` และ `show` ได้
- ใช้ Simulation Mode ติดตาม ARP และ ICMP ได้

## 4. แนวคิดพื้นฐาน

### 4.1 Router เชื่อม Network อย่างไร

Router มี Interface หลายพอร์ต แต่ละ Interface สามารถเชื่อมกับ IP Network ที่ต่างกัน ในแล็บนี้

```text
R1 G0/0 → 192.168.10.0/24
R1 G0/1 → 192.168.20.0/24
```

เมื่อ Interface ทั้งสองเป็น Up และกำหนด IP ถูกต้อง R1 จะสร้าง Route สำหรับ Network ทั้งสองโดยอัตโนมัติ

### 4.2 Directly Connected Route

Directly Connected Route คือเส้นทางไปยัง Network ที่เชื่อมตรงกับ Router Interface และทำงานอยู่ โดยมักแสดงรหัส `C` ใน Routing Table

ตัวอย่าง

```text
C    192.168.10.0/24 is directly connected, GigabitEthernet0/0
C    192.168.20.0/24 is directly connected, GigabitEthernet0/1
```

### 4.3 Local Route

Local Route คือเส้นทาง `/32` ไปยัง IP Address ของ Router Interface เอง โดยมักแสดงรหัส `L`

```text
L    192.168.10.1/32 is directly connected, GigabitEthernet0/0
L    192.168.20.1/32 is directly connected, GigabitEthernet0/1
```

> IOS จำลองบางรุ่นอาจแสดงรายละเอียดต่างออกไปเล็กน้อย ให้พิจารณาว่ามีเส้นทางไปยัง Network ทั้งสองผ่าน Interface ที่ถูกต้องเป็นหลัก

### 4.4 หน้าที่ของ Default Gateway

| PC | Network ของ PC | Default Gateway |
|---|---|---|
| LAN-A-PC1/PC2 | `192.168.10.0/24` | R1 G0/0 `192.168.10.1` |
| LAN-B-PC1/PC2 | `192.168.20.0/24` | R1 G0/1 `192.168.20.1` |

Host จะส่ง Packet ให้ Default Gateway เมื่อ Destination IP ไม่ได้อยู่ใน Local Network ตามการคำนวณด้วย Subnet Mask

### 4.5 ลำดับการส่ง Packet ข้าม LAN

ตัวอย่าง LAN-A-PC1 ส่ง Ping ไป LAN-B-PC1

```text
1. LAN-A-PC1 เปรียบเทียบ Destination IP กับ /24
2. พบว่า 192.168.20.10 อยู่ต่าง Network
3. LAN-A-PC1 หา MAC ของ Default Gateway 192.168.10.1 ด้วย ARP
4. ส่ง Ethernet Frame ไปยัง MAC ของ R1 G0/0
5. R1 ถอด Layer 2 Header และตรวจ Destination IP
6. R1 ค้น Routing Table และเลือก G0/1
7. R1 หา MAC ของ LAN-B-PC1 บน LAN B ด้วย ARP หากยังไม่ทราบ
8. R1 สร้าง Ethernet Header ใหม่แล้วส่งออก G0/1
9. LAN-B-PC1 รับ ICMP Echo Request และส่ง Echo Reply กลับ
```

### 4.6 สิ่งที่เปลี่ยนและไม่เปลี่ยนเมื่อผ่าน Router

ในการส่ง Packet ปกติที่ไม่มี NAT

| ข้อมูล | LAN A ก่อนเข้า R1 | LAN B หลังออก R1 |
|---|---|---|
| Source IP | IP ของ LAN-A-PC1 | ยังคงเป็น IP ของ LAN-A-PC1 |
| Destination IP | IP ของ LAN-B-PC1 | ยังคงเป็น IP ของ LAN-B-PC1 |
| Source MAC | MAC ของ LAN-A-PC1 | เปลี่ยนเป็น MAC ของ R1 G0/1 |
| Destination MAC | MAC ของ R1 G0/0 | เปลี่ยนเป็น MAC ของ LAN-B-PC1 |
| TTL | ค่าเดิมก่อนเข้า Router | ลดลงหนึ่งเมื่อ Router ส่งต่อ |

> MAC Address ใช้ส่ง Frame ภายในแต่ละ Layer 2 Segment จึงเปลี่ยนเมื่อผ่าน Router ส่วน Source/Destination IP โดยทั่วไปคงเดิมตลอดเส้นทางเมื่อไม่มี NAT

## 5. อุปกรณ์และซอฟต์แวร์ที่ใช้

### 5.1 ซอฟต์แวร์

- โปรแกรม Cisco Packet Tracer จำนวน 1 ชุด
- โฟลเดอร์สำหรับบันทึกไฟล์งานของผู้เรียน

### 5.2 อุปกรณ์จำลองใน Packet Tracer

| ลำดับ | ประเภทอุปกรณ์ | รุ่นที่แนะนำ | จำนวน | ชื่อที่กำหนด |
|---:|---|---|---:|---|
| 1 | Router | Cisco 2911 | 1 | `R1` |
| 2 | Switch | Cisco 2960 | 2 | `SW-A`, `SW-B` |
| 3 | PC ผู้ใช้งาน | PC-PT | 4 | `LAN-A-PC1`, `LAN-A-PC2`, `LAN-B-PC1`, `LAN-B-PC2` |
| 4 | PC สำหรับ Console | PC-PT | 1 | `ADMIN-PC` |

## 6. แผนผังเครือข่าย

```text
                 LAN A: 192.168.10.0/24

LAN-A-PC1 Fa0 -------- Fa0/1 SW-A Gi0/1 -------- Gi0/0 R1
192.168.10.10                                       .1    |
                                                          |
LAN-A-PC2 Fa0 -------- Fa0/2 SW-A                        | Gi0/1
192.168.10.20                                             | .1
                                                          |
                 LAN B: 192.168.20.0/24                   |
                                                          |
LAN-B-PC1 Fa0 -------- Fa0/1 SW-B Gi0/1 ------------------+
192.168.20.10

LAN-B-PC2 Fa0 -------- Fa0/2 SW-B
192.168.20.20

ADMIN-PC RS-232 -------- Console R1
```

มุมมองอย่างย่อ

```text
LAN-A-PC1 --\
             SW-A ---- R1 ---- SW-B ---- LAN-B-PC1
LAN-A-PC2 --/                    \
                                  \---- LAN-B-PC2

ADMIN-PC -------- Console -------- R1
```

## 7. ตารางกำหนด IP Address

| อุปกรณ์ | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | `192.168.10.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| LAN-A-PC1 | FastEthernet0 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| LAN-A-PC2 | FastEthernet0 | `192.168.10.20` | `255.255.255.0` | `192.168.10.1` |
| R1 | GigabitEthernet0/1 | `192.168.20.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| LAN-B-PC1 | FastEthernet0 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |
| LAN-B-PC2 | FastEthernet0 | `192.168.20.20` | `255.255.255.0` | `192.168.20.1` |
| SW-A | VLAN 1 | ไม่กำหนด | ไม่กำหนด | ไม่กำหนด |
| SW-B | VLAN 1 | ไม่กำหนด | ไม่กำหนด | ไม่กำหนด |
| ADMIN-PC | RS-232 | ไม่กำหนด | ไม่กำหนด | ไม่กำหนด |

## 8. ตารางการเชื่อมต่อและสายสัญญาณ

| ลำดับ | อุปกรณ์ต้นทาง | พอร์ตต้นทาง | อุปกรณ์ปลายทาง | พอร์ตปลายทาง | สายที่ใช้ |
|---:|---|---|---|---|---|
| 1 | LAN-A-PC1 | FastEthernet0 | SW-A | FastEthernet0/1 | Copper Straight-Through |
| 2 | LAN-A-PC2 | FastEthernet0 | SW-A | FastEthernet0/2 | Copper Straight-Through |
| 3 | SW-A | GigabitEthernet0/1 | R1 | GigabitEthernet0/0 | Copper Straight-Through |
| 4 | R1 | GigabitEthernet0/1 | SW-B | GigabitEthernet0/1 | Copper Straight-Through |
| 5 | SW-B | FastEthernet0/1 | LAN-B-PC1 | FastEthernet0 | Copper Straight-Through |
| 6 | SW-B | FastEthernet0/2 | LAN-B-PC2 | FastEthernet0 | Copper Straight-Through |
| 7 | ADMIN-PC | RS-232 | R1 | Console | Console Cable |

## 9. ตารางบันทึก MAC Address สำหรับ Simulation

บันทึก MAC Address ของ FastEthernet0 บน LAN-A-PC1 และ LAN-B-PC1 และ MAC ของ R1 ทั้งสอง Interface

| อุปกรณ์/Interface | IP Address | MAC Address ที่พบ |
|---|---|---|
| LAN-A-PC1 Fa0 | `192.168.10.10` | ........................................................ |
| R1 G0/0 | `192.168.10.1` | ........................................................ |
| R1 G0/1 | `192.168.20.1` | ........................................................ |
| LAN-B-PC1 Fa0 | `192.168.20.10` | ........................................................ |

## 10. ขั้นตอนการปฏิบัติงาน

### ตอนที่ 1: สร้างและบันทึกไฟล์งาน

1. เปิด Cisco Packet Tracer
2. สร้างไฟล์ใหม่
3. บันทึกเป็น `CPT-U03-L09-ชื่อผู้เรียน.pkt`
4. วางและตั้งชื่ออุปกรณ์ตามข้อ 5
5. ต่อสายตามข้อ 8
6. ตรวจว่าพอร์ต PC–Switch เป็นสีเขียว
7. พอร์ต Router อาจยังเป็นสีแดงจนกว่าจะใช้ `no shutdown`

### ตอนที่ 2: กำหนด IP ให้ PC ฝั่ง LAN A

เปิด Desktop > IP Configuration แล้วกำหนด

#### LAN-A-PC1

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

#### LAN-A-PC2

```text
IP Address:      192.168.10.20
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

### ตอนที่ 3: กำหนด IP ให้ PC ฝั่ง LAN B

#### LAN-B-PC1

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

#### LAN-B-PC2

```text
IP Address:      192.168.20.20
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

### จุดตรวจสอบที่ 1: PC

ใช้ `ipconfig` บน PC ทุกเครื่องแล้วบันทึก

| PC | IP ที่พบ | Mask ที่พบ | Gateway ที่พบ | ถูกต้องหรือไม่ |
|---|---|---|---|---|
| LAN-A-PC1 | ........................ | ........................ | ........................ | ถูก / ผิด |
| LAN-A-PC2 | ........................ | ........................ | ........................ | ถูก / ผิด |
| LAN-B-PC1 | ........................ | ........................ | ........................ | ถูก / ผิด |
| LAN-B-PC2 | ........................ | ........................ | ........................ | ถูก / ผิด |

### ตอนที่ 4: เปิด Console ของ R1

1. เปิด **ADMIN-PC > Desktop > Terminal**
2. ใช้ค่า Terminal เริ่มต้น
3. หากพบ Initial Configuration Dialog ให้ตอบ `no`
4. กด Enter จนเห็น `Router>`

### ตอนที่ 5: กำหนดค่าพื้นฐาน R1

พิมพ์คำสั่ง

```text
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret Enable123
username admin secret Admin123
banner motd #AUTHORIZED USERS ONLY#
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
```

### ตอนที่ 6: กำหนด Interface ฝั่ง LAN A

ที่ `R1(config)#` พิมพ์

```text
interface gigabitEthernet 0/0
 description LAN_A_TO_SW_A
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
```

### ตอนที่ 7: กำหนด Interface ฝั่ง LAN B

```text
interface gigabitEthernet 0/1
 description LAN_B_TO_SW_B
 ip address 192.168.20.1 255.255.255.0
 no shutdown
end
```

รอจน Interface และ Line Protocol เป็น Up

### ตอนที่ 8: ตรวจสอบ Interface

ที่ `R1#` พิมพ์

```text
show ip interface brief
show interfaces description
```

บันทึกผล

| Interface | IP Address | Status | Protocol | Description |
|---|---|---|---|---|
| G0/0 | ........................ | ........................ | ........................ | ........................ |
| G0/1 | ........................ | ........................ | ........................ | ........................ |

ค่าที่คาดหวัง

```text
G0/0    192.168.10.1    up    up
G0/1    192.168.20.1    up    up
```

### ตอนที่ 9: ตรวจสอบ Routing Table

พิมพ์

```text
show ip route
```

ค้นหา Route ของ `192.168.10.0/24` และ `192.168.20.0/24` แล้วบันทึก

| Code | Network/Prefix | Interface | ความหมาย |
|---|---|---|---|
| ........ | `192.168.10.0/24` | ........................ | ........................ |
| ........ | `192.168.10.1/32` | ........................ | ........................ |
| ........ | `192.168.20.0/24` | ........................ | ........................ |
| ........ | `192.168.20.1/32` | ........................ | ........................ |

คำตอบที่คาดหวัง

- `C` หมายถึง Connected Network
- `L` หมายถึง Local Address ของ Interface

### ตอนที่ 10: บันทึก Configuration

```text
copy running-config startup-config
```

กด Enter ยอมรับ Destination Filename แล้วตรวจ

```text
show startup-config
```

### ตอนที่ 11: ทดสอบ Same-LAN

ที่ LAN-A-PC1

```text
ping 192.168.10.20
```

ที่ LAN-B-PC1

```text
ping 192.168.20.20
```

ผลที่คาดหวังคือผ่านทั้งสองรายการ โดย Frame วิ่งผ่าน Switch ใน LAN เดียวกันและไม่ต้องผ่าน R1

### ตอนที่ 12: ทดสอบ Default Gateway

ที่ LAN-A-PC1

```text
ping 192.168.10.1
```

ที่ LAN-B-PC1

```text
ping 192.168.20.1
```

ผลที่คาดหวังคือได้รับ Reply จาก Router Interface ของแต่ละ LAN

### ตอนที่ 13: ทดสอบ Inter-LAN

ที่ LAN-A-PC1

```text
ping 192.168.20.10
ping 192.168.20.20
```

ที่ LAN-B-PC1

```text
ping 192.168.10.10
ping 192.168.10.20
```

ทำซ้ำหากครั้งแรกสูญหายระหว่าง ARP ผลสุดท้ายควรผ่านทุกคู่

### ตอนที่ 14: ตรวจ ARP Table ของ R1

หลังสร้าง Traffic แล้ว ที่ R1 พิมพ์

```text
show arp
```

บันทึกอย่างน้อยหนึ่งรายการจากแต่ละ LAN

| Protocol | Address | Hardware Address | Interface |
|---|---|---|---|
| ........................ | `192.168.10.___` | ........................ | G0/0 |
| ........................ | `192.168.20.___` | ........................ | G0/1 |

### ตอนที่ 15: เตรียม Simulation Mode

1. บันทึก MAC Address ในข้อ 9
2. ที่ LAN-A-PC1 และ LAN-B-PC1 ล้าง ARP Cache หากรุ่นรองรับ

```text
arp -d
```

3. เปลี่ยนเป็น Simulation Mode
4. เลือก Filter เฉพาะ ARP และ ICMP
5. ล้าง Event List เก่า
6. Add Simple PDU จาก LAN-A-PC1 ไป LAN-B-PC1

### ตอนที่ 16: สังเกตฝั่ง LAN A

ใช้ Capture/Forward จน LAN-A-PC1 ส่ง ICMP Echo Request ไปยัง R1

เปิด PDU Details และบันทึก

| Field | ค่าที่พบก่อนเข้า R1 |
|---|---|
| Source IP | ........................................................ |
| Destination IP | ........................................................ |
| Source MAC | ........................................................ |
| Destination MAC | ........................................................ |

ค่าที่คาดหวัง

- Source IP = `192.168.10.10`
- Destination IP = `192.168.20.10`
- Source MAC = MAC ของ LAN-A-PC1
- Destination MAC = MAC ของ R1 G0/0

ตอบคำถาม: เหตุใด Destination MAC ไม่ใช่ MAC ของ LAN-B-PC1

....................................................................................................

### ตอนที่ 17: สังเกตการประมวลผลที่ R1

1. กด Capture/Forward ให้ Frame เข้า R1 G0/0
2. เปิด OSI Model ของเหตุการณ์ที่ R1
3. สังเกตว่า R1 ถอด Layer 2 Header ฝั่ง LAN A
4. R1 ตรวจ Destination IP และ Routing Table
5. R1 เลือกส่งออก G0/1
6. หาก R1 ยังไม่รู้ MAC ของ LAN-B-PC1 จะสร้าง ARP Request บน LAN B ก่อน

บันทึก Route ที่ R1 ใช้

```text
Destination Network: ................................
Outgoing Interface:  ................................
```

### ตอนที่ 18: สังเกตฝั่ง LAN B

ใช้ Capture/Forward จน R1 ส่ง ICMP Echo Request ออก G0/1 แล้วบันทึก

| Field | ค่าที่พบหลังออก R1 |
|---|---|
| Source IP | ........................................................ |
| Destination IP | ........................................................ |
| Source MAC | ........................................................ |
| Destination MAC | ........................................................ |

ค่าที่คาดหวัง

- Source IP ยังเป็น `192.168.10.10`
- Destination IP ยังเป็น `192.168.20.10`
- Source MAC เปลี่ยนเป็น MAC ของ R1 G0/1
- Destination MAC เปลี่ยนเป็น MAC ของ LAN-B-PC1

### ตอนที่ 19: ติดตาม Echo Reply

1. กด Capture/Forward จน LAN-B-PC1 รับ Echo Request
2. ติดตาม Echo Reply กลับผ่าน R1
3. ตรวจว่าทิศทาง Source/Destination IP สลับกัน
4. ตรวจว่า Ethernet Header ถูกสร้างใหม่ในแต่ละ LAN
5. ปล่อย Scenario ทำงานจนแสดง Successful

### ตอนที่ 20: สรุปการเปลี่ยน Header

| จุดสังเกต | Source IP | Destination IP | Source MAC | Destination MAC |
|---|---|---|---|---|
| LAN A ก่อนเข้า R1 | ........................ | ........................ | ........................ | ........................ |
| LAN B หลังออก R1 | ........................ | ........................ | ........................ | ........................ |

ตอบคำถาม

```text
IP เปลี่ยนหรือไม่: ........................
MAC เปลี่ยนหรือไม่: .......................
เหตุผล: .........................................................................................
```

### ตอนที่ 21: แบบทดลอง Interface Shutdown

กลับ Realtime Mode และทำกิจกรรมตามลำดับ

#### 21.1 บันทึกสถานะปกติ

ที่ R1

```text
show ip interface brief
show ip route
```

ตรวจว่า G0/1 up/up และมี Connected Route `192.168.20.0/24`

#### 21.2 ปิด Interface ฝั่ง LAN B ชั่วคราว

```text
configure terminal
interface gigabitEthernet 0/1
shutdown
end
```

ตรวจสอบ

```text
show ip interface brief
show ip route
```

บันทึกผล

| รายการ | ก่อน Shutdown | หลัง Shutdown |
|---|---|---|
| G0/1 Status | ........................ | ........................ |
| G0/1 Protocol | ........................ | ........................ |
| Route `192.168.20.0/24` | มี / ไม่มี | มี / ไม่มี |

#### 21.3 ทดสอบขณะ G0/1 Down

ที่ LAN-B-PC1

```text
ping 192.168.20.20
ping 192.168.20.1
```

ที่ LAN-A-PC1

```text
ping 192.168.20.10
```

ผลที่คาดหวัง

- LAN-B-PC1 ยัง Ping LAN-B-PC2 ได้ เพราะอยู่ LAN เดียวกันผ่าน SW-B
- LAN-B-PC1 Ping Gateway ไม่ได้ เพราะ G0/1 ถูกปิด
- LAN-A-PC1 Ping LAN-B-PC1 ไม่ได้ เพราะ R1 ไม่มี Interface/Connected Route ที่ใช้งานได้ไป LAN B

#### 21.4 เปิด Interface คืน

```text
configure terminal
interface gigabitEthernet 0/1
no shutdown
end
```

รอให้ลิงก์กลับมา up/up แล้วตรวจ

```text
show ip interface brief
show ip route
```

Ping ข้าม LAN อีกครั้งจนผ่าน

### ตอนที่ 22: บันทึกขั้นสุดท้าย

1. ตรวจว่า G0/0 และ G0/1 up/up
2. ตรวจ Connected Route ของ Network ทั้งสอง
3. บันทึก Router

```text
copy running-config startup-config
```

4. กด `Ctrl+S` บันทึก Packet Tracer
5. ปิดและเปิดไฟล์ใหม่
6. ทดสอบ Same-LAN, Gateway และ Inter-LAN อีกครั้ง

## 11. ชุดคำสั่ง R1 ฉบับรวม

ใช้ตรวจเทียบหลังจากพิมพ์ตามขั้นตอนแล้ว

```text
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret Enable123
username admin secret Admin123
banner motd #AUTHORIZED USERS ONLY#
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
interface gigabitEthernet 0/0
 description LAN_A_TO_SW_A
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 description LAN_B_TO_SW_B
 ip address 192.168.20.1 255.255.255.0
 no shutdown
end
copy running-config startup-config
```

## 12. คำสั่งตรวจสอบที่ใช้ในแล็บ

### 12.1 บน R1

| คำสั่ง | หน้าที่ |
|---|---|
| `show ip interface brief` | แสดง IP, Status และ Protocol ของ Interface |
| `show interfaces description` | แสดง Interface Description และสถานะ |
| `show ip route` | แสดง Routing Table |
| `show arp` | แสดงความสัมพันธ์ IPv4–MAC ที่ Router เรียนรู้ |
| `show running-config` | ตรวจ Configuration ปัจจุบัน |
| `show startup-config` | ตรวจ Configuration ที่บันทึกไว้ |
| `copy running-config startup-config` | บันทึก Configuration |

### 12.2 บน PC

| คำสั่ง | หน้าที่ |
|---|---|
| `ipconfig` | ตรวจ IP, Mask และ Gateway |
| `ping <destination-ip>` | ทดสอบการเข้าถึงปลายทาง |
| `arp -a` | ตรวจ ARP Cache |
| `arp -d` | ล้าง Dynamic ARP ตาม Syntax ที่รุ่นรองรับ |

## 13. ตารางบันทึกผล Ping ขั้นสุดท้าย

| ต้นทาง | ปลายทาง | ประเภท | ผ่าน R1 หรือไม่ | ผล Ping |
|---|---|---|---|---|
| LAN-A-PC1 | LAN-A-PC2 | Same-LAN | ไม่ผ่าน | ผ่าน / ไม่ผ่าน |
| LAN-B-PC1 | LAN-B-PC2 | Same-LAN | ไม่ผ่าน | ผ่าน / ไม่ผ่าน |
| LAN-A-PC1 | R1 G0/0 | Gateway | ถึง R1 | ผ่าน / ไม่ผ่าน |
| LAN-B-PC1 | R1 G0/1 | Gateway | ถึง R1 | ผ่าน / ไม่ผ่าน |
| LAN-A-PC1 | LAN-B-PC1 | Inter-LAN | ผ่าน | ผ่าน / ไม่ผ่าน |
| LAN-A-PC2 | LAN-B-PC2 | Inter-LAN | ผ่าน | ผ่าน / ไม่ผ่าน |
| LAN-B-PC1 | LAN-A-PC1 | Inter-LAN | ผ่าน | ผ่าน / ไม่ผ่าน |

## 14. การทดสอบการทำงานขั้นสุดท้าย

| ลำดับ | รายการทดสอบ | เกณฑ์ที่ผ่าน | ผ่าน | ไม่ผ่าน |
|---:|---|---|:---:|:---:|
| 1 | Topology | อุปกรณ์ครบและต่อสายครบ 7 เส้น | ☐ | ☐ |
| 2 | LAN A PCs | IP/Mask/Gateway ตรงตามตาราง | ☐ | ☐ |
| 3 | LAN B PCs | IP/Mask/Gateway ตรงตามตาราง | ☐ | ☐ |
| 4 | R1 G0/0 | `192.168.10.1/24`, up/up | ☐ | ☐ |
| 5 | R1 G0/1 | `192.168.20.1/24`, up/up | ☐ | ☐ |
| 6 | Routing Table | มี Connected/Local Routes ของทั้งสอง LAN | ☐ | ☐ |
| 7 | Same-LAN | PC คู่ในแต่ละ LAN Ping กันได้ | ☐ | ☐ |
| 8 | Inter-LAN | PC ข้าม LAN Ping กันได้ทั้งสองทิศทาง | ☐ | ☐ |
| 9 | Simulation | อธิบาย IP คงเดิมและ MAC เปลี่ยนได้ | ☐ | ☐ |
| 10 | Shutdown Test | วิเคราะห์ Route/Connectivity และคืน G0/1 แล้ว | ☐ | ☐ |
| 11 | Persistence | Startup Config และไฟล์ `.pkt` สมบูรณ์ | ☐ | ☐ |

### ผลการทดสอบโดยรวม

- [ ] ผ่านครบทุกข้อ
- [ ] ผ่านบางข้อและแก้ไขเรียบร้อยแล้ว
- [ ] ยังมีปัญหาและต้องการความช่วยเหลือ

## 15. แนวทางแก้ไขปัญหาเบื้องต้น

ตรวจจากต้นทางไปทีละจุด

```text
1. ตรวจ IP/Mask/Gateway ของ PC
2. Ping IP ของตนเอง
3. Ping Host ใน LAN เดียวกัน
4. Ping Default Gateway
5. ตรวจ Router Interface ด้วย show ip interface brief
6. ตรวจ Routing Table ด้วย show ip route
7. ตรวจ Host ปลายทางและ Gateway ขากลับ
```

| อาการ | สาเหตุที่เป็นไปได้ | วิธีตรวจสอบหรือแก้ไข |
|---|---|---|
| PC ใน LAN เดียวกัน Ping กันไม่ได้ | IP/Mask สาย หรือ Switch Port ผิด | ตรวจ `ipconfig`, สาย และไฟลิงก์ |
| PC Ping Gateway ไม่ได้ | Gateway ผิดหรือ Router Interface Down | ตรวจตาราง IP และ `show ip interface brief` |
| Interface เป็น administratively down | ยังไม่มี `no shutdown` | เข้า Interface แล้วใช้ `no shutdown` |
| Interface up/down | ปัญหา Layer 2 ฝั่งตรงข้าม | ตรวจ Switch Port สาย และรอ Protocol |
| ไม่มี Connected Route | Interface Down หรือ IP/Mask ไม่ถูกต้อง | แก้ Interface แล้วตรวจ `show ip route` ใหม่ |
| Same-LAN ผ่านแต่ Inter-LAN ไม่ผ่าน | Gateway ผิดหรือ Router Interface/Route มีปัญหา | Ping Gateway และตรวจ Routing Table |
| Ping ไปได้แต่กลับไม่ได้ | Gateway ของปลายทางผิด | ตรวจ PC ปลายทางทุกช่อง |
| Simulation Destination MAC เป็น Host ปลายทางตั้งแต่ LAN A | เลือกปลายทางอยู่ Same-LAN หรือค่าการกำหนดผิด | ตรวจ Network/Mask และ Default Gateway |
| Ping ครั้งแรกสูญหายหนึ่ง Packet | ARP กำลังเรียนรู้ | Ping ซ้ำก่อนสรุป |
| หลังทดลอง G0/1 ยัง Down | ลืม `no shutdown` หรือสายไม่พร้อม | เปิด Interface รอ up/up แล้วบันทึกใหม่ |

## 16. แบบบันทึกผลการปฏิบัติงาน

| รายการ | ข้อมูลของผู้เรียน |
|---|---|
| ชื่อ–นามสกุล | ............................................................ |
| ชั้น/กลุ่ม | ............................................................ |
| วันที่ทำแล็บ | ............................................................ |
| ชื่อไฟล์ที่ส่ง | ............................................................ |
| เวลาที่ใช้จริง | ............................................................ |

### Route ที่ R1 ใช้ส่งจาก LAN A ไป LAN B

....................................................................................................

### สิ่งที่เปลี่ยนเมื่อ Packet ผ่าน Router

....................................................................................................

....................................................................................................

### ปัญหาที่พบและวิธีแก้ไข

....................................................................................................

....................................................................................................

## 17. คำถามท้ายแล็บ

1. เพราะเหตุใด LAN A และ LAN B จึงต้องใช้ Router ติดต่อกัน

   คำตอบ: ............................................................................................

2. R1 G0/0 และ G0/1 ใช้ IP Network เดียวกันได้หรือไม่ เพราะเหตุใด

   คำตอบ: ............................................................................................

3. รหัส `C` และ `L` ใน Routing Table หมายถึงอะไร

   คำตอบ: ............................................................................................

4. Connected Route จะปรากฏเมื่อมีเงื่อนไขใด

   คำตอบ: ............................................................................................

5. LAN-A-PC1 ส่ง Frame แรกของ ICMP ไปยัง MAC ของใครเมื่อปลายทางอยู่ LAN B

   คำตอบ: ............................................................................................

6. Source/Destination IP เปลี่ยนหรือไม่เมื่อผ่าน R1 ในแล็บนี้

   คำตอบ: ............................................................................................

7. Source/Destination MAC เปลี่ยนหรือไม่เมื่อผ่าน R1 เพราะเหตุใด

   คำตอบ: ............................................................................................

8. เมื่อ R1 G0/1 ถูก Shutdown เหตุใด LAN-B-PC1 ยังติดต่อ LAN-B-PC2 ได้

   คำตอบ: ............................................................................................

9. เมื่อ R1 G0/1 ถูก Shutdown เกิดอะไรกับ Route `192.168.20.0/24`

   คำตอบ: ............................................................................................

10. เหตุใดแล็บนี้ไม่ต้องกำหนด Static Route

    คำตอบ: ...........................................................................................

## 18. สรุปผลการเรียนรู้โดยผู้เรียน

หลังทำแล็บนี้ ฉันสามารถ

- [ ] กำหนด Router Interface สองพอร์ตผ่าน CLI ได้
- [ ] เปิด/ปิด Interface และแปลสถานะได้
- [ ] อ่าน Connected และ Local Route ได้
- [ ] กำหนด Default Gateway ให้แต่ละ LAN ได้
- [ ] ทดสอบ Same-LAN และ Inter-LAN ได้
- [ ] อธิบายลำดับการส่ง Packet ผ่าน Router ได้
- [ ] อธิบายการเปลี่ยน MAC และการคง IP ได้
- [ ] วิเคราะห์ผลของ Interface Down ต่อ Routing Table ได้

สิ่งสำคัญที่ได้เรียนรู้จากแล็บนี้คือ

....................................................................................................

....................................................................................................

สิ่งที่ต้องการฝึกเพิ่มเติมคือ

....................................................................................................

....................................................................................................

## 19. เกณฑ์การประเมิน

| หัวข้อประเมิน | คะแนนเต็ม |
|---|---:|
| Topology, IP และ Default Gateway ถูกต้อง | 2 |
| กำหนด/ตรวจสอบ Router Interface ผ่าน CLI ได้ | 2 |
| อ่าน Routing Table และทดสอบ Inter-LAN ได้ | 2 |
| วิเคราะห์ Packet Header และ Interface Shutdown ได้ | 2 |
| Troubleshooting บันทึกผล ตอบคำถาม และสรุปผลได้ | 2 |
| **รวม** | **10** |

เกณฑ์ผ่านที่แนะนำ: **อย่างน้อย 7 คะแนนจาก 10 คะแนน** โดย Interface ทั้งสองต้อง up/up และ PC ทุกเครื่องต้อง Ping ข้าม LAN ได้หลังคืนค่าที่ถูกต้อง

## 20. งานที่ต้องส่ง

1. ไฟล์ Packet Tracer ชื่อ `CPT-U03-L09-ชื่อผู้เรียน.pkt`
2. ใบงานที่กรอกผล Interface, Routing Table, Ping, ARP, Simulation, Shutdown Test, คำถามท้ายแล็บ และสรุปผลครบถ้วน

---

## ภาคผนวกสำหรับผู้สอน: แนวคำตอบย่อ

### Routing Table ที่คาดหวัง

```text
C 192.168.10.0/24 → GigabitEthernet0/0
L 192.168.10.1/32 → GigabitEthernet0/0
C 192.168.20.0/24 → GigabitEthernet0/1
L 192.168.20.1/32 → GigabitEthernet0/1
```

### Header ที่คาดหวังสำหรับ A-PC1 ไป B-PC1

| จุด | Source IP | Destination IP | Source MAC | Destination MAC |
|---|---|---|---|---|
| LAN A | `192.168.10.10` | `192.168.20.10` | MAC ของ A-PC1 | MAC ของ R1 G0/0 |
| LAN B | `192.168.10.10` | `192.168.20.10` | MAC ของ R1 G0/1 | MAC ของ B-PC1 |

### แนวคำตอบคำถามท้ายแล็บ

1. ทั้งสอง LAN ใช้ Network Address ต่างกัน จึงต้องมีอุปกรณ์ Layer 3 ส่งต่อ Packet
2. ไม่ควรและ Router จะปฏิเสธ Subnet ที่ซ้อนทับกัน เพราะแต่ละ Routed Interface ต้องระบุ Network ที่แยกจากกัน
3. `C` คือ Directly Connected Network และ `L` คือ Local `/32` ของ Interface
4. Interface ต้องมี IP/Mask ถูกต้อง อยู่ในสถานะ up/up และเชื่อมกับ Network นั้น
5. MAC ของ Default Gateway คือ R1 G0/0
6. ไม่เปลี่ยนในแล็บนี้เพราะไม่มี NAT แต่ TTL ลดลงเมื่อผ่าน Router
7. เปลี่ยน เพราะ Router ถอด Layer 2 Header เดิมแล้วสร้าง Header ใหม่สำหรับ Segment ขาออก
8. PC ทั้งสองอยู่ LAN เดียวกันและ Switch ส่ง Frame ให้กันโดยไม่ต้องผ่าน Router
9. Connected และ Local Route ของ G0/1 ถูกนำออกจาก Routing Table ขณะ Interface Down
10. Network ทั้งสองเชื่อมตรงกับ R1 จึงมี Connected Routes อยู่แล้ว

### ข้อสังเกตสำหรับผู้สอน

- หาก IOS ไม่แสดง Local Route `L` ให้ประเมินจาก Connected Route และความเข้าใจเรื่อง IP ของ Interface
- ก่อนรับไฟล์ ตรวจว่า G0/1 ถูกคืนเป็น `no shutdown` และ Routing Table มี Network ทั้งสอง
- Simulation อาจแสดง ARP หลายเหตุการณ์ ควรประเมินจาก Header ก่อน/หลัง Router มากกว่าจำนวน Event
- Same-LAN Ping ต้องผ่านแม้ Router Interface ฝั่งนั้นถูก Shutdown ตราบใดที่ PC และ Switch ยังทำงานและอยู่ Subnet เดียวกัน

### แล็บถัดไป

**หน่วยที่ 3 แล็บที่ 10: Static Route และ Default Route**
