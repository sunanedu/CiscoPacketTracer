# ชุดฝึกปฏิบัติ Cisco Packet Tracer สำหรับผู้เริ่มต้น

ชุดฝึกนี้ออกแบบสำหรับผู้ที่เริ่มเรียนรู้ทั้งการใช้งาน **Cisco Packet Tracer** และพื้นฐานระบบเครือข่าย โดยเรียงเนื้อหาจากการใช้งานโปรแกรม การต่ออุปกรณ์ การกำหนดหมายเลข IP การแบ่งเครือข่าย การ Routing และบริการเครือข่าย ไปจนถึงความปลอดภัยและโครงงานบูรณาการ

หลักสูตรประกอบด้วย **5 หน่วยการเรียนรู้ รวม 20 แล็บ** แต่ละแล็บออกแบบให้สามารถทำต่อเนื่องกันได้ ผู้เรียนควรทำตามลำดับเพื่อให้มีพื้นฐานเพียงพอสำหรับแล็บถัดไป

## ภาพรวมหลักสูตร

| หน่วย | เนื้อหา | จำนวนแล็บ |
|---|---|---:|
| หน่วยที่ 1 | รู้จักโปรแกรมและการเชื่อมต่อพื้นฐาน | 4 |
| หน่วยที่ 2 | IPv4 และระบบ Switching | 4 |
| หน่วยที่ 3 | การแบ่งเครือข่ายและ Routing | 4 |
| หน่วยที่ 4 | บริการและการเชื่อมต่อเครือข่าย | 4 |
| หน่วยที่ 5 | อินเทอร์เน็ต ความปลอดภัย และโครงงานรวม | 4 |
| **รวม** | | **20** |

## สถานะของใบงาน

- ✅ สร้างใบงานแล้วและเปิดอ่านได้
- 📝 วางแผนไว้และจะสร้างในอนาคต

> ลิงก์ที่มีสถานะ 📝 จะเปิดได้เมื่อสร้างไฟล์ของแล็บนั้นแล้ว

---

## หน่วยที่ 1: รู้จักโปรแกรมและการเชื่อมต่อพื้นฐาน

### ✅ แล็บที่ 1: การเริ่มต้นใช้งาน Cisco Packet Tracer

เรียนรู้ส่วนประกอบของหน้าจอ การเลือกและวางอุปกรณ์ การเปลี่ยนชื่อ การต่อสายเบื้องต้น การใช้งาน Realtime/Simulation Mode และการบันทึกไฟล์ `.pkt`

- ใบงาน: [lab-01-getting-started-with-cisco-packet-tracer.md](worksheets/unit-01/lab-01-getting-started-with-cisco-packet-tracer.md)

### ✅ แล็บที่ 2: รู้จักอุปกรณ์เครือข่าย พอร์ต และสายสัญญาณ

เรียนรู้หน้าที่ของ PC, Server, Switch, Router และ Access Point รวมถึงการเลือกใช้สาย Copper Straight-Through, Copper Crossover, Console และ Fiber

- ใบงาน: [lab-02-network-devices-ports-and-cables.md](worksheets/unit-01/lab-02-network-devices-ports-and-cables.md)

### ✅ แล็บที่ 3: สร้างเครือข่าย LAN แรก

สร้างเครือข่ายจาก PC และ Switch กำหนด IPv4 Address และ Subnet Mask แบบ Static แล้วทดสอบการสื่อสารด้วยคำสั่ง `ping`

- ใบงาน: [lab-03-building-the-first-lan.md](worksheets/unit-01/lab-03-building-the-first-lan.md)

### ✅ แล็บที่ 4: การทำงานของ ARP, ICMP และ Simulation Mode

ติดตามการทำงานของ ARP และ ICMP ผ่าน Simulation Mode เพื่อทำความเข้าใจการค้นหา MAC Address และการส่ง Echo Request/Echo Reply

- ใบงาน: [lab-04-arp-icmp-and-simulation-mode.md](worksheets/unit-01/lab-04-arp-icmp-and-simulation-mode.md)

---

## หน่วยที่ 2: IPv4 และระบบ Switching

### 📝 แล็บที่ 5: IPv4 Address, Subnet Mask และ Default Gateway

เรียนรู้ส่วนประกอบของ IPv4 Address การแยก Network ID และ Host ID ตลอดจนหน้าที่ของ Subnet Mask และ Default Gateway

- ใบงาน: [lab-05-ipv4-addressing-subnet-mask-and-default-gateway.md](worksheets/unit-02/lab-05-ipv4-addressing-subnet-mask-and-default-gateway.md)

### 📝 แล็บที่ 6: การแบ่งเครือข่ายด้วย Subnetting และ VLSM

ฝึกคำนวณ Network Address, Broadcast Address, ช่วงหมายเลข IP ที่ใช้งานได้ และออกแบบเครือข่ายย่อยให้เหมาะกับจำนวน Host

