# Subtask 1.1: Server Audit Checklist - Detail Report

**Project:** Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS  
**Deliverable:** Server audit report dengan detail konfigurasi  
**PIC:** Vendor Support, DBA  
**Dependency:** Independent  

---

## **Server Audit Checklist untuk 10 Server (MySQL Enterprise HA Environment)**

### **Template Inventory per Server**

| **Server ID** | **Hostname/IP** | **Role** | **Environment** | **Status** |
|---------------|-----------------|----------|-----------------|------------|
| SRV-01        | [IP/Hostname]   | Primary  | Production      | ☐ Audit Complete |
| SRV-02        | [IP/Hostname]   | Secondary| Production      | ☐ Audit Complete |
| SRV-03        | [IP/Hostname]   | Router   | Production      | ☐ Audit Complete |
| SRV-04        | [IP/Hostname]   | Primary  | Staging         | ☐ Audit Complete |
| SRV-05        | [IP/Hostname]   | Secondary| Staging         | ☐ Audit Complete |
| SRV-06        | [IP/Hostname]   | Router   | Staging         | ☐ Audit Complete |
| SRV-07        | [IP/Hostname]   | Primary  | Development     | ☐ Audit Complete |
| SRV-08        | [IP/Hostname]   | Secondary| Development     | ☐ Audit Complete |
| SRV-09        | [IP/Hostname]   | Router   | Development     | ☐ Audit Complete |
| SRV-10        | [IP/Hostname]   | Backup   | Production      | ☐ Audit Complete |

---

## **1. AUDIT OPERATING SYSTEM**

### **1.1 OS Version & Kernel Information**
| **Item** | **Command** | **Expected** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|----------|-------------|--------------|---------------|---------------|---------------|-----------|
| OS Distribution | `cat /etc/os-release` | CentOS/RHEL/Rocky Linux | ☐ | ☐ | ☐ | Current version |
| Kernel Version | `uname -r` | 3.x atau 4.x | ☐ | ☐ | ☐ | Compatible with MySQL 8.4 |
| Architecture | `uname -m` | x86_64 | ☐ | ☐ | ☐ | 64-bit required |
| Hostname | `hostname -f` | FQDN configured | ☐ | ☐ | ☐ | Cluster identification |
| Uptime | `uptime` | System stability | ☐ | ☐ | ☐ | Reboot requirements |

### **1.2 System Resources**
| **Item** | **Command** | **Minimum** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|----------|-------------|-------------|---------------|---------------|---------------|-----------|
| CPU Cores | `nproc` | 4+ cores | ☐ | ☐ | ☐ | Enterprise License |
| RAM Total | `free -h` | 8GB+ | ☐ | ☐ | ☐ | InnoDB buffer pool |
| Disk Space / | `df -h /` | 50GB+ free | ☐ | ☐ | ☐ | OS requirements |
| Disk Space /var | `df -h /var` | 20GB+ free | ☐ | ☐ | ☐ | MySQL logs |
| Swap Space | `swapon -s` | 2GB+ | ☐ | ☐ | ☐ | Performance tuning |

### **1.3 Network Configuration**
| **Item** | **Command** | **Requirement** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|----------|-------------|-----------------|---------------|---------------|---------------|-----------|
| IP Address | `ip addr show` | Static IP | ☐ | ☐ | ☐ | Cluster stability |
| DNS Resolution | `nslookup localhost` | Working | ☐ | ☐ | ☐ | Name resolution |
| Firewall Status | `systemctl status firewalld` | Configured | ☐ | ☐ | ☐ | Port accessibility |
| SELinux Status | `getenforce` | Enforcing/Permissive | ☐ | ☐ | ☐ | Security policy |
| NTP/Chrony | `timedatectl status` | Synchronized | ☐ | ☐ | ☐ | Cluster sync |

---

## **2. AUDIT MYSQL CONFIGURATION**

