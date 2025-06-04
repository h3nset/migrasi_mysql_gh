### **Project Timeline: Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS**  
**Environment:** MySQL 8.4 Enterprise dengan High Availability (MySQL Router + InnoDB Cluster)  
**Jumlah Server:** 10 (termasuk Router nodes)  
**Total Estimasi:** 16 Minggu (termasuk buffer & adaptasi HA setup)
**Person in Charge:** 
  - **INTERNAL**
    - **Project Manager:** BUNGARAN PANGGABEAN (BPA)
      - **Project Leader:** JAN HILTON (JAN)
        - **Developer Team:** BAYU WIRASMO (BAW), AKSAN (AKS), WAWAN (WAN), HERY (HER),FARHAN (FAR)
        - **DevOps Team:** HENDRA (HEN), TEDDY (TED), NOVAL (NOV), MALDI (MAL)
          - **DBA:** DWI (DWI)
        - **Infrastructure Team:** FAIZ (FAZ), 
  - **EXTERNAL**
    - **Vendor Support:** SUPPORT (SUP) 


---

## **DELIVERABLES PROJECT**

### **Deliverables Utama:**
1. **Infrastruktur Terupgrade**
   - 10 server menjalankan CentOS Linux (versi stable terbaru)
   - MySQL 8.4 Enterprise beroperasi penuh tanpa kehilangan data
   - Setup High Availability dengan InnoDB Cluster + MySQL Router
   - Seluruh aplikasi kompatibel dan berjalan di environment baru

2. **Implementasi Fitur Enterprise**
   - MySQL Enterprise Security (audit, encryption, firewall) terkonfigurasi
   - MySQL Enterprise Monitor dengan real-time alerting
   - Strategi MySQL Enterprise Backup terimplementasi
   - Optimasi performa untuk enterprise workloads

3. **Paket Dokumentasi**
   - Dokumentasi arsitektur sistem lengkap
   - Operational runbooks untuk maintenance harian
   - Prosedur emergency dan rencana rollback
   - Performance baseline dan panduan optimasi

4. **Transfer Pengetahuan**
   - Tim internal terlatih pada fitur MySQL 8.4 Enterprise
   - Tim operasional tersertifikasi untuk production support
   - Panduan troubleshooting dan best practices
   - Handover vendor dengan dokumentasi lengkap

### **Deliverables Teknis:**
- **Infrastruktur:** Environment produksi ter-migrasi dan tervalidasi penuh
- **Keamanan:** Implementasi security tingkat enterprise
- **Monitoring:** Sistem monitoring dan alerting komprehensif
- **Backup:** Strategi backup otomatis dengan disaster recovery
- **Performa:** Konfigurasi teroptimasi untuk high availability

### **Deliverables Manajemen:**
- **Laporan Project:** Laporan eksekusi project lengkap dengan metrik
- **Risk Assessment:** Analisa risiko pasca-implementasi
- **Lessons Learned:** Dokumentasi untuk project masa depan
- **Compliance:** Dokumentasi compliance license dan audit

**Kriteria Keberhasilan Project:** Migrasi zero downtime, peningkatan performa, security tingkat enterprise, tim operasional terlatih

---

#### **Fase 0: Persiapan (Minggu 1-2)**  
**Tujuan:** Risiko minimal, persiapan komprehensif.  
**Deliverables:** Dokumen assessment lengkap, rencana migrasi, environment staging siap  
**PIC:** Project Manager, Project Leader, Vendor Support  
**Task Dependency:** Independent (foundational phase)  

