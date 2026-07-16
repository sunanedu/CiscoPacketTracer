# ใบงานหน่วยที่ 2 แล็บที่ 7

## การตั้งค่าพื้นฐานด้วย Cisco IOS CLI

| รายการ | รายละเอียด |
|---|---|
| รหัสใบงาน | CPT-U02-L07 |
| ระดับ | ผู้เริ่มต้น–พื้นฐาน |
| ระยะเวลาโดยประมาณ | 180 นาที |
| รูปแบบการทำงาน | รายบุคคล หรือกลุ่มละไม่เกิน 2 คน |
| แล็บที่ควรทำก่อน | หน่วยที่ 1 แล็บที่ 2 และหน่วยที่ 2 แล็บที่ 5–6 |
| ไฟล์ที่ต้องส่ง | `CPT-U02-L07-ชื่อผู้เรียน.pkt` และใบงานที่บันทึกผลแล้ว |

---

## 1. เรื่องของใบงาน

ใบงานนี้เป็นการเริ่มต้นใช้งาน **Cisco IOS Command-Line Interface หรือ CLI** เพื่อกำหนดค่า Router และ Switch ผู้เรียนจะเชื่อมต่อผ่าน Console Cable เรียนรู้โหมดคำสั่ง กำหนด Hostname, Enable Secret, Local Username, Console/VTY Login, MOTD Banner และ IP Address ของ Interface

จากนั้นผู้เรียนจะตรวจสอบ Running Configuration บันทึกเป็น Startup Configuration ทดสอบการเข้าถึงจาก User-PC และ Reload อุปกรณ์เพื่อยืนยันว่าค่าที่บันทึกไว้ยังคงอยู่

> รหัสผ่านในใบงานเป็นตัวอย่างสำหรับห้องปฏิบัติการเท่านั้น ห้ามนำไปใช้กับอุปกรณ์จริงหรือระบบ Production

## 2. เป้าหมายการเรียนรู้

เมื่อทำใบงานเสร็จ ผู้เรียนควรสามารถทำสิ่งต่อไปนี้ได้

1. เชื่อม PC เข้ากับ Router/Switch ผ่าน Console Cable ได้
2. อธิบายความแตกต่างของ User EXEC, Privileged EXEC, Global Configuration และ Subconfiguration Mode ได้
3. ใช้ `?`, Tab, ลูกศรขึ้น และเครื่องหมาย `^` เพื่อช่วยทำงานใน CLI ได้
4. กำหนด Hostname และปิด DNS Lookup ได้
5. กำหนด Enable Secret และ Local Username ได้
6. กำหนด Console Line, VTY Line และ MOTD Banner ได้
7. กำหนด IPv4 Address ให้ Router Interface ผ่าน CLI ได้
8. กำหนด Management IP และ Default Gateway ให้ Switch ได้
9. ใช้คำสั่ง `show` ตรวจสอบสถานะและ Configuration ได้
10. บันทึก Running Configuration เป็น Startup Configuration ได้
11. ทดสอบการเชื่อมต่อด้วย `ping` และ Remote Login แบบ Telnet ภายในแล็บได้
12. Reload และตรวจสอบว่า Configuration ยังคงอยู่ได้

## 3. ความรู้พื้นฐานก่อนเริ่ม

ผู้เรียนควรสามารถ

- ระบุพอร์ต RS-232 และ Console ได้
- ต่อสาย Copper Straight-Through ได้
- กำหนด IP Address ให้ PC ผ่าน IP Configuration ได้
- อธิบาย IP Address, Subnet Mask และ Default Gateway ได้
- ใช้ `ipconfig` และ `ping` จาก PC Command Prompt ได้

## 4. แนวคิดพื้นฐานของ Cisco IOS

### 4.1 Cisco IOS คืออะไร

Cisco IOS เป็นระบบปฏิบัติการเครือข่ายที่ใช้ควบคุมและกำหนดค่าอุปกรณ์ Cisco หลายประเภท เช่น Router และ Switch ผู้ดูแลสามารถสั่งงานผ่าน CLI โดยคำสั่งที่ใช้ได้จะขึ้นอยู่กับโหมดปัจจุบัน

### 4.2 โหมดคำสั่งหลัก

| โหมด | ตัวอย่าง Prompt | เข้าโหมดด้วย | ออกจากโหมดด้วย | ใช้ทำอะไร |
|---|---|---|---|---|
| User EXEC | `Router>` | เปิด Console/Login | `exit` | ตรวจสอบข้อมูลพื้นฐานบางส่วน |
| Privileged EXEC | `Router#` | `enable` | `disable` หรือ `exit` | ใช้คำสั่ง Show, Copy, Reload และเข้า Config |
| Global Configuration | `Router(config)#` | `configure terminal` | `end` หรือ `exit` | ตั้งค่าระดับอุปกรณ์ |
| Interface Configuration | `Router(config-if)#` | `interface ...` | `exit` หรือ `end` | ตั้งค่า Interface |
| Line Configuration | `Router(config-line)#` | `line console 0` หรือ `line vty ...` | `exit` หรือ `end` | ตั้งค่าการ Login |

เส้นทางที่ใช้บ่อย