### **2.1 MySQL Version & Installation**
| **Item** | **Command** | **Current** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|----------|-------------|-------------|---------------|---------------|---------------|-----------|
| MySQL Version | `mysql --version` | 5.6.x atau 5.7.x | ☐ | ☐ | ☐ | Upgrade path validation |
| Installation Type | `rpm -qa \| grep mysql` | RPM/Source | ☐ | ☐ | ☐ | Package management |
| MySQL Status | `systemctl status mysqld` | Active/Running | ☐ | ☐ | ☐ | Service health |
| MySQL Port | `netstat -tlnp \| grep 3306` | 3306 listening | ☐ | ☐ | ☐ | Default port check |
| MySQL Socket | `mysql -e "SHOW VARIABLES LIKE 'socket';"` | Socket path | ☐ | ☐ | ☐ | Local connections |

### **2.2 MySQL Configuration Files**
| **Item** | **File Path** | **Exists** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|----------|---------------|------------|---------------|---------------|---------------|-----------|
| Main Config | `/etc/my.cnf` | ☐ Yes ☐ No | ☐ | ☐ | ☐ | Primary config |
| Include Dir | `/etc/my.cnf.d/` | ☐ Yes ☐ No | ☐ | ☐ | ☐ | Modular configs |
| MySQL Config | `/etc/mysql/my.cnf` | ☐ Yes ☐ No | ☐ | ☐ | ☐ | Alternative location |
| User Config | `~/.my.cnf` | ☐ Yes ☐ No | ☐ | ☐ | ☐ | User overrides |
| Log Config | `/var/log/mysqld.log` | ☐ Yes ☐ No | ☐ | ☐ | ☐ | Error logging |

### **2.3 Critical MySQL Variables**
| **Variable** | **Command** | **Current Value** | **Server 01** | **Server 02** | **Server 03** | **Notes** |
|--------------|-------------|-------------------|---------------|---------------|---------------|-----------|
| innodb_buffer_pool_size | `SHOW VARIABLES LIKE 'innodb_buffer_pool_size';` | | ☐ | ☐ | ☐ | Memory allocation |
| max_connections | `SHOW VARIABLES LIKE 'max_connections';` | | ☐ | ☐ | ☐ | Connection limits |
| innodb_log_file_size | `SHOW VARIABLES LIKE 'innodb_log_file_size';` | | ☐ | ☐ | ☐ | Transaction logging |
| sql_mode | `SHOW VARIABLES LIKE 'sql_mode';` | | ☐ | ☐ | ☐ | Compatibility check |
| default_authentication_plugin | `SHOW VARIABLES LIKE 'default_authentication_plugin';` | | ☐ | ☐ | ☐ | Auth method |

---

## **3. AUDIT DATABASE STRUCTURE & SIZE**

### **3.1 Database Inventory**
| **Database Name** | **Size (MB)** | **Tables Count** | **Engine Type** | **Character Set** | **Notes** |
|-------------------|---------------|------------------|-----------------|-------------------|-----------|
| [DB_NAME_1] | ☐ | ☐ | ☐ InnoDB ☐ MyISAM | ☐ | ☐ Critical ☐ Non-Critical |
| [DB_NAME_2] | ☐ | ☐ | ☐ InnoDB ☐ MyISAM | ☐ | ☐ Critical ☐ Non-Critical |
| [DB_NAME_3] | ☐ | ☐ | ☐ InnoDB ☐ MyISAM | ☐ | ☐ Critical ☐ Non-Critical |
| [DB_NAME_4] | ☐ | ☐ | ☐ InnoDB ☐ MyISAM | ☐ | ☐ Critical ☐ Non-Critical |
| **TOTAL** | **☐ GB** | **☐** | | | |

**Commands untuk mendapatkan data:**
```sql
-- Database sizes
SELECT 
    schema_name as 'Database',
    ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) as 'Size_MB'
FROM information_schema.SCHEMATA s
LEFT JOIN information_schema.TABLES t ON s.schema_name = t.table_schema
GROUP BY schema_name;

-- Table count per database
SELECT schema_name, COUNT(*) as table_count 
FROM information_schema.tables 
GROUP BY schema_name;

-- Storage engines
SELECT engine, COUNT(*) as table_count 
FROM information_schema.tables 
WHERE table_schema NOT IN ('information_schema','mysql','performance_schema','sys')
GROUP BY engine;
```