- ใบงาน: [lab-06-subnetting-and-vlsm.md](worksheets/unit-02/lab-06-subnetting-and-vlsm.md)

### 📝 แล็บที่ 7: การตั้งค่าพื้นฐานด้วย Cisco IOS CLI

เรียนรู้โหมดคำสั่งของ Cisco IOS การกำหนด Hostname และรหัสผ่าน การตั้งค่า Interface การตรวจสอบ Running Configuration และการบันทึกค่า

- ใบงาน: [lab-07-basic-cisco-ios-configuration.md](worksheets/unit-02/lab-07-basic-cisco-ios-configuration.md)

### 📝 แล็บที่ 8: Switch และ MAC Address Table

เรียนรู้วิธีที่ Switch เรียนรู้ MAC Address การส่งต่อ Frame การตรวจสอบ MAC Address Table และการกำหนด Management IP ให้กับ Switch

- ใบงาน: [lab-08-switching-and-mac-address-table.md](worksheets/unit-02/lab-08-switching-and-mac-address-table.md)

---

## หน่วยที่ 3: การแบ่งเครือข่ายและ Routing

### 📝 แล็บที่ 9: การใช้ Router เชื่อมต่อสองเครือข่าย LAN

กำหนด IP Address ให้ Interface ของ Router เชื่อมต่อ LAN สองเครือข่าย และกำหนด Default Gateway ให้กับเครื่องปลายทาง

- ใบงาน: [lab-09-router-connects-two-lans.md](worksheets/unit-03/lab-09-router-connects-two-lans.md)

### 📝 แล็บที่ 10: Static Route และ Default Route

เชื่อมต่อ Router หลายตัว สร้างเส้นทางแบบ Static และ Default Route ตรวจสอบ Routing Table และทดสอบเส้นทางด้วย `ping` และ `tracert`

- ใบงาน: [lab-10-static-and-default-routing.md](worksheets/unit-03/lab-10-static-and-default-routing.md)

### 📝 แล็บที่ 11: การแบ่งเครือข่ายด้วย VLAN

สร้าง VLAN กำหนด Access Port แยกอุปกรณ์ออกเป็นกลุ่ม และตรวจสอบผลด้วยคำสั่ง `show vlan brief`

- ใบงาน: [lab-11-vlan-configuration.md](worksheets/unit-03/lab-11-vlan-configuration.md)

### 📝 แล็บที่ 12: Trunk และ Inter-VLAN Routing

เชื่อมต่อ Switch ด้วย Trunk และสร้าง Router-on-a-Stick เพื่อให้อุปกรณ์ที่อยู่คนละ VLAN ติดต่อกันได้

- ใบงาน: [lab-12-trunk-and-inter-vlan-routing.md](worksheets/unit-03/lab-12-trunk-and-inter-vlan-routing.md)

---

## หน่วยที่ 4: บริการและการเชื่อมต่อเครือข่าย

### 📝 แล็บที่ 13: การแจก IP Address ด้วย DHCP

ตั้งค่า DHCP Pool บน Server และ Router เพื่อแจก IP Address, Subnet Mask, Default Gateway และ DNS Server ให้เครื่องลูกข่ายโดยอัตโนมัติ

- ใบงาน: [lab-13-dhcp-configuration.md](worksheets/unit-04/lab-13-dhcp-configuration.md)

### 📝 แล็บที่ 14: DNS และ Web Server

สร้างบริการ DNS และ HTTP ภายในเครือข่าย ทดสอบการเข้าถึงเว็บไซต์ด้วย IP Address และชื่อ Domain

- ใบงาน: [lab-14-dns-and-web-server.md](worksheets/unit-04/lab-14-dns-and-web-server.md)

### 📝 แล็บที่ 15: เครือข่ายไร้สาย WLAN

ตั้งค่า Access Point หรือ Wireless Router กำหนด SSID เลือกช่องสัญญาณ และรักษาความปลอดภัยด้วย WPA2

- ใบงาน: [lab-15-wireless-lan.md](worksheets/unit-04/lab-15-wireless-lan.md)

### 📝 แล็บที่ 16: Dynamic Routing ด้วย OSPF แบบ Single Area

กำหนด OSPF บน Router ให้แลกเปลี่ยนเส้นทางกันอัตโนมัติ ตรวจสอบ Neighbor และ Routing Table พร้อมทดสอบการเชื่อมต่อปลายทาง

- ใบงาน: [lab-16-ospf-single-area.md](worksheets/unit-04/lab-16-ospf-single-area.md)

---

## หน่วยที่ 5: อินเทอร์เน็ต ความปลอดภัย และโครงงานรวม

### 📝 แล็บที่ 17: การเชื่อมต่ออินเทอร์เน็ตด้วย NAT และ PAT

เรียนรู้การแปลง Private IP Address เป็น Public IP Address ตั้งค่า Static NAT และ PAT และตรวจสอบตารางการแปลง Address

- ใบงาน: [lab-17-nat-and-pat.md](worksheets/unit-05/lab-17-nat-and-pat.md)

