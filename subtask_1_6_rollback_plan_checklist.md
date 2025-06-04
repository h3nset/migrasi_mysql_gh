# Subtask 1.6: Rollback Plan Documentation - Detail Checklist

**Project:** Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS  
**Deliverable:** Comprehensive rollback procedures  
**PIC:** Project Leader, Vendor Support  
**Dependency:** Dependent pada Subtask 1.1-1.5  

---

## **Comprehensive Rollback Plan Documentation**

### **1. ROLLBACK STRATEGY OVERVIEW**

#### **1.1 Rollback Scope Assessment**
| **Komponen** | **Current State** | **Target Rollback** | **Kompleksitas** | **RTO** | **RPO** | **Dependencies** | **Status** |
|--------------|-------------------|---------------------|------------------|---------|---------|------------------|----------|
| CentOS OS | [Current Version] | [Previous Version] | ☐ Low ☐ Medium ☐ High | ☐ hrs | ☐ hrs | OS Dependencies | ☐ Planned ☐ Ready |
| MySQL Version | [Current Version] | [Previous Version] | ☐ Low ☐ Medium ☐ High | ☐ hrs | ☐ hrs | DB Dependencies | ☐ Planned ☐ Ready |
| InnoDB Cluster | [Current Topology] | [Previous Topology] | ☐ Low ☐ Medium ☐ High | ☐ hrs | ☐ hrs | Cluster Dependencies | ☐ Planned ☐ Ready |
| MySQL Router | [Current Config] | [Previous Config] | ☐ Low ☐ Medium ☐ High | ☐ hrs | ☐ hrs | Router Dependencies | ☐ Planned ☐ Ready |
| Applications | [Current State] | [Previous State] | ☐ Low ☐ Medium ☐ High | ☐ hrs | ☐ hrs | App Dependencies | ☐ Planned ☐ Ready |

**Assessment Commands:**
```bash
# Check current system versions
cat /etc/os-release
mysql --version
mysql -e "SELECT @@version, @@version_comment;"

# Check cluster status
mysqlsh --uri root@localhost -e "dba.getCluster().status()"

# Check router status
systemctl status mysqlrouter
```

### **2. PRE-ROLLBACK REQUIREMENTS**

#### **2.1 Backup Verification Matrix**
| **Backup Type** | **Location** | **Timestamp** | **Size** | **Integrity Check** | **Restoration Test** | **Access Verified** | **Status** |
|-----------------|--------------|---------------|----------|---------------------|---------------------|-------------------|----------|
| OS Snapshot | [Path/Location] | [Date Time] | [GB] | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Yes ☐ No | ☐ Ready ☐ Issue |
| MySQL Backup | [Path/Location] | [Date Time] | [GB] | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Yes ☐ No | ☐ Ready ☐ Issue |
| Cluster Metadata | [Path/Location] | [Date Time] | [MB] | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Yes ☐ No | ☐ Ready ☐ Issue |
| Router Config | [Path/Location] | [Date Time] | [KB] | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Yes ☐ No | ☐ Ready ☐ Issue |
| Application Data | [Path/Location] | [Date Time] | [GB] | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Yes ☐ No | ☐ Ready ☐ Issue |

**Verification Commands:**
```bash
# Verify backup integrity
md5sum /backup/mysql_backup_*.sql
sha256sum /backup/os_snapshot_*.img

# Test backup accessibility
mysql -u root -p < /backup/test_restore_sample.sql

# Verify cluster metadata backup
mysqlsh --file=/backup/cluster_metadata_backup.js
```

### **3. OS ROLLBACK PROCEDURES**

#### **3.1 CentOS Rollback Plan**
| **Server** | **Current OS** | **Target OS** | **Rollback Method** | **Dependencies** | **Downtime Window** | **Assigned PIC** | **Status** |
|------------|----------------|---------------|---------------------|------------------|-------------------|------------------|----------|
| SRV-01 | [Current] | [Target] | ☐ Snapshot ☐ Fresh Install | [List] | [Hours] | [PIC Name] | ☐ Ready ☐ Pending |
| SRV-02 | [Current] | [Target] | ☐ Snapshot ☐ Fresh Install | [List] | [Hours] | [PIC Name] | ☐ Ready ☐ Pending |
| SRV-03 | [Current] | [Target] | ☐ Snapshot ☐ Fresh Install | [List] | [Hours] | [PIC Name] | ☐ Ready ☐ Pending |
| SRV-04 | [Current] | [Target] | ☐ Snapshot ☐ Fresh Install | [List] | [Hours] | [PIC Name] | ☐ Ready ☐ Pending |
| SRV-05 | [Current] | [Target] | ☐ Snapshot ☐ Fresh Install | [List] | [Hours] | [PIC Name] | ☐ Ready ☐ Pending |

