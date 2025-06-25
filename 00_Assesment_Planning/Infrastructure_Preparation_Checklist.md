# Infrastructure Preparation Checklist - Migrasi MySQL

## 📋 Overview
**Project:** Migrasi MySQL ke High Availability Setup  
**Target Date:** Meeting Internal - 19 Juni 2025 16:30-18:00 WIB  
**Attendees:** JH, HS, MF, RI
**Prepared by:** Project Manager  
**For:** Tim Infrastructure  

### 👥 **Responsibility Legend**
- **[MF]** - Mohammad Faiz (Head of Infrastructure) - Hardware, VM, Network, Storage
- **[RI]** - Rizky (Infrastructure) - Hardware, VM, Network, Storage
- **[DS]** - Dwi Sasono (DB Administrator) - MySQL Configuration, Tuning, Backup
- **[JH]** - Jan Hilton (MIS Manager) - Cost, Budget, Business Decisions
- **[HS]** - Hendra Setiaji (Project Manager) - Project Orchestration, Planning
- **[BW]** - Bayu Wirasmo (Senior Dev) - Application, Stored Procedures, Tables
- **[AA]** - Aksan Azhar (Supervisor Dev) - Application, Stored Procedures, Tables

---

## 🎯 Tujuan Checklist
1. Membantu tim Infrastructure lebih aware dengan kebutuhan proyek
2. Bahan pertanyaan untuk meeting dengan Vendor minggu depan
3. Memastikan semua aspek infrastruktur telah dipertimbangkan

---

## 🏗️ **HIGH PRIORITY - Keputusan Arsitektur**

### ✅ **Pemilihan Platform Deployment**
- [ ] **VM vs Bare Metal Decision**
  - [x] Evaluasi kebutuhan resource sharing **[MF]**
  - [x] Analisis licensing VMware (current: max 3 license terpakai) **[MF]**
  - [x] Pertimbangan maintenance complexity **[MF]**
  - [x] Cost comparison antara VM dan Bare Metal **[JH]**
     - [x] Akurasi cost masih menggunakan pembulatan**[JH]**
  - [ ] Impact terhadap performance MySQL **[DS]**

- [ ] **3-Node Architecture Confirmation**
  - [x] Validasi kebutuhan minimum 3 node untuk HA **[DS] + [MF]**
  - [ ] Topology design (Active-Active-Active vs Active-Passive) **[DS]**
  - [ ] Node distribution strategy **[MF]**
  - [ ] Failover mechanism design **[DS]**

### ✅ **Storage Requirements**
- [ ] **NetApp Storage Calculation** (Due: 23/06 - MF)
  - [ ] Current database size assessment **[DS] + [BW]**
  - [ ] Growth projection (6 months, 1 year, 2 years) **[DS] + [BW] + [JH]**
  - [ ] Backup storage requirements **[DS]**
  - [ ] IOPS requirements untuk MySQL workload **[DS]**
  - [ ] Redundancy level (RAID configuration) **[MF] + [DS]**

---

## 🔧 **MySQL High Availability Setup**

### ✅ **Cluster Configuration**
- [ ] **MySQL Router Configuration**
  - [ ] Router placement strategy (per application server vs dedicated) **[DS] + [MF]**
  - [ ] Load balancing algorithm selection **[DS]**
     - [ ] Kemungkinan besar  tidak digunakan
  - [ ] Connection pooling configuration **[DS] + [BW] + [AA]**
  - [ ] Health check mechanism **[DS]**
  - [ ] Failover timeout settings **[DS]**

- [ ] **MySQL InnoDB Cluster Setup**
  - [ ] Primary/Secondary role assignment **[DS]**
  - [ ] Automatic failover configuration **[DS]**
  - [ ] Split-brain prevention mechanism **[DS]**
  - [ ] Cluster communication network design **[MF] + [DS]**

### ✅ **Network Architecture**
- [ ] **Network Segmentation**
  - [ ] Database network isolation ( Membuat  Kerajaan Sendiri ) **[MF]**
    - [ ] Terhubung / terintegrasi  dengan NetApp Existing  yang  sudah di Expand
  - [ ] Inter-node communication network **[MF]**
    - [ ] Check daftar IP yang available  di rencanakan akan di pakai
  - [ ] Management network setup **[MF]**
  - [ ] Backup network considerations **[MF] + [DS]**

