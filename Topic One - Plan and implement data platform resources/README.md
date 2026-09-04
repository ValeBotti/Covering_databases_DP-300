# PLAN AND IMPLEMENT DATA PLATFORM RESOURCES

## Layers: Physical - Virtual - Cloud Service Models (IaaS, PaaS, SaaS)
![Cloud Service Models](cloud_service_models.png)

## PaaS - Azure SQL Database And Azure SQL Managed Instance
This module focuses on ways to provision and deploy Azure SQL Database and Azure SQL Managed Instances, as well as provide guidance on the various options when performing a migration to these platforms.

- Gain an understanding of SQL Server in a Platform as a Service ( [PaaS](https://learn.microsoft.com/en-us/training/modules/deploy-paas-solutions-with-azure-sql/) ) offering
- Understand PaaS provisioning and deployment options
- Understand elastic pools and hyperscale features
- Examine Azure SQL Managed Instances
- Configure a template for PaaS deployment

The PaaS offering provides less granular control over the infrastructure. It also relegates management of the underlying components (memory, CPU, storage, operating system, etc.) to Microsoft Azure.<br>

**Explain PaaS options for deploying SQL Server in Azure - What's there vs what's not** <br>
When choosing between Azure SQL Database and Managed Instance, the key is understanding what each service provides and what it does not. <br>
This section highlights the architectural differences that impact compatibility, migrations, and application design.

### Azure SQL Database

**What's there:** <br>
A) Built upon the SQL Server engine. <br>
B) Fully managed multi‑tenant PaaS service. <br>
C) Hosted on a logical server (metadata container: firewall, logins, auditing). <br>
D) Automatic management: patching, backups, high availability. <br>
E) Flexible deployment models:
  - Single Database
  - Elastic Pool
  - Serverless (auto‑scale, auto‑pause)
  - Hyperscale (up to 100 TB)
  - DTU or vCore purchasing models
F) Ideal for modern applications: no OS or server management required.

#### E) *Deployment models*
Azure SQL Database is available in two different deployment models:

**What's not there:**
- No SQL Server instance (no master/msdb, no instance‑level features).
- No SQL Agent (no scheduled jobs).
- No cross‑database queries (databases are isolated).
- No linked servers.
- No CLR integration.
- No native backup/restore (.bak).
- No access to mdf/ldf files.
- No private VNet (only public endpoint + firewall rules).

### Azure SQL Managed Instance

**What's there:**
- Built on the full SQL Server engine (near 100% compatibility).
- Real SQL Server instance: master, msdb, SQL Agent, instance‑level features.
- Single‑tenant architecture (isolated environment).
- Native backup/restore (.bak) support.
- SQL Agent for scheduled jobs.
- Cross‑database queries.
- Linked servers.
- Runs inside a private VNet with a dedicated subnet.
- vCore model: General Purpose, Business Critical.
- Advanced business continuity: failover groups, geo‑replication.
- Ideal for enterprise migrations without rewriting the application.

**What's not there:**
- No access to the operating system (OS fully managed by Azure).
- No direct access to mdf/ldf files (storage abstracted).
- No hardware management (fully PaaS).
- No DTU model (vCore only).
- No free‑form networking (must use VNet/subnet architecture).

## IaaS - SQL Server on Azure VM

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

