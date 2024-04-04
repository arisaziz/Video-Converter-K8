# Devops Project: Video Converter
convert video to audio using microservice architecture

## Architecture
<p align="center">
  <img src="./Images/ProjectArchitecture.png" width="600" title="Architecture" alt="Architecture">
  </p>

## Deploying a Python-based Microservice Application on AWS EKS

### Introduction
The project aimed to deploy a Python-based microservice application on AWS Elastic Kubernetes Service (EKS) for video conversion. Leveraging Helm, the deployment included PostgreSQL and MongoDB databases to support the application's data needs. Additionally, RabbitMQ was integrated into the architecture to handle asynchronous messaging. The microservices were divided into four major components:

- `auth-server`: Responsible for handling user authentication and authorization.
- `converter-module`: Core microservice for converting video files from mp4 format to mp3.
- `database-server`: Manages the storage and retrieval of data in PostgreSQL and MongoDB databases.
- `notification-server`: Send notification to the user via email after video conversion done.

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
**
1. **Create AWS Account**
2. **Install Helm**
3. **Install Python**
4. **Install AWS CLI**
5. **Install Kubectl**
6. **Setup PostgreSQL and MongoDB**

### Step-by-step

1. **Login to WS console**
   - Access the AWS console with your credential or create new account.

2. **Create eksCluster IAM Role**
   - Follow the steps mentioned in [this](https://docs.aws.amazon.com/eks/latest/userguide/service_IAM_role.html) documentation using root user.
   - Please attach `AmazonEKS_CNI_Policy` explicitly if it is not attached by default.
   - After created it will look like this:

    <p align="center">
    <img src="./Images/ekscluster_role.png" width="600" title="ekscluster_role" alt="ekscluster_role">
    </p>
     
3. **Create Node Role -AmazonEKSNodeRole**
    - Follow the steps mention in [this](https://docs.aws.amazon.com/eks/latest/userguide/create-node-role.html#create-worker-node-role) documentaion useing root user.
    - We are using default VPC for this project thus no need to configure new VPC.
    - Once your role created, attach these policies `AmazonEKS_CNI_Polocy` , `AmazonEBSCSIDriverPolicy` , and `AmazonEC2ContainerRegistryReadOnly` incase it is not attached by default.
    - The AmazonEKSNodeRole will look like this
   
    <p align="center">
    <img src="./Images/node_iam.png" width="600" title="Node_IAM" alt="Node_IAM">
    </p>

4. **Open EKS Dashboard**
    - Find EKS service inside AWS Console dashboard.

5. **Create EKS Cluster**
    - Click "Create Cluster".
    - Give a name to your cluster.
    - Setup networking setting for VPN and subnet.
    - Select `eksCluster` IAM role we created before.
    - Review and create cluster.

6. **Cluster Creation** ⏱
    - Wait until cluster status show "Active", then can start create node groups.

7. **Node Group Creation**
    - In "Compute" section inside the cluster, click on "Add node group".
    - Choose the AMI(default), instance type(t3.medium) and the number of nodes(desire:1,minimum:1,maximum:1).
    - For subnet, use default.
  
8. **Adding Inbound rule to Nodes's Security Group**
    - To ensure that nessesary ports are open for inbound traffic.

   <p align="center">
     <img src="./Images/inbound_rules_sg.png" width="600" title="inbound_rule_sg" alt="inbound_rule_sg"
   </p>

9. **Enable EBS CSI Addon**
    - Enable addon `EBS container storage interface` to allow EKS cluster to use EBS in AWS as persistence storage.
  
   <p align="center">
     <img src="./Images/ebs_addon.png" width="600" title="ebs_addon" alt="ebs_addon"
  </p>

10. **Deploy application on EKS Cluster**
    - Clone the Python code from my repository.
    - **Connect AWS CLI with cluster**, enter this command inside your CLI:
      ```
      aws eks update-kubeconfig --name <cluster name> --region <cluster region>
      ```
    - for this case :
      ```
      aws eks update-kubeconfig --name Microservice --region us-east-1
      ```
    - To check if the connection is OK, enter
      ```
      kubectl get ns
      ```
      It will list down "default namespace" if the connection is good.
      
    **NOTE:** Before this command execute, your kubectl manage the kubernetes in your pc. After executed, kubectl will manage the cluster in AWS.

11. **Install MongoDB in Node** 
     - MongoDB is use for storing video and audio file.
     - MongoDB folder contain:
         - **chart**: a configuration file we set up that need to be install in cluster using helm.
         - **value**: it store username and password to login to mongo, can change password here.
         - **template folder**: contain some configuration for pod including pv.yaml.
    
     - Install MongoDB
     ```
     cd Helm_charts/MongoDB
     helm install mongo .
     ```
     - To check wether the pod that run the MongoDB is running or not. This command will show all pod status inside the cluster:
    ```
    kubectl get pods
    ```
    ```
    kubectl get all
    ```
    - Actually the we also define a persistence volume for the pod, to check pv status :
    ```
    kubectl get pv
    ```
    - Connect to MongoDB instance:
     ```
     mongosh mongodb://<username>:<pwd>@<nodeip>:30005/mp3s?authSource=admin
     ```
     **NOTE:** `helm` command is use to install MongoDB inside Kubernetes cluster.

12. **Install Postgres**
     - Postgres is use for user authentication.
     - Set the username and password in "values.yaml".
     - If you like to receive email notification, you can set your email in "init.sql" as well as in "/src/notification-service/manifest/secret.yaml".
     - For how to get the gmail password, you can follow the step [here](https://youtu.be/g8X5AoqCJHc?t=4222).
      ```
      cd Helm_charts/Postgres
      helm install postgres .
      ```
13. **Connect to Postgres**
     - Run the command in CLI to connect with Postgres. Nodeip you can get at node's ec2 public ip.
      ```
      psql 'postgres://<username>:<pwd>@<nodeip>:30003/authdb'
      ```
     - When connected to postgres, you will be in `authdb=#`. Here you can perform SQL query such as `\d` to show any relation available inside the database. Once you run the query it will show that there is no relation inside it since we havent create any table yet.
     - To create the table for authentication, copy the query inside Postgres/templates/init.sql and paste it in `authdb=#`.
     - This query will create a new table and insert one email and user password. Run `\d ` and it will show any table available.
    <p align="center">
      <img src=
    </p>
















