# Lab 11 - Application Migration

## Task 1 – Set Up Tooling for Discovery

### Discovery Method

Tailwind Traders should use a mix of agentless and agent-based discovery.
Agentless discovery is enough for collecting VMware VM details, while the agent-based approach is required for SQL01 (physical server) and for accurate dependency mapping across all tiers.

### Number of Appliances

Only one Azure Migrate appliance is needed because all servers are in the same datacenter and connected to the same VMware environment.

### Required Credentials

To run a complete discovery, the team must prepare a few types of credentials:

* **Software inventory:** a Windows domain account with local admin rights.
* **SQL discovery:** SQL Server admin credentials
* **Dependency mapping:** domain admin credentials to deploy the Dependency Agent on each server.

### Best Practices During Discovery

Tailwind should follow these recommended practices:

- Allow the discovery phase to run for at least 14–30 days so performance data is accurate.
- Make sure all credentials are validated before starting the scans.
- Install the Dependency Agent on every server involved to avoid missing connections.

## Task 2 – Perform Assessment Planning

### Assessment Type

Tailwind should choose a production assessment, since the environment is a real business application with strict uptime and performance requirements.

### Target Region and Performance History

The target region should be the closest Azure region to the business (for example, East US or the region specified by the lab).
Performance history should be 30 days to capture stable and peak usage patterns.

### Sizing Approach

Use performance-based sizing.
This provides a more accurate VM recommendation based on actual CPU, memory, and disk usage instead of simply copying the existing on-premises VM sizes, which may be oversized or outdated.

### Comfort Factor, Pricing Model, Licensing

* Comfort factor: 10–20 percent to avoid undersizing.
* Pricing model: pay-as-you-go for flexibility during migration planning.
* Licensing model: Azure Hybrid Benefit if Tailwind has valid Windows Server and SQL licenses; otherwise use standard Azure licensing.

## Task 3 – Dependency Analysis

### Application Components

The application stack includes:

* WEB01 and WEB02 (frontend servers)
* APP01 (application/API server)
* SQL01 (database server)
* LB01 (internal load balancer)

### Dependencies

Here are eight relevant dependencies the discovery will detect:

* HTTP traffic from clients to WEB servers on port 80
* HTTPS traffic to WEB servers on port 443
* WEB tier connecting to APP01 on port 8080 or similar API port
* APP01 connecting to SQL01 on port 1433 (SQL Server)
* DNS queries to internal DNS server (port 53)
* Active Directory or domain authentication (LDAP/kerberos)
* Backup traffic from servers to backup system
* Outbound internet access for updates (port 443)

### Filter Out as Noise

Some items collected by dependency mapping should be ignored, such as:

* Windows update connections
* Antivirus update services
* Telemetry or monitoring agents unrelated to the application
* Occasional admin RDP sessions not part of app communication

### Business Requirements

During interviews with the application owner, the following items should be documented:

1) Business criticality: the application is important for daily operations and must remain available.
2) Uptime and downtime: only a one-hour downtime window is permitted for migration.
3) Data classification: SQL data is considered sensitive and must be protected.
4) Licensing dependencies: SQL Server 2017 licensing and any application-specific licenses.
5) Patching requirements: regular Windows and SQL patch cycles must continue after migration.
6) Firewall and IP considerations: existing firewall rules, IP allow-lists, and load balancer rules must be replicated in Azure.

## Task 4 – Validate Assessment Results with Application Owner

### Recommended VM Sizes

3 recommended VM sizes:

* WEB servers: D2s v3 or B2ms
* APP server: D4s v3
* SQL server: D8s v4 or an E-series (E8s v4) for memory-optimized workloads

### Components to Replace or Optimize

On-prem components that should be replaced or improved when moving to Azure:

* LB01 replaced with Azure Load Balancer or Azure Application Gateway
* SQL01 may be optimized by choosing Azure SQL Managed Instance or SQL Server on Azure VM
* VMware backup jobs replaced with Azure Backup
* Firewall rules recreated using NSGs or Azure Firewall

### Dependency Validation

* WEB -> APP
* APP -> SQL
* DNS and Active Directory
* Backup connections
* Any external API or endpoint used by the app

### SLA and Downtime Confirmation

