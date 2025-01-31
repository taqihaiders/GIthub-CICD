# SonarCloud Setup and Integration with GitHub Actions

This section covers how to integrate **SonarCloud** for code quality analysis within the CI/CD pipeline using **GitHub Actions**.

---

## 1. Setup SonarCloud

1. Log in to your **SonarCloud** account.
2. Create an **organization**.
3. Analyze your project and create the corresponding project in SonarCloud.

---

## 2. Generate SonarCloud Token

1. Go to your **SonarCloud account** settings.
2. Under **Security**, generate a new **token** for GitHub integration.
   - Example token: `9483574d64abbd3760123299d9d7a0247ff200dc`

---

## 3. Add SonarCloud Token to GitHub Secrets

1. Go to the **GitHub repository** settings.
2. Navigate to **Actions > Secrets**.
3. Create the following repository secrets:
   - `SONAR_URL`: Your SonarCloud URL.
   - `SONAR_ORGANIZATION`: Your SonarCloud organization name.
   - `SONAR_TOKEN`: The token you generated.
   - `SONAR_PROJECT_KEY`: The project key from SonarCloud.

---

## 4. Update `main.yml` for SonarCloud Integration

Add the following steps in your GitHub Actions workflow YAML file:

```yaml
# Setup Java 17
- name: Set Java 17
  uses: actions/setup-java@v3
  with:
    distribution: 'temurin'
    java-version: '17'

# Setup SonarScanner
- name: Setup SonarQube
  uses: warchant/setup-sonar-scanner@v7

# Run SonarQube Scan
- name: SonarQube Scan
  run: sonar-scanner
    -Dsonar.host.url=${{ secrets.SONAR_URL }}
    -Dsonar.login=${{ secrets.SONAR_TOKEN }}
    -Dsonar.organization=${{ secrets.SONAR_ORGANIZATION }}
    -Dsonar.projectKey=${{ secrets.SONAR_PROJECT_KEY }}
    -Dsonar.sources=src/
    -Dsonar.junit.reportsPath=target/surefire-reports/
    -Dsonar.jacoco.reportsPath=target/jacoco.exec
    -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml
    -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/


5. Create and Configure Quality Gates
In SonarCloud, go to your Organization.
Navigate to Quality Gates and create a custom quality gate.
For example, set a condition on bugs: If the number of bugs is less than 30, it's fine.
Attach the quality gate to your organization:
Go to Administration > Quality Gates and assign the created quality gate.
6. Add Quality Gate Check to Workflow
Add another step in your main.yml to include a quality gate check in the workflow:

yaml
Copy
Edit
# Check Quality Gate status
- name: Quality Gate Check
  run: |
    sonar-quality-gate-cli -t ${SONAR_URL} -k ${SONAR_PROJECT_KEY} -s ${SONAR_TOKEN}
Conclusion
With this setup, SonarCloud will automatically analyze the quality of your code with each commit. It checks for bugs, code coverage, and adherence to style guidelines, helping to ensure high code quality before deployment.

This CI/CD pipeline includes SonarCloud integration to run code quality checks, while also using GitHub Actions to automate the process. Once the quality checks pass, your code is ready for deployment.
