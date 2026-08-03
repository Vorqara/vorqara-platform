# Vorqara Endpoint Database Model

## Document Information

| Property | Value |
|----------|-------|
| Product | Vorqara Endpoint |
| Version | 1.0 |
| Status | Draft |
| Database | PostgreSQL 18 |
| ORM | Prisma 6 |
| Author | Vorqara Engineering |
| Last Updated | August 2026 |

---

# Purpose

This document defines the database architecture for the Vorqara Endpoint platform.

It serves as the single source of truth for all database entities, relationships, naming conventions, and implementation standards.

All Prisma models must conform to this document.

---

# Design Principles

The database is designed around the following principles:

- Multi-tenant by design
- Organization-first architecture
- UUID primary keys
- Role-Based Access Control (RBAC)
- Soft delete support
- Auditability
- Horizontal scalability
- Cloud-native deployment
- Security by default

---

# Database Engine

| Item | Value |
|------|-------|
| Database | PostgreSQL |
| ORM | Prisma |
| Character Set | UTF-8 |
| Timezone | UTC |

---

# Naming Standards

## Tables

- Singular names
- PascalCase in Prisma
- snake_case in PostgreSQL

Examples

- Organization
- User
- Device
- Alert

---

## Primary Keys

Every table uses UUID.

Example

id

---

## Foreign Keys

Always use:

<entity>Id

Examples

organizationId

userId

deviceId

policyId

---

## Audit Fields

Every table contains:

- createdAt
- updatedAt

Optional:

- deletedAt

---

# Soft Delete Strategy

Records are never permanently removed unless explicitly purged.

Instead:

deletedAt

is populated.

---

# Multi-Tenant Strategy

Every business object belongs to an Organization.

Organization

↓

Users

Devices

Policies

Alerts

Scripts

Audit Logs

Jobs

Settings

This ensures complete tenant isolation.

---

# Core Domain Models

Phase 1

- Organization
- User
- Role
- Permission
- OrganizationMember

Phase 2

- Device
- DeviceGroup
- DeviceTag
- DevicePolicy

Phase 3

- Alert
- AuditLog
- RemoteSession
- Script
- Job

Phase 4

- TenantSetting
- ApiKey
- Notification
- Webhook

---

# Entity Relationship Overview

Organization

├── Users

├── Roles

├── Devices

├── Policies

├── Alerts

├── Scripts

├── Jobs

├── Audit Logs

└── Settings

---

# UUID Strategy

Every public identifier uses UUID Version 4.

Sequential IDs are never exposed through APIs.

---

# Indexing Strategy

Indexes will be created for:

- email
- organizationId
- deviceId
- policyId
- status
- createdAt

Composite indexes will be added where required.

---

# Security Standards

Passwords

- Never stored in plaintext
- Argon2 hashing

API Keys

- Hashed before storage

Tokens

- JWT
- Short-lived Access Tokens
- Refresh Token Rotation

---

# Future Expansion

The schema is designed to support future Vorqara products including:

- Vulnerability Management
- Patch Management
- Remote Desktop
- Compliance
- Asset Discovery
- Software Deployment
- Mobile Device Management
- Cloud Security
- SIEM Integration

without major database redesign.

---

# Change Management

Every schema change must:

1. Update this document
2. Update Prisma schema
3. Generate migration
4. Review migration
5. Commit changes
