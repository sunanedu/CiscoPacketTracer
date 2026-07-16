# ใบงานหน่วยที่ 3 แล็บที่ 10

## Static Route และ Default Route

| รายการ | รายละเอียด |
|---|---|
| รหัสใบงาน | CPT-U03-L10 |
| ระดับ | พื้นฐาน |
| ระยะเวลาโดยประมาณ | 210 นาที |
| รูปแบบการทำงาน | รายบุคคล หรือกลุ่มละไม่เกิน 2 คน |
| แล็บที่ควรทำก่อน | หน่วยที่ 3 แล็บที่ 9 |
| ไฟล์ที่ต้องส่ง | `CPT-U03-L10-ชื่อผู้เรียน.pkt` และใบงานที่บันทึกผลแล้ว |

---

## 1. เรื่องของใบงาน

ใบงานนี้เป็นการเชื่อม LAN สามเครือข่ายผ่าน Router สามตัว ผู้เรียนจะกำหนด **Static Route** ให้ Router ศูนย์กลาง และกำหนด **Default Route** ให้ Stub Router ทั้งสองฝั่ง

ผู้เรียนจะตรวจสอบ Routing Table ด้วยคำสั่ง `show ip route` สังเกตรหัส `S` และ `S*` ทดสอบการเชื่อมต่อด้วย `ping` และตรวจเส้นทางด้วย `tracert` จากนั้นทดลองลบ Forward Route และ Return Route ชั่วคราวเพื่อทำความเข้าใจว่า Packet ต้องมีเส้นทางครบทั้งขาไปและขากลับ

## 2. สถานการณ์จำลอง

องค์กรมีสำนักงานสามแห่ง

- สำนักงาน A เชื่อมกับ R1
- สำนักงาน B เป็นศูนย์กลาง เชื่อมกับ R2
- สำนักงาน C เชื่อมกับ R3

R1 และ R3 มีทางออกไปยัง Router เพื่อนบ้านเพียงทางเดียว จึงเหมาะกับ Default Route ส่วน R2 เชื่อมหลายทิศทางและต้องใช้ Static Route ที่ระบุ Network ปลายทางอย่างชัดเจน

## 3. เป้าหมายการเรียนรู้

เมื่อทำใบงานเสร็จ ผู้เรียนควรสามารถทำสิ่งต่อไปนี้ได้

1. อธิบายความแตกต่างระหว่าง Connected, Static และ Default Route ได้
2. อธิบายเหตุผลที่ Router ต้องมีเส้นทางทั้งขาไปและขากลับได้
3. ออกแบบ `/30` สำหรับลิงก์ Point-to-Point ได้
4. ใช้คำสั่ง `ip route` แบบ Next-Hop Address ได้
5. กำหนด Default Route `0.0.0.0/0` ได้
6. อ่านรหัส `C`, `L`, `S` และ `S*` ใน Routing Table ได้
7. อธิบาย Gateway of Last Resort ได้
8. ตรวจสอบ Route ด้วย `show ip route` และคำสั่งกรองได้
9. ทดสอบ End-to-End Connectivity ด้วย `ping` ได้
10. ใช้ `tracert` วิเคราะห์ลำดับ Router ตามเส้นทางได้
11. วิเคราะห์ผลเมื่อ Specific Static Route หายไปได้
12. วิเคราะห์ผลเมื่อ Return Route หายไปได้
13. ลบและคืน Static/Default Route ด้วยคำสั่ง `no ip route` ได้

## 4. ความรู้พื้นฐานก่อนเริ่ม

ผู้เรียนควรสามารถ

- กำหนด Router Interface ผ่าน Cisco IOS CLI ได้
- อ่าน Connected และ Local Route ได้
- คำนวณ Network, Host Range และ Broadcast ของ `/24` และ `/30` ได้
- กำหนด Default Gateway ให้ PC ได้
- ใช้ `ping`, `show ip interface brief` และ `show ip route` ได้

## 5. แนวคิดพื้นฐาน

### 5.1 Connected Route

Connected Route เกิดขึ้นอัตโนมัติเมื่อ Router Interface มี IP Address ที่ถูกต้องและอยู่ในสถานะ up/up โดยแสดงรหัส `C`

### 5.2 Static Route

Static Route คือเส้นทางที่ผู้ดูแลกำหนดเอง รูปแบบที่ใช้ในแล็บนี้คือ

```text
ip route <destination-network> <subnet-mask> <next-hop-ip>
```

ตัวอย่าง

```text
ip route 192.168.30.0 255.255.255.0 10.0.23.2
```

ความหมายคือ หากต้องการไป `192.168.30.0/24` ให้ส่งต่อไปยัง Next Hop `10.0.23.2`

### 5.3 Default Route

Default Route ใช้เมื่อไม่มี Route ที่เฉพาะเจาะจงกว่าตรงกับ Destination

```text
ip route 0.0.0.0 0.0.0.0 <next-hop-ip>
```