```text
Router>
   enable
Router#
   configure terminal
Router(config)#
   interface gigabitEthernet 0/0
Router(config-if)#
   exit
Router(config)#
   end
Router#
```

### 4.3 Running Configuration และ Startup Configuration

| Configuration | ตำแหน่งโดยทั่วไป | คุณสมบัติ |
|---|---|---|
| Running Configuration | RAM | ค่าที่กำลังใช้งาน เปลี่ยนทันที และหายได้เมื่อ Reload หากยังไม่บันทึก |
| Startup Configuration | NVRAM | ค่าที่โหลดขึ้นมาเมื่ออุปกรณ์เริ่มทำงาน |

คำสั่งบันทึก

```text
copy running-config startup-config
```

### 4.4 เครื่องมือช่วยใน CLI

| เครื่องมือ | ตัวอย่าง | หน้าที่ |
|---|---|---|
| `?` | `show ?` | แสดงคำสั่งหรือตัวเลือกที่ใช้ต่อได้ |
| Tab | พิมพ์ `conf` แล้วกด Tab | เติมคำสั่งให้สมบูรณ์เมื่อไม่กำกวม |
| ลูกศรขึ้น | กด ↑ | เรียกคำสั่งก่อนหน้ากลับมา |
| `Ctrl+C` | ระหว่างคำสั่ง/Prompt | ยกเลิกคำสั่งหรือขั้นตอนบางชนิด |
| `Ctrl+Z` | ใน Configuration Mode | กลับ Privileged EXEC คล้าย `end` |
| เครื่องหมาย `^` | แสดงใต้คำสั่งที่ผิด | ชี้บริเวณที่ IOS พบ Syntax Error |

### 4.5 Console และ VTY

- **Console Line** ใช้บริหารอุปกรณ์โดยต่อสายตรง ไม่ต้องมี IP Address
- **VTY Line** ใช้บริหารอุปกรณ์ผ่านเครือข่าย เช่น Telnet หรือ SSH
- แล็บนี้ใช้ Telnet เพื่อให้เห็นการทำงานของ VTY เท่านั้น Telnet ส่งข้อมูลแบบไม่เข้ารหัส ระบบจริงควรใช้ SSH

### 4.6 Enable Secret และ Password Encryption

- `enable secret` ใช้ป้องกันการเข้าสู่ Privileged EXEC และควรเลือกใช้แทน `enable password`
- `username ... secret ...` เก็บ Secret ของ Local User ในรูปแบบที่ไม่แสดงเป็นข้อความเดิมโดยตรง
- `service password-encryption` ช่วยซ่อน Line Password บางชนิด แต่การเข้ารหัส Type 7 ไม่ถือว่าปลอดภัยสำหรับระบบจริง

## 5. ข้อมูลบัญชีสำหรับแล็บ

| รายการ | ค่าในแล็บ |
|---|---|
| Local Username | `admin` |
| Local User Secret | `Admin123` |
| Enable Secret | `Enable123` |
| MOTD Banner | `AUTHORIZED USERS ONLY` |

> ใช้ค่าตามตารางเพื่อให้ผู้สอนตรวจงานได้ง่าย รหัสเหล่านี้ไม่ใช่แนวทางการตั้งรหัสผ่านสำหรับ Production

## 6. อุปกรณ์และซอฟต์แวร์ที่ใช้

### 6.1 ซอฟต์แวร์

- โปรแกรม Cisco Packet Tracer จำนวน 1 ชุด
- โฟลเดอร์สำหรับบันทึกไฟล์งานของผู้เรียน

### 6.2 อุปกรณ์จำลองใน Packet Tracer

| ลำดับ | ประเภทอุปกรณ์ | รุ่นที่แนะนำ | จำนวน | ชื่อที่กำหนด |
|---:|---|---|---:|---|
| 1 | Router | Cisco 2911 | 1 | `R1` |
| 2 | Switch | Cisco 2960 | 1 | `SW1` |
| 3 | PC สำหรับ Console | PC-PT | 1 | `R1-CONSOLE-PC` |
| 4 | PC สำหรับ Console | PC-PT | 1 | `SW1-CONSOLE-PC` |
| 5 | PC ผู้ใช้งาน | PC-PT | 1 | `USER-PC` |

## 7. แผนผังเครือข่าย

```text
R1-CONSOLE-PC RS-232 -------- Console R1
                         Console Cable |
                                       | Gi0/0
                                       |
                                       | Copper Straight-Through
                                       |
                                 Gi0/1 SW1 Fa0/1 -------- Fa0 USER-PC
                                                         Straight-Through
                                       |
                           Console SW1 -------- RS-232 SW1-CONSOLE-PC
                                   Console Cable
```

มุมมองอย่างย่อ

```text
R1-CONSOLE-PC --Console-- R1 ---- SW1 ---- USER-PC
                                  |
                               Console
                                  |
                          SW1-CONSOLE-PC
```

## 8. ตารางกำหนด IP Address

