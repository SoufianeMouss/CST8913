# Lab 10 - Lab: Design a Zero Trust Landing Zone for CloudMed Solutions

## 1. Compamy Overview
CloudMed Solutions is a healthcare technology company offering cloud based telemedicine and patient-management services to hospitals and clinics across Canada, USA, and Europe. Its main platform, MedConnect, provides secure virtual consultations, EMR management, and AI-driven health analytics.

### Why Zero Trust?

CloudMed handles sensitive patient data across multiple regions, making it a high-risk target for cyberattacks. A Zero Trust architecture ensures strict identity verification, least privilege access, and segmentation of workloads to protect EMR data and telehealth services.

### Key Compliance & Operational Drivers

CloudMed must meet HIPAA, GDPR, and PIPEDA requirements while ensuring secure multi-region operations. They also need centralized governance, consistent security policies, and reliable monitoring to support their global healthcare workloads.

## 2. Governance and Identity
```
Root
└── CloudMed
    ├── Management
    ├── Platform
    ├── LandingZones
    │     ├── Production
    │     └── NonProduction
    └── Compliance
          ├── HIPAA
          ├── GDPR
          └── PIPEDA
```
### Role-Based Access Control (RBAC)

Admins : manage platform and core Azure resources.

DevOps : deploy and operate application workloads.

Finance : view billing, budgets, and cost reports.

### Azure Policies
CloudMed uses Azure Policy to enforce consistent and compliant deployments:

Allowed regions (Canada Central, West Europe, US regions for HIPAA)

#### Mandatory tags:
Environment, Owner, CostCenter

#### Resource consistency:

- Encryption at rest required

- Diagnostic logs must be enabled

- No public IPs unless approved

- Private Endpoints required for SQL and Storage

#### Azure Entra ID

CloudMed uses Azure Entra ID for identity and access management:

- MFA and Conditional Access for all users

- Privileged Identity Management for admin roles

- Least privilege access enforcement

- Managed Identities for applications to remove secrets

## 3. Network Architecture
CloudMed uses Hub-and-Spoke design

### Hub VNet

The Hub contains shared network services:

* Azure Firewall
* Azure Bastion
* Private DNS Zones
* VPN/ExpressRoute gateways (Optional) 

These components provide centralized security and routing for all spokes.

### Spoke VNets

CloudMed uses separate spokes for each workload tier:

* App Spoke : web and mobile front-end components
* API Spoke : backend APIs and microservices
* Data Spoke : Azure SQL, Storage Accounts, and Key Vault (all using Private Endpoints)

Spokes are isolated from each other, and all traffic flows through the Hub for inspection.

### Zero Trust Network Controls

* East–west traffic passes through Azure Firewall
* Subnets are segmented using NSGs
* No public exposure for databases or internal APIs
* Admin access only through Bastion
* Private Link ensures data stays off the public internet

This design reduces lateral movement, enforces least privilege, and aligns with CloudMed’s compliance requirements.


## 4. Zero Trust Controls

CloudMed applies the three core Zero Trust principles across identity, network, and data to protect sensitive healthcare workloads.

### Verify Explicitly

Access to resources is authenticated and authorized based on identity, device compliance, and context.

* Identity-based authentication through Azure Entra ID
* Multi-Factor Authentication (MFA)
* Conditional Access to enforce trusted locations and compliant devices
* Azure Bastion for secure administrative access (no public RDP/SSH)

### Least Privilege Access

Permissions follow a minimum-access model to reduce potential misuse.

* Role-Based Access Control (RBAC) with tightly scoped roles
* Privileged Identity Management (PIM) for Just-in-Time (JIT) admin access
* Managed Identities to avoid credential exposure
* Segmented VNets and subnets to restrict unnecessary communication


### Assume Breach

The environment is designed to limit lateral movement and reduce impact if an incident occurs.

* Hub-and-Spoke network segmentation with Azure Firewall inspection
* Private Endpoints for SQL, Storage, and Key Vault
* Mandatory encryption at rest and in transit
* Continuous logging and threat detection through Defender for Cloud
* Diagnostic logs collected centrally for incident response


### Zero Trust Design Examples

* Azure Bastion: removes the need for public VM access
* Private Link for SQL and Storage: prevents exposure to the public internet
* Azure Policy blocking public IPs: ensures controlled network boundaries
* Firewall + NSGs: enforce strict east–west and north–south traffic control
* Key Vault: secures secrets and enforces access auditing