`0.0.0.0 0.0.0.0` เขียนแบบ Prefix ได้เป็น `0.0.0.0/0`

### 5.4 Stub Router

Stub Router คือ Router ที่มีทางออกไปยัง Network อื่นเพียงทิศทางหลักเดียว ใน Topology นี้

- R1 ส่ง Network ที่ไม่รู้จักทั้งหมดไป R2
- R3 ส่ง Network ที่ไม่รู้จักทั้งหมดไป R2

จึงไม่จำเป็นต้องสร้าง Static Route แยกทุก Remote Network บน R1 และ R3

### 5.5 Gateway of Last Resort

เมื่อ Routing Table มี Default Route ที่ใช้งานได้ IOS จะแสดง Gateway of Last Resort ซึ่งเป็น Next Hop ที่ใช้เมื่อไม่มี Route อื่นตรงกับ Destination

### 5.6 Administrative Distance

Static Route ปกติมี Administrative Distance เริ่มต้นเป็น 1 ซึ่งบอกระดับความน่าเชื่อถือของแหล่ง Route ค่าอาจแสดงใน Routing Table เช่น

```text
[1/0]
```

- เลขแรกคือ Administrative Distance
- เลขที่สองคือ Metric

### 5.7 Longest Prefix Match

หากมีหลาย Route ที่ตรงกับ Destination Router จะเลือก Route ที่มี Prefix ยาวและเฉพาะเจาะจงกว่า เช่น `/24` เฉพาะเจาะจงกว่า `/0`

### 5.8 Forward Path และ Return Path

Ping จะสำเร็จเมื่อ

```text
Echo Request เดินทางถึงปลายทาง
และ
Echo Reply มีเส้นทางกลับถึงต้นทาง
```

มี Forward Route อย่างเดียวไม่เพียงพอ หากปลายทางหรือ Router ขากลับไม่รู้เส้นทางกลับ Ping จะยังล้มเหลว

## 6. การออกแบบเครือข่าย

### 6.1 LAN Networks

| LAN | Network | Gateway | PC Address |
|---|---|---|---|
| LAN A | `192.168.10.0/24` | `192.168.10.1` | `192.168.10.10` |
| LAN B | `192.168.20.0/24` | `192.168.20.1` | `192.168.20.10` |
| LAN C | `192.168.30.0/24` | `192.168.30.1` | `192.168.30.10` |

### 6.2 Point-to-Point Networks

| ลิงก์ | Network/Prefix | Usable IP | Broadcast |
|---|---|---|---|
| R1–R2 | `10.0.12.0/30` | `10.0.12.1`, `10.0.12.2` | `10.0.12.3` |
| R2–R3 | `10.0.23.0/30` | `10.0.23.1`, `10.0.23.2` | `10.0.23.3` |

Subnet Mask ของ `/30` คือ `255.255.255.252`

## 7. อุปกรณ์และซอฟต์แวร์ที่ใช้

### 7.1 ซอฟต์แวร์

- โปรแกรม Cisco Packet Tracer จำนวน 1 ชุด
- โฟลเดอร์สำหรับบันทึกไฟล์งานของผู้เรียน

### 7.2 อุปกรณ์จำลองใน Packet Tracer

| ลำดับ | ประเภทอุปกรณ์ | รุ่นที่แนะนำ | จำนวน | ชื่อที่กำหนด |
|---:|---|---|---:|---|
| 1 | Router | Cisco 2911 | 3 | `R1`, `R2`, `R3` |
| 2 | Switch | Cisco 2960 | 3 | `SW-A`, `SW-B`, `SW-C` |
| 3 | PC | PC-PT | 3 | `PC-A`, `PC-B`, `PC-C` |

> เพื่อให้ Topology ไม่ซับซ้อน แล็บนี้เปิด CLI จากแท็บ CLI ของ Router โดยตรง ผู้เรียนสามารถเพิ่ม Console PC ได้หากผู้สอนต้องการทบทวน Console Connection

## 8. แผนผังเครือข่าย

```text
LAN A                  Point-to-Point             LAN B              Point-to-Point                 LAN C
192.168.10.0/24        10.0.12.0/30               192.168.20.0/24    10.0.23.0/30                  192.168.30.0/24

PC-A -- SW-A -- G0/0 R1 G0/1 ===== G0/0 R2 G0/1 -- SW-B -- PC-B
 .10             .1     .1          .2     .1
                                            |
                                            | G0/2 .1
                                            |
                                            ===== G0/0 .2 R3 G0/1 .1 -- SW-C -- PC-C .10
```

มุมมองอย่างย่อ

```text
PC-A -- SW-A -- R1 ===== R2 -- SW-B -- PC-B
                         |
                         ===== R3 -- SW-C -- PC-C
```

สัญลักษณ์

- `--` ใช้ Copper Straight-Through ระหว่าง PC–Switch และ Switch–Router
- `=====` ใช้ Copper Crossover ระหว่าง Router–Router

