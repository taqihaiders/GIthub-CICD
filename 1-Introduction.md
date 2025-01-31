# GitOps CICD with GitHub Actions

This project demonstrates a Continuous Integration and Continuous Deployment (CICD) workflow using **GitHub Actions**. The workflow includes testing code quality, building Docker images, pushing images to AWS ECR, and deploying the application to Amazon ECS with database integration.

---

## Workflow Overview

### **1. GitHub Setup**
- **Fork Repository:** Start by forking the source GitHub repository.
- **SSH Integration:** Set up SSH keys for secure access and authentication.
- **VS Code Integration:** Clone the repository into VS Code for development and commits.

### **2. Test Code**
- **Workflow and Job Definition:** Define jobs in a GitHub Actions workflow file.
- **Maven & Checkstyle:** Run Maven tests and apply Checkstyle rules to ensure coding standards are met.
- **Sonar Scanner & Sonar Cloud:** Scan the code using SonarQube to analyze code quality and integrate it with SonarCloud for detailed reporting.

### **3. Build and Upload Image**
- **Job Definition:** Create a second job in the GitHub Actions workflow.
- **Docker Build:** Build a Docker image for the application.
- **AWS ECR Push:** Push the Docker image to Amazon Elastic Container Registry (ECR).

### **4. Deploy to ECS**
- **ECS Task Definition:** Deploy the Docker container to Amazon ECS.
- **RDS Integration:** Configure Amazon RDS to provide a database for the application container.
- **Load Balancer:** Provide public access to the application through an Application Load Balancer (ALB) URL.

---

## Workflow Architecture

### **First Job: Code Testing**
1. **Fetch Code:** Pull the source code from the GitHub repository.
2. **Run Maven Tests and Checkstyle:**
   - Validate the code using Maven.
   - Apply Checkstyle to ensure coding standards.
3. **SonarQube Analysis:**
   - Perform static code analysis using SonarQube.
   - Upload the analysis to SonarCloud for detailed quality checks.

### **Second Job: Build and Deployment**
1. **Fetch Code:** Pull the repository's source code.
2. **Docker Image Build:**
   - Use the Dockerfile to build an application image.
   - Tag and push the image to AWS ECR.
3. **ECS Deployment:**
   - Deploy the image to an ECS cluster.
   - Configure ECS task definitions for running the container.
4. **Database Integration:**
   - Provide database access to the container using Amazon RDS.
5. **Application Access:**
   - Expose the application through an ALB URL for end-users.

---

## Summary of Steps
1. Fork the repository and integrate it with VS Code for local commits.
2. Create a GitHub Actions workflow file with two main jobs:
   - **Code Testing:** Ensures code quality and standards with Maven, Checkstyle, and SonarQube.
   - **Build and Deploy:** Builds the Docker image, pushes it to AWS ECR, and deploys it to ECS.
3. Ensure the container can access the RDS database and expose the application using a load balancer.

---

## Architecture Diagram
The following components are part of this architecture:
- **GitHub Actions:** Orchestrates the entire CICD workflow.
- **SonarQube and SonarCloud:** Ensure high code quality.
- **Docker:** Packages the application into a container.
- **AWS ECR:** Stores Docker images securely.
- **Amazon ECS:** Deploys and manages the application container.
- **Amazon RDS:** Provides database services to the application.
- **Load Balancer:** Facilitates public access to the deployed application.

---

Feel free to review this file, and let me know if you'd like to add any more details or diagrams. Once finalized, you can push this `.md` file to your GitHub repository.

