![snyk](https://github.com/user-attachments/assets/27bcfa76-aef2-49b0-a618-90e3095732ee)

# **POC of Dependency Scanning for Python CI Checks**
| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-16  | 2025-05-17   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Table of Contents

## Introduction
This document presents a Proof of Concept (POC) demonstrating the implementation of a Python-based Continuous Integration (CI) pipeline for two microservices in the OT-MICROSERVICES ecosystem:

- `attendance-api`: A Flask-based API that manages employee attendance records.For more detailed about attendenace-api you can refer to this [Document](https://github.com/Cloud-NInja-snaatak/Documentation/blob/Shubham_SCRUM-72/ot_ms_understanding/application/attendance/documentation/README.md)
- `notification-worker`: A Python-based background worker that sends notifications via email and SMS.For more detailed about notification-worker you can refer to this [Document](https://github.com/Cloud-NInja-snaatak/Documentation/blob/rajeev_SCRUM-68/ot_ms_understanding/application/notification/documentation/README.md)

This POC integrates code quality checks, unit testing, coverage analysis, and dependency vulnerability scanning using Snyk, while notifying stakeholders via Slack or email.

## Prerequisites
 Requirement         | Description                                                                 |
|---------------------|-----------------------------------------------------------------------------|
| Python Version       | Python 3.10 or higher                                                       |
| Jenkins Plugins      | Pipeline, Slack, Mailer, HTML Publisher                                     |
| GitHub Repositories  | [attendance-api](https://github.com/OT-MICROSERVICES/attendance-api.git), [notification-worker](https://github.com/OT-MICROSERVICES/notification-worker.git) |
| Snyk Setup           | Snyk account and API token configured on Jenkins agent node                |
| Slack Integration    | Slack webhook configured in Jenkins                                        |

## POC Steps - Dependency Scanning in Python CI
### Step 1: Jenkins Setup
**1. Install Required Plugins in Jenkins**
  - Go to: `Manage Jenkins → Plugins`
  - Install:
    - **Pipeline**
    - **HTML Publisher**
    - **Slack Notification**

**2. Create Jenkins Credentials for Snyk**
- Go to: `Manage Jenkins → Credentials`
- Add a **Secret Text** credential:
  - **ID**: `SNYK_TOKEN`
  - **Secret**: Your Snyk API token (from `Snyk account → Settings → API Token`)
  
### Step 2: Install Snyk on Jenkins Agent
**Log in to your Jenkins agent node** (Ubuntu/Debian):
```bash
curl https://static.snyk.io/cli/latest/snyk-linux -o snyk
chmod +x snyk
sudo mv snyk /usr/local/bin/
snyk --version
```
### Step 3: Create a Jenkins Pipeline Job
1. Go to **Jenkins → New Item**
2. Choose **"Pipeline"**, name it e.g., `python-ci-snyk-scan`, click OK.
3. Under **Pipeline Definition**, choose **"Pipeline script from SCM"** and paste the Jenkinsfile (see next step).
4. Add the `SCM` as `Git` and add the Project repository and Branch.

### Step 4: Jenkinsfile with Snyk Integration
Use this **Declarative Jenkinsfile** that:
- Checks out both attendance-api and notification-worker
- Installs dependencies
- Runs tests, linting, and Snyk scans
- Sends Slack notification
```bash
pipeline {
  agent any

  environment {
    SNYK_TOKEN = credentials('SNYK_TOKEN')
  }

  stages {
    stage('Checkout Repositories') {
      steps {
        dir('attendance-api') {
          git url: 'https://github.com/OT-MICROSERVICES/attendance-api.git', branch: 'main'
        }
        dir('notification-worker') {
          git url: 'https://github.com/OT-MICROSERVICES/notification-worker.git', branch: 'main'
        }
      }
    }

    stage('Setup Python and Install Dependencies') {
      steps {
        dir('attendance-api') {
          sh '''
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install flake8 pytest coverage snyk
          '''
        }
        dir('notification-worker') {
          sh '''
            python3 -m venv venv
            source venv/bin/activate
            pip install -r requirements.txt
            pip install flake8 pytest coverage snyk
          '''
        }
      }
    }

    stage('Lint Check') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && flake8 .'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && flake8 .'
        }
      }
    }

    stage('Test & Coverage') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && pytest --cov=. --cov-report=xml'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && pytest --cov=. --cov-report=xml'
        }
      }
    }

    stage('Snyk Vulnerability Scan') {
      steps {
        dir('attendance-api') {
          sh 'source venv/bin/activate && snyk test --file=requirements.txt'
        }
        dir('notification-worker') {
          sh 'source venv/bin/activate && snyk test --file=requirements.txt'
        }
      }
    }
  }

  post {
    success {
      slackSend(channel: '#ci-status', message: "✅ CI Passed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
    }
    failure {
      slackSend(channel: '#ci-status', message: "❌ CI Failed: ${env.JOB_NAME} #${env.BUILD_NUMBER}")
    }
  }
}
```
###  Step 5: Run the Pipeline
1. Save the Jenkins job.
2. Click **“Build Now”**.
3. Observe:
- Repositories are checked out.
- Python dependencies are installed.
- Linting and tests are run.
- Snyk scans for vulnerabilities.
- Slack receives notification of success/failure.

### Step 6: Validate Results
- **Lint Output** in Console logs.
- **Test & Coverage Report** (optionally archive).
- **Snyk Scan Output** will show vulnerable packages and recommendations.
- **Slack Notification** confirms build status.
 
## How it Works
### **Scenario: CI Workflow for attendance-api and notification-worker**
1. Developer pushes new commit to the main branch of attendance-api.
2. Jenkins automatically triggers the pipeline.
3. Pipeline perform:
   - Checks out both repositories
   - Creates Python virtual environments and installs dependencie
   - Lint check (e.g., detects unused import)
   - Unit tests (e.g., verifies API endpoints)
   - Snyk scan (e.g., detects vulnerability in flask) and fails if critical
   - Sends success/failure message to Slack
4. Simultaneously, notification-worker CI pipeline ensures all libraries used to send emails/SMS are safe and tests pass.

## Best Practice
| Practice | Description |
|----------|-------------|
| Use virtual environments or containers | Ensures clean, isolated environments for each CI build. |
| Fail builds on critical issues | Configure pipeline to fail on flake8 errors or Snyk-detected high/critical vulnerabilities. |
| Enforce coverage threshold | Maintain minimum 80% test coverage to ensure code quality. |
| Secure credentials | Use Jenkins credentials plugin to manage secrets like Snyk tokens. |
| Branch protection rules | Enforce CI pass checks before allowing merges to `main` via GitHub settings. |

## **Conclusion**
### **Tool Chosen:** Snyk  
**Reason:** Snyk is selected for its **developer-friendly interface**, **seamless Jenkins integration**, and **powerful CLI support** that fits well into CI pipelines. Unlike other dependency scanning tools such as OWASP Dependency-Check or Safety, Snyk provides **real-time vulnerability detection with remediation advice**, supports a **wide range of ecosystems including Python**, and integrates directly with GitHub for PR scanning. Its **detailed reports** and **ability to break builds on high-severity issues** make it ideal for maintaining secure dependencies in modern microservices architectures.


## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## References

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [Snyk Documentation](https://docs.snyk.io/)              | Official docs for Snyk, used for vulnerability scanning in dependencies. |
| [Safety CLI Tool](https://pyup.io/safety/)               | Lightweight Python CLI for scanning dependencies via PyUp database.       |
| [OT-MICROSERVICES GitHub](https://github.com/OT-MICROSERVICES) | GitHub organization containing the Attendance and Notification API code.  |

