# Subtask 1.3: InnoDB Cluster & MySQL Router Topology Audit - Detail Checklist

**Project:** Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS  
**Deliverable:** Cluster topology diagram dan konfigurasi router  
**PIC:** Vendor Support, DBA  
**Dependency:** Independent  

---

## **InnoDB Cluster & MySQL Router Topology Assessment**

### **1. CURRENT REPLICATION TOPOLOGY AUDIT**

#### **1.1 Existing Replication Status**
| **Server** | **Hostname/IP** | **Role** | **Replication Status** | **Lag (seconds)** | **GTID Mode** | **Binary Log** | **Relay Log** |
|------------|-----------------|----------|----------------------|-------------------|---------------|----------------|---------------|
| SRV-01 | [HOSTNAME/IP] | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped ☐ Error | ☐ | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | ☐ Enabled ☐ Disabled |
| SRV-02 | [HOSTNAME/IP] | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped ☐ Error | ☐ | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | ☐ Enabled ☐ Disabled |
| SRV-03 | [HOSTNAME/IP] | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped ☐ Error | ☐ | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | ☐ Enabled ☐ Disabled |
| SRV-04 | [HOSTNAME/IP] | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped ☐ Error | ☐ | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | ☐ Enabled ☐ Disabled |
| SRV-05 | [HOSTNAME/IP] | ☐ Master ☐ Slave ☐ Standalone | ☐ Running ☐ Stopped ☐ Error | ☐ | ☐ ON ☐ OFF | ☐ Enabled ☐ Disabled | ☐ Enabled ☐ Disabled |

**Assessment Commands:**
```sql
-- Check master status
SHOW MASTER STATUS;

-- Check slave status  
SHOW SLAVE STATUS\G

-- Check GTID configuration
SHOW VARIABLES LIKE 'gtid_mode';
SHOW VARIABLES LIKE 'enforce_gtid_consistency';
SHOW VARIABLES LIKE 'server_id';

-- Check binary logging
SHOW VARIABLES LIKE 'log_bin';
SHOW VARIABLES LIKE 'log_slave_updates';
```

#### **1.2 Replication Configuration Analysis**
| **Configuration Item** | **Current Value** | **InnoDB Cluster Requirement** | **Compliant** | **Action Required** |
|------------------------|-------------------|--------------------------------|---------------|-------------------|
| `server_id` | ☐ | Unique per server | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `gtid_mode` | ☐ | `ON` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `enforce_gtid_consistency` | ☐ | `ON` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `master_info_repository` | ☐ | `TABLE` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `relay_log_info_repository` | ☐ | `TABLE` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `log_slave_updates` | ☐ | `ON` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `log_bin` | ☐ | `ON` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `binlog_format` | ☐ | `ROW` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |
| `binlog_checksum` | ☐ | `NONE` | ☐ Yes ☐ No | ☐ Configure ☐ Modify ☐ No Action |

---

### **2. INNODB CLUSTER READINESS ASSESSMENT**

#### **2.1 Network Prerequisites**
| **Server** | **Hostname Resolution** | **Port 3306** | **Port 33061 (X Protocol)** | **Port 33060 (X Plugin)** | **Firewall Rules** | **Notes** |
|------------|------------------------|---------------|----------------------------|---------------------------|-------------------|-----------|
| SRV-01 | ☐ Forward ☐ Reverse | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Configured ☐ Missing | Primary candidate |
| SRV-02 | ☐ Forward ☐ Reverse | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Configured ☐ Missing | Secondary candidate |
| SRV-03 | ☐ Forward ☐ Reverse | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Configured ☐ Missing | Secondary candidate |
| SRV-04 | ☐ Forward ☐ Reverse | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Configured ☐ Missing | Router candidate |
| SRV-05 | ☐ Forward ☐ Reverse | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Open ☐ Blocked | ☐ Configured ☐ Missing | Router candidate |

