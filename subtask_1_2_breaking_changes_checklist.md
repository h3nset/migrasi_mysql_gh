# Subtask 1.2: Breaking Changes MySQL 8.4 Enterprise - Detail Checklist

**Project:** Migrasi & Upgrade MySQL 5.6/5.7 ke 8.4 Enterprise + Upgrade OS ke CentOS  
**Deliverable:** Breaking changes checklist dan impact analysis  
**PIC:** Vendor Support, Developer Supervisor  
**Dependency:** Independent  

---

## **MySQL 8.4 Enterprise Breaking Changes Assessment**

### **1. AUTHENTICATION PLUGIN CHANGES**

#### **1.1 Default Authentication Plugin**
| **Item** | **MySQL 5.6/5.7** | **MySQL 8.4** | **Impact Level** | **Check Status** | **Mitigation Required** | **Notes** |
|----------|-------------------|----------------|------------------|------------------|-------------------------|---------|
| Default Plugin | `mysql_native_password` | `caching_sha2_password` | ☐ High ☐ Medium ☐ Low | ☐ Compatible ☐ Issues | ☐ Yes ☐ No | Client compatibility |
| SSL Requirement | Optional | Required for `caching_sha2_password` | ☐ High ☐ Medium ☐ Low | ☐ Compatible ☐ Issues | ☐ Yes ☐ No | Network encryption |
| Client Libraries | Legacy support | Updated required | ☐ High ☐ Medium ☐ Low | ☐ Compatible ☐ Issues | ☐ Yes ☐ No | Application updates |

**Assessment Commands:**
```sql
-- Check current user authentication methods
SELECT user, host, plugin FROM mysql.user WHERE user NOT IN ('mysql.sys', 'mysql.session');

-- Check client connections
SHOW VARIABLES LIKE 'default_authentication_plugin';
SHOW VARIABLES LIKE 'have_ssl';
```

#### **1.2 User Account Migration Plan**
| **Username** | **Current Plugin** | **New Plugin** | **SSL Required** | **Application Impact** | **Migration Plan** |
|--------------|-------------------|----------------|------------------|----------------------|-------------------|
| [APP_USER_1] | ☐ | ☐ `caching_sha2_password` ☐ `mysql_native_password` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ |
| [APP_USER_2] | ☐ | ☐ `caching_sha2_password` ☐ `mysql_native_password` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ |
| [APP_USER_3] | ☐ | ☐ `caching_sha2_password` ☐ `mysql_native_password` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ |

---

### **2. SQL MODE & SYNTAX CHANGES**

#### **2.1 SQL Mode Strictness**
| **Setting** | **MySQL 5.6/5.7** | **MySQL 8.4** | **Impact Assessment** | **Current Value** | **Action Required** |
|-------------|-------------------|----------------|----------------------|-------------------|-------------------|
| Default SQL Mode | `''` or lenient | `ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION` | ☐ High ☐ Medium ☐ Low | ☐ | ☐ Code Review ☐ Config Change |
| GROUP BY Behavior | Implicit grouping allowed | Must specify all non-aggregate columns | ☐ High ☐ Medium ☐ Low | ☐ | ☐ Query Review ☐ No Action |
| Zero Dates | `0000-00-00` allowed | Rejected by default | ☐ High ☐ Medium ☐ Low | ☐ | ☐ Data Cleanup ☐ No Action |
| Division by Zero | Warning | Error by default | ☐ High ☐ Medium ☐ Low | ☐ | ☐ Code Review ☐ No Action |

**Assessment Commands:**
```sql
-- Check current SQL mode
SELECT @@sql_mode;

-- Find problematic GROUP BY queries
SELECT SCHEMA_NAME, VIEW_DEFINITION 
FROM INFORMATION_SCHEMA.VIEWS 
WHERE VIEW_DEFINITION REGEXP 'GROUP BY.*[^,]$';

-- Find zero dates
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE DATA_TYPE IN ('date', 'datetime', 'timestamp');
```

#### **2.2 Query Compatibility Assessment**
| **Query Pattern** | **Example Issue** | **Found in Code** | **Impact Level** | **Fix Required** | **Test Status** |
|-------------------|-------------------|-------------------|------------------|------------------|-----------------|
| Non-aggregate in GROUP BY | `SELECT name, COUNT(*) FROM users GROUP BY id` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No | ☐ Tested ☐ Pending |
| Zero dates | `INSERT INTO table VALUES ('0000-00-00')` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No | ☐ Tested ☐ Pending |
| Division by zero | `SELECT 1/0` without error handling | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No | ☐ Tested ☐ Pending |
| Empty string defaults | `DEFAULT ''` for NOT NULL columns | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No | ☐ Tested ☐ Pending |

---

