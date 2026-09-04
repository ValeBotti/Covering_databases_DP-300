# PLAN AND IMPLEMENT DATA PLATFORM RESOURCES - PLAN!

## A) Layers: Physical - Virtual - Cloud Service Models (IaaS, PaaS, SaaS)
![Cloud Service Models](cloud_service_models.png)

## B) PaaS - Azure SQL Database And Azure SQL Managed Instance
This module focuses on ways to provision and deploy Azure SQL Database and Azure SQL Managed Instance, as well as provide guidance on the various options when performing a migration to these platforms.

- Gain an understanding of SQL Server in a Platform as a Service ( [PaaS](https://learn.microsoft.com/en-us/training/modules/deploy-paas-solutions-with-azure-sql/) ) offering
- Understand PaaS provisioning and deployment options
- Understand elastic pools and hyperscale features
- Examine Azure SQL Managed Instances
- Configure a template for PaaS deployment

The PaaS offering provides less granular control over the infrastructure. It also relegates management of the underlying components (memory, CPU, storage, operating system, etc.) to Microsoft Azure.<br>

<h3><p align="center"><strong>Explain PaaS options - What's there vs what's not</strong></p></h3>
<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

When choosing between Azure SQL Database and Managed Instance, the key is understanding what each service provides and what it does not. <br>
This section highlights the architectural differences that impact compatibility, migrations, and application design.
<u>─────────────────────────────────────────────────────────────────────────────────────────</u>
### > Azure SQL Database

**What's there:**

1 - Built upon the SQL Server engine. <br>
2 - Fully managed multi‑tenant PaaS service. <br>
3 - Hosted on a logical server (metadata container: firewall, logins, auditing). <br>
4 - Automatic management: patching, backups, high availability. <br>
5 - Flexible deployment models:
  - Single Database
  - Elastic Pool
  - Serverless (auto‑scale, auto‑pause)
  - Hyperscale (up to 100 TB)
  - DTU or vCore purchasing models <br>
  
6 - Ideal for modern applications: no OS or server management required.

5 - *Flexible Deployment models*
Azure SQL Database is available in two different deployment models:

**What's not there:**

1 - No SQL Server instance (no master/msdb, no instance‑level features). <br>
2 - No SQL Agent (no scheduled jobs). <br>
3 - No cross‑database queries (databases are isolated). <br>
4 - No linked servers. <br>
5 - No CLR integration. <br>
6 - No native backup/restore (.bak). <br>
7 - No access to mdf/ldf files. <br>
8 - No private VNet (only public endpoint + firewall rules). <br>

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

### > Azure SQL Managed Instance

**What's there:**

1 - Built on the full SQL Server engine (near 100% compatibility). <br>
2 - Real SQL Server instance: master, msdb, SQL Agent, instance‑level features. <br>
3 - Single‑tenant architecture (isolated environment). <br>
4 - Native backup/restore (.bak) support. <br>
5 - SQL Agent for scheduled jobs. <br>
6 - Cross‑database queries. <br>
7 - Linked servers. <br>
8 - Runs inside a private VNet with a dedicated subnet. <br>
9 - vCore model: General Purpose, Business Critical. <br>
10 - Advanced business continuity: failover groups, geo‑replication. <br>
11 - Ideal for enterprise migrations without rewriting the application. <br>

**What's not there:**

1 - No access to the operating system (OS fully managed by Azure). <br>
2 - No direct access to mdf/ldf files (storage abstracted). <br>
3 - No hardware management (fully PaaS). <br>
4 - No DTU model (vCore only). <br>
5 - No free‑form networking (must use VNet/subnet architecture). <br>

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

## C) IaaS - SQL Server on Azure VM

### > SQL Server on Azure VM

## D) Full Comparison: Azure SQL Database vs Managed Instance vs SQL Server on VM

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

