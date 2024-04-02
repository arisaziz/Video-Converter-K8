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
drag & drop screenshots here or use imgur and reference them using imgsrc

Every screenshot should have some text explaining what the screenshot is about.

Example below.

*Ref 1: Network Diagram*
