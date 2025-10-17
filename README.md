# Java Movie Management Web App

A **Java Servlet-based movie management system** designed for deployment in a **hybrid Azure + AWS CI/CD pipeline**.  
The project builds a `.war` file, using **Azure DevOps Pipelines** with a **self-hosted agent on AWS EC2**, and stores build artifacts in **AWS S3** and **Azure Blob Storage**.

---

##  Table of Contents
1. [Project Overview](#project-overview)  
2. [Architecture Diagram](#architecture-diagram)  
3. [Project Structure](#project-structure)  
4. [Technologies Used](#technologies-used)  
5. [Local Setup](#local-setup)  
6. [Build Instructions](#build-instructions)  
7. [Azure DevOps Setup](#azure-devops-setup)  
8. [AWS EC2 Self-Hosted Agent Setup](#aws-ec2-self-hosted-agent-setup)  
9. [Artifact Upload to AWS S3 and Azure Blob](#Done)  


---

##  Project Overview

This is a **Java web application** that manages a list of movies (add, list, search, delete).  
It uses **JSP + Servlets**, **JDBC**, and **Log4j2** for logging.  

The CI/CD workflow uses **Azure DevOps Pipelines**, where:
- Source code is pushed to **Azure Repos**
- Pipeline runs on a **self-hosted AWS EC2 agent**
- Build artifact (`ROOT.war`) is stored in:
  - **AWS S3 bucket**
  - **Azure Blob Storage**

---

##  Architecture Diagram

```text
        ┌───────────────────────────┐
        │     Developer Machine     │
        │ (pushes code to Azure Repo)│
        └──────────────┬────────────┘
                       │
                       ▼
        ┌───────────────────────────┐
        │     Azure DevOps Repos    │
        └──────────────┬────────────┘
                       │
                       ▼
        ┌───────────────────────────┐
        │   Azure DevOps Pipeline   │
        │  (runs on AWS EC2 Agent)  │
        └──────────────┬────────────┘
                       │
                       ▼
   ┌───────────────────────────────┬──────────────────────────────┐
   │             AWS S3            │          Azure Blob Storage  │
   │ (Artifact storage bucket)     │ (Artifact backup container)  │
   └───────────────────────────────┴──────────────────────────────┘
```

---

##  Project Structure

```
azure-project-main/
│
├── .ebextensions/              # AWS Elastic Beanstalk config (optional)
├── src/
│   ├── com/snakes/models/      # Java models (Movie, Media)
│   ├── com/snakes/servlets/    # Servlet classes (AddMovie, ListMovies)
│   ├── WEB-INF/
│   │   ├── web.xml             # Servlet configuration
│   │   ├── lib/                # Third-party JARs
│   │   └── log4j2.xml          # Logging configuration
│   └── index.jsp               # Main JSP page
│
├── build.sh                    # Linux build script
├── build-windows.sh            # Windows build script
├── azure-pipelines.yml         # CI/CD pipeline definition
└── README.md                   # Documentation (this file)
```

---

##  Technologies Used

| Category | Tool |
|-----------|------|
| Language | Java (JDK 11 or higher) |
| Framework | JSP + Servlets |
| Build | Custom shell script (`build.sh`) |
| CI/CD | Azure DevOps Pipelines |
| Artifact Hosting | AWS S3, Azure Blob |
| Hosting (optional) | AWS Elastic Beanstalk |
| Logging | Log4j2 |

---

##  Local Setup

### Prerequisites
- **Java 11+**
- **Git**
- **AWS CLI**
- **Azure CLI**
- **Maven or build tools (optional)**

### Clone Repo
```bash
https://github.com/tribhuwanpandey/Azure-AWS-Project.git
cd Azure-AWS-Project-main
```

### Build Locally
```bash
chmod +x build.sh
./build.sh
```

Generated artifact:
```
src/ROOT.war
```

---

##  Azure DevOps Setup

1. Clone the repo:
   ```bash
   git clone https://github.com/tribhuwanpandey/Azure-AWS-Project.git

   ```

2. Create Pipeline
   - Go to **Azure DevOps → Pipelines → New Pipeline**
   - Choose **Azure Repos (YAML)**
   - Point to `azure-pipelines.yml`

---

##  AWS EC2 Self-Hosted Agent Setup

1. Launch EC2 Instance
   - AMI: Amazon Linux 2 (or Ubuntu 22.04)
   - Instance type: `t2.micro` or higher
   - Allow outbound internet

2. Install dependencies
   ```bash
   sudo yum update -y
   sudo yum install -y git java-11-openjdk unzip zip
   ```

3. Install AWS CLI
   ```bash
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   unzip awscliv2.zip
   sudo ./aws/install
   ```

4. Configure AWS credentials
   ```bash
   aws configure
   ```

5. Install Azure DevOps Agent
   ```bash
   mkdir myagent && cd myagent
   wget https://vstsagentpackage.azureedge.net/agent/3.x.x.tar.gz
   tar zxvf vsts-agent-linux-x64-3.x.x.tar.gz
   ./config.sh
   ./run.sh
   ```

---

---

## Summary

| Step | Platform | Purpose |
|------|------------|----------|
| 1 | Developer | Push code to Azure Repo |
| 2 | Azure DevOps | Run pipeline |
| 3 | AWS EC2 Agent | Build WAR |
| 4 | AWS S3 | Store artifact |
| 5 | Azure Blob | Backup artifact |


## Done

### Job sucess
![alt text](<Screenshot 2025-10-17 143746.png>)

### Self-Hosted-Agent
![alt text](<Screenshot 2025-10-17 143914.png>)

### Artifacts in S3 bucket
![alt text](<Screenshot 2025-10-17 140643.png>)

### Artifacts in Blobs storage
![alt text](<Screenshot 2025-10-17 143605.png>)