## 5. Monitoring, Compliance, and Cost

### Monitoring

CloudMed uses centralized monitoring tools to track performance, security, and activity across all subscriptions.

* Azure Monitor: for metrics, logs, and alerts
* Log Analytics Workspace: to collect resource, network, and diagnostic logs
* Defender for Cloud: for threat detection, vulnerability assessments, and regulatory compliance checks
* Application Insights: to monitor application performance and API behavior

All logs flow into the management subscription for unified visibility and incident response.


### Compliance Enforcement

Compliance with HIPAA, GDPR, and PIPEDA is enforced using Azure Policy and built-in regulatory standards.

* Azure Policy Initiatives: apply rules for encryption, allowed regions, private endpoints, and diagnostic logging
* Regulatory Compliance Dashboard: in Defender for Cloud tracks CloudMed’s alignment with HIPAA, GDPR, and PIPEDA
* Auditing and reporting: ensure workloads continuously meet legal and organizational requirements
* Policy inheritance: ensures all subscriptions follow consistent governance controls


### Cost Management

CloudMed uses Azure Cost Management features to control spending and maintain cost visibility.

* Budgets: to define spending limits per environment (Production, NonProduction, Compliance)
* Cost alerts: triggered when usage approaches budget thresholds
* Tagging standards: ('Environment', 'Owner', CostCenter) used for cost allocation and reporting
* Right-sizing recommendations: from Azure Advisor to optimize resource consumption

These measures help CloudMed maintain predictable spending while supporting global healthcare operations.

## 6. Conceptual Diagram
```
flowchart TB

    %% ============================
    %% MANAGEMENT GROUPS
    %% ============================
    subgraph MG[Management Groups]
        ROOT[Root]
        CM[CloudMed]
        MGMT[Management - Governance and Monitoring]
        PLATFORM[Platform]
        LZ[Landing Zones]
        COMP[Compliance]
    end

    ROOT --> CM
    CM --> MGMT
    CM --> PLATFORM
    CM --> LZ
    CM --> COMP

    %% ============================
    %% GOVERNANCE & MONITORING
    %% ============================
    subgraph GOV[Governance and Monitoring Layer]
        POLICY[Azure Policy - Governance]
        DEFENDER[Defender for Cloud - Threat Detection]
        LOG[Log Analytics - Monitoring]
        RBAC[RBAC / PIM - Identity Governance]
    end

    MGMT --> GOV

    %% ============================
    %% HUB VNET
    %% ============================
    subgraph HUB[Hub VNet - Shared Services]
        FW[Azure Firewall - Zero Trust: Traffic Inspection]
        BAS[Azure Bastion - Zero Trust: Verify Explicitly]
        DNS[Private DNS]
    end

    MGMT --> HUB

    %% ============================
    %% SPOKES
    %% ============================
    subgraph APP[App Spoke]
        WEB[Web/Mobile Apps]
    end

    subgraph API[API Spoke]
        APISVC[API Services and AKS]
    end

    subgraph DATA[Data Spoke]
        SQL[Azure SQL - Private Endpoint]
        STG[Storage - Private Endpoint]
        KV[Key Vault - Secrets]
    end

    HUB --- APP
    HUB --- API
    HUB --- DATA

    APP --> API
    API --> DATA

```

## 7. Summary and Recommendations

The proposed Zero Trust Azure Landing Zone for CloudMed provides a secure, compliant, and scalable foundation for hosting healthcare workloads. The use of management groups, subscription separation, hub-and-spoke networking, and Azure Policy, it ensures strong governance and consistent security across all environments. 
Identity protection is used through Azure Entra ID, MFA, and least-privilege RBAC, while Private Endpoints, network segmentation, and Azure Firewall reduce the attack surface and prevent lateral movement. The monitoring with Log Analytics and Defender for Cloud, supports continuous compliance with HIPAA, GDPR, and PIPEDA requirements.

### Recommendations for Future Improvement

* **Automation:** Implement Bicep or Terraform for deploying landing zone components and governance controls to ensure consistency, reduce configuration drift, and support repeatable deployments for new regions or workloads.
* **Multi-cloud Expansion:** Expand the landing zone other cloud providers for global reach, and flexibility in meeting future data sovereignty or business requirements.
