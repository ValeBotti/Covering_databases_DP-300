# <p align="center">PLAN AND IMPLEMENT DATA PLATFORM RESOURCES</p>
### <p align="center">WE NEED A PLAN! - first things first: planning and scheming</p>
<p align="center">I need to deploy a database with Azure; what are the solutions available?</p>

## A) Layers: Physical - Virtual - Cloud Service Models (IaaS, PaaS, SaaS)
![Cloud Service Models](./image/cloud_service_models.png)

## B) PaaS - Azure SQL Database And Azure SQL Managed Instance
Let's get a grip on what the PaaS options actually are and what they provide. <br>

The PaaS offering provides less granular control over the infrastructure. It also relegates management of the underlying components (memory, CPU, storage, operating system, etc.) to Microsoft Azure.<br>

<h3><p align="center"><strong>Explain PaaS options - What's there vs what's not</strong></p></h3>
<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

When choosing between Azure SQL Database and Managed Instance, the key is understanding what each service provides and what it does not. <br>
This section highlights the architectural differences that impact compatibility, migrations, and application design.
<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

### > Azure SQL Database

**What's there:**

1. Built upon the SQL Server engine. <br>
2. Fully managed multi‑tenant PaaS service. <br>
3. Hosted on a logical server (metadata container: firewall, logins, auditing). <br>
4. Automatic management: patching, backups, high availability. <br>
5. Flexible deployment models:
    - Single Database
    - Elastic Pool — shared DTU/vCore resources across multiple databases with variable, non‑synchronized usage patterns; ideal for multi‑tenant scenarios with many small databases
    - Serverless (auto‑scale, auto‑pause)
    - Hyperscale (up to 100 TB)
    - DTU or vCore purchasing models <br> 
6. Ideal for modern applications: no OS or server management required.

**What's not there:**

1. No SQL Server instance (no master/msdb, no instance‑level features). <br>
2. No SQL Agent (no scheduled jobs). <br>
3. No cross‑database queries (databases are isolated). <br>
4. No linked servers. <br>
5. No CLR integration (only "safe" CLR assemblies are supported; "unsafe" / "external access" CLR is not). <br>
6. No native backup/restore (.bak). <br>
7. No access to mdf/ldf files. <br>
8. No private VNet (only public endpoint + firewall rules). <br>

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

### > Azure SQL Managed Instance

**What's there:**

1. Built on the full SQL Server engine (near 100% compatibility). <br>
2. Real SQL Server instance: master, msdb, SQL Agent, instance‑level features. <br>
3. Single‑tenant architecture (isolated environment). <br>
4. Native backup/restore (.bak) support. <br>
5. SQL Agent for scheduled jobs. <br>
6. Cross‑database queries. <br>
7. Linked servers. <br>
8. Runs inside a private VNet with a dedicated subnet. <br>
9. vCore model:
    - General Purpose — remote storage (Azure Premium Storage), lower cost
    - Business Critical — local SSD storage, built‑in synchronous replica (includes a readable secondary), lower latency <br>
10. Advanced business continuity: failover groups, geo‑replication. <br>
11. Ideal for enterprise migrations without rewriting the application. <br>

**What's not there:**

1. No access to the operating system (OS fully managed by Azure). <br>
2. No direct access to mdf/ldf files (storage abstracted). <br>
3. No hardware management (fully PaaS). <br>
4. No DTU model (vCore only). <br>
5. No free‑form networking (must use VNet/subnet architecture). <br>

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

## C) IaaS - SQL Server on Azure VM
Now let's focus on the IaaS solution. <br>
The most common reason for deploying SQL Server in an Azure Virtual Machine (VM) is because you want an easy, straightforward method to migrate an existing on-premises SQL Server into the cloud.
Infrastructure as a Service (IaaS) allows for greater flexibility in configuration. This flexibility means that you (not me LOL), as a database administrator, must plan your configuration carefully. Choosing the proper VM sizing, storage, and networking options is crucial to ensure adequate performance for your workloads.

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

### > SQL Server on Azure VM

**What's there**

1. Full SQL Server instance: <br>
  Complete access to the SQL Server engine, system databases (master, msdb, model), tempdb, and all instance‑level features—just like on‑premises. <br>
2. Full operating system access: <br>
  You control the Windows Server VM: install additional software, configure services, manage patching, adjust registry settings, and customize the environment.  <br>
3. Support for older SQL Server versions: <br>
  Ideal when applications require legacy SQL Server versions for vendor compatibility or cannot be upgraded. <br>
4. Support for SSIS / SSRS / SSAS: <br>
  You can install Integration Services, Reporting Services, and Analysis Services on the same VM. <br>
5. Cross‑database queries, linked servers, CLR: <br>
  All advanced SQL Server features are available, including instance‑level operations not supported in PaaS. <br>
6. Full control over storage layout: <br>
  Choose disk types, IOPS, throughput, premium storage, configure tempdb placement, and optimize storage for performance. <br>
7. Full control over networking: <br>
  Manage VNet, subnets, NSGs, routing, firewalls, DNS, Active Directory, ExpressRoute, and VPN connectivity. <br>
8. SQL Server IaaS Agent Extension:
  Provides:
      - Automated backups <br>
      - Automated patching <br>
      - Azure Key Vault integration <br>
      - Microsoft Defender for Cloud integration <br>
      - Disk utilization insights <br>
      - Flexible licensing <br>
      - SQL best practices assessment <br>
9. Flexible licensing options:
    - Pay‑as‑you‑go SQL Server images <br>
    - Bring Your Own License (BYOL) <br>
    - Software Assurance benefits — prerequisite for accessing Azure Hybrid Benefit <br>
    - Azure Hybrid Benefit (Windows + SQL) — the actual cost discount unlocked by having Software Assurance <br>
    - Reserved VM instances (1–3 years) <br>
10. Multiple VM families for every workload: <br>
  General Purpose, Compute Optimized, Memory Optimized, Storage Optimized, GPU, HPC, FPGA.
11. Azure Marketplace templates: <br>
  Deploy SQL Server 2019/2022 on Windows Server with OLTP or Data Warehouse presets in just a few clicks. <br>
12. Full SQL Server configuration options: <br>
  Authentication, networking, collation, instance settings, encryption, security policies, and more.

<u>─────────────────────────────────────────────────────────────────────────────────────────</u>

## D) Decision matrix: Azure SQL Database vs Managed Instance vs SQL Server on VM

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

