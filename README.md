# Devops Project: Video Converter
convert video to audio using microservice architecture

## Architecture
<p align="center">
  <img src="./Images/ProjectArchitecture.png" width="600" title="Architecture" alt="Architecture">
  </p>

## Deploying a Python-based Microservice Application on AWS EKS

### Introduction
The project aimed to deploy a Python-based microservice application on AWS Elastic Kubernetes Service (EKS) for video conversion. Leveraging Helm, the deployment included PostgreSQL and MongoDB databases to support the application's data needs. Additionally, RabbitMQ was integrated into the architecture to handle asynchronous messaging. The microservices were divided into four major components:

- auth-server: Responsible for handling user authentication and authorization.
- converter-module: Core microservice for converting video files from one format to another.
- database-server: Manages the storage and retrieval of data in PostgreSQL and MongoDB databases.
- notification-server: Handles notifications and alerts within the application.**

### Skills Learned

- **Python**: use to develop the program and use Flask to build RESTful API endpoint of microservices.
- **AWS services**: understand the basics of EKS, IAM Roles and networking configuration.
- **Container Orchestration**: utilizing Kubernetes for managing containerized application.
- **Infrastructure as Code**: Implementing infrastructure setup using Helm and AWS CLI.
- **Database Management**: setting up and configuring database (PostgreSQL and MongoDB) using Helm chart.
- **Message Queueing**: Intergrating RAbbitMQ for asynchrounous messaging, ensuring reliable communication between microservice.
- **Security Measures**: Implementing user authentication and authorization mechanisms, including JWT token generation and validation.

## Steps

### Prerequisites
Before you start, make sure following prerequisites are met:

1. Create AWS Account
2. Install Helm
3. Install Python
4. Install AWS CLI
5. Install `Kubectl`
6. Setup PostgreSQL and MongoDB

### Step-by-step

1. **Login to WS console**
  - Access the AWS console with your credential or create new account.

2. **Create eksCluster IAM Role**
   - Follow the steps mentioned in [this](https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html) documentation using root user
   - After creating it will look like this:

   <p align="center">
  <img src="./Project documentation/ekscluster_role.png" width="600" title="ekscluster_role" alt="ekscluster_role">
  </p>

   - Please attach `AmazonEKS_CNI_Policy` explicitly if it is not attached by default

