### **3.2 Large Tables Analysis (>1GB)**
| **Database** | **Table Name** | **Size (GB)** | **Engine** | **Partitioned** | **Migration Priority** |
|--------------|----------------|---------------|------------|-----------------|----------------------|
| | | ☐ | ☐ InnoDB ☐ MyISAM | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| | | ☐ | ☐ InnoDB ☐ MyISAM | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| | | ☐ | ☐ InnoDB ☐ MyISAM | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |

---

## **4. AUDIT APPLICATION DEPENDENCIES**

### **4.1 Connected Applications**
| **Application Name** | **Connection Method** | **Database** | **User Account** | **Connection Count** | **Critical Level** |
|---------------------|----------------------|--------------|------------------|---------------------|-------------------|
| [APP_NAME_1] | ☐ Direct ☐ Connection Pool | [DB_NAME] | [USERNAME] | ☐ | ☐ Critical ☐ Normal |
| [APP_NAME_2] | ☐ Direct ☐ Connection Pool | [DB_NAME] | [USERNAME] | ☐ | ☐ Critical ☐ Normal |
| [APP_NAME_3] | ☐ Direct ☐ Connection Pool | [DB_NAME] | [USERNAME] | ☐ | ☐ Critical ☐ Normal |

**Commands untuk audit:**
```sql
-- Active connections
SHOW PROCESSLIST;

-- User accounts
SELECT user, host, authentication_string, password_expired 
FROM mysql.user 
WHERE user NOT IN ('root','mysql.sys','mysql.session');
```

### **4.2 Database Users & Privileges**
| **Username** | **Host** | **Privileges** | **Password Plugin** | **SSL Required** | **Notes** |
|--------------|----------|----------------|-------------------|------------------|-----------|
| [USER_1] | [HOST] | ☐ | ☐ mysql_native_password ☐ Other | ☐ Yes ☐ No | ☐ App User ☐ Admin |
| [USER_2] | [HOST] | ☐ | ☐ mysql_native_password ☐ Other | ☐ Yes ☐ No | ☐ App User ☐ Admin |
| [USER_3] | [HOST] | ☐ | ☐ mysql_native_password ☐ Other | ☐ Yes ☐ No | ☐ App User ☐ Admin |

### **4.3 Scheduled Jobs & Cron**
| **Job Name** | **Schedule** | **Description** | **Database Dependent** | **Modification Required** |
|--------------|--------------|-----------------|----------------------|--------------------------|
| [JOB_1] | [CRON_SCHEDULE] | [DESCRIPTION] | ☐ Yes ☐ No | ☐ Yes ☐ No |
| [JOB_2] | [CRON_SCHEDULE] | [DESCRIPTION] | ☐ Yes ☐ No | ☐ Yes ☐ No |

**Command:** `crontab -l` dan `systemctl list-timers`

---

## **5. AUDIT CLUSTER TOPOLOGY (InnoDB Cluster)**

### **5.1 Current Replication Setup**
| **Server** | **Role** | **Replication Status** | **Lag (seconds)** | **GTID Enabled** | **Notes** |
|------------|----------|----------------------|-------------------|------------------|-----------|
| SRV-01 | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped | ☐ | ☐ Yes ☐ No | Primary candidate |
| SRV-02 | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped | ☐ | ☐ Yes ☐ No | Secondary candidate |
| SRV-03 | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped | ☐ | ☐ Yes ☐ No | Router candidate |

**Commands:**
```sql
-- Replication status
SHOW MASTER STATUS;
SHOW SLAVE STATUS\G

-- GTID status
SHOW VARIABLES LIKE 'gtid_mode';
SHOW VARIABLES LIKE 'enforce_gtid_consistency';
```

### **5.2 MySQL Router Requirements**
| **Item** | **Requirement** | **Server 03** | **Server 06** | **Server 09** | **Notes** |
|----------|-----------------|---------------|---------------|---------------|-----------|
| Router Installed | MySQL Router 8.x | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Yes ☐ No | Package availability |
| Port 6446 (RW) | Free/Available | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Yes ☐ No | Read-Write port |
| Port 6447 (RO) | Free/Available | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Yes ☐ No | Read-Only port |
| Config Directory | /etc/mysqlrouter/ | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Yes ☐ No | Configuration path |