## 9. ตารางกำหนด IP Address

| อุปกรณ์ | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| PC-A | FastEthernet0 | `192.168.10.10` | `255.255.255.0` | `192.168.10.1` |
| R1 | GigabitEthernet0/0 | `192.168.10.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| R1 | GigabitEthernet0/1 | `10.0.12.1` | `255.255.255.252` | ไม่เกี่ยวข้อง |
| R2 | GigabitEthernet0/0 | `10.0.12.2` | `255.255.255.252` | ไม่เกี่ยวข้อง |
| R2 | GigabitEthernet0/1 | `192.168.20.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| PC-B | FastEthernet0 | `192.168.20.10` | `255.255.255.0` | `192.168.20.1` |
| R2 | GigabitEthernet0/2 | `10.0.23.1` | `255.255.255.252` | ไม่เกี่ยวข้อง |
| R3 | GigabitEthernet0/0 | `10.0.23.2` | `255.255.255.252` | ไม่เกี่ยวข้อง |
| R3 | GigabitEthernet0/1 | `192.168.30.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| PC-C | FastEthernet0 | `192.168.30.10` | `255.255.255.0` | `192.168.30.1` |

Switch ทั้งสามทำงานด้วยค่าเริ่มต้นและไม่ต้องกำหนด Management IP ในแล็บนี้

## 10. ตารางการเชื่อมต่อและสายสัญญาณ

| ลำดับ | อุปกรณ์ต้นทาง | พอร์ตต้นทาง | อุปกรณ์ปลายทาง | พอร์ตปลายทาง | สายที่ใช้ |
|---:|---|---|---|---|---|
| 1 | PC-A | FastEthernet0 | SW-A | FastEthernet0/1 | Copper Straight-Through |
| 2 | SW-A | GigabitEthernet0/1 | R1 | GigabitEthernet0/0 | Copper Straight-Through |
| 3 | R1 | GigabitEthernet0/1 | R2 | GigabitEthernet0/0 | Copper Crossover |
| 4 | R2 | GigabitEthernet0/1 | SW-B | GigabitEthernet0/1 | Copper Straight-Through |
| 5 | SW-B | FastEthernet0/1 | PC-B | FastEthernet0 | Copper Straight-Through |
| 6 | R2 | GigabitEthernet0/2 | R3 | GigabitEthernet0/0 | Copper Crossover |
| 7 | R3 | GigabitEthernet0/1 | SW-C | GigabitEthernet0/1 | Copper Straight-Through |
| 8 | SW-C | FastEthernet0/1 | PC-C | FastEthernet0 | Copper Straight-Through |

## 11. แผน Static และ Default Route

| Router | Destination | Mask/Prefix | Next Hop | ประเภท |
|---|---|---|---|---|
| R1 | ทุก Network ที่ไม่รู้จัก | `/0` | `10.0.12.2` | Default Route |
| R2 | `192.168.10.0` | `/24` | `10.0.12.1` | Static Route |
| R2 | `192.168.30.0` | `/24` | `10.0.23.2` | Static Route |
| R3 | ทุก Network ที่ไม่รู้จัก | `/0` | `10.0.23.1` | Default Route |

เส้นทางไป LAN B ไม่ต้องกำหนดเพิ่มบน R2 เพราะ `192.168.20.0/24` เชื่อมตรงกับ R2

## 12. ขั้นตอนการปฏิบัติงาน

### ตอนที่ 1: สร้างและบันทึกไฟล์งาน

1. เปิด Cisco Packet Tracer
2. สร้างไฟล์ใหม่
3. บันทึกเป็น `CPT-U03-L10-ชื่อผู้เรียน.pkt`
4. วางและตั้งชื่ออุปกรณ์ตามข้อ 7
5. ต่อสายตามข้อ 10
6. ตรวจพอร์ตและชนิดสายทุกเส้น

### ตอนที่ 2: กำหนด IP ให้ PC

กำหนดผ่าน Desktop > IP Configuration

#### PC-A

```text
IP Address:      192.168.10.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.10.1
```

#### PC-B

```text
IP Address:      192.168.20.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.20.1
```

#### PC-C

```text
IP Address:      192.168.30.10
Subnet Mask:     255.255.255.0
Default Gateway: 192.168.30.1
```

ใช้ `ipconfig` ตรวจทุกเครื่องก่อนกำหนด Router

### ตอนที่ 3: กำหนด Interface ของ R1

เปิดแท็บ CLI ของ R1 หากพบ Initial Configuration Dialog ให้ตอบ `no` แล้วพิมพ์

```text
enable
configure terminal
hostname R1
no ip domain-lookup
interface gigabitEthernet 0/0
 description LAN_A
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 description LINK_TO_R2
 ip address 10.0.12.1 255.255.255.252
 no shutdown
