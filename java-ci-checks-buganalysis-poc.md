<div align="center">
  <img src="https://github.com/user-attachments/assets/1ba6a662-c846-4af8-b996-38f756459cb0" alt="Diagram 1" width="400" style="margin-right: 10px;" />
  <img src="https://github.com/user-attachments/assets/84a1a315-138f-40de-8012-7309941f4628" alt="Diagram 2" width="400" />
</div>

# **Java Application CI Design with Bug Analysis of POC**

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-15  | 2025-05-15   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Table of Contents 
1. [Introduction](#introduction)
2. [Prerequisites](#prerequisites)
3. [POC Steps - Bugs Analysis in Java CI](#poc-steps---bugs-analysis-in-java-ci)
   - [Step 1: Sample Project Setup](#step-1-sample-project-setup)
   - [Step 2: Jenkins Freestyle Job Configuration](#step-2-jenkins-freestyle-job-configuration)
   - [Step 3: Integrate SonarQube in Jenkins](#step-3-integrate-sonarqube-in-jenkins)
   - [Step 4: Sample Jenkinsfile](#step-4-sample-jenkinsfile)
   - [Step 5: View Reports](#step-5-view-reports)
4. [Benefits](#benefits)
5. [Conclusion](#conclusion)
6. [Contact Information](#contact-information)
7. [References](#references)

## Introduction 
This POC demonstrates a Java CI pipeline using Jenkins integrated with static analysis tools like SonarQube and SpotBugs. It automates bug detection and code quality checks to help developers find and fix issues early, improving the overall reliability of the application.For to know more about SonarQube you can refer to this [Document](https://github.com/Cloud-NInja-snaatak/Documentation/blob/shubham_scrum56/commonstack/sonarqube/Documentation.md) you will get clarity of SonarQube.

## Prerequisites
| Prerequisite                  | Description                          |
|------------------------------|------------------------------------|
| Java 17                      | Installed on the build environment |
| Maven                        | Installed for build automation     |
| Git                          | Installed for version control      |
| Jenkins server               | Running and accessible             |
| SonarQube server             | Running and accessible             |
| Sample Java project          | Maven-based sample project         |
| GitHub repository            | With webhook configured            |
| Jenkins plugins              | Git plugin, Maven Integration, SonarQube Scanner |

## POC Steps - Bugs Analysis in Java CI 
### Step 1: Sample Project Setup
```bash
git clone https://github.com/opstree/java-sample-app.git
cd java-sample-app
```
### Step 2: Jenkins Freestyle Job Configuration
- **Source Code Management**: Git – Provide GitHub repo URL
- **Build Triggers**: Poll SCM or webhook
- **Build Environment**: Inject environment variables

**Build Steps**:

- `mvn clean install`
- `sonar-scanner`

### Step 3: Integrate SonarQube in Jenkins
- Jenkins → Manage Jenkins → Configure System
- Add SonarQube server URL and token
- Use `withSonarQubeEnv` in the Jenkinsfile or Freestyle job

## Step 4: Sample Jenkinsfile 
```bash
pipeline {
  agent any
  stages {
    stage('Checkout') {
      steps {
        git 'https://github.com/opstree/java-sample-app.git'
      }
    }
    stage('Build') {
      steps {
        sh 'mvn clean install'
      }
    }
    stage('Code Analysis') {
      steps {
        withSonarQubeEnv('SonarQube') {
          sh 'mvn sonar:sonar'
        }
      }
    }
  }
}
```
### Step 5: View Reports
- Navigate to SonarQube Dashboard
- See bugs, vulnerabilities, code smells, and duplication metrics

## Benefits
Benefits of using Bugs Analysis tools in Java CI are below,
- Real-time code quality feedback
- Prevents bugs before production
- Helps enforce coding standards
- Supports team collaboration
- Visual dashboard for management

## Conclusion
### **Tool Chosen: SonarQube**

**Reason**:
SonarQube provides **in-depth static code analysis**, supports multiple languages, integrates seamlessly with Jenkins and Maven, and allows setting up **quality gates** and automatic failure of builds on issues. It outperforms PMD and Checkstyle due to its **rich UI**, **multi-language support**, and **security detection features**.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## References

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [Jenkins – Official Site](https://www.jenkins.io/)                                                                                                                  | Official Jenkins documentation for setting up CI/CD automation pipelines.                                       |
| [SonarQube – Static Code Analysis Tool](https://www.sonarqube.org/)                                                                                                 | SonarQube documentation for integrating static code analysis into CI pipelines.                                 |
| [Tutorials of CI/CD Pipeline with Static Code Analysis](https://www.tutorials24x7.com/java/creating-a-cicd-pipeline-with-static-code-analysis-for-java-projects) | Step-by-step tutorial for building a Java CI/CD pipeline with static analysis tools like SonarQube and Jenkins. |




