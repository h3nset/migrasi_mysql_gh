# Subtask 1.7: Staging Environment Setup - Detail Checklist

**Project:** Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS  
**Deliverable:** Staging environment fully configured  
**PIC:** Infrastructure Team, Vendor Support  
**Dependency:** Dependent pada Subtask 1.3  

---

## **Staging Environment Setup & Configuration**

### **1. STAGING INFRASTRUCTURE REQUIREMENTS**

#### **1.1 Hardware & Resource Allocation**
| **Server** | **Role** | **CPU Cores** | **RAM (GB)** | **Storage (GB)** | **Network** | **Assigned Host** | **Status** |
|------------|----------|---------------|--------------|------------------|-------------|-------------------|----------|
| STG-01 | InnoDB Primary | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-02 | InnoDB Secondary 1 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-03 | InnoDB Secondary 2 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-04 | MySQL Router 1 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-05 | MySQL Router 2 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-06 | Application Server 1 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-07 | Application Server 2 | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-08 | Load Balancer | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-09 | Monitoring Server | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |
| STG-10 | Backup Server | [Cores] | [RAM] | [Storage] | [Network Config] | [Host IP/Name] | ☐ Allocated ☐ Pending |

**Resource Validation Commands:**
```bash
# Check hardware resources
lscpu
free -h
df -h
lsblk

# Network validation
ip addr show
netstat -tuln
ping -c 4 [target_hosts]
```

### **2. NETWORK CONFIGURATION**

#### **2.1 Network Topology Setup**
| **Component** | **IP Address** | **Port** | **Protocol** | **Firewall Rules** | **Load Balancer** | **SSL/TLS** | **Status** |
|---------------|---------------|----------|--------------|-------------------|-------------------|-------------|----------|
| Primary MySQL | [IP] | 3306 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Secondary MySQL 1 | [IP] | 3306 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Secondary MySQL 2 | [IP] | 3306 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Router Read/Write | [IP] | 6446 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Router Read Only | [IP] | 6447 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Router X Protocol | [IP] | 6448/6449 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Application HTTP | [IP] | 80/443 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |
| Monitoring | [IP] | 3000/9090 | TCP | [Rules] | [Config] | ☐ Yes ☐ No | ☐ Configured ☐ Pending |

**Network Configuration Commands:**
```bash
# Configure firewall rules
firewall-cmd --permanent --add-port=3306/tcp
firewall-cmd --permanent --add-port=6446-6449/tcp
firewall-cmd --reload

# Configure SSL certificates
openssl req -new -x509 -days 365 -nodes -out server-cert.pem -keyout server-key.pem

# Test network connectivity
telnet [mysql_host] 3306
telnet [router_host] 6446
```

### **3. OPERATING SYSTEM INSTALLATION**

#### **3.1 CentOS Installation Matrix**
| **Server** | **OS Version** | **Installation Method** | **Partitioning** | **Security Config** | **Updates** | **Hostname** | **Status** |
|------------|---------------|------------------------|------------------|-------------------|-------------|--------------|----------|
| STG-01 | CentOS [Version] | ☐ ISO ☐ PXE ☐ Clone | [Partition Scheme] | [Security Settings] | ☐ Applied ☐ Pending | [Hostname] | ☐ Complete ☐ Pending |
| STG-02 | CentOS [Version] | ☐ ISO ☐ PXE ☐ Clone | [Partition Scheme] | [Security Settings] | ☐ Applied ☐ Pending | [Hostname] | ☐ Complete ☐ Pending |
| STG-03 | CentOS [Version] | ☐ ISO ☐ PXE ☐ Clone | [Partition Scheme] | [Security Settings] | ☐ Applied ☐ Pending | [Hostname] | ☐ Complete ☐ Pending |
| STG-04 | CentOS [Version] | ☐ ISO ☐ PXE ☐ Clone | [Partition Scheme] | [Security Settings] | ☐ Applied ☐ Pending | [Hostname] | ☐ Complete ☐ Pending |
| STG-05 | CentOS [Version] | ☐ ISO ☐ PXE ☐ Clone | [Partition Scheme] | [Security Settings] | ☐ Applied ☐ Pending | [Hostname] | ☐ Complete ☐ Pending |

**OS Installation Commands:**
```bash
# System update
yum update -y
systemctl enable --now chronyd

# Security configuration
systemctl enable --now firewalld
setenforce 1

# User management
useradd mysql
usermod -aG wheel admin_user
```

### **4. MYSQL 8.4 ENTERPRISE INSTALLATION**