**Network Test Commands:**
```bash
# Test hostname resolution
nslookup [SERVER_HOSTNAME]
dig [SERVER_HOSTNAME]
dig -x [SERVER_IP]

# Test port connectivity
telnet [SERVER_IP] 3306
telnet [SERVER_IP] 33061
telnet [SERVER_IP] 33060

# Check firewall
firewall-cmd --list-ports
iptables -L -n
```

#### **2.2 MySQL Configuration Requirements**
| **Parameter** | **Required Value** | **SRV-01** | **SRV-02** | **SRV-03** | **Action Required** |
|---------------|-------------------|------------|------------|------------|-------------------|
| `disabled_storage_engines` | `MyISAM,BLACKHOLE,FEDERATED,ARCHIVE,MEMORY` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `server_id` | Unique (e.g., 1, 2, 3) | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `log_bin` | `ON` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `binlog_format` | `ROW` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `gtid_mode` | `ON` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `enforce_gtid_consistency` | `ON` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `master_info_repository` | `TABLE` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `relay_log_info_repository` | `TABLE` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `transaction_write_set_extraction` | `XXHASH64` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `slave_parallel_workers` | `0` (single-threaded) | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `slave_preserve_commit_order` | `ON` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |
| `slave_parallel_type` | `LOGICAL_CLOCK` | ☐ | ☐ | ☐ | ☐ Configure ☐ OK |

---

### **3. TARGET INNODB CLUSTER TOPOLOGY DESIGN**

#### **3.1 Cluster Architecture Plan**
| **Environment** | **Primary Node** | **Secondary Node 1** | **Secondary Node 2** | **Router Node 1** | **Router Node 2** |
|-----------------|------------------|---------------------|---------------------|-------------------|-------------------|
| **Production** | SRV-01 | SRV-02 | SRV-10 | SRV-03 | SRV-06 |
| **Staging** | SRV-04 | SRV-05 | - | SRV-06 | - |
| **Development** | SRV-07 | SRV-08 | - | SRV-09 | - |

#### **3.2 Cluster Specifications**
| **Cluster Name** | **Environment** | **Node Count** | **Quorum** | **Failover Mode** | **Backup Node** |
|------------------|-----------------|----------------|------------|-------------------|-----------------|
| prod-cluster | Production | 3 | 2/3 | ☐ Automatic ☐ Manual | SRV-10 |
| staging-cluster | Staging | 2 | 1/2 | ☐ Automatic ☐ Manual | - |
| dev-cluster | Development | 2 | 1/2 | ☐ Automatic ☐ Manual | - |

#### **3.3 Network Topology Diagram Template**
```
Production Cluster:
┌─────────────────────────────────────────────────────────────┐
│                    Production Environment                   │
├─────────────────────────────────────────────────────────────┤
│  Application Layer                                          │
│  ┌──────┐  ┌──────┐  ┌──────┐                               │
│  │App-1 │  │App-2 │  │App-3 │                               │
│  └──┬───┘  └──┬───┘  └──┬───┘                               │
│     │         │         │                                   │
├─────┼─────────┼─────────┼───────────────────────────────────┤
│     │         │         │    Router Layer                   │
│  ┌──▼─────────▼─────────▼──┐  ┌─────────────────────────┐   │
│  │    MySQL Router-1       │  │    MySQL Router-2       │   │
│  │      (SRV-03)           │  │      (SRV-06)           │   │
│  │  R/W: 6446              │  │  R/W: 6446              │   │
│  │  R/O: 6447              │  │  R/O: 6447              │   │
│  └──┬──────────────────────┘  └───────────────────────┬─┘   │
│     │                                                 │     │
├─────┼─────────────────────────────────────────────────┼─────┤
│     │               InnoDB Cluster                    │     │
│  ┌──▼────┐      ┌─────────┐      ┌─────────┐      ┌───▼───┐ │
│  │Primary│◄────►│Secondary│◄────►│Secondary│◄────►│Backup │ │
│  │SRV-01 │      │ SRV-02  │      │ SRV-10  │      │       │ │
│  │:3306  │      │ :3306   │      │ :3306   │      │       │ │
│  └───────┘      └─────────┘      └─────────┘      └───────┘ │
└─────────────────────────────────────────────────────────────┘

Legend:
◄────► : Group Replication
──────► : MySQL Router Connection
```