| อุปกรณ์ | Interface | IP Address | Subnet Mask | Default Gateway |
|---|---|---|---|---|
| R1 | GigabitEthernet0/0 | `192.168.50.1` | `255.255.255.0` | ไม่เกี่ยวข้อง |
| SW1 | VLAN 1 | `192.168.50.2` | `255.255.255.0` | `192.168.50.1` |
| USER-PC | FastEthernet0 | `192.168.50.10` | `255.255.255.0` | `192.168.50.1` |
| R1-CONSOLE-PC | RS-232 | ไม่กำหนด | ไม่กำหนด | ไม่กำหนด |
| SW1-CONSOLE-PC | RS-232 | ไม่กำหนด | ไม่กำหนด | ไม่กำหนด |

> Console PC ไม่ต้องมี IP Address สำหรับการใช้งาน Console เพราะเป็นการเชื่อมต่อโดยตรงแบบ Out-of-Band

## 9. ตารางการเชื่อมต่อและสายสัญญาณ

| ลำดับ | อุปกรณ์ต้นทาง | พอร์ตต้นทาง | อุปกรณ์ปลายทาง | พอร์ตปลายทาง | สายที่ใช้ |
|---:|---|---|---|---|---|
| 1 | R1-CONSOLE-PC | RS-232 | R1 | Console | Console Cable |
| 2 | SW1-CONSOLE-PC | RS-232 | SW1 | Console | Console Cable |
| 3 | R1 | GigabitEthernet0/0 | SW1 | GigabitEthernet0/1 | Copper Straight-Through |
| 4 | SW1 | FastEthernet0/1 | USER-PC | FastEthernet0 | Copper Straight-Through |

## 10. แผนการกำหนดค่า

| อุปกรณ์ | Hostname | Interface ที่กำหนด IP | IP/Prefix | จุดประสงค์ |
|---|---|---|---|---|
| Router | `R1` | G0/0 | `192.168.50.1/24` | Default Gateway ของ LAN |
| Switch | `SW1` | VLAN 1 | `192.168.50.2/24` | Management IP |
| PC | `USER-PC` | Fa0 | `192.168.50.10/24` | ทดสอบ Ping และ VTY |

## 11. ขั้นตอนการปฏิบัติงาน

### ตอนที่ 1: สร้างและบันทึกไฟล์งาน

1. เปิด Cisco Packet Tracer
2. เลือก File > New
3. บันทึกเป็น `CPT-U02-L07-ชื่อผู้เรียน.pkt`
4. วางอุปกรณ์ทั้งหมดตามข้อ 6
5. เปลี่ยน Display Name ตามตารางอุปกรณ์
6. ต่อสายตามข้อ 9
7. กด `Ctrl+S`

### ตอนที่ 2: กำหนด USER-PC

1. เปิด **USER-PC > Desktop > IP Configuration**
2. เลือก Static
3. กำหนด IP Address เป็น `192.168.50.10`
4. กำหนด Subnet Mask เป็น `255.255.255.0`
5. กำหนด Default Gateway เป็น `192.168.50.1`
6. ปิดหน้าต่าง IP Configuration
7. เปิด Command Prompt แล้วพิมพ์ `ipconfig`
8. ตรวจสอบค่ากับตารางในข้อ 8

### ตอนที่ 3: เปิด Console ของ R1

1. คลิก `R1-CONSOLE-PC`
2. เปิด **Desktop > Terminal**
3. ใช้ค่า Terminal เริ่มต้น ได้แก่ 9600 bps, 8 Data Bits, No Parity, 1 Stop Bit และ No Flow Control
4. คลิก OK
5. กด Enter หากหน้าจอยังไม่แสดง Prompt
6. หากพบข้อความ

```text
Would you like to enter the initial configuration dialog? [yes/no]:
```

ให้พิมพ์

```text
no
```

7. กด Enter จนเห็น Prompt `Router>`

### ตอนที่ 4: ทดลองโหมดและ Help บน R1

ที่ Prompt `Router>` ให้ทดลองทีละข้อ

1. พิมพ์ `?` เพื่อดูคำสั่งใน User EXEC
2. พิมพ์ `show ?` แล้วสังเกตตัวเลือก
3. พิมพ์ `en` แล้วกด Tab เพื่อเติมเป็น `enable`
4. กด Enter เพื่อเข้าสู่ Privileged EXEC
5. ตรวจว่า Prompt เปลี่ยนเป็น `Router#`
6. กดลูกศรขึ้นเพื่อเรียกคำสั่งก่อนหน้า
7. ทดลองพิมพ์คำสั่งที่ไม่มีจริง เช่น

```text
shiw
```

8. สังเกตข้อความ Error และตำแหน่งเครื่องหมาย `^`
9. ไม่ต้องบันทึกคำสั่งที่ผิดลง Configuration

### ตอนที่ 5: ตั้งค่าพื้นฐานของ R1

เริ่มจาก Prompt `Router#` พิมพ์คำสั่งตามลำดับ

```text
configure terminal
hostname R1
no ip domain-lookup
enable secret Enable123
service password-encryption
banner motd #AUTHORIZED USERS ONLY#
username admin secret Admin123
```

ตรวจว่า Prompt เปลี่ยนจาก `Router(config)#` เป็น `R1(config)#` หลังคำสั่ง `hostname R1`

คำอธิบาย

