# PLAN AND IMPLEMENT DATA PLATFORM RESOURCES

### Implementing data platform resources: customization

## <p align="center">PaaS</p>

### <p align="center">> Azure SQL Database</p>

#### Customization options
- Deployment models
- Purchasing model
- DTU
- vCore
- DTU vs vCore
- Serverless
- Backups
- Active geo‑replication
- Failover groups
- Hyperscale (tier + architecture)

#### Deployment options
- Deploying via the portal
- Deploying via PowerShell/CLI
- Deploying via ARM templates
- Deploy SQL Database elastic pool
- Deploy Azure SQL Database Hyperscale

#### Management options
- Managing pool resources
- Understanding SQL Database Hyperscale

**<p align="center">Hands-On Scenario Tasks - (DP-300 is a scenario-based exam)</p>**

<ins>COST TIP — before you start:</ins><br>
* Create everything inside one dedicated resource group (e.g. `rg-dp300-labs`).
* Delete the whole resource group at the end of each study session — this zeroes out costs instead of leaving expensive resources (Business Critical, Hyperscale, Elastic Pools) running.

### <p align="center">Session 1 — Single Database basics</p>

#### Single Database — DTU model
<ins>TASK:</ins><br>

-> Deploy a Single Database using the DTU purchasing model (Basic, Standard, or Premium).

<ins>WHAT TO OBSERVE:</ins><br>
* The available DTU tiers (Basic/Standard/Premium).
* How storage is fixed per tier.
* How performance is represented as DTUs instead of CPU/RAM.
* The absence of vCore-level configuration.
* The simplified pricing model.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Predictable, simple workloads where you don't need to reason in CPU/RAM.
* Small apps/dev-test environments where budget simplicity matters more than fine-grained control.

#### Single Database — vCore model
<ins>TASK:</ins><br>

-> Deploy a Single Database using the vCore purchasing model (General Purpose or Business Critical).

<ins>WHAT TO OBSERVE:</ins><br>
* Separation of compute and storage configuration.
* The difference between GP and BC (IO latency, SSD, HA architecture).
* How vCores map to memory and IO.
* The presence of zone redundancy options.
* The ability to scale compute independently.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Need to size compute/storage independently or match on-prem core licensing.
* Business Critical for low-latency/high-availability requirements; General Purpose for standard production workloads.

#### Single Database — Serverless compute
<ins>TASK:</ins><br>

-> Deploy a Serverless Azure SQL Database (General Purpose tier).

<ins>WHAT TO OBSERVE:</ins><br>
* Minimum and maximum vCore range.
* Autopause delay configuration.
* Database pausing after inactivity.
* Cold-start behavior when reconnecting.
* Billing differences (per-second compute + storage only when paused).
* Unsupported features (LTR, geo-replication, elastic jobs).
* Memory scaling: RAM scales proportionally with vCores (e.g. max 4 vCores → a fixed max RAM); billing is based on whichever of vCore or RAM usage is higher at any given second.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Intermittent, unpredictable workloads with idle periods (dev/test, low-traffic apps).
* Cost optimization is the priority over guaranteed HA/DR features.

#### Deployment via Azure Portal
<ins>TASK:</ins><br>

-> Deploy a database using the Azure Portal.

<ins>WHAT TO OBSERVE:</ins><br>
* Visual differences between DTU and vCore creation flows.
* How serverless options appear.
* How elastic pools are created and configured.
* How Hyperscale differs visually.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* One-off deployments, exploration, or when a repeatable script isn't needed.

### <p align="center">Session 2 — Elastic Pool & Hyperscale</p>

#### Elastic Pool — DTU or vCore
<ins>TASK:</ins><br>

-> Deploy an Elastic Pool and add two databases to it.

<ins>WHAT TO OBSERVE:</ins><br>
* How shared resources work (min/max per DB).
* How pool DTUs or vCores are allocated.
* How performance changes when one DB spikes.
* Pool-level vs database-level configuration.
* Cost differences compared to two single databases.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Many databases with unpredictable, non-overlapping usage patterns (e.g., multi-tenant SaaS).
* Cost savings needed by sharing resources instead of over-provisioning each DB individually.

#### Managing Elastic Pool resources
<ins>TASK:</ins><br>

-> Modify the pool configuration (increase/decrease compute or storage).

<ins>WHAT TO OBSERVE:</ins><br>
* How changes affect all databases in the pool.
* How min/max per-database limits impact performance.
* How scaling the pool differs from scaling a single DB.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Pool-wide demand grows/shrinks, and you need one lever instead of resizing each DB.
* A single database in the pool needs a guaranteed floor/ceiling (set per-DB min/max).

#### Hyperscale — Deployment
<ins>TASK:</ins><br>

-> Deploy an Azure SQL Database Hyperscale.

<ins>WHAT TO OBSERVE:</ins><br>
* The distributed architecture (compute + page servers + log service).
* The massive storage limit (up to 100 TB).
* Fast scaling behavior.
* Differences from GP/BC (no zone redundancy, different HA model).
* Restore and backup behavior (very fast log replay).

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Very large databases (beyond the 4 TB single-DB limit) or fast-growing data.
* Need for near-instant backups/restores regardless of database size.

### <p align="center">Session 3 — Automation & IaC</p>

#### Deployment via PowerShell/CLI
<ins>TASK:</ins><br>

-> Deploy the same Single Database (DTU or vCore) using Azure CLI or PowerShell instead of the portal.

<ins>WHAT TO OBSERVE:</ins><br>
* Required parameters (resource group, server, edition, service objective).
* How the purchasing model is specified in the command (e.g., --edition, --capacity).
* Differences in speed/repeatability compared to the portal.
* How to check deployment status from the CLI.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Scripted, repeatable deployments or integration into automation pipelines.

#### Deployment via ARM templates
<ins>TASK:</ins><br>

-> Deploy a Single Database using an ARM (or Bicep) template.<br>

<ins>WHAT TO OBSERVE:</ins><br>
* Structure of the template (resources, properties, sku).
* How the purchasing model and the tier are expressed in JSON/Bicep.
* Idempotency (re-running the same template doesn't recreate the resource).
* How this compares to portal/CLI for repeatable environments.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Infrastructure-as-code requirements, consistent multi-environment deployments (dev/test/prod).

#### Read Scale-Out<br>
<ins>TASK:</ins><br>

-> Enable read scale-out on a Business Critical (or Premium) single database and route a read-only connection to it.<br>

<ins>WHAT TO OBSERVE:</ins><br>
* Which tiers support read scale-out.
* How the read-only replica differs from an active geo-replication secondary.
* How to target the replica via connection string (ApplicationIntent=ReadOnly).
* Replication lag behavior on the secondary.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Need to offload read-only reporting queries within the same region without setting up geo-replication.

#### Azure Hybrid Benefit
<ins>TASK:</ins><br>

-> Explore the Azure Hybrid Benefit option during database creation/configuration. <br>

<ins>WHAT TO OBSERVE:</ins><br>
* Eligibility requirements (existing SQL Server licenses with Software Assurance).
* How the cost estimate changes when AHB is enabled.
* Which purchasing models/tiers support it.

<ins>WHEN TO CHOOSE THIS:</ins><br>
* Already own SQL Server licenses with active Software Assurance and want to reduce Azure compute costs.

### <p align="center">> Azure SQL Managed Instance</p>

**Customization options:**