---

### **4. MYSQL ROUTER CONFIGURATION PLAN**

#### **4.1 Router Node Specifications**
| **Router Node** | **Hostname/IP** | **Environment** | **Port Config** | **Bootstrap Cluster** | **HA Pair** |
|-----------------|-----------------|-----------------|-----------------|----------------------|-------------|
| Router-01 | SRV-03 | Production | R/W:6446, R/O:6447, X:64460, XRO:64470 | prod-cluster | Router-02 |
| Router-02 | SRV-06 | Production | R/W:6446, R/O:6447, X:64460, XRO:64470 | prod-cluster | Router-01 |
| Router-03 | SRV-06 | Staging | R/W:6448, R/O:6449, X:64480, XRO:64490 | staging-cluster | - |
| Router-04 | SRV-09 | Development | R/W:6450, R/O:6451, X:64500, XRO:64510 | dev-cluster | - |

#### **4.2 Router Configuration Template**
```ini
# MySQL Router Configuration Template
[DEFAULT]
name = mysql_router_prod
user = mysqlrouter
connect_timeout = 15
read_timeout = 30

[logger]
level = INFO

[metadata_cache]
cluster_name = prod-cluster
router_id = [ROUTER_ID]
bootstrap_server_addresses = SRV-01:3306,SRV-02:3306,SRV-10:3306
user = mysql_router_user
metadata_cluster = prod-cluster
ttl = 0.5
auth_cache_ttl = -1
auth_cache_refresh_interval = 2

[routing:prod_rw]
bind_address = 0.0.0.0
bind_port = 6446
destinations = metadata-cache://prod-cluster/default?role=PRIMARY
routing_strategy = first-available
protocol = classic

[routing:prod_ro]
bind_address = 0.0.0.0
bind_port = 6447
destinations = metadata-cache://prod-cluster/default?role=SECONDARY
routing_strategy = round-robin-with-fallback
protocol = classic

[routing:prod_x_rw]
bind_address = 0.0.0.0
bind_port = 64460
destinations = metadata-cache://prod-cluster/default?role=PRIMARY
routing_strategy = first-available
protocol = x

[routing:prod_x_ro]
bind_address = 0.0.0.0
bind_port = 64470
destinations = metadata-cache://prod-cluster/default?role=SECONDARY
routing_strategy = round-robin-with-fallback
protocol = x
```

---

### **5. CLUSTER PREREQUISITES CHECKLIST**

#### **5.1 MySQL Shell Requirements**
| **Server** | **MySQL Shell Installed** | **Version** | **Python Support** | **Admin User Created** | **Status** |
|------------|---------------------------|-------------|-------------------|----------------------|------------|
| SRV-01 | ☐ Yes ☐ No | ☐ 8.4.x | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Ready ☐ Config Needed |
| SRV-02 | ☐ Yes ☐ No | ☐ 8.4.x | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Ready ☐ Config Needed |
| SRV-03 | ☐ Yes ☐ No | ☐ 8.4.x | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Ready ☐ Config Needed |
| SRV-10 | ☐ Yes ☐ No | ☐ 8.4.x | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ Ready ☐ Config Needed |

**Installation Commands:**
```bash
# Install MySQL Shell
wget https://dev.mysql.com/get/Downloads/MySQL-Shell/mysql-shell-8.4.0-1.el7.x86_64.rpm
sudo rpm -ivh mysql-shell-8.4.0-1.el7.x86_64.rpm

# Verify installation
mysqlsh --version
```