| คำสั่ง | หน้าที่ |
|---|---|
| `configure terminal` | เข้าสู่ Global Configuration Mode |
| `hostname R1` | เปลี่ยนชื่ออุปกรณ์และ Prompt |
| `no ip domain-lookup` | ป้องกัน IOS พยายามค้นหา DNS เมื่อพิมพ์คำสั่งผิด |
| `enable secret Enable123` | ตั้ง Secret สำหรับ Privileged EXEC |
| `service password-encryption` | ซ่อน Line Password บางชนิดจากข้อความธรรมดา |
| `banner motd ...` | แสดงข้อความเตือนก่อน Login |
| `username admin secret Admin123` | สร้าง Local User ชื่อ admin |

### ตอนที่ 6: ตั้งค่า Console และ VTY ของ R1

ที่ `R1(config)#` พิมพ์

```text
line console 0
login local
logging synchronous
exec-timeout 10 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 10 0
exit
```

คำอธิบาย

| คำสั่ง | หน้าที่ |
|---|---|
| `line console 0` | เข้าโหมดตั้งค่า Console Line |
| `line vty 0 4` | ตั้งค่า VTY Line 0 ถึง 4 |
| `login local` | ใช้ Local Username/Secret ตรวจสอบการ Login |
| `logging synchronous` | จัด Prompt ใหม่เมื่อมี Log แทรกระหว่างพิมพ์ |
| `exec-timeout 10 0` | ตัด Session เมื่อไม่มีการใช้งาน 10 นาที |
| `transport input telnet` | อนุญาต Telnet สำหรับกิจกรรมในแล็บ |

> ระบบจริงควรใช้ SSH แทน Telnet เพราะ Telnet ไม่เข้ารหัส Username, Password และข้อมูลใน Session

### ตอนที่ 7: ตั้งค่า Interface ของ R1

ที่ `R1(config)#` พิมพ์

```text
interface gigabitEthernet 0/0
description LAN_TO_SW1
ip address 192.168.50.1 255.255.255.0
no shutdown
end
```

หลัง `no shutdown` อาจเห็นข้อความว่า Interface เปลี่ยนสถานะเป็น Up ให้รอสักครู่จน Link พร้อม

คำอธิบาย

| คำสั่ง | หน้าที่ |
|---|---|
| `interface gigabitEthernet 0/0` | เข้า Interface Configuration Mode |
| `description LAN_TO_SW1` | บันทึกคำอธิบายการเชื่อมต่อ |
| `ip address ...` | กำหนด IPv4 Address และ Subnet Mask |
| `no shutdown` | เปิดใช้งาน Interface |
| `end` | กลับสู่ Privileged EXEC |

### ตอนที่ 8: ตรวจสอบ R1

ที่ `R1#` พิมพ์ทีละคำสั่ง

```text
show ip interface brief
show running-config
show interfaces description
```

ตรวจผล

- [ ] G0/0 มี IP `192.168.50.1`
- [ ] Status เป็น `up`
- [ ] Protocol เป็น `up`
- [ ] Hostname เป็น R1
- [ ] มี Username admin
- [ ] Console และ VTY ใช้ `login local`
- [ ] Banner ตรงตามโจทย์
- [ ] Interface Description เป็น `LAN_TO_SW1`

### ตอนที่ 9: บันทึก R1

ที่ `R1#` พิมพ์

```text
copy running-config startup-config
```

เมื่อพบ

```text
Destination filename [startup-config]?
```

ให้กด Enter เพื่อยอมรับค่าเริ่มต้น จากนั้นตรวจด้วย

```text
show startup-config
```

ทำเครื่องหมาย

- [ ] เห็นข้อความยืนยันการ Copy สำเร็จ
- [ ] Startup Configuration มี Hostname R1
- [ ] Startup Configuration มี IP ของ G0/0

### ตอนที่ 10: เปิด Console ของ SW1

1. คลิก `SW1-CONSOLE-PC`
2. เปิด **Desktop > Terminal**
3. ใช้ค่า Terminal เริ่มต้นแล้วคลิก OK
4. กด Enter
5. หากพบ Initial Configuration Dialog ให้ตอบ `no`
6. กด Enter จนเห็น `Switch>`
7. พิมพ์ `enable`
8. ตรวจว่า Prompt เป็น `Switch#`

### ตอนที่ 11: ตั้งค่าพื้นฐานของ SW1

ที่ `Switch#` พิมพ์

```text
configure terminal
hostname SW1
no ip domain-lookup
enable secret Enable123
service password-encryption
banner motd #AUTHORIZED USERS ONLY#
username admin secret Admin123
```

### ตอนที่ 12: ตั้งค่า Console และ VTY ของ SW1

ที่ `SW1(config)#` พิมพ์

```text
line console 0
login local
logging synchronous
exec-timeout 10 0
exit
line vty 0 4
login local
transport input telnet
exec-timeout 10 0
exit
```

### ตอนที่ 13: ตั้งค่า Management IP ของ SW1

Switch Layer 2 ใช้ SVI หรือ Switch Virtual Interface สำหรับ Management IP

```text
interface vlan 1
description MANAGEMENT_SVI
ip address 192.168.50.2 255.255.255.0
no shutdown
exit
ip default-gateway 192.168.50.1
```

