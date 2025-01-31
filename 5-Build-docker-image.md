Here’s a refined version of the **Docker Build and ECS Setup** documentation in **Markdown** format:

```markdown
# Docker Image Build and ECS Setup

This guide explains the process of building a Docker image, uploading it to Amazon ECR, and running it in ECS (Elastic Container Service) using GitHub Actions.

---

## 1. Update application.properties File

Before building the Docker image, we need to configure the `application.properties` file with the correct RDS details (username, password, and endpoint). To automate this, we’ll use a separate job in the GitHub Actions pipeline that uses the `sed` command to update these values.

### Example Workflow Job for Updating `application.properties`:

```yaml
BUILD_AND_PUBLISH:
    needs: Testing
    runs-on: ubuntu-latest
    steps:
      - name: Code checkout
        uses: actions/checkout@v4

      - name: Update application.properties file
        run: |
          sed -i "s/^jdbc.username.*$/jdbc.username\=${{ secrets.RDS_USER }}/" src/main/resources/application.properties
          sed -i "s/^jdbc.password.*$/jdbc.password\=${{ secrets.RDS_PASS }}/" src/main/resources/application.properties
          sed -i "s/db01/${{ secrets.RDS_ENDPOINT }}/" src/main/resources/application.properties
```

This job updates the database connection details in the `application.properties` file using the `sed` command.

---

## 2. Build Docker Image and Upload to ECR

Next, we'll build the Docker image and upload it to Amazon ECR. Search for **Docker ECR** in the GitHub Marketplace, and select the **appleboy/docker-ecr-action** action.

### Example Workflow Code for Building and Uploading Docker Image:

```yaml
- name: Build & Upload image to ECR
  uses: appleboy/docker-ecr-action@master
  with:
    access_key: ${{ secrets.AWS_ACCESS_KEY_ID }}
    secret_key: ${{ secrets.AWS_SECRET_ACCESS_KEY }}
    registry: ${{ secrets.REGISTRY }}
    repo: actapp
    region: ${{ env.AWS_REGION }}
    tags: latest,${{ github.run_number }}
    daemon_off: false
    dockerfile: ./Dockerfile
    context: ./
```

### Steps:
1. This action will build the Docker image using the `Dockerfile` in your project and push it to ECR.
2. Make sure to add **ECR permissions** to your IAM user. If the build fails, check your IAM permissions.
3. After successful build, check your **ECR** for the newly created Docker images.

---

## 3. Setting Up ECS (Elastic Container Service)

Now, let's create an ECS cluster and configure it to run the Docker container.

### Step 1: Create an ECS Cluster

1. Go to **ECS** in the AWS Management Console.
2. Create a new **Cluster**:
   - Provide a name for your cluster.
   - Select **Fargate** as the infrastructure.
   - Add a tag if necessary and create the cluster.

### Step 2: Create Task Definition

A **Task Definition** tells ECS what container to run and where to pull the image from.

1. Go to the **Task Definitions** section in ECS.
2. Click **Create new Task Definition**.
3. Select **Fargate** and provide a name for the task definition.
4. Add container information:
   - Name your container.
   - Provide the **ECR Image URI** for the container.
   - Set the container port to `8080`.
5. Enable **CloudWatch Logs** for monitoring the container logs.
6. Assign IAM permissions for CloudWatch during task definition creation.
7. Click **Create** to save the task definition.

### Step 3: Create and Run ECS Service

1. Go to your **Cluster** and click on the **Services** tab.
2. Click **Create** to start a new service.
3. Provide a name for the service and use the default settings.
4. For the **Security Group (SG)**, create a new SG and allow ports **80** and **8080** from anywhere.
5. Select **Application Load Balancer** and create a target group for your application.
6. Add tags and create the service.

---

## 4. Configure RDS Security Group

To allow your ECS service to connect to the **RDS** instance, add an inbound rule in the RDS security group:

1. Go to the **RDS Security Group**.
2. In the **Inbound Rules**, allow port **3306** from the ECS **Security Group**.

---

## Conclusion

With these steps, you have successfully set up a GitHub Actions workflow to build and push a Docker image to Amazon ECR, then deploy it on ECS using a load-balanced service. Your ECS service will be able to connect to the MySQL database hosted on **RDS**.

---

Feel free to modify the workflow as needed to match your project requirements.
```

### Key Updates:
- **Consolidated Information**: Clear steps from Docker build to ECS setup.
- **Simple Workflow**: Provides code snippets ready for use.
- **Security Best Practices**: Emphasized the need for proper IAM permissions and RDS security group configuration.

Let me know if you need further adjustments or additional details!