#### **5.2 Cluster Admin User Setup**
| **User Account** | **Privileges** | **Created On** | **Replication User** | **Router User** | **Notes** |
|------------------|----------------|---------------|---------------------|-----------------|---------|
| `clusteradmin` | ALL PRIVILEGES | ☐ All Nodes | - | - | Cluster management |
| `repl_user` | REPLICATION SLAVE | ☐ All Nodes | ☐ Yes | - | Internal replication |
| `mysql_router_user` | SELECT, EXECUTE | ☐ All Nodes | - | ☐ Yes | Router metadata access |

**User Creation Commands:**
```sql
-- Cluster admin user
CREATE USER 'clusteradmin'@'%' IDENTIFIED BY 'ClusterPass123!';
GRANT ALL PRIVILEGES ON *.* TO 'clusteradmin'@'%' WITH GRANT OPTION;
GRANT RELOAD, SHUTDOWN, PROCESS, FILE, SUPER, REPLICATION SLAVE, REPLICATION CLIENT, CREATE USER ON *.* TO 'clusteradmin'@'%';

-- Replication user (will be created automatically by cluster setup)
-- CREATE USER 'mysql_innodb_cluster_[server_id]'@'%' IDENTIFIED BY 'password';

-- Router user
CREATE USER 'mysql_router_user'@'%' IDENTIFIED BY 'RouterPass123!';
GRANT SELECT ON mysql_innodb_cluster_metadata.* TO 'mysql_router_user'@'%';
GRANT SELECT ON performance_schema.replication_group_members TO 'mysql_router_user'@'%';
GRANT SELECT ON performance_schema.replication_group_member_stats TO 'mysql_router_user'@'%';
GRANT SELECT ON performance_schema.global_variables TO 'mysql_router_user'@'%';
```

---

### **6. CLUSTER DEPLOYMENT PLAN**

#### **6.1 Bootstrap Sequence**
| **Step** | **Action** | **Target Server** | **Command/Method** | **Validation** | **Rollback** |
|----------|------------|------------------|------------------|---------------|--------------|
| 1 | Create cluster | SRV-01 | `dba.createCluster('prod-cluster')` | `cluster.status()` | `dba.dropMetadataSchema()` |
| 2 | Add instance 1 | SRV-02 | `cluster.addInstance('srv-02:3306')` | Check member status | `cluster.removeInstance()` |
| 3 | Add instance 2 | SRV-10 | `cluster.addInstance('srv-10:3306')` | Check member status | `cluster.removeInstance()` |
| 4 | Configure router 1 | SRV-03 | `mysqlrouter --bootstrap` | Test connectivity | Stop router service |
| 5 | Configure router 2 | SRV-06 | `mysqlrouter --bootstrap` | Test connectivity | Stop router service |

#### **6.2 Bootstrap Commands Template**
```javascript
// MySQL Shell commands for cluster creation

// Step 1: Check instance configuration
dba.checkInstanceConfiguration('clusteradmin@srv-01:3306')
dba.configureInstance('clusteradmin@srv-01:3306')

// Step 2: Create cluster
\c clusteradmin@srv-01:3306
var cluster = dba.createCluster('prod-cluster')

// Step 3: Add instances
cluster.addInstance('clusteradmin@srv-02:3306')
cluster.addInstance('clusteradmin@srv-10:3306')

// Step 4: Check cluster status
cluster.status()

// Step 5: Describe cluster
cluster.describe()
```

```bash
# MySQL Router bootstrap commands
sudo mysqlrouter --bootstrap clusteradmin@srv-01:3306 --user=mysqlrouter --force

# Start router
sudo systemctl start mysqlrouter
sudo systemctl enable mysqlrouter

# Check router status
sudo systemctl status mysqlrouter
mysqlrouter --help --config
```

---

### **7. FAILOVER & HIGH AVAILABILITY TESTING**