- [ ] **Load Balancer Integration**
  - [ ] Application to MySQL Router connectivity **[MF] + [BW] + [AA]**
  - [ ] Health check endpoints **[DS] + [MF]**
  - [ ] SSL/TLS termination strategy **[MF] + [DS]**

---

## 🖥️ **Server Specifications & Requirements**

### ✅ **Hardware Specifications (Per Node)**
- [ ] **CPU Requirements**
  - [ ] Core count untuk MySQL workload **[MF] + [DS]**
    - [ ] Ada di  Dokumen Excel pak Jan 
  - [ ] CPU architecture compatibility **[MF]**
  - [ ] Performance benchmarking requirements **[DS]**

- [ ] **Memory (RAM) Requirements**
  - [ ] InnoDB buffer pool sizing **[DS]**
  - [ ] OS overhead calculation **[MF] + [DS]**
  - [ ] Query cache requirements **[DS]**
  - [ ] Connection overhead estimation **[DS] + [BW] + [AA]**

- [ ] **Storage Specifications**
  - [ ] Local storage untuk OS dan logs **[MF]**
  - [x] Shared storage configuration (NetApp) **[MF]**
  - [ ] Disk I/O performance requirements **[DS] + [MF]**
  - [ ] Backup storage allocation **[DS] + [MF]**

### ✅ **Operating System & Software**
- [ ] **OS Selection & Configuration**
  - [x] Linux distribution choice **[MF]**
  - [ ] Kernel optimization untuk database **[DS]**
  - [ ] Security hardening requirements **[MF]**
  - [ ] Monitoring agent installation **[MF] + [DS]**

- [ ] **MySQL Version & Configuration**
  - [x] MySQL version selection (8.4) **[DS]**
  - [ ] InnoDB configuration optimization **[DS]**
  - [ ] Security configuration **[DS]**
  - [ ] Performance tuning parameters **[DS]**

---

## 🔄 **Disaster Recovery & Backup**

### ✅ **DRC (Disaster Recovery Center) Strategy**
- [ ] **HA-based DRC Implementation**
  - [ ] Confirm DRC akan dilayani oleh HA (bukan skema DRC terpisah) **[DS] + [MF] + [HS]**
  - [ ] Cross-site Group replication setup **[DS] + [MF]**
  - [ ] RTO/RPO requirements definition **[DS] + [HS] + [JH]**
  - [ ] Disaster recovery testing procedure **[DS] + [HS]**

- [ ] **Backup Strategy**
  - [ ] Full backup schedule **[DS]**
  - [ ] Incremental backup strategy **[DS]**
  - [ ] Point-in-time recovery capability **[DS]**
  - [ ] Backup retention policy **[DS] + [JH]**
  - [ ] Backup verification process **[DS]**

---

## 🔧 **Maintenance & Operations**

### ✅ **Maintenance Procedures**
- [ ] **Planned Maintenance**
  - [ ] Rolling maintenance procedure (untuk 3-node) **[DS] + [MF]**
  - [ ] OS patching strategy **[DS] + [MF]**
  - [ ] MySQL upgrade procedure **[DS]**
  - [ ] Zero-downtime maintenance capability **[DS] + [MF]**

- [ ] **Monitoring & Alerting**
  - [ ] Database performance monitoring **[DS]**
  - [ ] Cluster health monitoring **[DS] + [MF]**
  - [ ] Storage utilization monitoring **[MF] + [DS]**
  - [ ] Alert escalation procedures **[DS] + [MF] + [HS]**

### ✅ **Security Considerations**
- [ ] **Access Control**
  - [ ] Database user management **[DS] + [BW] + [AA]**
  - [ ] Network access control **[MF]**
  - [ ] Encryption at rest **[DS] + [MF]**
  - [ ] Encryption in transit **[DS] + [MF]**

---

## 💰 **Cost Considerations**

### ✅ **Budget Planning**
- [x] **Hardware Costs**
  - [x] Server hardware (VM vs Bare Metal) **[JH] + [MF]**
  - [x] Storage costs (NetApp expansion) **[JH] + [MF]**
  - [x] Network equipment requirements **[JH] + [MF]**
  - [x] Licensing costs (MySQL, OS, Monitoring tools) **[JH]**

- [ ] **Operational Costs**
  - [ ] Maintenance contract costs **[JH]**
  - [ ] Support costs **[JH]**
  - [ ] Training costs untuk tim operations **[JH] + [HS]**

---

## 🤔 **Questions for Vendor Meeting**