end
```

ตรวจสอบ

```text
show ip interface brief
```

### ตอนที่ 4: กำหนด Interface ของ R2

```text
enable
configure terminal
hostname R2
no ip domain-lookup
interface gigabitEthernet 0/0
 description LINK_TO_R1
 ip address 10.0.12.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 description LAN_B
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/2
 description LINK_TO_R3
 ip address 10.0.23.1 255.255.255.252
 no shutdown
end
```

ตรวจสอบ

```text
show ip interface brief
```

### ตอนที่ 5: กำหนด Interface ของ R3

```text
enable
configure terminal
hostname R3
no ip domain-lookup
interface gigabitEthernet 0/0
 description LINK_TO_R2
 ip address 10.0.23.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 description LAN_C
 ip address 192.168.30.1 255.255.255.0
 no shutdown
end
```

ตรวจสอบ

```text
show ip interface brief
```

### จุดตรวจสอบที่ 1: Interface

| Router | Interface | IP/Prefix | Status/Protocol ที่ต้องได้ | ผ่าน |
|---|---|---|---|:---:|
| R1 | G0/0 | `192.168.10.1/24` | up/up | ☐ |
| R1 | G0/1 | `10.0.12.1/30` | up/up | ☐ |
| R2 | G0/0 | `10.0.12.2/30` | up/up | ☐ |
| R2 | G0/1 | `192.168.20.1/24` | up/up | ☐ |
| R2 | G0/2 | `10.0.23.1/30` | up/up | ☐ |
| R3 | G0/0 | `10.0.23.2/30` | up/up | ☐ |
| R3 | G0/1 | `192.168.30.1/24` | up/up | ☐ |

### ตอนที่ 6: ทดสอบ Directly Connected Networks

ก่อนเพิ่ม Static Route ให้ทดสอบเฉพาะ Neighbor และ Gateway

ที่ R1

```text
ping 10.0.12.2
ping 192.168.10.10
```

ที่ R2

```text
ping 10.0.12.1
ping 10.0.23.2
ping 192.168.20.10
```

ที่ R3

```text
ping 10.0.23.1
ping 192.168.30.10
```

ทุก Directly Connected Test ต้องผ่านก่อนสร้าง Route

### ตอนที่ 7: ตรวจ Routing Table ก่อนเพิ่ม Static Route

บน R1, R2 และ R3 พิมพ์

```text
show ip route
```

บันทึกจำนวน Connected Networks

| Router | Connected Networks ที่พบ | มี Remote LAN Route หรือยัง |
|---|---|---|
| R1 | ........................................................ | มี / ไม่มี |
| R2 | ........................................................ | มี / ไม่มี |
| R3 | ........................................................ | มี / ไม่มี |

ผลที่คาดหวังคือแต่ละ Router รู้เฉพาะ Network ที่เชื่อมตรง และยังไม่รู้ Remote LAN

### ตอนที่ 8: ทดสอบก่อนเพิ่ม Route

ที่ PC-A

```text
ping 192.168.20.10
ping 192.168.30.10
```

ผลควรไม่ผ่าน เพราะ Router ยังไม่มีเส้นทางครบไปยัง Remote LAN และเส้นทางขากลับ

บันทึกผล

| ปลายทาง | ผลก่อน Static/Default Route |
|---|---|
| PC-B | ผ่าน / ไม่ผ่าน |
| PC-C | ผ่าน / ไม่ผ่าน |

### ตอนที่ 9: กำหนด Static Route บน R2

R2 เป็น Router ศูนย์กลางและต้องรู้เส้นทางเฉพาะไป LAN A และ LAN C

```text
configure terminal
ip route 192.168.10.0 255.255.255.0 10.0.12.1
ip route 192.168.30.0 255.255.255.0 10.0.23.2
end
```

ตรวจสอบ

```text
show ip route
```

บันทึก

| Code | Destination | Next Hop | ถูกต้องหรือไม่ |
|---|---|---|---|
| ........ | `192.168.10.0/24` | ........................ | ถูก / ผิด |
| ........ | `192.168.30.0/24` | ........................ | ถูก / ผิด |

ควรเห็น Code `S` และ Next Hop ตรงตามแผน

### ตอนที่ 10: กำหนด Default Route บน R1

```text
configure terminal
ip route 0.0.0.0 0.0.0.0 10.0.12.2
end
```

ตรวจสอบ

```text
show ip route
```

ควรพบ

- Default Route รหัส `S*`
- Next Hop `10.0.12.2`
- Gateway of Last Resort ชี้ไป R2

### ตอนที่ 11: ทดสอบ PC-A ไป PC-B

หลัง R1 มี Default Route และ R2 มี Route กลับ LAN A ให้ทดสอบ

```text
ping 192.168.20.10
```

ผลที่คาดหวังคือ PC-A ติดต่อ PC-B ได้

เหตุผล

```text
ขาไป:    PC-A → R1 Default → R2 → LAN B
ขากลับ:  PC-B → R2 Static Route → R1 → LAN A
```

### ตอนที่ 12: กำหนด Default Route บน R3

```text
configure terminal
ip route 0.0.0.0 0.0.0.0 10.0.23.1
end
```

ตรวจสอบ

```text
show ip route
```

ควรพบ `S* 0.0.0.0/0` ผ่าน `10.0.23.1`

### ตอนที่ 13: ทดสอบ End-to-End ทุก LAN

ที่ PC-A

```text
ping 192.168.20.10
ping 192.168.30.10
```

ที่ PC-B

```text
ping 192.168.10.10
ping 192.168.30.10
```

ที่ PC-C

```text
ping 192.168.10.10
ping 192.168.20.10
```

ทุกคู่ควร Ping ผ่านหลัง ARP Learning เสร็จ

### ตอนที่ 14: ตรวจ Route แบบเจาะจง

บน R1

```text
show ip route 192.168.30.0
```

บน R2

```text
show ip route 192.168.10.0
show ip route 192.168.30.0
```

บน R3

```text
show ip route 192.168.10.0
```

> หาก IOS จำลองไม่รองรับการระบุ Network ต่อท้าย ให้ใช้ `show ip route` แล้วค้นหาบรรทัดด้วยตนเอง

### ตอนที่ 15: ใช้ tracert จาก PC-A

ที่ PC-A พิมพ์

```text
tracert 192.168.20.10
```

บันทึก Hop ที่พบ

| Hop | IP Address ที่พบ | อุปกรณ์ที่คาดว่าเป็น |
|---:|---|---|
| 1 | ................................ | R1 |
| 2 | ................................ | R2 หรือปลายทางตามการแสดงผล |
| 3 | ................................ | PC-B หากแสดง |

จากนั้นพิมพ์

```text
tracert 192.168.30.10
```

| Hop | IP Address ที่พบ | อุปกรณ์ที่คาดว่าเป็น |
|---:|---|---|
| 1 | ................................ | R1 |
| 2 | ................................ | R2 |
| 3 | ................................ | R3 |
| 4 | ................................ | PC-C หากแสดง |

> IP ที่ Router ใช้ตอบ Traceroute อาจเป็น Interface ที่ต่างจากการคาดเดาตาม IOS จำลอง ให้พิจารณาลำดับอุปกรณ์และจำนวน Hop เป็นหลัก

### ตอนที่ 16: เปรียบเทียบ Ping กับ Tracert

| เครื่องมือ | ตอบคำถามอะไร | ข้อมูลที่ได้ |
|---|---|---|
| `ping` | ปลายทางตอบกลับได้หรือไม่ | Success/Failure, Packet Loss |
| `tracert` | Packet ผ่าน Router ใดบ้าง | ลำดับ Hop และจุดที่อาจหยุด |

### ตอนที่ 17: ทดลองลบ Forward Route บน R2

กิจกรรมนี้แสดงผลเมื่อ Router ศูนย์กลางไม่มี Route ไป LAN C

1. บน R2 ลบ Route ชั่วคราว

```text
configure terminal
no ip route 192.168.30.0 255.255.255.0 10.0.23.2
end
```

2. ตรวจ

```text
show ip route
```

3. ที่ PC-A ทดสอบ

```text
ping 192.168.30.10
tracert 192.168.30.10
```

4. บันทึกจุดที่ Traffic หยุด

....................................................................................................

5. คืน Route ทันที

```text
configure terminal
ip route 192.168.30.0 255.255.255.0 10.0.23.2
end
```

6. ตรวจ Route และ Ping PC-C จนผ่าน

### ตอนที่ 18: ทดลองลบ Return Default Route บน R3

กิจกรรมนี้แสดงว่า Echo Request อาจเดินทางถึง LAN C แต่ Echo Reply ไม่มีเส้นทางกลับ

1. บน R3 ลบ Default Route

```text
configure terminal
no ip route 0.0.0.0 0.0.0.0 10.0.23.1
end
```

2. ตรวจว่า `S*` และ Gateway of Last Resort หายไป

```text
show ip route
```

3. ที่ PC-A Ping PC-C

```text
ping 192.168.30.10
```

4. บันทึกผล

```text
ผล Ping: ................................
Forward Path มีหรือไม่: .................
Return Path มีหรือไม่: ..................
```

5. คืน Default Route บน R3

```text
configure terminal
ip route 0.0.0.0 0.0.0.0 10.0.23.1
end
```

6. ตรวจ `show ip route` และ Ping ซ้ำจนผ่าน

### ตอนที่ 19: บันทึก Routing Table ขั้นสุดท้าย

กรอกเฉพาะ Static/Default Routes

| Router | Code | Destination | Next Hop |
|---|---|---|---|
| R1 | ........ | ........................ | ........................ |
| R2 | ........ | ........................ | ........................ |
| R2 | ........ | ........................ | ........................ |
| R3 | ........ | ........................ | ........................ |

### ตอนที่ 20: บันทึก Configuration และไฟล์

บน Router ทั้งสามใช้

```text
copy running-config startup-config
```

จากนั้น

1. ตรวจ Route ทั้งหมดอีกครั้ง
2. ทดสอบ PC-A, PC-B และ PC-C หากันทุกคู่
3. กด `Ctrl+S`
4. ปิดและเปิดไฟล์ใหม่
5. ตรวจ Interface, Routing Table, Ping และ Tracert

## 13. ชุดคำสั่งฉบับรวม

### 13.1 R1

```text
enable
configure terminal
hostname R1
no ip domain-lookup
interface gigabitEthernet 0/0
 description LAN_A
 ip address 192.168.10.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/1
 description LINK_TO_R2
 ip address 10.0.12.1 255.255.255.252
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 10.0.12.2
end
copy running-config startup-config
```

### 13.2 R2

```text
enable
configure terminal
hostname R2
no ip domain-lookup
interface gigabitEthernet 0/0
 description LINK_TO_R1
 ip address 10.0.12.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 description LAN_B
 ip address 192.168.20.1 255.255.255.0
 no shutdown