#### **4.1 MySQL Installation & Configuration**
| **Server** | **MySQL Version** | **Installation Source** | **Configuration File** | **Data Directory** | **Log Directory** | **Service Status** | **Status** |
|------------|-------------------|------------------------|----------------------|-------------------|------------------|-------------------|----------|
| STG-01 | MySQL 8.4 Enterprise | ☐ RPM ☐ Tarball ☐ Source | [Config Path] | [Data Path] | [Log Path] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| STG-02 | MySQL 8.4 Enterprise | ☐ RPM ☐ Tarball ☐ Source | [Config Path] | [Data Path] | [Log Path] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| STG-03 | MySQL 8.4 Enterprise | ☐ RPM ☐ Tarball ☐ Source | [Config Path] | [Data Path] | [Log Path] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |

**MySQL Installation Commands:**
```bash
# Install MySQL 8.4 Enterprise
yum install mysql-commercial-server mysql-commercial-client

# Configure MySQL
systemctl enable --now mysqld

# Secure installation
mysql_secure_installation

# Basic configuration
mysql -e "CREATE USER 'clusteradmin'@'%' IDENTIFIED BY 'password';"
mysql -e "GRANT ALL PRIVILEGES ON *.* TO 'clusteradmin'@'%' WITH GRANT OPTION;"
```

### **5. INNODB CLUSTER SETUP**

#### **5.1 InnoDB Cluster Configuration Matrix**
| **Node** | **Role** | **Cluster Name** | **Group Replication** | **GTID Mode** | **Binary Log** | **Cluster Status** | **Status** |
|----------|----------|------------------|----------------------|---------------|----------------|-------------------|----------|
| STG-01 | Primary | staging_cluster | ☐ Enabled ☐ Disabled | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | [Status] | ☐ Ready ☐ Pending |
| STG-02 | Secondary | staging_cluster | ☐ Enabled ☐ Disabled | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | [Status] | ☐ Ready ☐ Pending |
| STG-03 | Secondary | staging_cluster | ☐ Enabled ☐ Disabled | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | [Status] | ☐ Ready ☐ Pending |

**InnoDB Cluster Setup Commands:**
```javascript
// MySQL Shell cluster creation
mysqlsh --uri clusteradmin@stg-01:3306

// Check instance readiness
dba.checkInstanceConfiguration('clusteradmin@stg-01:3306')
dba.configureInstance('clusteradmin@stg-01:3306')

// Create cluster
var cluster = dba.createCluster('staging_cluster')

// Add instances
cluster.addInstance('clusteradmin@stg-02:3306')
cluster.addInstance('clusteradmin@stg-03:3306')

// Check cluster status
cluster.status()
```

### **6. MYSQL ROUTER CONFIGURATION**

#### **6.1 Router Setup & Configuration**
| **Router Node** | **Bootstrap Source** | **Configuration File** | **Read/Write Port** | **Read Only Port** | **X Protocol Ports** | **Service Status** | **Status** |
|-----------------|---------------------|----------------------|-------------------|------------------|---------------------|-------------------|----------|
| STG-04 | STG-01:3306 | [Config Path] | 6446 | 6447 | 6448/6449 | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| STG-05 | STG-01:3306 | [Config Path] | 6446 | 6447 | 6448/6449 | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |

**MySQL Router Setup Commands:**
```bash
# Install MySQL Router
yum install mysql-router

# Bootstrap router
mysqlrouter --bootstrap clusteradmin@stg-01:3306 --user=mysqlrouter --conf-use-sockets

# Start router service
systemctl enable --now mysqlrouter

# Test router connectivity
mysql -h stg-04 -P 6446 -u testuser -p -e "SELECT @@hostname, @@port;"
mysql -h stg-04 -P 6447 -u testuser -p -e "SELECT @@hostname, @@port;"
```

### **7. DATA MIGRATION & REPLICATION**