### **3. RESERVED KEYWORDS**

#### **3.1 New Reserved Keywords in MySQL 8.4**
| **Keyword** | **Usage Context** | **Found in Schema** | **Found in Code** | **Table/Column** | **Mitigation Strategy** |
|-------------|-------------------|-------------------|-------------------|------------------|------------------------|
| `RANK` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `DENSE_RANK` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `FIRST_VALUE` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `LAST_VALUE` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `LAG` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `LEAD` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `NTH_VALUE` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `NTILE` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `PERCENT_RANK` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |
| `ROW_NUMBER` | Window function | ☐ Yes ☐ No | ☐ Yes ☐ No | ☐ | ☐ Backticks ☐ Rename ☐ No Action |

**Detection Commands:**
```sql
-- Check for reserved keywords in table names
SELECT TABLE_SCHEMA, TABLE_NAME 
FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_NAME REGEXP '^(RANK|DENSE_RANK|FIRST_VALUE|LAST_VALUE|LAG|LEAD|NTH_VALUE|NTILE|PERCENT_RANK|ROW_NUMBER)$';

-- Check for reserved keywords in column names
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE COLUMN_NAME REGEXP '^(RANK|DENSE_RANK|FIRST_VALUE|LAST_VALUE|LAG|LEAD|NTH_VALUE|NTILE|PERCENT_RANK|ROW_NUMBER)$';
```

---

### **4. CHARACTER SET & COLLATION CHANGES**

#### **4.1 Default Character Set Changes**
| **Setting** | **MySQL 5.6/5.7** | **MySQL 8.4** | **Current Value** | **Impact Assessment** | **Action Required** |
|-------------|-------------------|----------------|-------------------|----------------------|-------------------|
| Default Charset | `latin1` | `utf8mb4` | ☐ | ☐ High ☐ Medium ☐ Low | ☐ Schema Review ☐ No Action |
| Default Collation | `latin1_swedish_ci` | `utf8mb4_0900_ai_ci` | ☐ | ☐ High ☐ Medium ☐ Low | ☐ Data Migration ☐ No Action |
| Connection Charset | Various | `utf8mb4` preferred | ☐ | ☐ High ☐ Medium ☐ Low | ☐ App Config ☐ No Action |

**Assessment Commands:**
```sql
-- Check current character sets
SHOW VARIABLES LIKE 'character_set%';
SHOW VARIABLES LIKE 'collation%';

-- Check table character sets
SELECT TABLE_SCHEMA, TABLE_NAME, TABLE_COLLATION 
FROM INFORMATION_SCHEMA.TABLES 
WHERE TABLE_TYPE = 'BASE TABLE';

-- Check column character sets
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, CHARACTER_SET_NAME, COLLATION_NAME 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE CHARACTER_SET_NAME IS NOT NULL;
```

#### **4.2 Character Set Migration Plan**
| **Database** | **Current Charset** | **Target Charset** | **Data Size** | **Migration Complexity** | **Downtime Estimate** |
|--------------|-------------------|-------------------|---------------|-------------------------|----------------------|
| [DB_NAME_1] | ☐ | `utf8mb4` | ☐ GB | ☐ Low ☐ Medium ☐ High | ☐ hours |
| [DB_NAME_2] | ☐ | `utf8mb4` | ☐ GB | ☐ Low ☐ Medium ☐ High | ☐ hours |
| [DB_NAME_3] | ☐ | `utf8mb4` | ☐ GB | ☐ Low ☐ Medium ☐ High | ☐ hours |

---

### **5. DEPRECATED FEATURES REMOVAL**

#### **5.1 Removed Features**
| **Feature** | **Removed in 8.4** | **Current Usage** | **Replacement** | **Migration Required** | **Priority** |
|-------------|-------------------|-------------------|-----------------|----------------------|--------------|
| Query Cache | ☐ Yes | ☐ Used ☐ Not Used | Performance Schema | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| `mysql_old_password` | ☐ Yes | ☐ Used ☐ Not Used | `mysql_native_password` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| `SQL_CALC_FOUND_ROWS` | ☐ Yes | ☐ Used ☐ Not Used | `COUNT()` over window | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| `FOUND_ROWS()` | ☐ Yes | ☐ Used ☐ Not Used | `COUNT()` over window | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |
| `GROUP BY ASC/DESC` | ☐ Yes | ☐ Used ☐ Not Used | `ORDER BY` | ☐ Yes ☐ No | ☐ High ☐ Medium ☐ Low |

**Detection Commands:**
```sql
-- Check for query cache usage
SHOW VARIABLES LIKE 'query_cache%';

-- Check for SQL_CALC_FOUND_ROWS usage (manual code review required)
-- Search application code for: SQL_CALC_FOUND_ROWS, FOUND_ROWS()

-- Check for GROUP BY with ASC/DESC
-- Manual review of stored procedures and views required
```