#### **7.1 Failover Test Scenarios**
| **Test Scenario** | **Method** | **Expected Behavior** | **Test Status** | **Result** | **Recovery Time** |
|-------------------|------------|----------------------|-----------------|------------|------------------|
| Primary node failure | Stop MySQL on primary | Auto-elect new primary | ☐ Planned ☐ Executed | ☐ Pass ☐ Fail | ☐ seconds |
| Network partition | Block network to primary | Cluster continues with majority | ☐ Planned ☐ Executed | ☐ Pass ☐ Fail | ☐ seconds |
| Router failure | Stop router service | Apps connect to backup router | ☐ Planned ☐ Executed | ☐ Pass ☐ Fail | ☐ seconds |
| Split-brain scenario | Isolate nodes | Prevent data inconsistency | ☐ Planned ☐ Executed | ☐ Pass ☐ Fail | ☐ minutes |
| Full cluster restart | Stop all nodes, restart sequence | Cluster reforms correctly | ☐ Planned ☐ Executed | ☐ Pass ☐ Fail | ☐ minutes |

#### **7.2 Performance Baseline Testing**
| **Metric** | **Standalone** | **Cluster (3-node)** | **With Router** | **Performance Impact** | **Acceptable** |
|------------|----------------|---------------------|-----------------|----------------------|----------------|
| SELECT QPS | ☐ | ☐ | ☐ | ☐ % | ☐ Yes ☐ No |
| INSERT TPS | ☐ | ☐ | ☐ | ☐ % | ☐ Yes ☐ No |
| Update TPS | ☐ | ☐ | ☐ | ☐ % | ☐ Yes ☐ No |
| Connection Time | ☐ ms | ☐ ms | ☐ ms | ☐ % | ☐ Yes ☐ No |
| Replication Lag | N/A | ☐ ms | ☐ ms | ☐ ms | ☐ Yes ☐ No |

---

### **8. MONITORING & ALERTING SETUP**

#### **8.1 Cluster Health Monitoring**
| **Metric** | **Monitoring Method** | **Alert Threshold** | **Action Required** | **Responsible Team** |
|------------|----------------------|-------------------|-------------------|---------------------|
| Cluster Status | `cluster.status()` | Member OFFLINE | ☐ Investigate ☐ Auto-recover | ☐ DBA ☐ DevOps |
| Replication Lag | Performance Schema | > 5 seconds | ☐ Investigate ☐ Alert | ☐ DBA ☐ Monitor |
| Router Connectivity | Connection test | Connection failure | ☐ Failover ☐ Restart | ☐ DevOps ☐ Auto |
| Quorum Status | Group members count | < Majority | ☐ Manual intervention | ☐ DBA ☐ Escalate |
| Certificate Expiry | SSL certificate check | < 30 days | ☐ Renew certificates | ☐ Security ☐ DBA |

#### **8.2 Monitoring Queries**
```sql
-- Cluster member status
SELECT 
    MEMBER_ID,
    MEMBER_HOST,
    MEMBER_PORT,
    MEMBER_STATE,
    MEMBER_ROLE
FROM performance_schema.replication_group_members;

-- Replication lag
SELECT 
    CHANNEL_NAME,
    SERVICE_STATE,
    COUNT_TRANSACTIONS_IN_QUEUE as QUEUE_SIZE,
    COUNT_TRANSACTIONS_CHECKED as APPLIED_TRANSACTIONS
FROM performance_schema.replication_applier_status_by_coordinator;

-- Group replication status
SELECT * FROM performance_schema.replication_group_member_stats;
```

---

### **9. MIGRATION IMPACT ASSESSMENT**

