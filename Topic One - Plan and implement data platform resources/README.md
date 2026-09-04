# PLAN AND IMPLEMENT DATA PLATFORM RESOURCES

## Layers: Physical - Virtual - Cloud Service Models (IaaS, PaaS, SaaS)
![Cloud Service Models](cloud_service_models.png)

## [PaaS](https://learn.microsoft.com/en-us/training/modules/deploy-paas-solutions-with-azure-sql/)
This module focuses on ways to provision and deploy Azure SQL Database and Azure SQL Managed Instances, as well as provide guidance on the various options when performing a migration to these platforms.

- Gain an understanding of SQL Server in a Platform as a Service (PaaS) offering
- Understand PaaS provisioning and deployment options
- Understand elastic pools and hyperscale features
- Examine Azure SQL Managed Instances
- Configure a template for PaaS deployment

The PaaS offering provides less granular control over the infrastructure. It also relegates management of the underlying components (memory, CPU, storage, operating system, etc.) to Microsoft Azure.

**Explain PaaS options for deploying SQL Server in Azure:**

### Azure SQL Database
- Built upon the SQL Server engine.
- Runs in the Azure cloud environment as a **multi‑tenant** PaaS service.
- Hosted on a **logical server** (a metadata container for firewall rules, logins, auditing).
- Low-maintenance PaaS solution (patching, backup, HA handled by Azure).
- High flexibility for developers: no server/OS management, easy integration with applications.
- Granular deployment options at scale: Single Database, Elastic Pool, Serverless, Hyperscale, DTU/vCore models.
- Some advanced SQL Server features aren’t supported (SQL Agent, cross‑database queries, CLR, native backup/restore).

### Azure SQL Managed Instance
- Built upon the SQL Server engine, fully managed in the Azure cloud.
- Single‑tenant architecture with near 100% SQL Server compatibility.
- Ideal for migration scenarios: supports native backup/restore, SQL Agent, cross‑database queries, linked servers.
- Low‑maintenance PaaS solution (patching, backups, HA/DR handled by Azure).
- Runs inside a private virtual network (VNet) with a dedicated subnet for isolation and security.
- Supports the vCore purchasing model (General Purpose, Business Critical).
- Provides advanced business continuity options: failover groups, geo‑replication.
- Suitable for enterprise workloads requiring full SQL Server capabilities without managing servers or OS.



## IaaS

### SQL Server on Azure VM

### Full Comparison: Azure SQL Database vs Managed Instance vs SQL Server on VM

| **Aspect** | **Azure SQL Database (PaaS)** | **Azure SQL Managed Instance (PaaS)** | **SQL Server on VM (IaaS)** |
|-----------|-------------------------------|---------------------------------------|------------------------------|
| **Infrastructure Model** | PaaS (database-level) | PaaS (instance-level) | IaaS (full control) |
| **Operating System** | ❌ Not present | ✔️ Exists but hidden | ✔️ Full access |
| **SQL Server Instance** | ❌ No instance | ✔️ Full instance | ✔️ Full instance |
| **SQL Engine** | ✔️ Complete | ✔️ Complete | ✔️ Complete |
| **File System (mdf/ldf)** | ❌ Not accessible | ✔️ Internal only | ✔️ Full access |
| **SQL Agent** | ❌ Not available | ✔️ Available | ✔️ Available |
| **Cross‑database queries** | ❌ Not supported | ✔️ Supported | ✔️ Supported |
| **Linked Servers** | ❌ Not supported | ✔️ Supported | ✔️ Supported |
| **Native Backup (.bak)** | ❌ Not supported | ✔️ Supported | ✔️ Full support |
| **Networking** | ❌ Logical server only | ✔️ Private VNet | ✔️ Full control |
| **Isolation Model** | Multi‑tenant | Single‑tenant | Single‑tenant |
| **High Availability** | Automatic | Automatic | Manual |
| **Disaster Recovery** | Automatic | Automatic | Manual |
| **TempDB** | ✔️ Present (managed by Azure) | ✔️ Present (managed by Azure) | ✔️ Fully configurable |
| **Scalability** | Vertical + Serverless + Hyperscale | Vertical scaling | Manual |
| **Pricing Model** | DTU or vCore | vCore | VM-based |
| **Best Use Cases** | Modern apps, SaaS, microservices | Enterprise workloads, migrations | Legacy apps, full control, custom configs |