---

## **6. AUDIT BACKUP & RECOVERY**

### **6.1 Current Backup Strategy**
| **Backup Type** | **Schedule** | **Location** | **Retention** | **Last Successful** | **Size** |
|-----------------|--------------|--------------|---------------|--------------------| ---------|
| ☐ mysqldump ☐ Snapshot ☐ Other | [SCHEDULE] | [PATH] | [DAYS] | [DATE] | [SIZE] |
| ☐ mysqldump ☐ Snapshot ☐ Other | [SCHEDULE] | [PATH] | [DAYS] | [DATE] | [SIZE] |

### **6.2 Recovery Testing**
| **Test Type** | **Last Tested** | **Success** | **Recovery Time** | **Notes** |
|---------------|-----------------|-------------|-------------------|-----------|
| Full Restore | [DATE] | ☐ Yes ☐ No | [MINUTES] | Point-in-time capability |
| Table Restore | [DATE] | ☐ Yes ☐ No | [MINUTES] | Granular recovery |
| PITR | [DATE] | ☐ Yes ☐ No | [MINUTES] | Binary log based |

---

## **7. SECURITY AUDIT**

### **7.1 MySQL Security Settings**
| **Setting** | **Command** | **Secure Value** | **Current** | **Compliant** | **Notes** |
|-------------|-------------|------------------|-------------|---------------|-----------|
| root password | | Set | ☐ | ☐ Yes ☐ No | Strong password |
| Anonymous users | `SELECT user,host FROM mysql.user WHERE user='';` | None | ☐ | ☐ Yes ☐ No | Remove anonymous |
| Test database | `SHOW DATABASES LIKE 'test';` | Not exist | ☐ | ☐ Yes ☐ No | Remove test DB |
| Remote root | `SELECT user,host FROM mysql.user WHERE user='root' AND host!='localhost';` | Disabled | ☐ | ☐ Yes ☐ No | Local root only |
| SSL/TLS | `SHOW VARIABLES LIKE 'have_ssl';` | YES | ☐ | ☐ Yes ☐ No | Encrypted connections |

### **7.2 File Permissions**
| **File/Directory** | **Owner** | **Permissions** | **Current** | **Compliant** | **Notes** |
|-------------------|-----------|-----------------|-------------|---------------|-----------|
| /var/lib/mysql | mysql:mysql | 700 | ☐ | ☐ Yes ☐ No | Data directory |
| /etc/my.cnf | root:root | 644 | ☐ | ☐ Yes ☐ No | Config file |
| MySQL socket | mysql:mysql | 777 | ☐ | ☐ Yes ☐ No | Socket file |

---

## **8. PERFORMANCE BASELINE**

### **8.1 Current Performance Metrics**
| **Metric** | **Command** | **Current Value** | **Baseline Date** | **Notes** |
|------------|-------------|-------------------|-------------------|-----------|
| QPS (Queries/sec) | `SHOW GLOBAL STATUS LIKE 'Questions';` | ☐ | [DATE] | Query throughput |
| TPS (Transactions/sec) | `SHOW GLOBAL STATUS LIKE 'Com_commit';` | ☐ | [DATE] | Transaction rate |
| Connection Usage | `SHOW GLOBAL STATUS LIKE 'Threads_connected';` | ☐ | [DATE] | Current connections |
| InnoDB Buffer Hit Ratio | `SHOW GLOBAL STATUS LIKE 'Innodb_buffer_pool%';` | ☐ % | [DATE] | Memory efficiency |
| Slow Query Count | `SHOW GLOBAL STATUS LIKE 'Slow_queries';` | ☐ | [DATE] | Performance issues |