#### **7.1 Production Data Replication**
| **Database** | **Size (GB)** | **Migration Method** | **Sync Status** | **Data Consistency** | **Performance Impact** | **Completion Time** | **Status** |
|--------------|---------------|---------------------|-----------------|-------------------|----------------------|-------------------|----------|
| prod_db1 | [Size] | ☐ mysqldump ☐ Clone ☐ Backup/Restore | ☐ Synced ☐ Lag | ☐ Verified ☐ Pending | ☐ Low ☐ Medium ☐ High | [Time] | ☐ Complete ☐ Pending |
| prod_db2 | [Size] | ☐ mysqldump ☐ Clone ☐ Backup/Restore | ☐ Synced ☐ Lag | ☐ Verified ☐ Pending | ☐ Low ☐ Medium ☐ High | [Time] | ☐ Complete ☐ Pending |
| prod_db3 | [Size] | ☐ mysqldump ☐ Clone ☐ Backup/Restore | ☐ Synced ☐ Lag | ☐ Verified ☐ Pending | ☐ Low ☐ Medium ☐ High | [Time] | ☐ Complete ☐ Pending |
| prod_db4 | [Size] | ☐ mysqldump ☐ Clone ☐ Backup/Restore | ☐ Synced ☐ Lag | ☐ Verified ☐ Pending | ☐ Low ☐ Medium ☐ High | [Time] | ☐ Complete ☐ Pending |

**Data Migration Commands:**
```bash
# Create backup from production
mysqldump --single-transaction --routines --triggers --all-databases > prod_full_backup.sql

# Restore to staging
mysql -h stg-04 -P 6446 -u root -p < prod_full_backup.sql

# Clone method (if available)
mysql -h production_host -e "CLONE INSTANCE FOR REPLICATION FROM 'cloneuser'@'production_host':3306 IDENTIFIED BY 'password';"

# Verify data consistency
mysql -h stg-04 -P 6446 -e "CHECKSUM TABLE database.table;"
```

### **8. APPLICATION DEPLOYMENT**

#### **8.1 Application Stack Installation**
| **Application** | **Technology Stack** | **Version** | **Configuration** | **Database Connection** | **Dependencies** | **Service Status** | **Status** |
|-----------------|---------------------|-------------|-------------------|------------------------|------------------|-------------------|----------|
| Web App 1 | [Tech Stack] | [Version] | [Config File] | [Connection String] | [Dependencies] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Web App 2 | [Tech Stack] | [Version] | [Config File] | [Connection String] | [Dependencies] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| API Service | [Tech Stack] | [Version] | [Config File] | [Connection String] | [Dependencies] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Background Jobs | [Tech Stack] | [Version] | [Config File] | [Connection String] | [Dependencies] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Load Balancer | [Tech Stack] | [Version] | [Config File] | [Backend Servers] | [Dependencies] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |

**Application Deployment Commands:**
```bash
# Deploy application files
rsync -av /production/apps/ /staging/apps/

# Update database connections
sed -i 's/production_host/stg-04/g' /staging/apps/config/database.conf
sed -i 's/3306/6446/g' /staging/apps/config/database.conf

# Install dependencies
cd /staging/apps && npm install
pip install -r requirements.txt

# Start applications
systemctl enable --now webapp1 webapp2 api-service
```

### **9. MONITORING & ALERTING SETUP**

#### **9.1 Monitoring Stack Configuration**
| **Monitoring Tool** | **Version** | **Configuration** | **Data Sources** | **Alert Rules** | **Dashboard** | **Service Status** | **Status** |
|--------------------|-------------|-------------------|------------------|-----------------|---------------|-------------------|----------|
| MySQL Enterprise Monitor | [Version] | [Config] | MySQL Cluster | [Alert Rules] | [Dashboard URL] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Prometheus | [Version] | [Config] | MySQL Exporter | [Alert Rules] | [Dashboard URL] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Grafana | [Version] | [Config] | Prometheus | [Alert Rules] | [Dashboard URL] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |
| Alertmanager | [Version] | [Config] | Prometheus | [Alert Rules] | [Dashboard URL] | ☐ Running ☐ Stopped | ☐ Complete ☐ Pending |

**Monitoring Setup Commands:**
```bash
# Install Prometheus
wget https://github.com/prometheus/prometheus/releases/download/v2.x.x/prometheus-2.x.x.linux-amd64.tar.gz
tar xzf prometheus-2.x.x.linux-amd64.tar.gz

# Configure MySQL Exporter
./mysqld_exporter --config.my-cnf=.my.cnf

# Install Grafana
yum install grafana
systemctl enable --now grafana-server
```

### **10. SECURITY CONFIGURATION**

#### **10.1 Security Hardening Checklist**
| **Security Component** | **Configuration** | **Status** | **Validation** | **Notes** |
|------------------------|-------------------|------------|---------------|-----------|
| SSL/TLS Certificates | ☐ Generated ☐ Installed ☐ Verified | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |
| Firewall Rules | ☐ Configured ☐ Tested ☐ Documented | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |
| SELinux | ☐ Enforcing ☐ Permissive ☐ Disabled | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |
| User Accounts | ☐ Created ☐ Permissions Set ☐ Passwords Set | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |
| MySQL Enterprise Security | ☐ Audit ☐ Firewall ☐ Encryption | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |
| Backup Encryption | ☐ Configured ☐ Tested ☐ Keys Secured | ☐ Complete ☐ Pending | ☐ Pass ☐ Fail | [Notes] |