#### **5.2 Deprecated Functions**
| **Function** | **Status** | **Found in Code** | **Replacement** | **Migration Plan** | **Test Status** |
|--------------|------------|-------------------|-----------------|-------------------|-----------------|
| `PASSWORD()` | Deprecated | ☐ Yes ☐ No | `SHA2()` or proper auth | ☐ | ☐ Tested ☐ Pending |
| `ENCODE()`/`DECODE()` | Deprecated | ☐ Yes ☐ No | `AES_ENCRYPT()`/`AES_DECRYPT()` | ☐ | ☐ Tested ☐ Pending |
| `DES_ENCRYPT()`/`DES_DECRYPT()` | Removed | ☐ Yes ☐ No | `AES_ENCRYPT()`/`AES_DECRYPT()` | ☐ | ☐ Tested ☐ Pending |

---

### **6. DATA TYPE CHANGES**

#### **6.1 Temporal Data Types**
| **Data Type** | **MySQL 5.6/5.7 Behavior** | **MySQL 8.4 Behavior** | **Impact Assessment** | **Migration Required** |
|---------------|----------------------------|------------------------|----------------------|----------------------|
| `TIMESTAMP` | Auto-update first column | Explicit `DEFAULT CURRENT_TIMESTAMP` required | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |
| `DATETIME` | No fractional seconds support (5.6) | Fractional seconds supported | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |
| `YEAR(2)` | 2-digit year | Removed, use `YEAR(4)` | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |

**Assessment Commands:**
```sql
-- Check for YEAR(2) columns
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_TYPE 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE COLUMN_TYPE = 'year(2)';

-- Check TIMESTAMP columns without explicit defaults
SELECT TABLE_SCHEMA, TABLE_NAME, COLUMN_NAME, COLUMN_DEFAULT, EXTRA 
FROM INFORMATION_SCHEMA.COLUMNS 
WHERE DATA_TYPE = 'timestamp';
```

---

### **7. ENTERPRISE SECURITY CHANGES**

#### **7.1 Security Enhancements**
| **Feature** | **Change in 8.4** | **Current State** | **Impact** | **Configuration Required** |
|-------------|-------------------|-------------------|------------|---------------------------|
| Password Validation | Enhanced complexity rules | ☐ Enabled ☐ Disabled | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |
| Role-based Access | Enhanced RBAC | ☐ Used ☐ Not Used | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |
| Audit Logging | Extended audit capabilities | ☐ Enabled ☐ Disabled | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |
| Connection Encryption | TLS 1.2+ required | ☐ Configured ☐ Not Configured | ☐ High ☐ Medium ☐ Low | ☐ Yes ☐ No |

---

### **8. APPLICATION COMPATIBILITY MATRIX**

#### **8.1 Client Libraries & Drivers**
| **Technology** | **Current Version** | **MySQL 8.4 Compatible** | **Upgrade Required** | **Test Status** | **Notes** |
|----------------|-------------------|--------------------------|--------------------|-----------------|---------| 
| PHP MySQL Extension | ☐ | ☐ Yes ☐ No ☐ Unknown | ☐ Yes ☐ No | ☐ Tested ☐ Pending | PDO/MySQLi |
| Java JDBC Driver | ☐ | ☐ Yes ☐ No ☐ Unknown | ☐ Yes ☐ No | ☐ Tested ☐ Pending | Connector/J |
| Python MySQL Connector | ☐ | ☐ Yes ☐ No ☐ Unknown | ☐ Yes ☐ No | ☐ Tested ☐ Pending | PyMySQL/mysql-connector |
| .NET MySQL Connector | ☐ | ☐ Yes ☐ No ☐ Unknown | ☐ Yes ☐ No | ☐ Tested ☐ Pending | Connector/NET |
| Node.js MySQL | ☐ | ☐ Yes ☐ No ☐ Unknown | ☐ Yes ☐ No | ☐ Tested ☐ Pending | mysql2/mysql |

#### **8.2 Application Test Results**
| **Application** | **Test Environment** | **Connection Test** | **Query Test** | **Performance Test** | **Issues Found** |
|-----------------|---------------------|-------------------|----------------|---------------------|------------------|
| [APP_NAME_1] | ☐ Staging ☐ Dev | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ |
| [APP_NAME_2] | ☐ Staging ☐ Dev | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ |
| [APP_NAME_3] | ☐ Staging ☐ Dev | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ Pass ☐ Fail | ☐ |

---

### **9. MYSQL SHELL UPGRADE CHECKER RESULTS**