> `ip default-gateway` ของ Switch ใช้ส่ง Management Traffic ไป Network อื่น แตกต่างจาก Default Gateway ของ USER-PC ที่กำหนดผ่าน IP Configuration

### ตอนที่ 14: ใส่ Description ให้พอร์ต SW1

ที่ `SW1(config)#` พิมพ์

```text
interface fastEthernet 0/1
description USER_PC
switchport mode access
no shutdown
exit
interface gigabitEthernet 0/1
description UPLINK_TO_R1
no shutdown
end
```

### ตอนที่ 15: ตรวจสอบและบันทึก SW1

ที่ `SW1#` พิมพ์

```text
show ip interface brief
show running-config
show interfaces description
```

ตรวจผล

- [ ] VLAN 1 มี IP `192.168.50.2`
- [ ] VLAN 1 เป็น `up/up`
- [ ] Fa0/1 มี Description `USER_PC`
- [ ] Gi0/1 มี Description `UPLINK_TO_R1`
- [ ] Hostname, Username, Banner, Console และ VTY ถูกต้อง

จากนั้นบันทึก

```text
copy running-config startup-config
```

กด Enter เพื่อยอมรับ Destination Filename แล้วตรวจด้วย

```text
show startup-config
```

> หาก VLAN 1 ไม่เป็น up/up ให้ตรวจว่ามีพอร์ตใน VLAN 1 อย่างน้อยหนึ่งพอร์ตเชื่อมต่อและทำงานอยู่ เช่น Fa0/1 หรือ Gi0/1

### ตอนที่ 16: ทดสอบจาก USER-PC

เปิด **USER-PC > Desktop > Command Prompt**

1. ตรวจค่า PC

```text
ipconfig
```

2. ทดสอบ R1

```text
ping 192.168.50.1
```

3. ทดสอบ Management IP ของ SW1

```text
ping 192.168.50.2
```

ผลที่คาดหวังคือได้รับ Reply จากทั้ง R1 และ SW1

### ตอนที่ 17: ทดสอบ VTY ด้วย Telnet ไป R1

กิจกรรมนี้ใช้ Telnet เพื่อเรียนรู้ VTY เท่านั้น

ที่ USER-PC พิมพ์

```text
telnet 192.168.50.1
```

เมื่อระบบถาม Username และ Password ให้ใช้

```text
Username: admin
Password: Admin123
```

หลัง Login ควรเห็น `R1>` จากนั้นพิมพ์

```text
enable
```

ใช้ Enable Secret

```text
Enable123
```

แล้วตรวจสอบ

```text
show ip interface brief
exit
```

> ขณะพิมพ์ Password หน้าจออาจไม่แสดงตัวอักษรหรือเครื่องหมายใด เป็นพฤติกรรมปกติ

### ตอนที่ 18: ทดสอบ VTY ด้วย Telnet ไป SW1

ที่ USER-PC พิมพ์

```text
telnet 192.168.50.2
```

ใช้ Username `admin` และ Password `Admin123` จากนั้นใช้ `enable` กับ Secret `Enable123`

ตรวจสอบ

```text
show ip interface brief
exit
```

### ตอนที่ 19: ตรวจสอบการซ่อน Secret

เปิด Console หรือ Telnet ของอุปกรณ์ แล้วใช้

```text
show running-config
```

ค้นหาบรรทัด `enable secret` และ `username admin secret`

- [ ] ไม่แสดง `Enable123` เป็นข้อความเดิมโดยตรง
- [ ] ไม่แสดง `Admin123` เป็นข้อความเดิมโดยตรง

ตอบคำถาม: เพราะเหตุใดไม่ควรถ่ายภาพหรือเผยแพร่ Running Configuration ของอุปกรณ์จริง แม้ Secret ไม่แสดงเป็นข้อความเดิม

....................................................................................................

....................................................................................................

### ตอนที่ 20: Reload เพื่อทดสอบ Startup Configuration

ทำทีละอุปกรณ์เพื่อไม่ให้สับสน

#### 20.1 Reload R1

ที่ Console ของ R1 ตรวจว่าบันทึกแล้ว จากนั้นพิมพ์

```text
reload
```

หากระบบถามให้บันทึก Configuration ให้เลือก `yes` แล้วกด Enter ยืนยันการ Reload ตาม Prompt

รอให้อุปกรณ์เริ่มทำงาน แล้วตรวจสอบ

1. MOTD Banner ปรากฏ
2. Login ด้วย Username `admin` และ Password `Admin123`
3. พิมพ์ `enable` และใช้ `Enable123`
4. พิมพ์

```text
show ip interface brief
```

5. ตรวจว่า G0/0 ยังเป็น `192.168.50.1` และกลับมา up/up

#### 20.2 Reload SW1

ทำขั้นตอนเดียวกันบน SW1 แล้วตรวจว่า VLAN 1 ยังเป็น `192.168.50.2`

> หลัง Reload ให้รอพอร์ต Switch เริ่มทำงานก่อนทดสอบ Ping

### ตอนที่ 21: ทดสอบและบันทึกขั้นสุดท้าย

