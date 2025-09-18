# IAAS Infrastracture Explanation
At first, we need to create Virtual Network with subnets, Web, App, DB. afterwards, we need to add NSGs.
Then, we need to put a Load Balancer that will evenly distribute incoming traffic.
For VMs, we need to create 3 VMs, the first one will be for the Front-end (React), the second will be for Flask, and the third for Database (Postgres)
# PAAS Infrastracture Explanation
The PAAS infrastructure has fewer component than the IAAS, because anything related to VMs is now handled by Azure
In this scenario the front-end (React) will be deployed to Azure static web apps using automatic CI/CD from GitHub.
The API (Flask) will be deployed to Azure app service and the Database(Postgres) will be deployed to Azure Database for Postgresql

# IAAS Diagram
flowchart LR
 subgraph S1["Subnet - Web"]
        WEBSS["Web Tier - VM Scale Set<br>React frontend"]
  end
 subgraph S2["Subnet - App"]
        APPSS["App Tier - VM Scale Set<br>Flask API layer"]
  end
 subgraph S3["Subnet - DB"]
        DB["PostgreSQL VM<br>Stores application data"]
  end
 subgraph VNET["Azure Virtual Network"]
        S1
        S2
        S3
  end
    U["Users / Browser<br>Access the app via HTTPS"] -- HTTPS 80/443 --> PLB["Azure Load Balancer - Public<br>Distributes web traffic across web VMs"]
    PLB --> WEBSS
    WEBSS -- /api proxy --> ALB["Azure Load Balancer - Private<br>Distributes traffic to API VMs"]
    ALB --> APPSS
    APPSS -- TCP 5432 --> DB

# PAAS Diagram
flowchart LR
    U["Users / Browser"] -- HTTPS --> SWA["Azure Static Web Apps - React"]
    SWA -- "/api - HTTPS" --> API["Azure App Service - Flask"]
    API --> PG["Azure Database for PostgreSQL"]