#### **9.1 Application Connection String Changes**
| **Application** | **Current Connection** | **New Connection (Router)** | **Code Changes Required** | **Test Status** |
|-----------------|----------------------|----------------------------|--------------------------|-----------------|
| [APP_NAME_1] | `srv-01:3306` | `router-01:6446,router-02:6446` | ☐ Minimal ☐ Significant | ☐ Tested ☐ Pending |
| [APP_NAME_2] | `srv-02:3306` | `router-01:6447,router-02:6447` | ☐ Minimal ☐ Significant | ☐ Tested ☐ Pending |
| [APP_NAME_3] | Multiple servers | `router-01:6446,router-02:6446` | ☐ Minimal ☐ Significant | ☐ Tested ☐ Pending |

#### **9.2 Operational Changes**
| **Operation** | **Current Method** | **Cluster Method** | **Training Required** | **Documentation Updated** |
|---------------|-------------------|-------------------|---------------------|--------------------------|
| Backup | mysqldump + cron | Enterprise Backup + cluster-aware | ☐ Yes ☐ No | ☐ Yes ☐ No |
| Failover | Manual DNS change | Automatic via router | ☐ Yes ☐ No | ☐ Yes ☐ No |
| Maintenance | Stop/start single server | Rolling maintenance | ☐ Yes ☐ No | ☐ Yes ☐ No |
| Monitoring | Single server metrics | Cluster + router metrics | ☐ Yes ☐ No | ☐ Yes ☐ No |
| Scaling | Add read replicas | Add cluster members | ☐ Yes ☐ No | ☐ Yes ☐ No |

---

### **10. RISK ASSESSMENT & MITIGATION**

#### **10.1 Cluster-Specific Risks**
| **Risk** | **Impact** | **Probability** | **Mitigation Strategy** | **Owner** | **Status** |
|----------|------------|-----------------|------------------------|-----------|------------|
| Split-brain scenario | ☐ High ☐ Medium ☐ Low | ☐ High ☐ Medium ☐ Low | Proper quorum setup (odd nodes) | ☐ Vendor ☐ DBA | ☐ Planned ☐ Implemented |
| Router single point of failure | ☐ High ☐ Medium ☐ Low | ☐ High ☐ Medium ☐ Low | Deploy multiple routers | ☐ DevOps ☐ Infrastructure | ☐ Planned ☐ Implemented |
| Network partition | ☐ High ☐ Medium ☐ Low | ☐ High ☐ Medium ☐ Low | Network redundancy, monitoring | ☐ Network ☐ Infrastructure | ☐ Planned ☐ Implemented |
| Performance degradation | ☐ High ☐ Medium ☐ Low | ☐ High ☐ Medium ☐ Low | Baseline testing, tuning | ☐ DBA ☐ Vendor | ☐ Planned ☐ Implemented |
| Cluster bootstrap failure | ☐ High ☐ Medium ☐ Low | ☐ High ☐ Medium ☐ Low | Thorough testing, rollback plan | ☐ Vendor ☐ DBA | ☐ Planned ☐ Implemented |

#### **10.2 Rollback Strategy**
| **Scenario** | **Rollback Method** | **Time Required** | **Data Loss Risk** | **Validation Steps** |
|--------------|-------------------|------------------|-------------------|---------------------|
| Cluster creation fails | Drop metadata, restore replication | ☐ hours | ☐ None ☐ Minimal ☐ Significant | Connection tests, data consistency |
| Router configuration issues | Revert to direct connections | ☐ minutes | ☐ None | Application connectivity |
| Performance issues | Switch back to master-slave | ☐ hours | ☐ None ☐ Minimal ☐ Significant | Performance benchmarks |
| Node addition fails | Remove problematic instance | ☐ minutes | ☐ None | Cluster status check |

---

### **11. DELIVERABLE ARTIFACTS**