#### **9.1 Automated Check Results**
| **Check Category** | **Status** | **Issues Found** | **Severity** | **Action Required** |
|-------------------|------------|------------------|--------------|-------------------|
| Authentication | ☐ Pass ☐ Fail ☐ Warning | ☐ | ☐ High ☐ Medium ☐ Low | ☐ |
| SQL Compatibility | ☐ Pass ☐ Fail ☐ Warning | ☐ | ☐ High ☐ Medium ☐ Low | ☐ |
| Character Set | ☐ Pass ☐ Fail ☐ Warning | ☐ | ☐ High ☐ Medium ☐ Low | ☐ |
| Reserved Keywords | ☐ Pass ☐ Fail ☐ Warning | ☐ | ☐ High ☐ Medium ☐ Low | ☐ |
| Deprecated Features | ☐ Pass ☐ Fail ☐ Warning | ☐ | ☐ High ☐ Medium ☐ Low | ☐ |

**MySQL Shell Command:**
```bash
mysqlsh -- util checkForServerUpgrade root@localhost:3306 --outputFormat=JSON --configPath=/tmp/upgrade_check.json
```

---

### **10. MITIGATION & REMEDIATION PLAN**

#### **10.1 Priority Matrix**
| **Issue Category** | **Priority** | **Effort Estimate** | **Assigned Team** | **Target Completion** | **Status** |
|-------------------|--------------|-------------------|------------------|----------------------|------------|
| Authentication Changes | ☐ P0 ☐ P1 ☐ P2 | ☐ hours/days | ☐ | [DATE] | ☐ Not Started ☐ In Progress ☐ Complete |
| SQL Mode Changes | ☐ P0 ☐ P1 ☐ P2 | ☐ hours/days | ☐ | [DATE] | ☐ Not Started ☐ In Progress ☐ Complete |
| Reserved Keywords | ☐ P0 ☐ P1 ☐ P2 | ☐ hours/days | ☐ | [DATE] | ☐ Not Started ☐ In Progress ☐ Complete |
| Character Set Migration | ☐ P0 ☐ P1 ☐ P2 | ☐ hours/days | ☐ | [DATE] | ☐ Not Started ☐ In Progress ☐ Complete |
| Application Updates | ☐ P0 ☐ P1 ☐ P2 | ☐ hours/days | ☐ | [DATE] | ☐ Not Started ☐ In Progress ☐ Complete |

#### **10.2 Rollback Contingency**
| **Change Type** | **Rollback Method** | **Rollback Time** | **Data Loss Risk** | **Validation Method** |
|-----------------|-------------------|-------------------|-------------------|---------------------|
| Authentication | User account reset | ☐ minutes | ☐ High ☐ Medium ☐ Low ☐ None | Connection test |
| SQL Mode | Configuration revert | ☐ minutes | ☐ High ☐ Medium ☐ Low ☐ None | Query execution |
| Schema Changes | DDL rollback script | ☐ hours | ☐ High ☐ Medium ☐ Low ☐ None | Data consistency check |
| Character Set | Full data restore | ☐ hours | ☐ High ☐ Medium ☐ Low ☐ None | Character validation |

---

### **11. SIGN-OFF & APPROVAL**

#### **11.1 Impact Analysis Summary**
- **Total Issues Identified:** ☐
- **High Priority Issues:** ☐  
- **Medium Priority Issues:** ☐
- **Low Priority Issues:** ☐
- **Estimated Effort:** ☐ person-days
- **Risk Level:** ☐ High ☐ Medium ☐ Low

#### **11.2 Approval for Next Phase**
**Vendor Support (Technical Lead):**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**Developer Supervisor:**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

**Project Leader:**  
Name: ______________________  
Signature: ______________________  
Date: ______________________  

---

## **APPENDIX: Testing Scripts**

### **Comprehensive Compatibility Test Script**
```sql
-- MySQL 8.4 Compatibility Test Suite

-- Test 1: Authentication Plugin
SELECT user, host, plugin, password_last_changed 
FROM mysql.user 
WHERE user NOT IN ('mysql.sys', 'mysql.session', 'mysql.infoschema');

-- Test 2: SQL Mode Compatibility
SET sql_mode = 'ONLY_FULL_GROUP_BY,STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION';
-- Run critical queries here

-- Test 3: Character Set
SHOW VARIABLES LIKE 'character_set%';
SHOW VARIABLES LIKE 'collation%';

-- Test 4: Deprecated Features
SHOW VARIABLES LIKE 'query_cache%';
SHOW VARIABLES LIKE 'old_passwords';

-- Test 5: Reserved Keywords Check
-- (Use information_schema queries from above)
```

**End of Breaking Changes Checklist**  
**Estimated Assessment Time:** 1-2 weeks  
**Critical for Project Success:** High Priority
