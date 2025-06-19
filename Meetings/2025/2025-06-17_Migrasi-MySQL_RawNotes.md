# Migrasi MySQL - Meeting

## ⚡ Quick Meeting Info

**Meeting:** Internal dan  Vendor Meeting

**Date:** 17/06

**Time:** 09.00 - 12.00 WIB

**Attendees:** 
- Jan Hilton(Manager) | JH
- Hendra Setiaji (Project Leader) | HS
- Bayu Wirasmo (SeniorDev) | BW
- Aksan Azhar(Supervisor Dev) | AA
- Dwi Sasono (DB Administrator ) | DS
- Mohammad Faiz (Infrastructure) | MF
- **Vendor**: Vina, Kevin , Gunawan,  (MII) 

**Note Taker:** Hendra Setiaji

---

  

## 🗣️ Catatan  Meeting

### 📍 Agenda 1: Pembahasan  Jumlah  Node  terkait HA

**Time:** 09.00 - 10.00
**Lead:** JH
**Key Points:**
- Ada 3 skenario  Jumlah  Node DCORE : 3 node, 2 Node dan 3 Node Bare metal
- Pembahasan   Skema DRC pada  node
- Nilai  Plus-minus  dari   3  skenario Jumlah  Node
- Bagaimana skenario maintenance dari  masing-masing skenario
- Cost dari  masing--masing  skenario
- Ada penambahan Storage NetApp

**Pertanyaan-pertanyaan:**

- ? [APakah  akan  menggunakan VM atau  menggunakan  Baremetal?] - [JH]
- ? [Apakah  DCORE dan SCMS mau  digabung servernya ?] - [AA]


**Decisions/Agreements:**
- ✅  Menggunakan 3 Node  dengan  skenario masih dalam pilihan:
  1. menggunakan VM  
  2. Menggunakan Bare Metal
     
- ✅  DRC hanya  dilayani oleh  HA, bukan  dari  Skema DRC  




**Action Items:**
- → Menghitung  Kebutuhan Storage NetApp | MF | due: 23/06
- → Menghitung kembali  Cost  Proposal  
  - opsi penambahan  storage dan  baremetal | Vina/Kevin/Gunawan | Due: 23/06

  

**Concerns/Issues:**

- ⚠️ DRC dengan  Pasif server tidak  memungkinkan dilakukan karena Licence VMWare max 3 sudah  terpakai semua. 

- ⚠️ Bisa memaksakan penambahan  HOST baru  ( Host ke-empat) non licence, namun jadi  tidak bisa  berbagi Resource

- ⚠️  JIka  menggunakan  Bare Metal , Check List Kebutuhan apa saja? 
Teknis Maintenance nya  dipertanyakan 
  
---

  
## 🎯 Quick Capture Section

  

### 💡 Ideas/Suggestions

- [Skenario Menggunakan VM --> Menggunakan unlicenced New HOST dan  tidak  bisa berbagi  resource ] - [MF,DS]
- [Skenario Baremetal] - [maintenancenya  mau  di buat  seperti apa?]
  

### 🔥 Hot Topics/Debates

- [Jumlah  Node] → [Apakah  3 atau  2 Node] → [Menggunakan 3 Node/Final]

- [VM atau Baremetal] → [VM  tidak bisa berbagi resource atau  BareMetal dengan maintenance yang  lebih  ribet] → [Belum ada Kesepakatan/Pending]


---


### 🔴 HIGH PRIORITY

- [Menentukan  apakah  3 node menggunakan VM atau  BareMetal] - [Landasan dasar untuk membangun platform]

### 🟡 MEDIUM PRIORITY

- [Menentukan  apakah SCMS dan DCORE digabung  ke dalam satu server] - [Belum di putuskan]
  
---
