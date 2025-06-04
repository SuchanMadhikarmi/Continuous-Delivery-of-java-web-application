# Continuous-Delivery Project

## Project Description

This project demonstrates a Continuous Delivery (CD) workflow for a Java web application using Jenkins, Docker, AWS ECR, and AWS ECS. It is designed as the second part of a full CI/CD pipeline, building on the Continuous Integration setup from Part 1.

In this CD phase, the focus is on automating the delivery of application builds from staging to production environments in a controlled, realistic manner.

## Key Highlights:
Jenkins orchestrates the entire delivery process — from Docker image creation to deployment on AWS ECS.

The pipeline uses separate environments (Staging and Production) to mimic real-world DevOps workflows.

Code is first built and deployed to ECS Staging, where it is verified.

Once approved, the changes are merged into the production branch, triggering a new Jenkins job that deploys the application to ECS Production.

The project ensures environment isolation, Docker-based delivery, and manual promotion to production, following enterprise-grade practices.

This setup enables safe, predictable releases and reduces the risk of deploying untested code to production.

> **Note**: All configuration scripts including EC2 userdata and Jenkinsfile are available in the repository. Screenshots of the deployment steps are provided in the `screenshots/` folder.

---

## Tools & Technologies Used

- **CI/CD Tool**: Jenkins  
- **Container Registry**: Amazon ECR  
- **Deployment Platform**: Amazon ECS (Fargate)  
- **Build Tool**: Maven  
- **Source Control**: GitHub  
- **Containerization**: Docker  
- **Scripting**: Jenkinsfile (Pipeline-as-Code), Bash  
- **Infrastructure**: AWS EC2, ECR, ECS, IAM  
---

## Project Architecture
<p align="center">
  <img src="https://i.imgur.com/hRQSqfz.png" height="80%" width="80%" alt="Geolocation Lookup"/>
</p>

</br>

## GitHub Repository Setup

1. **Repository Selection & Webhook Update**
   - Used the existing Maven project repository from Part 1.
   - Updated the GitHub webhook to reflect the new Jenkins EC2 public IP.

2. **Folder Structure for CI/CD Pipelines**
   - Created two new folders inside the repository:
     - `StagePipeline/` – Contains the `Jenkinsfile` for staging deployment.
     - `ProdPipeline/` – Contains the `Jenkinsfile` for production deployment.

3. **Branching Strategy**
   - Established `stage` and `prod` branches for deployment automation:
     - **stage branch**: Facilitates the CI/CD process for the staging environment.
     - **prod branch**: Used for production deployment automation.

---

## Amazon ECR & IAM Setup

1. **ECR Repository Creation**
   - Created an **Amazon ECR repository** to store Docker images.

2. **IAM Role & User Setup**
   - Created a dedicated **IAM user** with permissions for:
     - Pulling images from **ECR**.
     - Managing **ECS services** and **task definitions**.

3. **Secure Credential Management**
   - Stored **AWS Access Key** and **Secret Key** securely in **Jenkins credentials** for authentication.

---

## Jenkins Environment Configuration

1. **Jenkins Plugin Installation**
   - Installed necessary plugins for AWS and container management:
     - **Docker Pipeline**
     - **CloudBees Docker Build and Publish**
     - **Amazon ECR**
     - **AWS Steps Pipeline**

2. **Jenkins EC2 Instance Setup**
   - Installed:
     - **Docker Engine** for containerized build processes.
     - **AWS CLI** for seamless interaction with AWS services.

3. **User Permissions & Access**
   - Added the `jenkins` user to the **Docker group** to allow Jenkins to execute Docker commands.

---

## Staging Deployment Pipeline (StagePipeline/)

### **Automated Build & Deployment Process**

1. **Jenkinsfile Execution Steps**
   - Jenkins pulls the **Maven project** from GitHub (`stage` branch).
   - Builds a **Docker image** from the project source code.
   - **Authenticates** with ECR using stored AWS credentials.
   - Tags the image with a **-stage** suffix.
   - Pushes the image to the **ECR repository**.

2. **ECS Staging Environment Configuration**
   - Created a **staging ECS cluster** with:
     - **Task Definition** – Defines container configurations.
     - **ECS Service** – Ensures high availability and auto-scaling.
     - **Application Load Balancer (ALB)** – Routes traffic to the deployed app.

3. **Deployment Verification**
   - Configured ECS to **auto-pull Docker images** from ECR.
   - Jenkins deploys the containerized app to ECS.
   - Verified successful deployment via the **staging ALB URL**.

---

## Production Deployment Pipeline (ProdPipeline/)

### **Deployment Strategy with Git Flow**

1. **Branching & CI/CD Trigger**
   - After staging validation, the `stage` branch is **manually merged** into `prod`.
   - Jenkins **monitors** the `prod` branch for changes.

2. **Jenkinsfile Execution for Production**
   - Jenkins detects the `prod` branch update.
   - **Pulls the latest image from ECR** tagged as `-stage`.
   - **Retags** the image with a `-prod` suffix.
   - Deploys the image to **ECS production environment**.

3. **ECS Production Environment Configuration**
   - Created a **production ECS cluster** with:
     - **Task Definition** – Specifies app deployment configuration.
     - **ECS Service** – Ensures redundancy and scaling.
     - **Application Load Balancer (ALB)** – Handles incoming traffic.

4. **Final Deployment & Validation**
   - Jenkins **automatically deploys** the image from ECR to ECS.
   - Production deployment verified via the **production ALB endpoint**.

---

##  Screenshots

All relevant screenshots can be found inside the `screenshots/` folder.


---

##  Learning Outcomes

- Understood the difference between **CI** and **CD** pipelines.
- Practiced branching strategies for real-world delivery pipelines.
- Automated the deployment process using **Jenkins + ECR + ECS**.
- Built hands-on knowledge in containerized delivery and deployment rollouts.
- Strengthened knowledge of **Pipeline-as-Code** and **AWS DevOps workflows**.

---