- **Task 1: Assessment & Perencanaan**  
  **Deliverables:** Server inventory, compatibility matrix, rollback plan  
  **PIC:** Project Leader, Vendor Support  
  **Dependency:** Independent  
  - Subtask 1.1: Audit server (OS version, MySQL config, dependency aplikasi, ukuran database).  
    **Deliverables:** Server audit report dengan detail konfigurasi  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent  
  - Subtask 1.2: Identifikasi breaking changes MySQL 8.4 Enterprise (e.g., `caching_sha2_password`, reserved keywords, syntax deprecated).  
    **Deliverables:** Breaking changes checklist dan impact analysis  
    **PIC:** Vendor Support, Developer Supervisor  
    **Dependency:** Independent  
  - Subtask 1.3: **Audit InnoDB Cluster & MySQL Router topology** (primary/secondary nodes, router endpoints).  
    **Deliverables:** Cluster topology diagram dan konfigurasi router  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent  
  - Subtask 1.4: Cek kompatibilitas aplikasi dengan CentOS versi terbaru (e.g., CentOS 8 Stream/Rocky Linux).  
    **Deliverables:** Compatibility matrix aplikasi vs OS  
    **PIC:** Developer Team, DevOps Team  
    **Dependency:** Independent  
  - Subtask 1.5: **Analisa MySQL Router configuration** (read/write split, failover policy, load balancing).  
    **Deliverables:** Router configuration analysis dan optimization plan  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 1.3  
  - Subtask 1.6: Buat dokumen rollback plan (OS, MySQL, dan cluster topology).  
    **Deliverables:** Comprehensive rollback procedures  
    **PIC:** Project Leader, Vendor Support  
    **Dependency:** Dependent pada Subtask 1.1-1.5  
  - Subtask 1.7: Siapkan lingkungan staging (replikasi 1:1 dari produksi termasuk HA setup).  
    **Deliverables:** Staging environment fully configured  
    **PIC:** DevOps Team, Vendor Support  
    **Dependency:** Dependent pada Subtask 1.3  

- **Task 2: Backup & Pengujian Awal**  
  **Deliverables:** Backup terkonfirmasi, performance baseline report  
  **PIC:** Vendor Support, DBA  
  **Dependency:** Dependent pada Task 1  
  - Subtask 2.1: Backup fisik/logis semua database (`mysqldump`, snapshot VM/disk, **MySQL Enterprise Backup**).  
    **Deliverables:** Complete backup sets dengan verifikasi integrity  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 1.7  
  - Subtask 2.2: **Backup InnoDB Cluster metadata** (`cluster.dumpMetadata()` dari MySQL Shell).  
    **Deliverables:** Cluster metadata backup file  
    **PIC:** Vendor Support  
    **Dependency:** Dependent pada Subtask 1.3  
  - Subtask 2.3: **Backup MySQL Router configuration** dan endpoint mappings.  
    **Deliverables:** Router config backup dan dokumentasi endpoints  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Dependent pada Subtask 1.5  
  - Subtask 2.4: Uji restore backup di lingkungan staging termasuk cluster setup.  
    **Deliverables:** Successful restore validation report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 2.1-2.3  
  - Subtask 2.5: Benchmark performa baseline (CPU, RAM, I/O, query latency, cluster latency).  
    **Deliverables:** Performance baseline metrics report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 2.4  

---

#### **Fase 1: Upgrade OS ke CentOS (Minggu 3-5)**  
**Tujuan:** Migrasi OS dengan minimal disruption  
**Deliverables:** Server dengan OS baru, aplikasi ter-migrasi, dokumentasi konfigurasi  
**PIC:** DevOps Team, Infrastructure Team, Vendor Support  
**Task Dependency:** Dependent pada Fase 0  

**Pendekatan:** Server-by-server (prioritaskan non-produksi pertama).  
- **Task 3: Migrasi OS ke CentOS**  
  **Deliverables:** Server dengan CentOS baru, validasi konektivitas lengkap  
  **PIC:** DevOps Team, Infrastructure Team, Vendor Support  
  **Dependency:** Dependent pada Task 2  
  - Subtask 3.1: Deploy CentOS baru di server cadangan (fresh install).  
    **Deliverables:** Fresh CentOS installation ready  
    **PIC:** DevOps Team  
    **Dependency:** Independent (dapat paralel)  
  - Subtask 3.2: Migrasi data/config aplikasi ke OS baru.  
    **Deliverables:** Aplikasi ter-konfigurasi di OS baru  
    **PIC:** Developer Team, DevOps Team  
    **Dependency:** Dependent pada Subtask 3.1  
  - Subtask 3.3: Validasi konektivitas jaringan, storage, dan security (SELinux/firewall).  
    **Deliverables:** Network connectivity validation report  
    **PIC:** DevOps Team  
    **Dependency:** Dependent pada Subtask 3.2  
  - Subtask 3.4: Uji failover aplikasi ke server baru.  
    **Deliverables:** Successful failover test report  
    **PIC:** Developer Team, DevOps Team  
    **Dependency:** Dependent pada Subtask 3.3  
  - Subtask 3.5: Dokumentasi perubahan konfigurasi OS.  
    **Deliverables:** OS configuration change documentation  
    **PIC:** DevOps Team  
    **Dependency:** Dependent pada Subtask 3.4  

