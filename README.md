# CI/CD pipelines for Flask Application

This repository contains a simple Python Flask application demonstrating two distinct CI/CD pipelines:
1. Jenkins Pipeline
2. GitHub Actions Pipeline

## Project Prerequisites
- **Python 3.9+** (For local execution and testing)
- **Git**
- **Docker & Docker Compose** (Optional, but used here to easily deploy Jenkins locally)
- A GitHub Account

---

## 1. Jenkins CI/CD Pipeline

### Setup and Configuration
Since Jenkins requires Python installed to build and test our Flask application, we use a customized Docker container.

**Steps to run Jenkins locally:**
1. Navigate to the `jenkins-setup/` directory.
2. Run `docker-compose up -d --build`. This starts a Jenkins server on port 8080 with Python, Pip, and Venv pre-installed.
3. Access Jenkins at `http://localhost:8080` and unlock it using the initial admin password from `docker logs jenkins_server`.
4. Install the **"Suggested Plugins"** and create an admin user.
5. Create a new **Pipeline** Job named `flask-app-pipeline`.
6. Under the pipeline definition, choose `Pipeline script from SCM`, specify `Git`, and provide the repository URL. Ensure the script path is `Jenkinsfile`.

### Pipeline Stages
Our `Jenkinsfile` contains the following stages:
- **Build**: Creates a virtual environment and installs dependencies using `pip`.
- **Test**: Runs unit tests mapped in `test_app.py` using `pytest`.
- **Deploy**: If tests pass without error, triggers a deployment to the staging environment (simulated).

### Pipeline Execution Screenshots
*(Insert Screenshots here showing Build, Test, and Deployment stages in Jenkins UI)*

---

## 2. GitHub Actions CI/CD Pipeline

### Setup and Configuration
GitHub actions is configured locally directly via the `.github/workflows/main.yml` file.

**Branches:**
- `main` branch
- `staging` branch

**Secrets:**
GitHub Secrets are used to mask sensitive data needed for deployments.
1. Navigate to `Settings -> Secrets and variables -> Actions`.
2. Add a new repository secret named `DEPLOYMENT_TOKEN` (value can be any sample string).
3. These are securely referenced inside the `main.yml` workflow via `${{ secrets.DEPLOYMENT_TOKEN }}`.

### Workflow Jobs
The implemented GitHub actions workflow (`.github/workflows/main.yml`) includes:
- **Install Dependencies**: Fetches `pip` requirements.
- **Run Tests**: Executes `pytest` to guarantee code integrity.
- **Build**: Prepares the application (simulated build step).
- **Deploy to Staging**: This step automatically executes **only** when a push is made to the `staging` branch.
- **Deploy to Production**: Deploys to a production environment when changes happen on main branch.

### GitHub Actions Execution Screenshots
*(Insert Screenshots here showing the successful execution of your GitHub Actions workflows)*