#### **11.1 Documentation Deliverables**
| **Document** | **Content** | **Owner** | **Status** | **Review Required** |
|--------------|-------------|-----------|------------|-------------------|
| Cluster Topology Diagram | Network, server, service layout | ☐ Vendor ☐ DBA | ☐ Draft ☐ Final | ☐ Yes ☐ No |
| Router Configuration Files | Complete router configs per environment | ☐ Vendor ☐ DevOps | ☐ Draft ☐ Final | ☐ Yes ☐ No |
| Bootstrap Procedures | Step-by-step cluster creation | ☐ Vendor ☐ DBA | ☐ Draft ☐ Final | ☐ Yes ☐ No |
| Failover Testing Report | Test results and procedures | ☐ Vendor ☐ DBA | ☐ Draft ☐ Final | ☐ Yes ☐ No |
| Monitoring Setup Guide | Cluster health monitoring implementation | ☐ Vendor ☐ DevOps | ☐ Draft ☐ Final | ☐ Yes ☐ No |
| Application Migration Guide | Connection string changes, code updates | ☐ Developer ☐ Vendor | ☐ Draft ☐ Final | ☐ Yes ☐ No |

#### **11.2 Configuration Artifacts**
| **Artifact** | **Description** | **Environment** | **Status** | **Backup Location** |
|--------------|-----------------|-----------------|------------|-------------------|
| my.cnf templates | Cluster-ready MySQL configs | All | ☐ Ready ☐ Draft | ☐ |
| Router config files | Production-ready router configs | All | ☐ Ready ☐ Draft | ☐ |
| Bootstrap scripts | Automated cluster creation scripts | All | ☐ Ready ☐ Draft | ☐ |
| Monitoring scripts | Health check and alerting scripts | All | ☐ Ready ☐ Draft | ☐ |
| Firewall rules | Required port configurations | All | ☐ Ready ☐ Draft | ☐ |

---

### **12. SIGN-OFF & APPROVAL**

#### **12.1 Topology Assessment Summary**
- **Current Architecture:** ☐ Standalone ☐ Master-Slave ☐ Master-Master ☐ Other
- **Target Architecture:** InnoDB Cluster (3-node) + MySQL Router (2-node)
- **Complexity Level:** ☐ Low ☐ Medium ☐ High
- **Estimated Migration Time:** ☐ days
- **Risk Level:** ☐ Low ☐ Medium ☐ High

#### **12.2 Readiness Assessment**
- **Infrastructure Ready:** ☐ Yes ☐ No ☐ Partial
- **Network Configured:** ☐ Yes ☐ No ☐ Partial
- **Prerequisites Met:** ☐ Yes ☐ No ☐ Partial
- **Team Trained:** ☐ Yes ☐ No ☐ Partial

#### **12.3 Approval for Next Phase**
**Vendor Support (Technical Lead):**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**DBA:**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**Infrastructure Team Lead:**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

---

## **APPENDIX: Technical References**

### **Quick Reference Commands**
```bash
# MySQL Shell cluster management
mysqlsh --uri clusteradmin@srv-01:3306
dba.getCluster('prod-cluster').status()
dba.getCluster('prod-cluster').describe()

# Router management
sudo systemctl status mysqlrouter
sudo mysqlrouter --config=/etc/mysqlrouter/mysqlrouter.conf --verbose

# Network testing
for port in 3306 33060 33061; do
  nc -zv srv-01 $port
done

# Performance testing
mysqlslap --user=test --password=test --host=router-01 --port=6446 --create-schema=test --query="SELECT 1;" --concurrency=10 --iterations=100
```

### **Troubleshooting Common Issues**
```sql
-- Check group replication status
SELECT * FROM performance_schema.replication_group_members;
SELECT * FROM performance_schema.replication_group_member_stats;

-- Check router connectivity
SELECT * FROM performance_schema.host_cache;
SHOW PROCESSLIST;

-- Reset group replication (emergency)
STOP GROUP_REPLICATION;
RESET MASTER;
RESET SLAVE ALL;
SET GLOBAL group_replication_bootstrap_group=ON;
START GROUP_REPLICATION;
SET GLOBAL group_replication_bootstrap_group=OFF;
```

**End of InnoDB Cluster Topology Checklist**  
**Estimated Assessment Time:** 1-2 weeks  
**Critical Success Factor:** High Priority