1. จาก USER-PC Ping `192.168.50.1`
2. จาก USER-PC Ping `192.168.50.2`
3. ทดสอบ Telnet ไป R1 และ SW1 อย่างน้อยอุปกรณ์ละหนึ่งครั้ง
4. กด `Ctrl+S` เพื่อบันทึกไฟล์ Packet Tracer
5. ปิดและเปิดไฟล์ใหม่
6. ทำการทดสอบซ้ำ

## 12. ชุดคำสั่งฉบับรวมสำหรับตรวจสอบ

ส่วนนี้ใช้ทบทวนหรือตรวจเทียบหลังจากผู้เรียนพิมพ์ตามขั้นตอนทีละส่วนแล้ว

### 12.1 R1

```text
enable
configure terminal
hostname R1
no ip domain-lookup
enable secret Enable123
service password-encryption
banner motd #AUTHORIZED USERS ONLY#
username admin secret Admin123
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
line vty 0 4
 login local
 transport input telnet
 exec-timeout 10 0
exit
interface gigabitEthernet 0/0
 description LAN_TO_SW1
 ip address 192.168.50.1 255.255.255.0
 no shutdown
end
copy running-config startup-config
```

### 12.2 SW1

```text
enable
configure terminal
hostname SW1
no ip domain-lookup
enable secret Enable123
service password-encryption
banner motd #AUTHORIZED USERS ONLY#
username admin secret Admin123
line console 0
 login local
 logging synchronous
 exec-timeout 10 0
exit
line vty 0 4
 login local
 transport input telnet
 exec-timeout 10 0
exit
interface vlan 1
 description MANAGEMENT_SVI
 ip address 192.168.50.2 255.255.255.0
 no shutdown
exit
ip default-gateway 192.168.50.1
interface fastEthernet 0/1
 description USER_PC
 switchport mode access
 no shutdown
exit
interface gigabitEthernet 0/1
 description UPLINK_TO_R1
 no shutdown
end
copy running-config startup-config
```

## 13. คำสั่งตรวจสอบที่ควรรู้

| คำสั่ง | โหมดที่ใช้ | หน้าที่ |
|---|---|---|
| `show running-config` | Privileged EXEC | แสดงค่าที่กำลังใช้งาน |
| `show startup-config` | Privileged EXEC | แสดงค่าที่บันทึกไว้สำหรับ Boot |
| `show ip interface brief` | Privileged EXEC | สรุป IP และสถานะ Interface |
| `show interfaces description` | Privileged EXEC | แสดง Interface, Status และ Description |
| `show history` | EXEC | แสดงประวัติคำสั่งใน Session หาก IOS รองรับ |
| `copy running-config startup-config` | Privileged EXEC | บันทึก Configuration |
| `reload` | Privileged EXEC | เริ่มการทำงานอุปกรณ์ใหม่ |

## 14. ตารางบันทึกผลการตรวจสอบ

### 14.1 R1

| รายการ | ค่าที่พบ | ถูกต้องหรือไม่ |
|---|---|---|
| Hostname | ................................ | ถูก / ผิด |
| G0/0 IP Address | ................................ | ถูก / ผิด |
| G0/0 Status/Protocol | ................................ | ถูก / ผิด |
| Interface Description | ................................ | ถูก / ผิด |
| Local Username | ................................ | ถูก / ผิด |
| Startup Config มีข้อมูล | มี / ไม่มี | ถูก / ผิด |

### 14.2 SW1

| รายการ | ค่าที่พบ | ถูกต้องหรือไม่ |
|---|---|---|
| Hostname | ................................ | ถูก / ผิด |
| VLAN 1 IP Address | ................................ | ถูก / ผิด |
| VLAN 1 Status/Protocol | ................................ | ถูก / ผิด |
| Default Gateway | ................................ | ถูก / ผิด |
| Fa0/1 Description | ................................ | ถูก / ผิด |
| Gi0/1 Description | ................................ | ถูก / ผิด |
| Startup Config มีข้อมูล | มี / ไม่มี | ถูก / ผิด |

## 15. การทดสอบการทำงานขั้นสุดท้าย

| ลำดับ | รายการทดสอบ | เกณฑ์ที่ผ่าน | ผ่าน | ไม่ผ่าน |
|---:|---|---|:---:|:---:|
| 1 | Topology | อุปกรณ์ครบและต่อสายครบ 4 เส้น | ☐ | ☐ |
| 2 | USER-PC | `192.168.50.10/24`, Gateway `.1` | ☐ | ☐ |
| 3 | R1 Basic Config | Hostname, Secret, User, Banner, Lines ถูกต้อง | ☐ | ☐ |
| 4 | R1 Interface | G0/0 `192.168.50.1/24` และ up/up | ☐ | ☐ |
| 5 | SW1 Basic Config | Hostname, Secret, User, Banner, Lines ถูกต้อง | ☐ | ☐ |
| 6 | SW1 Management | VLAN 1 `192.168.50.2/24` และ Gateway `.1` | ☐ | ☐ |
| 7 | Interface Description | R1, SW1 Fa0/1 และ Gi0/1 มี Description | ☐ | ☐ |
| 8 | Ping R1 | USER-PC Ping `192.168.50.1` ผ่าน | ☐ | ☐ |
| 9 | Ping SW1 | USER-PC Ping `192.168.50.2` ผ่าน | ☐ | ☐ |
| 10 | VTY R1 | Telnet และ Login Local ผ่าน | ☐ | ☐ |
| 11 | VTY SW1 | Telnet และ Login Local ผ่าน | ☐ | ☐ |
| 12 | Persistence | Reload แล้ว Configuration ยังอยู่ | ☐ | ☐ |
| 13 | Packet Tracer File | ปิดและเปิด `.pkt` แล้วทดสอบผ่าน | ☐ | ☐ |