exit
interface gigabitEthernet 0/2
 description LINK_TO_R3
 ip address 10.0.23.1 255.255.255.252
 no shutdown
exit
ip route 192.168.10.0 255.255.255.0 10.0.12.1
ip route 192.168.30.0 255.255.255.0 10.0.23.2
end
copy running-config startup-config
```

### 13.3 R3

```text
enable
configure terminal
hostname R3
no ip domain-lookup
interface gigabitEthernet 0/0
 description LINK_TO_R2
 ip address 10.0.23.2 255.255.255.252
 no shutdown
exit
interface gigabitEthernet 0/1
 description LAN_C
 ip address 192.168.30.1 255.255.255.0
 no shutdown
exit
ip route 0.0.0.0 0.0.0.0 10.0.23.1
end
copy running-config startup-config
```

## 14. คำสั่งที่ใช้ในแล็บ

| คำสั่ง | หน้าที่ |
|---|---|
| `ip route <network> <mask> <next-hop>` | สร้าง Static Route |
| `ip route 0.0.0.0 0.0.0.0 <next-hop>` | สร้าง Default Route |
| `no ip route ...` | ลบ Route ที่ตรงกับ Parameter |
| `show ip route` | แสดง Routing Table |
| `show ip route <network>` | แสดง Route เฉพาะ Network หาก IOS รองรับ |
| `show ip interface brief` | ตรวจ Interface และ IP |
| `ping` | ทดสอบ Reachability |
| `tracert` | แสดงลำดับ Hop จาก Packet Tracer PC |

## 15. ตารางทดสอบขั้นสุดท้าย

| ต้นทาง | ปลายทาง | เส้นทางที่คาดหวัง | Ping | Tracert สอดคล้อง |
|---|---|---|---|---|
| PC-A | PC-B | R1 → R2 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |
| PC-A | PC-C | R1 → R2 → R3 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |
| PC-B | PC-A | R2 → R1 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |
| PC-B | PC-C | R2 → R3 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |
| PC-C | PC-A | R3 → R2 → R1 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |
| PC-C | PC-B | R3 → R2 | ผ่าน / ไม่ผ่าน | ใช่ / ไม่ใช่ |

## 16. การทดสอบการทำงานขั้นสุดท้าย

| ลำดับ | รายการทดสอบ | เกณฑ์ที่ผ่าน | ผ่าน | ไม่ผ่าน |
|---:|---|---|:---:|:---:|
| 1 | Topology | อุปกรณ์ครบและต่อสายครบ 8 เส้น | ☐ | ☐ |
| 2 | LAN Addressing | PC และ Gateway ทั้งสาม LAN ถูกต้อง | ☐ | ☐ |
| 3 | Point-to-Point | `/30` ทั้งสองลิงก์ถูกต้องและ up/up | ☐ | ☐ |
| 4 | R1 Route | มี `S* 0.0.0.0/0` ผ่าน R2 | ☐ | ☐ |
| 5 | R2 Routes | มี `S` ไป LAN A และ LAN C | ☐ | ☐ |
| 6 | R3 Route | มี `S* 0.0.0.0/0` ผ่าน R2 | ☐ | ☐ |
| 7 | End-to-End Ping | PC ทุก LAN Ping หากันได้ | ☐ | ☐ |
| 8 | Tracert | ระบุลำดับ Router ไป LAN B/C ได้ | ☐ | ☐ |
| 9 | Forward Route Test | ลบ วิเคราะห์ และคืน Route บน R2 แล้ว | ☐ | ☐ |
| 10 | Return Route Test | ลบ วิเคราะห์ และคืน Default บน R3 แล้ว | ☐ | ☐ |
| 11 | Persistence | Router ทั้งสามและไฟล์ `.pkt` บันทึกครบ | ☐ | ☐ |

### ผลการทดสอบโดยรวม

- [ ] ผ่านครบทุกข้อ
- [ ] ผ่านบางข้อและแก้ไขเรียบร้อยแล้ว
- [ ] ยังมีปัญหาและต้องการความช่วยเหลือ

## 17. แนวทางแก้ไขปัญหาเบื้องต้น

ตรวจตามลำดับ

```text
1. ตรวจ PC IP/Mask/Gateway
2. Ping Gateway ของแต่ละ LAN
3. ตรวจ Router Interface up/up
4. Ping Router เพื่อนบ้านบน /30
5. ตรวจ Connected Route
6. ตรวจ Static/Default Route
7. ตรวจ Forward Path
8. ตรวจ Return Path
9. ใช้ tracert หาจุดที่หยุด
```

| อาการ | สาเหตุที่เป็นไปได้ | วิธีตรวจสอบหรือแก้ไข |
|---|---|---|
| Router เพื่อนบ้าน Ping กันไม่ได้ | `/30` ผิด สายผิด หรือ Interface Down | ตรวจ IP/Mask, Crossover และ `show ip interface brief` |
| Static Route ไม่ปรากฏ | Next Hop ไม่ Reachable หรือพิมพ์ Network/Mask ผิด | ตรวจ Connected Route ไป Next Hop และคำสั่ง `ip route` |
| R1 ไม่มี Gateway of Last Resort | Default Route ขาดหรือ Next Hop ใช้ไม่ได้ | ตรวจ `S*` และ `10.0.12.2` |
| PC-A Ping PC-B ได้แต่ PC-C ไม่ได้ | R2 Route ไป LAN C หรือ R3 Default ขาด | ตรวจ R2/R3 Routing Table |
| Echo Request ถึงปลายทางแต่ไม่มี Reply | Return Route ขาด | ตรวจ Route จากฝั่งปลายทางกลับ Source LAN |
| Tracert หยุดที่ R2 | R2 ไม่มี Route ถัดไปหรือ Link R2–R3 ผิด | ตรวจ `192.168.30.0/24` และ `10.0.23.0/30` |
| Route ใช้ Network Address ผิด | ใส่ Host Address แทน Destination Network | ใช้ `.0` พร้อม Mask `/24` ใน Static Route |
| Next Hop เป็น `.3` บน `/30` | `.3` เป็น Broadcast Address | ใช้ Usable Address `.1` หรือ `.2` ตามตาราง |
| Ping ครั้งแรกสูญหาย | ARP Learning | Ping ซ้ำก่อนสรุป |
| หลังแบบทดลองยัง Ping ไม่ผ่าน | ลืมคืน Route | เปรียบเทียบกับแผน Route ในข้อ 11 |

## 18. แบบบันทึกผลการปฏิบัติงาน

| รายการ | ข้อมูลของผู้เรียน |
|---|---|
| ชื่อ–นามสกุล | ............................................................ |
| ชั้น/กลุ่ม | ............................................................ |
| วันที่ทำแล็บ | ............................................................ |
| ชื่อไฟล์ที่ส่ง | ............................................................ |
| เวลาที่ใช้จริง | ............................................................ |

### Route ที่กำหนดบนแต่ละ Router

....................................................................................................

....................................................................................................

### จุดที่ Tracert หยุดเมื่อ Route หาย

....................................................................................................

### ปัญหาที่พบและวิธีแก้ไข

....................................................................................................

....................................................................................................

## 19. คำถามท้ายแล็บ

1. Static Route แตกต่างจาก Connected Route อย่างไร

   คำตอบ: ............................................................................................

2. Default Route `0.0.0.0/0` ใช้เมื่อใด

   คำตอบ: ............................................................................................

3. เพราะเหตุใด R1 และ R3 เหมาะกับ Default Route

   คำตอบ: ............................................................................................

4. เพราะเหตุใด R2 ต้องมี Specific Static Route ไป LAN A และ LAN C

   คำตอบ: ............................................................................................

5. `S` และ `S*` ใน Routing Table ต่างกันอย่างไร

   คำตอบ: ............................................................................................

6. Gateway of Last Resort หมายถึงอะไร

   คำตอบ: ............................................................................................

7. หาก R2 มี Route ไป LAN C แต่ R3 ไม่มี Route กลับ LAN A เหตุใด Ping จึงไม่สำเร็จ

   คำตอบ: ............................................................................................

8. `ping` และ `tracert` ให้ข้อมูลต่างกันอย่างไร

   คำตอบ: ............................................................................................

9. เพราะเหตุใดลิงก์ R1–R2 และ R2–R3 จึงใช้ `/30`

   คำตอบ: ............................................................................................

10. เมื่อมี Specific Route `/24` และ Default Route `/0` ที่ตรงกับ Destination เดียวกัน Router เลือกเส้นทางใด

    คำตอบ: ...........................................................................................

## 20. สรุปผลการเรียนรู้โดยผู้เรียน

หลังทำแล็บนี้ ฉันสามารถ

- [ ] ออกแบบและกำหนด Point-to-Point `/30` ได้
- [ ] สร้าง Static Route ด้วย Next-Hop Address ได้
- [ ] สร้าง Default Route ได้
- [ ] อ่าน `S`, `S*` และ Gateway of Last Resort ได้
- [ ] ตรวจ Forward Path และ Return Path ได้
- [ ] ใช้ Ping และ Tracert ร่วมกันวิเคราะห์ปัญหาได้
- [ ] ลบและคืน Route ได้อย่างถูกต้อง
- [ ] ตรวจสอบ End-to-End Connectivity ผ่านหลาย Router ได้

สิ่งสำคัญที่ได้เรียนรู้จากแล็บนี้คือ

....................................................................................................

....................................................................................................

สิ่งที่ต้องการฝึกเพิ่มเติมคือ

....................................................................................................

....................................................................................................

## 21. เกณฑ์การประเมิน

| หัวข้อประเมิน | คะแนนเต็ม |
|---|---:|
| Topology, LAN และ Point-to-Point Addressing ถูกต้อง | 2 |
| กำหนด Static/Default Route และอ่าน Routing Table ได้ | 2 |
| ทดสอบ Ping/Tracert และอธิบายเส้นทางได้ | 2 |
| วิเคราะห์ Forward/Return Route ที่หายและคืนค่าได้ | 2 |
| Troubleshooting บันทึกผล ตอบคำถาม และสรุปผลได้ | 2 |
| **รวม** | **10** |

เกณฑ์ผ่านที่แนะนำ: **อย่างน้อย 7 คะแนนจาก 10 คะแนน** โดย PC-A, PC-B และ PC-C ต้อง Ping หากันได้ทุกคู่ และ Routing Table ต้องตรงตามแผนหลังคืน Route แล้ว

## 22. งานที่ต้องส่ง

1. ไฟล์ Packet Tracer ชื่อ `CPT-U03-L10-ชื่อผู้เรียน.pkt`
2. ใบงานที่กรอก Interface, Routing Table, Ping, Tracert, Forward/Return Route Test, คำถามท้ายแล็บ และสรุปผลครบถ้วน

---

## ภาคผนวกสำหรับผู้สอน: แนวคำตอบย่อ

### Routing Table สำคัญที่คาดหวัง

#### R1

```text
C 192.168.10.0/24
C 10.0.12.0/30
S* 0.0.0.0/0 via 10.0.12.2
```

#### R2

```text
C 10.0.12.0/30
C 192.168.20.0/24
C 10.0.23.0/30
S 192.168.10.0/24 via 10.0.12.1
S 192.168.30.0/24 via 10.0.23.2
```

#### R3

```text
C 10.0.23.0/30
C 192.168.30.0/24
S* 0.0.0.0/0 via 10.0.23.1
```

Local `/32` Routes อาจแสดงเพิ่มเติมตาม IOS

### แนวคำตอบคำถามท้ายแล็บ

1. Connected Route เกิดอัตโนมัติจาก Interface up/up ส่วน Static Route ผู้ดูแลกำหนดเอง
2. ใช้เมื่อไม่มี Route ที่เฉพาะเจาะจงกว่าตรงกับ Destination
3. ทั้งสองมีทางออกหลักไป Remote Networks ผ่าน R2 เพียงทิศทางเดียว
4. R2 ต้องเลือกส่ง LAN A ไป R1 และ LAN C ไป R3 ซึ่งเป็นคนละ Next Hop
5. `S` คือ Static Route ส่วน `S*` คือ Candidate Default Static Route
6. Next Hop ที่ Router ใช้ส่ง Packet เมื่อไม่มี Route อื่นตรงกับ Destination
7. Echo Request ไปถึงได้ แต่ Echo Reply ไม่มีเส้นทางกลับ Source Network
8. Ping บอก Reachability และ Packet Loss ส่วน Tracert แสดงลำดับ Hop และจุดที่เส้นทางหยุด
9. Point-to-Point ต้องการ IP ใช้งานสองหมายเลข `/30` จึงให้ Usable Hosts 2 หมายเลข
10. เลือก Specific `/24` เพราะ Longest Prefix Match เฉพาะเจาะจงกว่า `/0`

### ข้อสังเกตสำหรับผู้สอน

- ก่อนรับไฟล์ตรวจว่า Route ที่ลบในกิจกรรมถูกคืนครบทั้ง R2 และ R3
- Tracert ใน Packet Tracer แต่ละรุ่นอาจแสดง Interface Address หรือ Timeout ต่างกัน ควรประเมินลำดับ Router และความสามารถระบุจุดหยุด
- หาก Straight-Through ใช้งานได้ระหว่าง Router เพราะ Auto-MDI/MDIX ให้ยึด Crossover ตามหลักการและตารางของใบงาน
- การใช้ Default Route บน R1/R3 เหมาะกับ Topology แบบ Stub นี้ แต่ไม่ควรสรุปว่า Default Route เหมาะกับทุก Router

### แล็บถัดไป

**หน่วยที่ 3 แล็บที่ 11: การแบ่งเครือข่ายด้วย VLAN**