We ensure the one-hour downtime window is acceptable for the selected migration approach
We Verify that the VM SKUs chosen meet the required uptime and performance expectations.

### SQL Migration Options

There are three valid options for SQL01:

* Rehost: move SQL Server to an Azure VM with same SQL version
* Azure SQL Managed Instance: compatible with most SQL Server features, automated patching and backups.
* Azure SQL Database: PaaS option, suitable only if the app supports full modernization


## Task 5 – Migration Plan

### Pre-Migration Tasks

| Step | Action                                                    |
| ---- | --------------------------------------------------------- |
| 1    | Confirm discovery and assessment are complete             |
| 2    | Validate credentials, firewall rules, and access to Azure |
| 3    | Enable Azure Migrate replication for all servers          |
| 4    | Prepare resource group, VNet, subnets, NSGs               |
| 5    | Confirm 1-hour downtime window with owner                 |
| 6    | Perform full backup of SQL01 and application files        |


### Migration Steps (Per Server Group)

#### Wave 1 – WEB01, WEB02

| Step | Action                                       |
| ---- | -------------------------------------------- |
| 1    | Drain or stop web traffic                    |
| 2    | Cutover WEB01 and WEB02 using Azure Migrate  |
| 3    | Start VMs and validate IIS/homepage in Azure |

#### Wave 2 – APP01

| Step | Action                                     |
| ---- | ------------------------------------------ |
| 1    | Stop API services                          |
| 2    | Cutover APP01                              |
| 3    | Validate API connectivity with WEB servers |

#### Wave 3 – SQL01

| Step | Action                                            |
| ---- | ------------------------------------------------- |
| 1    | Stop SQL workloads and run final on-prem backup   |
| 2    | Perform SQL migration (rehost, MI, or restore DB) |
| 3    | Validate SQL connectivity and performance         |


### DNS Updates

| Step | Action                                                       |
| ---- | ------------------------------------------------------------ |
| 1    | Update internal DNS records for WEB and APP to new Azure IPs |
| 2    | Update public DNS if application is externally reachable     |


### Connection String Changes

| Step | Action                                                      |
| ---- | ----------------------------------------------------------- |
| 1    | Update WEB and APP connection strings with new SQL endpoint |
| 2    | Test SQL authentication and firewall rules                  |


### Load Balancer Considerations

| Step | Action                                                       |
| ---- | ------------------------------------------------------------ |
| 1    | Replace LB01 with Azure Load Balancer or Application Gateway |
| 2    | Add WEB01 and WEB02 to backend pool                          |
| 3    | Configure health probes and LB rules                         |


### SQL Migration Considerations

| Step | Action                                                 |
| ---- | ------------------------------------------------------ |
| 1    | Choose SQL migration method (rehost, MI, DB)           |
| 2    | Restore databases and configure logins/SQL Agent tasks |
| 3    | Validate performance (CPU, RAM, IOPS)                  |


### Post-Migration Validation Checklist

| Item to Validate                    |
| ----------------------------------- |
| All VMs running and reachable       |
| Full app flow: web → app → SQL      |
| DNS resolution and firewall rules   |
| Load balancer functioning           |
| CPU/RAM/I/O performance             |
| Backups and monitoring enabled      |
| Confirmation from application owner |


### Back-Out Plan (If Migration Fails)

| Step | Action                                     |
| ---- | ------------------------------------------ |
| 1    | Shut down Azure VMs                        |
| 2    | Revert DNS to on-premises IPs              |
| 3    | Restart WEB01, WEB02, APP01, SQL01 on-prem |
| 4    | Re-enable on-prem load balancing           |
| 5    | Validate on-prem functionality             |
| 6    | Schedule new migration window              |
## Task 6 – Plan the Migration Waves

### Migration Wave Logic

Stateless and low-risk components move first, while the database moves last because it has the highest impact and requires deeper testing.

### Waves Breakdown

| Wave   | Servers      | Reason                                                                            |
| ------ | ------------ | --------------------------------------------------------------------------------- |
| Wave 1 | WEB01, WEB02 | Stateless tier, simple cutover, easy to validate and roll back                    |
| Wave 2 | APP01        | Depends on WEB tier, needs API testing after web servers are online               |
| Wave 3 | SQL01        | Most critical component, requires database validation and shortest cutover window |