### 📝 แล็บที่ 18: ความปลอดภัยด้วย ACL และ Port Security

สร้าง Standard/Extended ACL เพื่อควบคุม Traffic และกำหนด Port Security เพื่อจำกัดอุปกรณ์ที่สามารถเชื่อมต่อกับ Switch

- ใบงาน: [lab-18-acl-and-port-security.md](worksheets/unit-05/lab-18-acl-and-port-security.md)

### 📝 แล็บที่ 19: IPv6 และ Dual Stack

กำหนด IPv6 Address และ Default Gateway ทดสอบการสื่อสารด้วย IPv6 และสร้างเครือข่ายที่ใช้งาน IPv4 และ IPv6 ร่วมกัน

- ใบงาน: [lab-19-ipv6-and-dual-stack.md](worksheets/unit-05/lab-19-ipv6-and-dual-stack.md)

### 📝 แล็บที่ 20: โครงงานเครือข่ายและการแก้ไขปัญหาแบบบูรณาการ

ออกแบบเครือข่ายสำนักงานขนาดเล็กที่มีหลายแผนก โดยประยุกต์ใช้ VLAN, DHCP, DNS, OSPF, NAT และ ACL พร้อมฝึกวิเคราะห์และแก้ไขปัญหา

- ใบงาน: [lab-20-integrated-network-and-troubleshooting.md](worksheets/unit-05/lab-20-integrated-network-and-troubleshooting.md)

---

## โครงสร้างมาตรฐานของใบงาน

ใบงานแต่ละแล็บควรประกอบด้วยหัวข้อต่อไปนี้

1. ชื่อใบงาน รหัสใบงาน ระดับ และระยะเวลา
2. เรื่องของใบงานและเป้าหมายการเรียนรู้
3. ความรู้พื้นฐานก่อนเริ่ม
4. รายการอุปกรณ์และซอฟต์แวร์
5. แผนผังเครือข่าย
6. ตาราง IP Address, Subnet Mask, Default Gateway และ VLAN
7. ตารางพอร์ตและสายสัญญาณ
8. ขั้นตอนการวางและเชื่อมต่ออุปกรณ์
9. คำสั่ง Cisco IOS พร้อมคำอธิบาย
10. วิธีทดสอบและตรวจสอบการทำงาน
11. แนวทางแก้ไขปัญหาเบื้องต้น
12. แบบบันทึกและสรุปผล
13. คำถามท้ายแล็บ
14. เกณฑ์การประเมินและงานที่ต้องส่ง
15. แนวคำตอบสำหรับผู้สอน

## โครงสร้างโฟลเดอร์

```text
CiscoPacketTracer/
├── README.md
└── worksheets/
    ├── unit-01/
    │   ├── lab-01-getting-started-with-cisco-packet-tracer.md
    │   ├── lab-02-network-devices-ports-and-cables.md
    │   ├── lab-03-building-the-first-lan.md
    │   └── lab-04-arp-icmp-and-simulation-mode.md
    ├── unit-02/
    │   ├── lab-05-ipv4-addressing-subnet-mask-and-default-gateway.md
    │   ├── lab-06-subnetting-and-vlsm.md
    │   ├── lab-07-basic-cisco-ios-configuration.md
    │   └── lab-08-switching-and-mac-address-table.md
    ├── unit-03/
    │   ├── lab-09-router-connects-two-lans.md
    │   ├── lab-10-static-and-default-routing.md
    │   ├── lab-11-vlan-configuration.md
    │   └── lab-12-trunk-and-inter-vlan-routing.md
    ├── unit-04/
    │   ├── lab-13-dhcp-configuration.md
    │   ├── lab-14-dns-and-web-server.md
    │   ├── lab-15-wireless-lan.md
    │   └── lab-16-ospf-single-area.md
    └── unit-05/
        ├── lab-17-nat-and-pat.md
        ├── lab-18-acl-and-port-security.md
        ├── lab-19-ipv6-and-dual-stack.md
        └── lab-20-integrated-network-and-troubleshooting.md
```

## ลำดับการเรียนที่แนะนำ

ควรเริ่มจากแล็บที่ 1 และทำต่อเนื่องตามลำดับจนถึงแล็บที่ 20 เนื่องจากแต่ละแล็บจะนำความรู้และทักษะจากแล็บก่อนหน้ามาใช้ โดยเฉพาะตั้งแต่แล็บที่ 9 เป็นต้นไป

## ผลลัพธ์เมื่อเรียนครบหลักสูตร

เมื่อทำครบทั้ง 20 แล็บ ผู้เรียนควรสามารถใช้งาน Cisco Packet Tracer ออกแบบเครือข่ายสำนักงานขนาดเล็ก กำหนด IPv4/IPv6 แบ่ง VLAN สร้างเส้นทาง ให้บริการพื้นฐาน เชื่อมต่อเครือข่ายภายนอก กำหนดความปลอดภัย และตรวจสอบแก้ไขปัญหาเบื้องต้นได้