**OS Rollback Commands:**
```bash
# Snapshot rollback method
virsh snapshot-revert domain_name snapshot_name

# Fresh install preparation
mkdir -p /rollback/os_configs
cp -r /etc/network /rollback/os_configs/
cp -r /etc/systemd /rollback/os_configs/

# Service restoration
systemctl enable --now mysql
systemctl enable --now mysqlrouter
```

### **4. MYSQL ROLLBACK PROCEDURES**

#### **4.1 MySQL Version Rollback Matrix**
| **Server** | **Current MySQL** | **Target MySQL** | **Rollback Method** | **Data Migration** | **Config Restore** | **Validation** | **Status** |
|------------|-------------------|------------------|---------------------|-------------------|-------------------|--------------|----------|
| SRV-01 | [Version] | [Version] | ☐ Backup Restore ☐ Downgrade | ☐ Required ☐ Not Required | ☐ Required ☐ Not Required | ☐ Planned ☐ Ready | ☐ Ready ☐ Pending |
| SRV-02 | [Version] | [Version] | ☐ Backup Restore ☐ Downgrade | ☐ Required ☐ Not Required | ☐ Required ☐ Not Required | ☐ Planned ☐ Ready | ☐ Ready ☐ Pending |
| SRV-03 | [Version] | [Version] | ☐ Backup Restore ☐ Downgrade | ☐ Required ☐ Not Required | ☐ Required ☐ Not Required | ☐ Planned ☐ Ready | ☐ Ready ☐ Pending |
| SRV-04 | [Version] | [Version] | ☐ Backup Restore ☐ Downgrade | ☐ Required ☐ Not Required | ☐ Required ☐ Not Required | ☐ Planned ☐ Ready | ☐ Ready ☐ Pending |
| SRV-05 | [Version] | [Version] | ☐ Backup Restore ☐ Downgrade | ☐ Required ☐ Not Required | ☐ Required ☐ Not Required | ☐ Planned ☐ Ready | ☐ Ready ☐ Pending |

**MySQL Rollback Commands:**
```sql
-- Stop MySQL service
systemctl stop mysql

-- Restore data from backup
mysql -u root -p < /backup/full_backup_pre_upgrade.sql

-- Restore configuration
cp /backup/my.cnf.backup /etc/mysql/my.cnf

-- Restart and validate
systemctl start mysql
mysql -e "SELECT @@version; SHOW DATABASES;"
```

### **5. INNODB CLUSTER ROLLBACK PROCEDURES**

#### **5.1 Cluster Topology Rollback**
| **Component** | **Current State** | **Target State** | **Rollback Steps** | **Dependencies** | **Validation** | **PIC** | **Status** |
|---------------|-------------------|------------------|-------------------|------------------|---------------|---------|----------|
| Primary Node | [Current] | [Target] | 1. Stop cluster<br>2. Restore backup<br>3. Reconfigure | Cluster metadata | ☐ Topology ☐ Data | [PIC] | ☐ Ready ☐ Pending |
| Secondary Node 1 | [Current] | [Target] | 1. Remove from cluster<br>2. Restore backup<br>3. Rejoin | Primary node ready | ☐ Topology ☐ Data | [PIC] | ☐ Ready ☐ Pending |
| Secondary Node 2 | [Current] | [Target] | 1. Remove from cluster<br>2. Restore backup<br>3. Rejoin | Primary node ready | ☐ Topology ☐ Data | [PIC] | ☐ Ready ☐ Pending |
| Cluster Metadata | [Current] | [Target] | 1. Load metadata backup<br>2. Validate consistency | All nodes ready | ☐ Consistency ☐ Status | [PIC] | ☐ Ready ☐ Pending |

**Cluster Rollback Commands:**
```javascript
// MySQL Shell cluster rollback
mysqlsh --uri root@primary_node

// Dissolve current cluster
dba.getCluster().dissolve()

// Restore from metadata backup
\source /backup/cluster_metadata_backup.js

// Recreate cluster
var cluster = dba.createCluster('prodCluster')
cluster.addInstance('root@secondary1:3306')
cluster.addInstance('root@secondary2:3306')
```

