### **Project Timeline: Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 + Upgrade OS ke CentOS**  
**Jumlah Server:** 10  
**Total Estimasi:** 14 Minggu (termasuk buffer & adaptasi)  

---

#### **Fase 0: Persiapan (Minggu 1-2)**  
**Tujuan:** Risiko minimal, persiapan komprehensif.  
- **Task 1: Assessment & Perencanaan**  
  - Subtask 1.1: Audit server (OS version, MySQL config, dependency aplikasi, ukuran database).  
  - Subtask 1.2: Identifikasi breaking changes MySQL 8.4 (e.g., `caching_sha2_password`, reserved keywords, syntax deprecated).  
  - Subtask 1.3: Cek kompatibilitas aplikasi dengan CentOS versi terbaru (e.g., CentOS 8 Stream/Rocky Linux).  
  - Subtask 1.4: Buat dokumen rollback plan (OS & MySQL).  
  - Subtask 1.5: Siapkan lingkungan staging (replikasi 1:1 dari produksi).  

- **Task 2: Backup & Pengujian Awal**  
  - Subtask 2.1: Backup fisik/logis semua database (`mysqldump`, snapshot VM/disk).  
  - Subtask 2.2: Uji restore backup di lingkungan staging.  
  - Subtask 2.3: Benchmark performa baseline (CPU, RAM, I/O, query latency).  

---

#### **Fase 1: Upgrade OS ke CentOS (Minggu 3-5)**  
**Pendekatan:** Server-by-server (prioritaskan non-produksi pertama).  
- **Task 3: Migrasi OS ke CentOS**  
  - Subtask 3.1: Deploy CentOS baru di server cadangan (fresh install).  
  - Subtask 3.2: Migrasi data/config aplikasi ke OS baru.  
  - Subtask 3.3: Validasi konektivitas jaringan, storage, dan security (SELinux/firewall).  
  - Subtask 3.4: Uji failover aplikasi ke server baru.  
  - Subtask 3.5: Dokumentasi perubahan konfigurasi OS.  

---

#### **Fase 2: Upgrade MySQL 5.6 → 5.7 (Minggu 6-7)**  
**Catatan:** Server MySQL 5.6 **harus** upgrade ke 5.7 sebelum ke 8.4 ([Official Upgrade Path](https://dev.mysql.com/doc/refman/8.4/en/upgrade-paths.html)).  
- **Task 4: In-Place Upgrade ke MySQL 5.7**  
  - Subtask 4.1: Stop layanan MySQL, backup data & config.  
  - Subtask 4.2: Install MySQL 5.7, jalankan `mysql_upgrade`.  
  - Subtask 4.3: Validasi data integrity (`CHECK TABLE`, checksum).  
  - Subtask 4.4: Uji koneksi aplikasi dan query critical.  
  - Subtask 4.5: Optimasi parameter `my.cnf` untuk versi 5.7.  

---

#### **Fase 3: Simulasi & Adaptasi MySQL 8.4 (Minggu 8-9)**  
**Tujuan:** Beri waktu tim beradaptasi dengan perubahan drastis di MySQL 8.4.  
- **Task 5: Pelatihan & Uji Coba**  
  - Subtask 5.1: Workshop fitur baru MySQL 8.4 (CTE, Window Functions, atomic DDL).  
  - Subtask 5.2: Simulasi error scenario (auth plugin issue, query failure).  
  - Subtask 5.3: Uji kompatibilitas aplikasi di staging (e.g., driver konektor lama).  
  - Subtask 5.4: Update script/CRON job yang bergantung pada MySQL.  
  - Subtask 5.5: Trial run prosedur rollback.  

---

#### **Fase 4: Upgrade ke MySQL 8.4 (Minggu 10-13)**  
**Strategi:** Paralel 2 server/minggu (total 6 server).  
- **Task 6: Migrasi ke MySQL 8.4**  
  - Subtask 6.1: Stop MySQL, backup terakhir, snapshot OS.  
  - Subtask 6.2: Install MySQL 8.4, jalankan `mysql_upgrade` dengan opsi `--upgrade=FORCE`.  
  - Subtask 6.3: Konversi tabel ke format baru (`ALTER TABLE ... ENGINE=InnoDB`).  
  - Subtask 6.4: Update plugin auth (`ALTER USER ... IDENTIFIED WITH caching_sha2_password`).  
  - Subtask 6.5: Validasi:  
    - Replikasi (jika ada).  
    - Scheduled events/stored procedures.  
    - Performance vs baseline.  
  - Subtask 6.6: Monitoring 48 jam pasca-upgrade (slow query, error log, resource usage).  

- **Task 7: Post-Upgrade**  
  - Subtask 7.1: Optimasi konfigurasi MySQL 8.4 (e.g., `innodb_dedicated_server`).  
  - Subtask 7.2: Hapus tabel/file legacy (mysql_old database).  
  - Subtask 7.3: Implementasi backup baru (menggunakan fitur MySQL 8.4 seperti clone plugin).  

---

#### **Fase 5: Stabilisasi & Dokumentasi (Minggu 14)**  
- **Task 8: Final Check**  
  - Subtask 8.1: Audit security (user privileges, exposed ports).  
  - Subtask 8.2: Update dokumentasi: runbook, diagram arsitektur.  
  - Subtask 8.3: Laporan akhir: metrik keberhasilan, lesson learned.  
  - Subtask 8.4: Sertifikasi lingkungan produksi (handover ke tim ops).  

---

### **Mitigasi Risiko & Hal yang Sering Terlewat**  
1. **Breaking Changes:**  
   - MySQL 8.4 menghapus `GROUP BY` implisit, default charset `utf8mb4`, dan strict SQL mode.  
   - **Solusi:** Gunakan `mysql_upgrade_checker` (dari MySQL Shell) sebelum upgrade.  

2. **Dependency Tersembunyi:**  
   - Aplikasi lawas yang bergantung pada client library versi lama (e.g., `libmysqlclient.so.18`).  
   - **Solusi:** Test semua konektor (PHP, Python, JDBC) di staging.  

3. **Downtime Tak Terduga:**  
   - Upgrade OS/MySQL bisa memakan waktu 2-4 jam/server (tergantung ukuran DB).  
   - **Solusi:** Gunakan replication untuk mengurangi downtime (promote replica yang sudah di-upgrade).  

4. **Rollback Kompleks:**  
   - Rollback MySQL 8.4 → 5.7 tidak didukung secara native.  
   - **Solusi:** Andalkan backup VM snapshot + mysqldump.  

5. **Training Gap:**  
   - Tim tidak paham fitur baru seperti Resource Group, Invisible Index.  
   - **Solusi:** Sertakan sesi hands-on di Fase 3.  

---

### **Rekomendasi Tools**  
- **Upgrade Checker:** `mysqlsh -- util checkForServerUpgrade`  
- **Backup:** `mysqldump` + `xtrabackup` (Percona), snapshot filesystem.  
- **Monitoring:** Prometheus + Grafana (track QPS, connections, buffer pool usage).  
- **OS Migration:** CloneZilla (P2V), Vagrant untuk testing konfigurasi.  

### **Catatan Akhir**  
- **Tes Kritis:** Uji load dengan data >100 GB di staging sebelum produksi.  
- **Buffer Time:** Alokasi waktu 20% lebih panjang untuk tiap server (antisipasi masalah tak terduga).  
- **Komunikasi:** Koordinasi dengan tim dev/ops untuk jadwal downtime.  

Jika ada server dengan beban tinggi, prioritaskan di akhir untuk meminimalkan dampak bisnis.
