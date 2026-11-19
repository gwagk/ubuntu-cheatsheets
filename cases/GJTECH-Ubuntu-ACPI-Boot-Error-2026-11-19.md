# GJTECH — Ubuntu Boot ACPI Error: Case Study
วันที่ตรวจ: 19 พฤศจิกายน 2026 | สถานะ: เฝ้าสังเกต ไม่แก้ไข (เครื่องใช้งานได้)

## อาการ
ตอนเปิดเครื่องมีข้อความ ACPI BIOS Error (bug), AE_NOT_FOUND ใน text mode; ปิดเครื่องไม่พบข้อความดังกล่าว และเข้าสู่ Ubuntu Desktop ได้

## ข้อมูลเครื่อง
- Hostname: gjtech
- Ubuntu 26.04.1 LTS; kernel 7.0.0-43-generic
- Intel Core i5-10400, 6 cores / 12 threads; Intel H510 LPC/eSPI [8086:4388]
- BIOS American Megatrends International LLC 5.19, 2022-09-07
- SMBIOS manufacturer/model/baseboard = Default string; ยังไม่ทราบรุ่นเมนบอร์ดแน่ชัด

## หลักฐานสำคัญ
- journalctl -b -p err: ACPI BIOS Error Could not resolve symbol \\_SB.UBTC.CR01._PLD และ CR02._PLD; AE_NOT_FOUND; method \\_SB.PC00.XHCI.RHUB.HS01/HS02._PLD/_UPC abort
- systemctl --failed: 0 loaded units listed
- systemd-analyze: 6.486s firmware + 4.841s loader + 0.515s kernel + 2.907s initrd + 4min22.045s userspace = 4min36.797s; graphical.target reached after 12.384s in userspace. เวลารวมไม่ใช่เวลารอ Desktop
- apt-daily.service ~3 นาที; apt-daily-upgrade.service ~1m11s; ทั้งคู่จบสำเร็จ ไม่ใช่ service failure
- USB HID, audio, printer, UPS monitor ตรวจพบ; usb 1-4 reset high-speed หนึ่งครั้ง; ยังไม่ได้ทดสอบทุกพอร์ต
- Brother scanner udev rules: Invalid key 'SYSFS' เป็นประเด็นแยก

## วิเคราะห์
มีความผิดพลาดในข้อมูล/เมธอด ACPI ที่ firmware ส่งให้ kernel แต่ยังไม่มีหลักฐานว่าทำให้ USB ใช้งานไม่ได้หรือบูตล้มเหลว ข้อมูล SMBIOS เป็นค่าเริ่มต้นไม่พิสูจน์ว่าเป็นสาเหตุ ACPI

## การตัดสินใจ
**ปล่อยไว้และเฝ้าสังเกต** เพราะเครื่องทำงานปกติ ไม่เปลี่ยน GRUB, kernel หรือ BIOS; ไม่ใช้ acpi=off; ไม่แฟลช BIOS โดยเดารุ่นเมนบอร์ด

## คำสั่งอ่านอย่างเดียวสำหรับตรวจซ้ำ
```bash
journalctl -b -p err --no-pager
journalctl -b -k --no-pager | grep -Ei 'acpi|usb|xhci'
systemctl --failed --no-pager
systemd-analyze
systemd-analyze blame | head -25
sudo dmidecode -t bios
lspci -nn | grep -Ei 'host bridge|isa bridge|smbus'
```
บันทึก log การบูตปัจจุบัน: `journalctl -b -p err --no-pager > ~/Desktop/GJTECH-Boot-Errors.txt` (เขียนทับไฟล์เดิม)

## เปิดเคสใหม่เมื่อ
บูตไม่เข้า, USB หลุดซ้ำ, เครื่องค้าง, มี service สำคัญล้มเหลว หรือมีอาการใหม่ที่สัมพันธ์กับ log

GARAGE 11 — Build • Test • Measure • Prove.
