# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This repository contains documentation and checklists for a comprehensive MySQL migration project: upgrading MySQL 5.6/5.7 to MySQL 8.4 Enterprise with High Availability (InnoDB Cluster + MySQL Router) and migrating the operating system to CentOS.

The project involves:
- **10 servers** across production, staging, and development environments
- **16-week timeline** with 5 major phases
- **Zero-downtime upgrade strategy** using MySQL Router for transparent failover
- **Enterprise features implementation** including security, monitoring, and backup
- **High Availability setup** with InnoDB Cluster (3-node) and MySQL Router (2-node)

## Architecture & Structure

### Project Phases
1. **Phase 0 (Weeks 1-2)**: Assessment & Planning
2. **Phase 1 (Weeks 3-5)**: OS Upgrade to CentOS
3. **Phase 2 (Weeks 6-7)**: MySQL 5.6 → 5.7 Upgrade (stepping stone)
4. **Phase 3 (Weeks 8-9)**: Training & Adaptation for MySQL 8.4
5. **Phase 4 (Weeks 10-15)**: MySQL 8.4 Enterprise + InnoDB Cluster Implementation
6. **Phase 5 (Week 16)**: Stabilization & Documentation

### Key Components
- **InnoDB Cluster**: 3-node cluster for production (Primary + 2 Secondary nodes)
- **MySQL Router**: 2-node setup for high availability and load balancing
- **Enterprise Features**: Security, monitoring, backup, and audit capabilities
- **Zero-downtime Strategy**: Rolling upgrades using MySQL Router for transparent failover

### Critical Success Factors
- **GTID-based replication** for proper cluster setup
- **MySQL Router configuration** for read/write split and failover
- **Enterprise License compliance** monitoring
- **Performance baseline maintenance** throughout migration
- **Comprehensive backup strategy** with MySQL Enterprise Backup

## File Structure

### Main Documentation
- `README.md`: Complete project timeline, deliverables, and dependency matrix
- `subtask_1_1_server_audit_checklist.md`: Comprehensive server inventory and audit procedures
- `subtask_1_2_breaking_changes_checklist.md`: MySQL 8.4 compatibility assessment and breaking changes analysis
- `subtask_1_3_cluster_topology_checklist.md`: InnoDB Cluster and MySQL Router setup procedures
- `subtask_1_4_application_compatibility_checklist.md`: Application compatibility matrix for CentOS migration

### Document Types
Each checklist follows a structured format with:
- Detailed assessment matrices and checklists
- Technical commands and scripts for validation
- Risk assessment and mitigation strategies
- Sign-off sections for approval workflow
- Appendices with reference commands and troubleshooting

## Key Technical Requirements

### MySQL 8.4 Enterprise Prerequisites
- GTID mode enabled (`gtid_mode = ON`)
- Binary logging with ROW format
- InnoDB as default storage engine
- Enterprise Security features (audit, encryption, firewall)
- MySQL Router 8.4+ for cluster management

### High Availability Setup
- **InnoDB Cluster**: Minimum 3 nodes for proper quorum
- **MySQL Router**: Multi-instance deployment for redundancy
- **Network Requirements**: Ports 3306, 33060, 33061 for cluster communication
- **Failover Strategy**: Automatic primary election and router-based routing

### Critical Upgrade Path
- MySQL 5.6 → 5.7 → 8.4 (direct 5.6 → 8.4 not supported)
- OS migration before MySQL upgrade for stability
- Rolling upgrade strategy to minimize downtime

## Important Conventions

### Language Requirements
- **All explanations, descriptions, and documentation** must be written in **Bahasa Indonesia**
- **Technical terms and parameters** should remain in their **original English form**
- Examples of terms to keep in English: `GTID`, `InnoDB`, `MySQL Router`, `caching_sha2_password`, `sql_mode`, `PRIMARY`, `SECONDARY`
- Examples of what should be in Bahasa Indonesia: explanations, instructions, descriptions, analysis text, error descriptions, recommendations

### Assessment Methodology
- Each subtask includes comprehensive checklists with checkboxes for tracking
- Risk levels classified as High/Medium/Low with specific mitigation strategies
- Sign-off requirements from multiple stakeholders (Vendor Support, DBA, Project Leader)
- Detailed command references for technical validation

### Documentation Standards
- Structured tables for compatibility matrices and assessment results
- Appendices with technical scripts and troubleshooting guides
- Timeline estimates and resource allocation planning
- Rollback procedures for each major change

### Risk Management
- **Split-brain prevention**: Odd number of cluster nodes for quorum
- **Performance monitoring**: Baseline establishment and continuous monitoring
- **License compliance**: MySQL Enterprise License usage tracking
- **Backup validation**: Multiple backup strategies with restore testing

## Testing Strategy

### Environment Sequence
1. **Development Environment**: Initial compatibility testing
2. **Staging Environment**: Full integration validation  
3. **Production Environment**: Phased rollout with rollback capability

### Key Test Categories
- Application compatibility with new OS and MySQL version
- Performance baseline comparison (within 10% tolerance)
- Security controls and compliance validation
- Failover and disaster recovery procedures
- Enterprise features functionality

## Enterprise Considerations

### MySQL Enterprise Features
- **Enterprise Security**: Audit logging, transparent data encryption, firewall
- **Enterprise Monitor**: Real-time monitoring and alerting
- **Enterprise Backup**: Hot backup with point-in-time recovery
- **Query Analyzer**: Performance optimization tools

### Operational Changes
- Backup strategy migration to Enterprise Backup tools
- Monitoring integration with Enterprise Monitor
- Security policy implementation with Enterprise Security
- Cluster management using MySQL Shell and Router

This project requires careful coordination between infrastructure, development, and operations teams with strong emphasis on testing, validation, and rollback procedures to ensure successful zero-downtime migration.