**Security Configuration Commands:**
```bash
# Generate SSL certificates
mysql_ssl_rsa_setup --datadir=/var/lib/mysql

# Configure firewall
firewall-cmd --permanent --add-rich-rule="rule family='ipv4' source address='10.0.0.0/8' port protocol='tcp' port='3306' accept"

# Configure MySQL Enterprise Security
mysql -e "INSTALL PLUGIN audit_log SONAME 'audit_log.so';"
mysql -e "INSTALL PLUGIN mysql_firewall SONAME 'firewall.so';"
```

### **11. TESTING & VALIDATION**

#### **11.1 Staging Environment Testing Matrix**
| **Test Category** | **Test Description** | **Expected Result** | **Actual Result** | **Pass/Fail** | **Issues** |
|-------------------|---------------------|-------------------|------------------|---------------|------------|
| Connectivity | MySQL Router connectivity test | All connections successful | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Failover | Primary node failover test | Automatic failover works | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Load Balancing | Read/write split validation | Queries routed correctly | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Application | Application functionality test | All features working | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Performance | Database performance test | Performance meets baseline | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Backup/Restore | Backup and restore test | Backup/restore successful | [Result] | ☐ Pass ☐ Fail | [Issues] |
| Monitoring | Monitoring and alerting test | Alerts trigger correctly | [Result] | ☐ Pass ☐ Fail | [Issues] |

**Testing Commands:**
```bash
# Connectivity test
mysql -h stg-04 -P 6446 -e "SELECT 'Read/Write Connection' as test;"
mysql -h stg-04 -P 6447 -e "SELECT 'Read Only Connection' as test;"

# Failover test
mysqlsh --uri clusteradmin@stg-01:3306 -e "dba.getCluster().status()"
systemctl stop mysqld  # On primary node

# Performance test
sysbench oltp_read_write --mysql-host=stg-04 --mysql-port=6446 --mysql-user=testuser --mysql-password=password --tables=10 --table_size=100000 run
```

### **12. DOCUMENTATION & HANDOVER**

#### **12.1 Staging Environment Documentation**
| **Document Type** | **Document Name** | **Version** | **Author** | **Review Status** | **Location** | **Status** |
|-------------------|-------------------|-------------|------------|-------------------|--------------|----------|
| Architecture Diagram | Staging Topology Diagram | v1.0 | [Author] | ☐ Reviewed ☐ Pending | [Location] | ☐ Complete ☐ Pending |
| Configuration Guide | MySQL Cluster Config Guide | v1.0 | [Author] | ☐ Reviewed ☐ Pending | [Location] | ☐ Complete ☐ Pending |
| Operations Manual | Staging Ops Manual | v1.0 | [Author] | ☐ Reviewed ☐ Pending | [Location] | ☐ Complete ☐ Pending |
| Test Results | Staging Test Report | v1.0 | [Author] | ☐ Reviewed ☐ Pending | [Location] | ☐ Complete ☐ Pending |
| Troubleshooting Guide | Common Issues Guide | v1.0 | [Author] | ☐ Reviewed ☐ Pending | [Location] | ☐ Complete ☐ Pending |

### **13. SIGN-OFF & APPROVAL**

#### **13.1 Staging Environment Approval Matrix**
| **Role** | **Name** | **Review Date** | **Approval Status** | **Comments** | **Signature** |
|----------|----------|-----------------|-------------------|--------------|---------------|
| Infrastructure Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| DBA Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| Development Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| DevOps Lead | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| Project Manager | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |
| Vendor Support | [Name] | [Date] | ☐ Approved ☐ Rejected ☐ Pending | [Comments] | [Signature] |

**Final Staging Environment Status:** ☐ Ready for Production Migration ☐ Needs Revision ☐ Not Ready  
**Approval Date:** ___________  
**Production Migration Go Date:** ___________  

---

**Catatan Penting:**
1. **Staging environment harus merefleksikan produksi 1:1** dalam hal konfigurasi, data, dan aplikasi
2. **Semua testing** harus dilakukan dan documented sebelum production migration
3. **Monitoring dan alerting** harus fully functional sebelum handover
4. **Security hardening** harus complete dan verified
5. **Team training** pada staging environment untuk familiarisasi dengan HA setup