### ผลการทดสอบโดยรวม

- [ ] ผ่านครบทุกข้อ
- [ ] ผ่านบางข้อและแก้ไขเรียบร้อยแล้ว
- [ ] ยังมีปัญหาและต้องการความช่วยเหลือ

## 16. แนวทางแก้ไขปัญหาเบื้องต้น

| อาการ | สาเหตุที่เป็นไปได้ | วิธีตรวจสอบหรือแก้ไข |
|---|---|---|
| Terminal ไม่แสดง Prompt | Console Cable/พอร์ตผิด หรือยังไม่กด Enter | ตรวจ RS-232–Console แล้วกด Enter |
| IOS พยายาม Translating คำสั่งผิด | ยังไม่ได้ใช้ `no ip domain-lookup` | รอหรือกด `Ctrl+Shift+6` แล้วกำหนดคำสั่งดังกล่าว |
| คำสั่งถูกปฏิเสธ | อยู่ผิดโหมดหรือ Syntax ผิด | ดู Prompt ใช้ `?` และดูตำแหน่ง `^` |
| `enable` ขอ Password แต่เข้าไม่ได้ | Enable Secret ไม่ตรง | ใช้ `Enable123` และตรวจ Caps Lock |
| G0/0 เป็น administratively down | ยังไม่มี `no shutdown` | เข้า Interface แล้วใช้ `no shutdown` |
| G0/0 up/down | Layer 1 ขึ้นแต่ Protocol ยังไม่พร้อม | ตรวจพอร์ตฝั่ง SW1 และรอ Link |
| VLAN 1 down/down | ไม่มีพอร์ตใน VLAN 1 ที่ทำงาน | ตรวจ Fa0/1, Gi0/1, สาย และอุปกรณ์ปลายทาง |
| USER-PC Ping R1 ไม่ได้ | IP/Mask/Gateway หรือ R1 G0/0 ผิด | ตรวจ `ipconfig` และ `show ip interface brief` |
| USER-PC Ping SW1 ไม่ได้ | VLAN 1 IP/Mask หรือ SVI Status ผิด | ตรวจ `show ip interface brief` บน SW1 |
| Telnet Connection Failed | Ping ไม่ผ่านหรือ VTY ไม่อนุญาต Telnet | ให้ Ping ผ่านก่อน ตรวจ `login local` และ `transport input telnet` |
| Login Local ไม่ผ่าน | Username/Secret ผิดหรือยังไม่สร้าง User | ตรวจ Running Config และใช้ `admin`/`Admin123` |
| Reload แล้วค่า Configuration หาย | ไม่ได้ Copy Running ไป Startup | กำหนดใหม่แล้ว `copy running-config startup-config` |
| Password ไม่แสดงขณะพิมพ์ | เป็นพฤติกรรมปกติของ CLI | พิมพ์ต่อให้ครบแล้วกด Enter |

## 17. แบบบันทึกผลการปฏิบัติงาน

| รายการ | ข้อมูลของผู้เรียน |
|---|---|
| ชื่อ–นามสกุล | ............................................................ |
| ชั้น/กลุ่ม | ............................................................ |
| วันที่ทำแล็บ | ............................................................ |
| ชื่อไฟล์ที่ส่ง | ............................................................ |
| เวลาที่ใช้จริง | ............................................................ |

### โหมดคำสั่งที่ผู้เรียนสับสนมากที่สุด

....................................................................................................

### คำสั่งที่พิมพ์ผิดและวิธีแก้ไข

....................................................................................................

....................................................................................................

### ปัญหาที่พบ

....................................................................................................

### วิธีที่ใช้แก้ไข

....................................................................................................

....................................................................................................

## 18. คำถามท้ายแล็บ

1. Prompt `>` และ `#` แสดงว่าอยู่โหมดใด และมีสิทธิ์ต่างกันอย่างไร

   คำตอบ: ............................................................................................

2. คำสั่ง `configure terminal` ใช้ทำอะไร

   คำตอบ: ............................................................................................

3. `no ip domain-lookup` ช่วยผู้ดูแลอย่างไร

   คำตอบ: ............................................................................................

4. เพราะเหตุใดจึงควรใช้ `enable secret` แทน `enable password`

   คำตอบ: ............................................................................................

5. `login local` ทำงานร่วมกับคำสั่งใด

   คำตอบ: ............................................................................................

6. `no shutdown` มีผลอย่างไรกับ Router Interface

   คำตอบ: ............................................................................................

7. Management IP ของ Layer 2 Switch กำหนดที่ Interface ใดในแล็บนี้

   คำตอบ: ............................................................................................

8. Running Configuration และ Startup Configuration ต่างกันอย่างไร

   คำตอบ: ............................................................................................

