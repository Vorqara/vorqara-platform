# Database Architecture

Version: 1.0

Last Updated: July 2026

Repository: vorqara-platform

---

# Overview

The Vorqara Platform uses a relational database as the primary source of truth for operational data.

The database is designed to provide consistency, security, scalability, and maintainability while supporting a multi-tenant architecture.

It stores business-critical information including organizations, users, devices, policies, software deployments, reports, audit records, and platform configuration.

---

# Objectives

The database architecture is designed to:

- Support multi-tenancy
- Maintain data integrity
- Provide high performance
- Enable horizontal application scaling
- Protect sensitive information
- Support auditing and compliance
- Simplify future schema evolution

---

# Database Principles

The database should:

- Normalize business data where appropriate
- Enforce referential integrity
- Use transactions for critical operations
- Avoid unnecessary duplication
- Separate operational data from audit data
- Encrypt sensitive information
- Support reliable backup and recovery

---

# Database Technology

Version 1.0 uses:

- PostgreSQL

Reasons:

- Mature relational database
- ACID compliance
- Excellent indexing capabilities
- Strong JSON support
- Robust replication
- Enterprise reliability
- Excellent AWS support

---

# Multi-Tenant Model

Vorqara follows a shared database, shared schema model with tenant isolation.

Each business record is associated with an Organization.

Example:

Organization

↓

Users

↓

Devices

↓

Policies

↓

Reports

↓

Software

Every query must execute within the context of the authenticated organization.

Cross-tenant access is prohibited except for authorized platform administrators.

---

# Core Entities

The primary business entities include:

- Organizations
- Users
- Roles
- Permissions
- Devices
- Device Groups
- Policies
- Software Packages
- Patch Catalog
- Patch Deployments
- Remote Sessions
- Alerts
- Reports
- Notifications
- Audit Logs
- API Keys
- Integrations
- Settings

Additional entities may be introduced as the platform evolves.

---

# Entity Relationships

High-level relationships include:

Organization

├── Users

├── Devices

├── Policies

├── Reports

├── Software

└── Settings

Users

├── Roles

└── Permissions

Devices

├── Policies

├── Software Deployments

├── Patch Deployments

├── Security Events

└── Remote Sessions

---

# Primary Keys

Every table should include:

- UUID Primary Key

Benefits include:

- Globally unique identifiers
- Improved distributed compatibility
- Reduced predictability
- Easier future service decomposition

---

# Foreign Keys

Relationships should be enforced through foreign key constraints.

Examples:

OrganizationID

UserID

DeviceID

PolicyID

Referential integrity must be maintained throughout the database.

---

# Audit Data

Critical actions should generate immutable audit records.

Examples include:

- Login
- User Creation
- Role Changes
- Device Registration
- Policy Assignment
- Software Deployment
- Patch Deployment
- Remote Session
- Configuration Changes

Audit records should not be modified after creation.

---

# Soft Deletes

Business entities should use soft deletion where appropriate.

Deleted records should retain:

- Timestamp
- User
- Reason (where applicable)

This supports auditing and recovery.

---

# Timestamps

Business entities should include:

- Created At
- Updated At

Where applicable:

- Deleted At
- Last Seen
- Last Login

All timestamps should use UTC.

---

# Indexing Strategy

Indexes should be created for:

- Primary Keys
- Foreign Keys
- Frequently searched columns
- Authentication lookups
- Device identifiers
- Organization identifiers

Indexes should be reviewed regularly based on production workloads.

---

# Transactions

Transactions should be used for:

- User creation
- Device enrollment
- Policy assignment
- Software deployment
- Patch deployment
- License updates

Partial writes should never leave inconsistent data.

---

# Data Retention

Retention policies should be defined for:

- Audit Logs
- Device Telemetry
- Notifications
- Reports
- Security Events

Retention periods should support operational and compliance requirements.

---

# Backup Strategy

Backups should include:

- Automated daily backups
- Point-in-time recovery
- Encrypted backup storage
- Regular recovery testing

Backups should be validated periodically.

---

# Encryption

Sensitive data should be protected using:

At Rest

- Database encryption
- Encrypted backups

In Transit

- TLS encryption

Sensitive secrets should never be stored in plaintext.

---

# Performance

Performance should be maintained through:

- Efficient indexing
- Query optimization
- Connection pooling
- Pagination
- Batch processing

Long-running queries should be monitored.

---

# High Availability

The database should support:

- Multi-AZ deployment
- Automated failover
- Read replicas (future)
- Disaster recovery planning

Availability is a core platform requirement.

---

# Future Evolution

As the platform grows, additional storage technologies may complement PostgreSQL.

Examples:

- Object Storage
- Search Index
- Time-Series Database
- Analytics Database

The relational database remains the system of record.

---

# Architecture Decision Record

## Decision

Use PostgreSQL as the primary operational database.

### Reason

- Strong relational capabilities
- Enterprise reliability
- Excellent AWS integration
- Mature ecosystem
- ACID compliance

### Alternatives Considered

- MySQL
- Microsoft SQL Server
- MongoDB

### Why This Was Chosen

PostgreSQL provides the best balance of consistency, performance, extensibility, and operational maturity for an enterprise endpoint management platform.

---

# Trade-offs

Advantages

- Strong consistency
- Mature tooling
- Excellent relational modeling
- Reliable transactions

Trade-offs

- Horizontal database scaling is more complex than NoSQL databases.
- Complex analytical workloads may eventually require specialized data stores.
- Large telemetry datasets may benefit from dedicated storage technologies in future releases.

These trade-offs are acceptable for Version 1.0 and align with the modular monolith architecture.

---

# Success Criteria

The database architecture should:

- Protect organizational data
- Maintain referential integrity
- Scale with customer growth
- Support secure multi-tenancy
- Enable reliable auditing
- Deliver predictable performance

---

# Conclusion

The Vorqara Database Architecture provides a secure, reliable, and scalable foundation for storing operational data across the platform.

By combining strong relational modeling, tenant isolation, auditability, and enterprise-grade operational practices, the database supports the current MVP while remaining flexible enough to evolve alongside future platform growth.