### **6. MYSQL ROUTER ROLLBACK PROCEDURES**

#### **6.1 Router Configuration Rollback**
| **Router Node** | **Current Config** | **Target Config** | **Endpoint Changes** | **Load Balancing** | **Failover Config** | **Testing** | **Status** |
|-----------------|-------------------|-------------------|---------------------|-------------------|-------------------|-------------|----------|
| Router-01 | [Current] | [Target] | [Changes] | [Config] | [Config] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |
| Router-02 | [Current] | [Target] | [Changes] | [Config] | [Config] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |
| Router-03 | [Current] | [Target] | [Changes] | [Config] | [Config] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |

**Router Rollback Commands:**
```bash
# Stop MySQL Router
systemctl stop mysqlrouter

# Restore configuration
cp /backup/mysqlrouter.conf.backup /etc/mysqlrouter/mysqlrouter.conf

# Update routing rules
mysqlrouter --bootstrap root@primary_node:3306 --conf-use-sockets

# Restart and validate
systemctl start mysqlrouter
mysql -h router_host -P 6446 -e "SELECT @@hostname;"
```

### **7. APPLICATION ROLLBACK PROCEDURES**

#### **7.1 Application Component Rollback**
| **Application** | **Current Version** | **Target Version** | **Database Connection** | **Dependencies** | **Config Changes** | **Testing** | **Status** |
|-----------------|--------------------|--------------------|------------------------|------------------|-------------------|-------------|----------|
| Web App 1 | [Version] | [Version] | [Connection String] | [Dependencies] | [Changes] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |
| Web App 2 | [Version] | [Version] | [Connection String] | [Dependencies] | [Changes] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |
| API Service | [Version] | [Version] | [Connection String] | [Dependencies] | [Changes] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |
| Background Jobs | [Version] | [Version] | [Connection String] | [Dependencies] | [Changes] | ☐ Pass ☐ Fail | ☐ Ready ☐ Pending |

**Application Rollback Commands:**
```bash
# Stop applications
systemctl stop webapp1 webapp2 api-service

# Restore application files
cp -r /backup/applications/* /opt/applications/

# Update database connections
sed -i 's/new_mysql_host/old_mysql_host/g' /opt/applications/config/database.conf

# Restart applications
systemctl start webapp1 webapp2 api-service
```

### **8. ROLLBACK EXECUTION TIMELINE**

#### **8.1 Rollback Execution Matrix**
| **Phase** | **Duration** | **Components** | **Dependencies** | **Go/No-Go Criteria** | **PIC** | **Status** |
|-----------|--------------|----------------|------------------|----------------------|---------|----------|
| Phase 1: Pre-Rollback | 2 hours | Backup verification, team coordination | All backups verified | ☐ Backups ready ☐ Team ready | [PIC] | ☐ Ready ☐ Pending |
| Phase 2: Application Stop | 30 mins | Stop all applications | Phase 1 complete | ☐ Apps stopped ☐ Connections drained | [PIC] | ☐ Ready ☐ Pending |
| Phase 3: Cluster Rollback | 3 hours | InnoDB Cluster & Router | Phase 2 complete | ☐ Cluster restored ☐ Router functional | [PIC] | ☐ Ready ☐ Pending |
| Phase 4: MySQL Rollback | 2 hours | MySQL version & config | Phase 3 complete | ☐ MySQL restored ☐ Data consistent | [PIC] | ☐ Ready ☐ Pending |
| Phase 5: OS Rollback | 4 hours | Operating system | Phase 4 complete | ☐ OS restored ☐ Services running | [PIC] | ☐ Ready ☐ Pending |
| Phase 6: Application Start | 1 hour | Restart applications | Phase 5 complete | ☐ Apps running ☐ Connectivity OK | [PIC] | ☐ Ready ☐ Pending |
| Phase 7: Validation | 2 hours | End-to-end testing | Phase 6 complete | ☐ All tests pass ☐ Performance OK | [PIC] | ☐ Ready ☐ Pending |

### **9. ROLLBACK TRIGGER CONDITIONS**