### ✅ **Technical Questions**
- [ ] **Architecture Design**
  - [ ] Rekomendasi optimal: VM vs Bare Metal untuk use case kita? **[MF] + [DS] + [HS]**
  - [ ] Best practice untuk 3-node MySQL InnoDB Cluster? **[DS] + [HS]**
  - [ ] Network latency requirements antar node? **[MF] + [DS]**
  - [ ] Storage sharing strategy untuk HA setup? **[MF] + [DS]**

- [ ] **Integration Questions**
  - [ ] Apakah DCORE dan SCMS bisa digabung dalam satu server cluster? **[DS] + [BW] + [AA] + [HS]**
  - [ ] Impact terhadap existing application connectivity? **[BW] + [AA] + [HS]**
  - [ ] Migration strategy dari setup current ke HA? **[DS] + [HS]**
  - [ ] Rollback plan jika ada issues? **[DS] + [BW] + [AA] + [HS]**

### ✅ **Operational Questions**
- [ ] **Maintenance & Support**
  - [ ] Maintenance window requirements? **[DS] + [MF] + [HS]**
  - [ ] Support SLA dari vendor? **[JH] + [HS]**
  - [ ] Training requirements untuk tim internal? **[JH] + [HS]**
  - [ ] Documentation dan knowledge transfer? **[HS]**

### ✅ **Cost & Timeline Questions**
- [ ] **Project Planning**
  - [ ] Detailed cost breakdown (VM vs Bare Metal)? **[JH] + [HS]**
  - [ ] Implementation timeline? **[HS]**
  - [ ] Resource requirements dari internal tim? **[HS]**
  - [ ] Testing phase duration dan requirements? **[DS] + [BW] + [AA] + [HS]**

---

## 📅 **Action Items Tracking**

### ✅ **Current Open Items** (From Previous Meeting)
- [ ] **MF**: Menghitung Kebutuhan Storage NetApp | Due: 23/06 **[MF]**
- [ ] **Vendor (Vina/Kevin/Gunawan)**: Menghitung kembali Cost Proposal dengan opsi storage dan baremetal | Due: 23/06 **[Vendor] + [JH]**

### ✅ **New Action Items**
- [ ] **Tim Infra**: Review dan lengkapi checklist ini sebelum meeting vendor **[MF] + [HS]**
- [ ] **Tim Infra**: Prepare specific questions berdasarkan checklist **[MF] + [DS] + [HS]**
- [ ] **Tim Infra**: Gather current infrastructure metrics untuk vendor assessment **[MF] + [DS]**

---

## ⚠️ **Critical Issues to Address**

### 🔴 **Immediate Concerns**
- [ ] **VMware Licensing Limitation**
  - [x] Confirm impact jika menggunakan unlicensed host ke-4 **[MF] + [JH]**
  - [ ] Alternative solutions untuk resource sharing **[MF]**
    - [ ] Apakah  mau  menggunakan Independent NetApp  Baru  [ ]YA /[x] TIDAK **[MF]**
  - [ ] Cost-benefit analysis **[JH] + [MF]**

- [ ] **Bare Metal Considerations**
  - [ ] Detailed maintenance procedure untuk bare metal **[MF] + [DS]**
  - [ ] Hardware vendor support dan warranty **[MF] + [JH]**
  - [ ] Spare parts availability **[MF] + [JH]**

### 🟡 **Medium Priority**
- [ ] **Application Integration**
  - [ ] Current application connection pooling **[BW] + [AA]**
  - [ ] Connection string changes required **[BW] + [AA] + [DS]**
  - [ ] Testing dengan existing applications **[BW] + [AA] + [DS] + [HS]**

---

## 📝 **Notes & Additional Considerations**

### ✅ **Documentation Needed**
- [ ] Current database schema dan size **[DS] + [BW] + [AA]**
- [ ] Existing performance metrics **[DS]**
- [ ] Current backup dan recovery procedures **[DS]**
- [ ] Application dependency mapping **[BW] + [AA] + [HS]**

### ✅ **Risk Mitigation**
- [ ] Pilot testing plan **[DS] + [BW] + [AA] + [HS]**
- [ ] Rollback procedures **[DS] + [BW] + [AA] + [HS]**
- [ ] Business continuity planning **[HS] + [JH]**
- [ ] Communication plan dengan stakeholders **[HS]**

---

**Last Updated:** 20/06/2025  
**Next Review:** Before Vendor (Jumat, 20/06/2025)  
**Contact:** Project Manager - Hendra Setiaji