---

#### **Fase 2: Upgrade MySQL 5.6 → 5.7 (Minggu 6-7)**  
**Tujuan:** Stepping stone upgrade untuk mencapai MySQL 8.4  
**Deliverables:** MySQL 5.7 terkonfigurasi optimal, validasi aplikasi  
**PIC:** Vendor Support, DBA  
**Task Dependency:** Dependent pada Fase 1  

**Catatan:** Server MySQL 5.6 **harus** upgrade ke 5.7 sebelum ke 8.4 ([Official Upgrade Path](https://dev.mysql.com/doc/refman/8.4/en/upgrade-paths.html)).  
- **Task 4: In-Place Upgrade ke MySQL 5.7**  
  **Deliverables:** MySQL 5.7 running dengan validasi lengkap  
  **PIC:** Vendor Support, DBA  
  **Dependency:** Dependent pada Task 3  
  - Subtask 4.1: Stop layanan MySQL, backup data & config.  
    **Deliverables:** Pre-upgrade backup dan config files  
    **PIC:** Vendor Support  
    **Dependency:** Independent  
  - Subtask 4.2: Install MySQL 5.7, jalankan `mysql_upgrade`.  
    **Deliverables:** MySQL 5.7 installation completed  
    **PIC:** Vendor Support  
    **Dependency:** Dependent pada Subtask 4.1  
  - Subtask 4.3: Validasi data integrity (`CHECK TABLE`, checksum).  
    **Deliverables:** Data integrity validation report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 4.2  
  - Subtask 4.4: Uji koneksi aplikasi dan query critical.  
    **Deliverables:** Application connectivity test results  
    **PIC:** Developer Team, Vendor Support  
    **Dependency:** Dependent pada Subtask 4.3  
  - Subtask 4.5: Optimasi parameter `my.cnf` untuk versi 5.7.  
    **Deliverables:** Optimized MySQL 5.7 configuration  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 4.4  

---

#### **Fase 3: Simulasi & Adaptasi MySQL 8.4 (Minggu 8-9)**  
**Tujuan:** Beri waktu tim beradaptasi dengan perubahan drastis di MySQL 8.4.  
**Deliverables:** Tim siap untuk upgrade, prosedur teruji, training completed  
**PIC:** Project Leader, Developer Supervisor, Vendor Support  
**Task Dependency:** Dependent pada Fase 2  

- **Task 5: Pelatihan & Uji Coba**  
  **Deliverables:** Training materials, test scenarios completed, rollback procedures validated  
  **PIC:** Project Leader, Developer Supervisor, Vendor Support  
  **Dependency:** Dependent pada Task 4  
  - Subtask 5.1: Workshop fitur baru MySQL 8.4 (CTE, Window Functions, atomic DDL).  
    **Deliverables:** Training session completed, knowledge transfer documentation  
    **PIC:** Vendor Support, Developer Supervisor  
    **Dependency:** Independent  
  - Subtask 5.2: Simulasi error scenario (auth plugin issue, query failure).  
    **Deliverables:** Error simulation report dan troubleshooting guide  
    **PIC:** Vendor Support, Developer Team  
    **Dependency:** Independent (dapat paralel dengan 5.1)  
  - Subtask 5.3: Uji kompatibilitas aplikasi di staging (e.g., driver konektor lama).  
    **Deliverables:** Application compatibility test results  
    **PIC:** Developer Team, Vendor Support  
    **Dependency:** Independent (dapat paralel dengan 5.1-5.2)  
  - Subtask 5.4: Update script/CRON job yang bergantung pada MySQL.  
    **Deliverables:** Updated scripts dan automated jobs  
    **PIC:** Developer Team, DevOps Team  
    **Dependency:** Dependent pada Subtask 5.3  
  - Subtask 5.5: Trial run prosedur rollback.  
    **Deliverables:** Rollback procedure validation report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 5.1-5.4  

---

#### **Fase 4: Upgrade ke MySQL 8.4 Enterprise (Minggu 10-15)**  
**Tujuan:** Zero-downtime upgrade ke MySQL 8.4 Enterprise dengan HA setup  
**Deliverables:** MySQL 8.4 Enterprise running, InnoDB Cluster operational, Enterprise features configured  
**PIC:** Vendor Support, DBA, DevOps Team  
**Task Dependency:** Dependent pada Fase 3  

**Strategi:** Rolling upgrade cluster-by-cluster dengan zero-downtime menggunakan MySQL Router.  
- **Task 6: Migrasi ke MySQL 8.4 Enterprise + InnoDB Cluster Setup**  
  **Deliverables:** MySQL 8.4 Enterprise cluster fully operational  
  **PIC:** Vendor Support, DBA  
  **Dependency:** Dependent pada Task 5  
  - Subtask 6.1: **Upgrade secondary nodes terlebih dahulu** (rolling upgrade strategy).  
    **Deliverables:** Secondary nodes upgrade strategy plan  
    **PIC:** Vendor Support  
    **Dependency:** Independent  
  - Subtask 6.2: Stop MySQL pada secondary, backup terakhir, snapshot OS.  
    **Deliverables:** Pre-upgrade backup dan OS snapshot  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Dependent pada Subtask 6.1  
  - Subtask 6.3: Install MySQL 8.4 Enterprise, jalankan `mysql_upgrade` dengan opsi `--upgrade=FORCE`.  
    **Deliverables:** MySQL 8.4 Enterprise installation completed  
    **PIC:** Vendor Support  
    **Dependency:** Dependent pada Subtask 6.2  
  - Subtask 6.4: **Re-join secondary nodes ke InnoDB Cluster** (`cluster.rejoinInstance()`).  
    **Deliverables:** Secondary nodes successfully rejoined cluster  
    **PIC:** Vendor Support  
    **Dependency:** Dependent pada Subtask 6.3  
  - Subtask 6.5: **Switchover primary node** (`cluster.setPrimaryInstance()`) setelah secondary ready.  
    **Deliverables:** Primary switchover completed successfully  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 6.4  
  - Subtask 6.6: Upgrade former primary node (sekarang menjadi secondary).  
    **Deliverables:** Former primary node upgraded dan rejoined  
    **PIC:** Vendor Support  
    **Dependency:** Dependent pada Subtask 6.5  
  - Subtask 6.7: **Update MySQL Router configuration** untuk mengarah ke cluster baru.  
    **Deliverables:** Router configuration updated dan tested  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Dependent pada Subtask 6.6  
  - Subtask 6.8: Konversi tabel ke format baru (`ALTER TABLE ... ENGINE=InnoDB`).  
    **Deliverables:** All tables converted to new format  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 6.7  
  - Subtask 6.9: Update plugin auth (`ALTER USER ... IDENTIFIED WITH caching_sha2_password`).  
    **Deliverables:** Authentication plugin updated for all users  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 6.8  
  - Subtask 6.10: Validasi:  
    **Deliverables:** Comprehensive validation report  
    **PIC:** Vendor Support, DBA, Developer Team  
    **Dependency:** Dependent pada Subtask 6.9  
    - **InnoDB Cluster status** (`cluster.status()`).  
    - **MySQL Router connectivity** (read/write split).  
    - Scheduled events/stored procedures.  
    - Performance vs baseline.  
  - Subtask 6.11: Monitoring 48 jam pasca-upgrade (slow query, error log, cluster health).  
    **Deliverables:** 48-hour monitoring report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 6.10  

- **Task 7: Post-Upgrade & Enterprise Features**  
  **Deliverables:** Enterprise features configured, backup strategy implemented, failover tested  
  **PIC:** Vendor Support, DBA, DevOps Team  
  **Dependency:** Dependent pada Task 6  
  - Subtask 7.1: Optimasi konfigurasi MySQL 8.4 Enterprise (e.g., `innodb_dedicated_server`, Enterprise features).  
    **Deliverables:** Optimized MySQL 8.4 Enterprise configuration  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent  
  - Subtask 7.2: **Implement MySQL Enterprise Security** (audit log, transparent data encryption, firewall).  
    **Deliverables:** Enterprise Security features configured dan tested  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent (dapat paralel dengan 7.1)  
  - Subtask 7.3: **Configure MySQL Enterprise Monitor** (real-time monitoring, alerting).  
    **Deliverables:** Enterprise Monitor dashboard dan alerting setup  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Independent (dapat paralel dengan 7.1-7.2)  
  - Subtask 7.4: **Setup MySQL Router load balancing optimization** (connection pooling, query routing).  
    **Deliverables:** Optimized Router configuration  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Independent (dapat paralel dengan 7.1-7.3)  
  - Subtask 7.5: Hapus tabel/file legacy (mysql_old database).  
    **Deliverables:** Legacy cleanup completed  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 7.1-7.4  
  - Subtask 7.6: Implementasi backup baru menggunakan **MySQL Enterprise Backup** dan clone plugin.  
    **Deliverables:** New backup strategy implemented dan tested  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Dependent pada Subtask 7.2  
  - Subtask 7.7: **Test automatic failover scenarios** dengan MySQL Router.  
    **Deliverables:** Failover test results dan procedures  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Dependent pada Subtask 7.4  

---

#### **Fase 5: Stabilisasi & Dokumentasi (Minggu 16)**  
**Tujuan:** Validasi final, dokumentasi lengkap, handover ke tim operasional  
**Deliverables:** System certified, documentation complete, team trained  
**PIC:** Project Manager, Project Leader, Vendor Support  
**Task Dependency:** Dependent pada Fase 4  

- **Task 8: Final Check & Enterprise Validation**  
  **Deliverables:** Final certification report, complete documentation, trained operations team  
  **PIC:** Project Leader, Vendor Support  
  **Dependency:** Dependent pada Task 7  
  - Subtask 8.1: Audit security (user privileges, exposed ports, **Enterprise Security features**).  
    **Deliverables:** Security audit report dengan recommendations  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent  
  - Subtask 8.2: **Validate InnoDB Cluster health** dan automatic failover capabilities.  
    **Deliverables:** Cluster health validation report  
    **PIC:** Vendor Support, DBA  
    **Dependency:** Independent (dapat paralel dengan 8.1)  
  - Subtask 8.3: **Test MySQL Router endpoints** (read/write separation, load distribution).  
    **Deliverables:** Router endpoint testing report  
    **PIC:** Vendor Support, DevOps Team  
    **Dependency:** Independent (dapat paralel dengan 8.1-8.2)  
  - Subtask 8.4: Update dokumentasi: runbook, diagram arsitektur HA, **Enterprise monitoring setup**.  
    **Deliverables:** Complete system documentation  
    **PIC:** Project Leader, Developer Supervisor  
    **Dependency:** Dependent pada Subtask 8.1-8.3  
  - Subtask 8.5: Laporan akhir: metrik keberhasilan, lesson learned, **HA performance metrics**.  
    **Deliverables:** Final project report  
    **PIC:** Project Manager  
    **Dependency:** Dependent pada Subtask 8.4  
  - Subtask 8.6: Sertifikasi lingkungan produksi (handover ke tim ops dengan **Enterprise training**).  
    **Deliverables:** Certified production environment, trained operations team  
    **PIC:** Project Manager, Vendor Support  
    **Dependency:** Dependent pada Subtask 8.5  

---

### **Mitigasi Risiko & Hal yang Sering Terlewat (Enterprise HA Environment)**  
1. **Breaking Changes:**  
   - MySQL 8.4 menghapus `GROUP BY` implisit, default charset `utf8mb4`, dan strict SQL mode.  
   - **Solusi:** Gunakan `mysql_upgrade_checker` (dari MySQL Shell) sebelum upgrade.  

2. **InnoDB Cluster & Router Dependency:**  
   - MySQL Router cache yang tidak terupdate setelah cluster topology berubah.  
   - **Solusi:** Restart MySQL Router setelah setiap cluster operation, monitor router log.  

3. **Enterprise License Compliance:**  
   - Pastikan jumlah CPU cores tidak melebihi license Enterprise yang dimiliki.  
   - **Solusi:** Audit license usage dengan `SHOW STATUS LIKE 'Threads_connected'` dan monitor concurrent connections.  

4. **Split-Brain Scenario:**  
   - Kehilangan majority dalam InnoDB Cluster bisa menyebabkan cluster tidak bisa write.  
   - **Solusi:** Pastikan minimal 3 nodes untuk proper quorum, setup MySQL Router dengan proper timeout.  

5. **Dependency Tersembunyi:**  
   - Aplikasi lawas yang bergantung pada client library versi lama (e.g., `libmysqlclient.so.18`).  
   - **Solusi:** Test semua konektor (PHP, Python, JDBC) di staging dengan MySQL Router endpoints.  

6. **Downtime Tak Terduga:**  
   - Rolling upgrade InnoDB Cluster bisa memakan waktu 4-6 jam/cluster (tergantung ukuran DB).  
   - **Solusi:** Gunakan MySQL Router untuk transparent failover selama upgrade.  

7. **Rollback Kompleks:**  
   - Rollback MySQL 8.4 → 5.7 tidak didukung secara native, terutama dengan cluster setup.  
   - **Solusi:** Andalkan backup VM snapshot + mysqldump + cluster metadata backup.  

8. **Training Gap:**  
   - Tim tidak paham fitur Enterprise seperti MySQL Enterprise Monitor, Transparent Data Encryption.  
   - **Solusi:** Sertakan sesi hands-on di Fase 3 dengan focus pada Enterprise features.  

---

### **Rekomendasi Tools (Enterprise Environment)**  
- **Upgrade Checker:** `mysqlsh -- util checkForServerUpgrade`  
- **Cluster Management:** MySQL Shell (`cluster.status()`, `cluster.switchToMultiPrimaryMode()`)  
- **Backup:** **MySQL Enterprise Backup** + `mysqldump` + cluster metadata backup  
- **Monitoring:** **MySQL Enterprise Monitor** + Prometheus + Grafana (track cluster health, router metrics)  
- **Security:** **MySQL Enterprise Security** (audit, firewall, encryption)  
- **Router Management:** MySQL Router configuration tool, router status monitoring  
- **OS Migration:** CloneZilla (P2V), Vagrant untuk testing konfigurasi HA  

### **Catatan Akhir (Enterprise HA Considerations)**  
- **Tes Kritis:** Uji load dengan data >100 GB di staging termasuk **failover testing** dan **MySQL Router load balancing**.  
- **Buffer Time:** Alokasi waktu 25% lebih panjang untuk tiap cluster (antisipasi masalah HA setup).  
- **Komunikasi:** Koordinasi dengan tim dev/ops untuk jadwal **rolling upgrade** dan **zero-downtime strategy**.  
- **License Management:** Pastikan compliance dengan MySQL Enterprise License selama upgrade.  
- **Enterprise Support:** Manfaatkan Oracle MySQL Enterprise Support untuk critical issues.  

**Prioritas Server:** Jika ada cluster dengan beban tinggi, prioritaskan di akhir dan gunakan **read replica promotion** untuk meminimalkan dampak bisnis.

**Monitoring Khusus:**  
- Cluster health status via `cluster.status()`  
- Router endpoints connectivity  
- Enterprise Monitor alerts  
- License usage tracking

---

### **Task Dependency Matrix & Execution Strategy**

#### **Parallel Execution Opportunities:**
- **Fase 0:** Subtask 1.1, 1.2, 1.4 dapat dikerjakan paralel
- **Fase 1:** Subtask 3.1 dapat dilakukan paralel untuk multiple servers
- **Fase 3:** Subtask 5.1, 5.2, 5.3 dapat dikerjakan paralel
- **Fase 4:** Subtask 7.1, 7.2, 7.3, 7.4 dapat dikerjakan paralel
- **Fase 5:** Subtask 8.1, 8.2, 8.3 dapat dikerjakan paralel

#### **Critical Path Dependencies:**
1. **Fase 0 → Fase 1:** Staging environment harus siap sebelum OS migration
2. **Fase 1 → Fase 2:** OS migration harus complete sebelum MySQL 5.7 upgrade  
3. **Fase 2 → Fase 3:** MySQL 5.7 stable sebelum mulai training MySQL 8.4
4. **Fase 3 → Fase 4:** Team training complete sebelum production upgrade
5. **Fase 4 → Fase 5:** All clusters upgraded sebelum final validation

#### **Risk Mitigation untuk Dependencies:**
- **Vendor Support lead** untuk semua critical technical tasks
- **Internal team monitor** dan validate semua Vendor deliverables  
- **Parallel preparation** untuk reduce critical path duration
- **Go/No-Go checkpoints** setiap akhir fase untuk quality assurance