9. เพราะเหตุใด Reload แล้ว Configuration อาจหาย

   คำตอบ: ............................................................................................

10. เหตุใดระบบจริงควรใช้ SSH แทน Telnet

    คำตอบ: ...........................................................................................

## 19. สรุปผลการเรียนรู้โดยผู้เรียน

หลังทำแล็บนี้ ฉันสามารถ

- [ ] ใช้ Console Cable และเปิด Terminal ได้
- [ ] แยกโหมดคำสั่งจาก Prompt ได้
- [ ] ใช้ Help และแก้ Syntax Error เบื้องต้นได้
- [ ] กำหนด Hostname, Secret, User และ Banner ได้
- [ ] กำหนด Console และ VTY Line ได้
- [ ] กำหนด Router Interface ผ่าน CLI ได้
- [ ] กำหนด Management IP ให้ Switch ได้
- [ ] ใช้ Show Commands ตรวจสอบค่าได้
- [ ] บันทึกและทดสอบ Configuration หลัง Reload ได้

สิ่งสำคัญที่ได้เรียนรู้จากแล็บนี้คือ

....................................................................................................

....................................................................................................

สิ่งที่ต้องการฝึกเพิ่มเติมคือ

....................................................................................................

....................................................................................................

## 20. เกณฑ์การประเมิน

| หัวข้อประเมิน | คะแนนเต็ม |
|---|---:|
| ใช้ Console และโหมด CLI ได้ถูกต้อง | 2 |
| ตั้งค่าพื้นฐานและความปลอดภัยเบื้องต้นได้ | 2 |
| กำหนด Router Interface และ Switch Management ได้ | 2 |
| ตรวจสอบ Ping, VTY และ Configuration Persistence ได้ | 2 |
| Troubleshooting บันทึกผล ตอบคำถาม และสรุปผลได้ | 2 |
| **รวม** | **10** |

เกณฑ์ผ่านที่แนะนำ: **อย่างน้อย 7 คะแนนจาก 10 คะแนน** โดย R1 และ SW1 ต้องรักษา Configuration หลัง Reload และ USER-PC ต้อง Ping ทั้งสองอุปกรณ์ได้

## 21. งานที่ต้องส่ง

1. ไฟล์ Packet Tracer ชื่อ `CPT-U02-L07-ชื่อผู้เรียน.pkt`
2. ใบงานที่กรอกผล Show Commands, Ping, Telnet, Persistence Test, คำถามท้ายแล็บ และสรุปผลครบถ้วน

---

## ภาคผนวกสำหรับผู้สอน: แนวคำตอบย่อ

### สถานะที่คาดหวัง

```text
R1
GigabitEthernet0/0    192.168.50.1    up    up

SW1
Vlan1                 192.168.50.2    up    up
FastEthernet0/1       unassigned      up    up
GigabitEthernet0/1    unassigned      up    up
```

รูปแบบและคอลัมน์จริงอาจแตกต่างเล็กน้อยตาม IOS จำลองใน Packet Tracer

### แนวคำตอบคำถามท้ายแล็บ

1. `>` คือ User EXEC มีสิทธิ์จำกัด ส่วน `#` คือ Privileged EXEC ใช้คำสั่งตรวจสอบ บันทึก และจัดการได้มากกว่า
2. เข้าสู่ Global Configuration Mode จาก Privileged EXEC
3. ป้องกัน IOS นำข้อความที่พิมพ์ผิดไปพยายามค้นหาเป็น Domain Name
4. Enable Secret ถูกเก็บในรูปแบบที่ปลอดภัยกว่า Enable Password และมีลำดับความสำคัญเหนือกว่า
5. ทำงานร่วมกับ Local User ที่สร้างด้วย `username ... secret ...`
6. เปิด Interface จากสถานะ Administratively Down
7. `interface vlan 1` หรือ SVI VLAN 1
8. Running อยู่ใน RAM และกำลังใช้งาน ส่วน Startup อยู่ใน NVRAM และใช้ตอน Boot
9. มีการแก้ Running Config แต่ไม่ได้ Copy ไป Startup Config
10. SSH เข้ารหัส Authentication และ Session แต่ Telnet ส่งข้อมูลแบบไม่เข้ารหัส

### ข้อสังเกตสำหรับผู้สอน

- การพิมพ์ `shiw` เป็นการสาธิต Error เท่านั้น ไม่ส่งผลต่อ Configuration
- หาก Telnet Client หรือ Command บางรุ่นทำงานต่างออกไป ให้ประเมิน VTY จาก Running Config และการ Ping ควบคู่กัน
- หาก SVI VLAN 1 Down ให้ตรวจว่าพอร์ตอย่างน้อยหนึ่งพอร์ตใน VLAN 1 มีสถานะ Up
- การตั้งค่าความปลอดภัยในแล็บเป็นพื้นฐาน ไม่ใช่ Baseline สำหรับ Production
- ก่อน Reload ควรตรวจว่าผู้เรียนบันทึก Startup Config เพื่อไม่ต้องทำงานซ้ำโดยไม่ตั้งใจ

### แล็บถัดไป

**หน่วยที่ 2 แล็บที่ 8: Switch และ MAC Address Table**
