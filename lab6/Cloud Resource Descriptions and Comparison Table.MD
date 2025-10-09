# Lab 6: Cloud Resource Comparison Across AWS, Azure, and Google Cloud

## Cloud Resource Descriptions and Comparison Table:

| #  | Description | AWS (Service Name) | Azure (Service Name) | Google Cloud (Service Name) |
|----|-------------|--------------------|----------------------|----------------------------|
| 1  | A compute service that provides scalable virtual machines for running applications. | Amazon VM  | AWS Ec2 | Google Compute Engine |
| 2  | An object storage service used to store and retrieve data, commonly used for backups and static website content. | Azure Blob Storage | Amazon S3 | Cloud SQL |
| 3  | A managed relational database service that supports multiple database engines like MySQL, PostgreSQL, and SQL Server. | Azure Relational Database Services  | Amazon Relational Database Service  | Google Cloud Databases |
| 4  | A serverless compute service that allows you to run code in response to events without provisioning or managing servers. |Azure Functions  | AWS Lambda | Cloud Run Functions |
| 5  | A virtual private network service that allows you to create isolated networks within the cloud provider's infrastructure. | Azure Virtual Network |Amazon Private Network  | Google Cloud Private Network  |
| 6  | A content delivery network (CDN) service that delivers data, videos, applications, and APIs to customers around the world with low latency. | Azure Content Delivery Network | Amazon CloudFront | Google CDN   |
| 7  | A managed NoSQL database service designed for low-latency, high-scale applications. | Azure Cosmos DB For NoSQL | AWS Dynamo DB | Google Cloud NoSQL  |
| 8  | A block storage service for use with virtual machines, offering persistent storage for data. |Azure Block Storage Devices  |AWS Elastic Block Store (EBS) | Google Block Storage |
| 9  | A managed container orchestration service based on Kubernetes. |  Azure Kubernetes Service| Amazon Elastic Kubernetes Service | Google Kubernetes Engine |
| 10 | A service for managing user access and encryption keys to secure cloud resources. |Azure Key Vault  | AWS Key Management Service  | Google Cloud Key Management and Secret Storage |
| 11 | A platform that automates application deployment and scaling without needing to manage infrastructure. | Azure App Service | AWS Elastic Beanstalk | Google App Engine |
| 12 | A service that provides monitoring and logging of applications and infrastructure, offering insights into resource usage and performance. | Azure Monitor | Amazon CloudWatch | Cloud Monitoring |
| 13 | A domain name system (DNS) service that routes traffic globally and translates domain names to IP addresses. | Azure Monitor | Amazon Route 53 | Cloud DNS |
| 14 | A load balancing service that distributes incoming network traffic across multiple targets, improving application availability. |Azure Load Balancer  | AWS Application Load Balancer | Cloud Load Balancing |
| 15 | A service that automatically scales your cloud infrastructure based on demand, ensuring resources are available as needed. | Azure Virtual Machine Scale Sets | AWS EC2 Autoscaling | Compute Engine Autoscaler |
| 16 | A message queuing service that enables applications to send and receive messages between different components. |Azure Queue Storage  |Amazon Simple Queue Service (SQS  | Cloud Tasks |
| 17 | A managed real-time data streaming service that collects and processes large amounts of data from various sources. | Azure Event Hubs | Amazon Kinesis Data Streams | Pub/Sub |
| 18 | A fully managed, highly scalable data warehouse service optimized for analytics and large-scale queries. | Azure Synapse Analytics | Amazon Redshift | BigQuery |
| 19 | A service that automates the execution of workflows and allows the integration of different cloud services in a sequence of steps. | Azure Logic Apps | AWS Step Functions | Workflows |
| 20 | A service that integrates multiple data sources and enables data migration, transformation, and movement across platforms. | Azure Data Factory | AWS Appflow | BigQuery Data Transfer Service |
| 21 | A data catalog and governance service that helps manage metadata across your data estate, supporting compliance and security. | Azure Purview | AWS Glue | Dataplex Universal Catalog |
| 22 | A set of machine learning and AI services that provide pre-built models, APIs, and tools for developers to easily implement AI in their apps. | Azure Copilot | Amazon Q Business | Gemini for Google Cloud |
| 23 | A service that allows you to define and deploy infrastructure using code, automating the management of cloud resources. | Azure Resource Manager | AWS CloudFormation | Google Cloud Deployment Manager |
| 24 | A fully managed CI/CD service that automates the building, testing, and deployment of applications to production environments. |Azure DevOps  | AWS CodePipeline | Cloud Build |
| 25 | A desktop as a service (DaaS) offering that allows you to deploy virtual desktops in the cloud and access them remotely. | Azure Virtual Desktop | Amazon WorkSpaces | Google Cloud Workstations |
| 26 | A backup and disaster recovery service that helps to protect your data by creating backups and replicas of your cloud resources. |Azure Backup  | AWS Backup | Backup and DR Service |
| 27 | A service designed for big data analytics, allowing organizations to store, process, and analyze large datasets in real time. | Azure Synapse Analytics | Amazon Redshift  | BigQuery |
| 28 | A file storage service for storing and sharing files with users, typically used in shared file systems across applications. |Azure Files  | Amazon Elastic File System (Amazon EFS) | Filestore |
| 29 | A service that helps you transcode, process, and stream media content such as video and audio. | Azure Media Services | AWS Media Convert | Transcoder API |
| 30 | A real-time communication service used for sending notifications, emails, and text messages to users and devices. | Azure Notification Hubs | Amazon Simple Notification Service | Firebase Cloud Messaging |


## Key similarities and differences
#### Amazon Web Services
AWS provides web app and web hosting and administrative services, include the Amazon Elastic Compute Cloud (EC2).
The backup data can be maintained on AWS. But for a free tier account, it is not free.
We may distribute the user's load between multiple instances utilizing an AWS service comparable to a load balancer.

#### Microsoft Azure
Hosting and managing web applications and websites. And you have to pay for what you used.
Building and deploying machine learning models using Azure Machine Learning.
Developing the blockchain application using Azure.

#### Google Cloud Platform
GCP is also used in games. GCP is running multiplayer games.
GCP provides the virtual desktop experience.
GCP is used to process the big query and analyze the big data.

## Unique features or capabilities
The three cloud providers are currently in a state of high competitiveness. All three suppliers offer essential tools and services, and are likely to extend them in the future, based on current trends and customer wants.

### AWS
Amazon Web Services (AWS)

. Breadth & maturity: Widest range of cloud services (>200).
. Compute variety: Deep options (EC2 instance types, Graviton chips).
. AI/ML platform: Amazon Bedrock and SageMaker for customizable AI models.
. Global infrastructure: Largest number of regions and availability zones.
. Hybrid tools: Outposts for on-prem AWS hardware integration.

### Azure
. Enterprise integration: Seamless with Windows Server, Active Directory, Office 365, and Microsoft Copilot.
. Hybrid cloud: Azure Arc enables managing on-prem and multi-cloud resources.
. AI ecosystem: Tight integration with OpenAI models via Azure OpenAI Service.
. Developer tools: Strong DevOps and Visual Studio/GitHub integration.
. Compliance: Broadest set of certifications for regulated industries.

### Google Cloud
. Data & analytics: Industry-leading tools like BigQuery and Dataflow.
. AI & generative models: Gemini for Google Cloud with multimodal AI.
. Networking: Global VPC and private Google backbone with built-in redundancy.
. Container leadership: Originator of Kubernetes and Anthos for hybrid K8s.
. Billing model: Per-second billing and automatic sustained-use discounts.

## How the naming conventions between cloud providers differ for similar services.
| **Aspect** | **AWS** | **Microsoft Azure** | **Google Cloud Platform (GCP)** |
|-------------|----------|---------------------|---------------------------------|
| **Naming Style** | Uses **“Amazon”** or **“AWS”** prefix + **descriptive noun** (e.g., *Amazon S3*, *AWS Lambda*). | Uses **“Azure”** prefix + **functional name** (e.g., *Azure Blob Storage*, *Azure Functions*). | Uses **“Cloud”** or product name only (e.g., *Cloud Storage*, *BigQuery*, *Pub/Sub*). |
| **Tone & Structure** | Often **service-oriented** and **technical**, emphasizing AWS branding. | Generally **enterprise-oriented**, with consistent “Azure + service type” structure. | Simpler and **developer-friendly**, often omits “Google” in product names. |

- **AWS** → Technical, legacy-style names (many start with *Amazon*).  
- **Azure** → Consistent corporate branding (*Azure + Function*).  
- **Google Cloud** → Simpler, modern naming, emphasizing usability (*Cloud + Functionality*).