#### **9.1 Rollback Decision Matrix**
| **Trigger Condition** | **Severity** | **Auto Rollback** | **Manual Decision** | **Decision Maker** | **Time Limit** | **Action** |
|----------------------|--------------|-------------------|-------------------|-------------------|---------------|----------|
| Data corruption detected | ☐ Critical ☐ High ☐ Medium | ☐ Yes ☐ No | ☐ Yes ☐ No | [Role] | [Time] | ☐ Immediate ☐ Planned |
| Performance degradation >50% | ☐ Critical ☐ High ☐ Medium | ☐ Yes ☐ No | ☐ Yes ☐ No | [Role] | [Time] | ☐ Immediate ☐ Planned |
| Application compatibility issues | ☐ Critical ☐ High ☐ Medium | ☐ Yes ☐ No | ☐ Yes ☐ No | [Role] | [Time] | ☐ Immediate ☐ Planned |
| Cluster split-brain scenario | ☐ Critical ☐ High ☐ Medium | ☐ Yes ☐ No | ☐ Yes ☐ No | [Role] | [Time] | ☐ Immediate ☐ Planned |
| Security vulnerability exposed | ☐ Critical ☐ High ☐ Medium | ☐ Yes ☐ No | ☐ Yes ☐ No | [Role] | [Time] | ☐ Immediate ☐ Planned |

### **10. ROLLBACK VALIDATION PROCEDURES**

#### **10.1 Post-Rollback Validation Checklist**
| **Validation Area** | **Test Description** | **Expected Result** | **Actual Result** | **Pass/Fail** | **Notes** |
|-------------------|---------------------|-------------------|------------------|---------------|-----------|
| OS Functionality | System services, network, storage | All services running | [Result] | ☐ Pass ☐ Fail | [Notes] |
| MySQL Connectivity | Database connections, queries | All connections work | [Result] | ☐ Pass ☐ Fail | [Notes] |
| Cluster Health | InnoDB Cluster status and replication | Cluster healthy | [Result] | ☐ Pass ☐ Fail | [Notes] |
| Router Functionality | Read/write split, load balancing | Router working properly | [Result] | ☐ Pass ☐ Fail | [Notes] |
| Application Performance | Response time, throughput | Performance restored | [Result] | ☐ Pass ☐ Fail | [Notes] |
| Data Integrity | Data consistency checks | Data intact | [Result] | ☐ Pass ☐ Fail | [Notes] |

**Validation Commands:**
```bash
# System validation
systemctl status mysql mysqlrouter
df -h
free -m

# MySQL validation
mysql -e "SHOW SLAVE STATUS\G"
mysql -e "SHOW MASTER STATUS;"

# Cluster validation
mysqlsh --uri root@localhost -e "dba.getCluster().status()"

# Application validation
curl -I http://localhost/health
```

### **11. COMMUNICATION PLAN**

#### **11.1 Rollback Communication Matrix**
| **Stakeholder** | **Notification Method** | **Timing** | **Information Level** | **Contact Person** | **Escalation** |
|-----------------|------------------------|------------|----------------------|-------------------|---------------|
| Management | Email + Phone | Immediate | Executive summary | [Contact] | [Escalation] |
| Development Team | Slack + Email | Immediate | Technical details | [Contact] | [Escalation] |
| Operations Team | Phone + SMS | Immediate | Operational impact | [Contact] | [Escalation] |
| End Users | Website + Email | After completion | Service status | [Contact] | [Escalation] |
| Vendor Support | Phone + Ticket | Immediate | Technical assistance | [Contact] | [Escalation] |

### **12. DOCUMENTATION & SIGN-OFF**

#### **12.1 Rollback Plan Approval**
| **Role** | **Name** | **Review Date** | **Approval Status** | **Comments** | **Signature** |
|----------|----------|-----------------|-------------------|--------------|---------------|
| Project Manager | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| Technical Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| DBA | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| DevOps Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| Vendor Support | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |

**Final Approval:** ☐ Approved ☐ Rejected ☐ Pending Review  
**Approval Date:** ___________  
**Next Review Date:** ___________  

---

**Catatan Penting:**
1. **Rollback harus diuji** di staging environment sebelum production
2. **Tim siaga 24/7** selama periode rollback
3. **Backup terverifikasi** sebelum memulai rollback
4. **Komunikasi stakeholder** harus jelas dan tepat waktu
5. **Dokumentasi lengkap** semua langkah rollback untuk audit trail
