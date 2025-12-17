# database_architecture.md

# Fleet Management System - Database Architecture

**Version:** 1.0
**Date:** 2025-12-17

## 1. Overview

This document describes the high-level architecture of the database systems supporting the Fleet Management application. It covers the choice of databases, data distribution, scalability, and security strategies.

## 2. Database Model Selection

A hybrid database approach is chosen to optimize for different data types and access patterns.

- **Relational Database (PostgreSQL):**

  - **Purpose:** To store structured, transactional data such as user profiles, vehicle details, job assignments, and maintenance records.
  - **Justification:** PostgreSQL provides strong data consistency (ACID compliance), powerful querying capabilities, and robust support for relational integrity.

- **Time-Series Database (TimescaleDB):**
  - **Purpose:** To store high-volume, time-stamped data, primarily GPS location pings from telematics devices.
  - **Justification:** TimescaleDB is an extension for PostgreSQL, which simplifies the tech stack. It is highly optimized for ingesting and querying large volumes of time-series data, offering significant performance improvements and features like data retention policies and continuous aggregations.

## 3. Scalability and High Availability

- **Read Replicas:** We will use read replicas for the PostgreSQL database to distribute the read load, improving performance for reporting and dashboard queries.
- **Connection Pooling:** A connection pooler (e.g., PgBouncer) will be used to manage database connections efficiently.
- **Sharding (Future):** As the data grows, the `gps_data` hypertable can be sharded by `vehicle_id` to distribute the write and query load across multiple database nodes.

* **TimescaleDB Configuration:** The `gps_data` table will be configured as a hypertable partitioned by the `time` column, with chunk intervals of 7 days. This optimizes both data ingestion and time-based queries.

- **High Availability:** The database will be deployed in a primary/standby configuration with automated failover to ensure minimal downtime in case of a primary node failure.

## 4. Backup and Recovery

- **Point-in-Time Recovery (PITR):** Continuous archiving of WAL (Write-Ahead Log) files will be enabled to allow for recovery to any point in time.
- **Regular Backups:** Full database backups will be taken daily and stored in a separate, secure location (e.g., AWS S3).

## 5. Security

- **Encryption at Rest:** All data stored in the databases will be encrypted.
- **Encryption in Transit:** SSL/TLS will be enforced for all connections to the database.
- **Access Control:** A role-based access control (RBAC) model will be implemented. Application services will have dedicated database users with the minimum required privileges.