### **8.2 Resource Utilization**
| **Resource** | **Command** | **Current** | **Peak** | **Average** | **Notes** |
|--------------|-------------|-------------|----------|-------------|-----------|
| CPU Usage | `top -p $(pgrep mysqld)` | ☐ % | ☐ % | ☐ % | Process specific |
| Memory Usage | `ps aux \| grep mysqld` | ☐ GB | ☐ GB | ☐ GB | MySQL process |
| Disk I/O | `iostat -x 1 5` | ☐ IOPS | ☐ IOPS | ☐ IOPS | Storage performance |
| Network I/O | `iftop -i eth0` | ☐ Mbps | ☐ Mbps | ☐ Mbps | Network utilization |

---

## **9. MIGRATION READINESS ASSESSMENT**

### **9.1 Readiness Checklist**
| **Category** | **Item** | **Status** | **Risk Level** | **Action Required** |
|--------------|----------|------------|----------------|-------------------|
| **OS** | CentOS upgrade compatibility | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |
| **MySQL** | Version upgrade path | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |
| **Applications** | Compatibility testing | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |
| **Data** | Backup verification | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |
| **Cluster** | HA setup readiness | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |
| **Team** | Skills assessment | ☐ Ready ☐ Issues | ☐ Low ☐ Medium ☐ High | ☐ |

### **9.2 Critical Issues Identified**
| **Issue #** | **Description** | **Impact** | **Recommended Solution** | **Priority** |
|-------------|-----------------|------------|--------------------------|--------------|
| 1 | | ☐ High ☐ Medium ☐ Low | | ☐ Critical ☐ High ☐ Medium |
| 2 | | ☐ High ☐ Medium ☐ Low | | ☐ Critical ☐ High ☐ Medium |
| 3 | | ☐ High ☐ Medium ☐ Low | | ☐ Critical ☐ High ☐ Medium |

---

## **10. SIGN-OFF & APPROVAL**

### **10.1 Audit Completion Status**
| **Server** | **Audit Complete** | **Issues Found** | **Approved for Migration** | **Notes** |
|------------|-------------------|------------------|----------------------------|-----------|
| SRV-01 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-02 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-03 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-04 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-05 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-06 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-07 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-08 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-09 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |
| SRV-10 | ☐ Yes ☐ No | ☐ None ☐ Minor ☐ Major | ☐ Yes ☐ No ☐ Conditional | |

### **10.2 Approval Sign-off**

**Vendor Support (Technical Lead):**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**DBA (Database Administrator):**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**Project Leader:**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

---

## **APPENDIX: Commands Reference**

### **Quick Audit Script Template**
```bash
#!/bin/bash
# Server Audit Script for MySQL Migration Project

echo "=== OS Information ==="
cat /etc/os-release
uname -a
uptime

echo "=== Hardware Resources ==="
nproc
free -h
df -h

echo "=== MySQL Status ==="
systemctl status mysqld
mysql --version
netstat -tlnp | grep 3306

echo "=== MySQL Variables ==="
mysql -e "SHOW VARIABLES LIKE 'version%';"
mysql -e "SHOW VARIABLES LIKE 'innodb_buffer_pool_size';"
mysql -e "SHOW VARIABLES LIKE 'max_connections';"

echo "=== Database Sizes ==="
mysql -e "SELECT schema_name as 'Database', ROUND(SUM(data_length + index_length) / 1024 / 1024, 2) as 'Size_MB' FROM information_schema.SCHEMATA s LEFT JOIN information_schema.TABLES t ON s.schema_name = t.table_schema GROUP BY schema_name;"

echo "=== Replication Status ==="
mysql -e "SHOW MASTER STATUS;"
mysql -e "SHOW SLAVE STATUS\G"
```

### **MySQL 8.4 Enterprise License Verification**
```sql
-- Check current connections (for license compliance)
SHOW GLOBAL STATUS LIKE 'Threads_connected';
SHOW GLOBAL STATUS LIKE 'Max_used_connections';

-- Check for Enterprise features
SHOW PLUGINS;
SELECT * FROM INFORMATION_SCHEMA.PLUGINS WHERE PLUGIN_NAME LIKE '%enterprise%';
```

**End of Checklist**  
**Total Expected Completion Time:** 2-3 hours per server  
**Recommended Team:** 2-3 people (Vendor Support lead + DBA + Infrastructure)
