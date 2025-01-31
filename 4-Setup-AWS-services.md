Here’s the refined **README.md** version for **AWS Service Setup**:

```markdown
# AWS Services Setup for CI/CD Pipeline

This guide walks through setting up the necessary AWS services to fetch source code from GitHub, build a Docker image, and store it in AWS ECR (Elastic Container Registry).

---

## 1. IAM User Creation and Setup

1. Create an **IAM User** in AWS.
2. Attach the `ecs_full_access` policy to this user to allow full access to ECS.
3. Go to the user’s **Security Credentials** and generate a **Secret Access Key**.

---

## 2. ECR Setup (Elastic Container Registry)

1. Create a new **ECR Repository**.
2. Provide a name for the repository and create it.

---

## 3. RDS Setup (Relational Database Service)

1. Create an **RDS Instance** with **MySQL**.
2. Provide a name for the RDS instance, as well as a **username** and **password**.
3. Select the **Free Tier** option and create the RDS instance.

---

## 4. EC2 Instance Setup to Initialize RDS

1. Launch an **Ubuntu EC2 instance** to initialize the RDS.
2. SSH into the EC2 instance.
3. Run the following command to install the MySQL client:
   ```bash
   apt update && apt install mysql-client
   ```

---

## 5. Configuring Security Groups

1. Go to the **RDS Security Group (SG)** and add an inbound rule for port **3306**.
2. Attach the **RDS Security Group** to the EC2 instance's security group.

---

## 6. Connect EC2 Instance to RDS

1. Copy the **RDS Endpoint**.
2. SSH back into the EC2 instance and run the following command to connect to RDS:
   ```bash
   mysql -h "rds-endpoint" -u (username) -p(password) accounts
   ```

---

## 7. Cloning Repository for RDS Data

1. Clone the repository containing the RDS data:
   ```bash
   git clone https://github.com/hkhcoder/vprofile-project.git
   ```
2. Navigate to the cloned repository and locate the **db_backup.sql** file.

---

## 8. Restore Database from Backup

1. Run the following command to restore the RDS database from the backup file:
   ```bash
   mysql -h vpro-actions.crsk4imuo6ui.ap-south-1.rds.amazonaws.com -u admin -p accounts < src/main/resources/db_backup.sql
   ```

2. Confirm that the RDS instance is set up correctly.

---

## 9. Terminate RDS Instance

After confirming the setup, you can terminate the RDS instance if it's no longer needed.

---

## 10. Adding AWS Service Details to GitHub Secrets

To securely store AWS service credentials in GitHub, add the following secrets to your GitHub repository:

1. **AWS_ACCESS_KEY_ID**: The **Access Key** created in your IAM user.
2. **AWS_ACCOUNT_ID**: Your AWS account ID.
3. **AWS_SECRET_ACCESS_KEY**: The **Secret Access Key** generated for your IAM user.
4. **RDS_ENDPOINT**: The **RDS endpoint** you copied from the RDS instance.
5. **RDS_PASS**: The **RDS password**.
6. **RDS_USER**: The **RDS username**.
7. **REGISTRY**: The **ECR registry** name.

---

## Conclusion

With this setup, you now have the necessary AWS services configured and integrated into GitHub Actions to automate the build and deployment process. The workflow will fetch source code from GitHub, build Docker images, push them to AWS ECR, and connect to the MySQL database in RDS.

Make sure to configure the GitHub secrets with the correct AWS credentials for seamless integration.

---

Feel free to fork, clone, and contribute to this project!
```

### Key Updates:
- **Concise Process Flow**: Simplified the steps for clarity.
- **Command Syntax**: Included key commands for each stage, ready for use.
- **Headings & Structure**: Organized sections with proper headings for better readability.

Let me know if you need any more adjustments!
