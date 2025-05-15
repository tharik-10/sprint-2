<p align="center">
<img src="https://github.com/user-attachments/assets/f2604b76-0642-4711-b976-9415e8b6b64c" alt="Github Actons" width="500"/>
</p>

# **Commit Sign off in Generic CI operation of POC**

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-15  | 2025-05-15   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Table of Contents
- [Introduction](#introduction)
- [Prerequisites](#prerequisites)
- [POC Steps – Commit Signoff in CI](#poc-steps--commit-signoff-in-ci)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [Reference](#reference)

## Introduction 
- This document provides a detailed Proof of Concept (POC) demonstrating how **Commit Signoff** can be enforced within a CI Generic Operation to ensure code traceability, legal compliance, and contribution accountability.
- Commit Signoff is an essential practice in collaborative software development, especially in open-source and enterprise-grade projects. This POC shows how to implement a check for the `Signed-off-by` line in commits and integrates it into a CI workflow using **GitHub Actions**.
For more detail about Commit Sign Off refer to this repository [link]().

## Prerequisites

Before running this POC, ensure the following requirements are met:

| Requirement             | Details                                                                 |
|-------------------------|-------------------------------------------------------------------------|
| Git Installed           | Git CLI is available on the local machine                               |
| GitHub Repository       | A GitHub repository (public or private) where the workflow will be tested |
| GitHub Actions Knowledge| Familiarity with creating and managing GitHub Actions workflows          |
| Basic Git Knowledge     | Familiarity with Git commits and pushing to remote                      |

## POC Steps – Commit Signoff in CI

### Step 1: Clone a GitHub Repository
```bash
git clone https://github.com/your-username/your-repo.git
cd your-repo
```
### Step 2: Create a New Branch and Make Changes
```bash
git checkout -b feature/commit-signoff-demo
echo "POC for Commit Signoff" > poc.txt
git add poc.txt
```
### Step 3: Commit with Signoff
Use the -s or --signoff flag to add a signoff line automatically:
``bash
git commit -s -m "Add POC for commit signoff"
```
This will include a line like:
```text
Signed-off-by: Your Name <your.email@example.com>
```
### Step 4: Add CI Workflow to Enforce Signoff
Create a file named .github/workflows/ci-signoff.yml in the root of your repo with the following content:
```bash
# .github/workflows/ci-signoff.yml
name: CI Pipeline with Commit Signoff Check

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]

jobs:
  validate-commit-signoff:
    name: Check Commit Signoff
    runs-on: ubuntu-latest
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Check for Signed-off-by in Last Commit
        run: |
          SIGNED_OFF=$(git log -1 --pretty=%B | grep -i "Signed-off-by")
          if [ -z "$SIGNED_OFF" ]; then
            echo "❌ Commit is missing 'Signed-off-by' line."
            exit 1
          else
            echo "✅ Commit is signed off."
          fi

  build-and-test:
    name: Lint, Test, and Build
    runs-on: ubuntu-latest
    needs: validate-commit-signoff
    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Run Linting
        run: echo "🔍 Linting..."

      - name: Run Tests
        run: echo "🧪 Running tests..."

      - name: Build Application
        run: echo "📦 Building..."
```
### Step 5: Push to GitHub
```bash
git push origin feature/commit-signoff-demo
```
### Step 6: Trigger the Workflow
Create a Pull Request or push to the target branch. The CI pipeline will run and:

- Pass ✅ if the commit contains a signoff line
- Fail ❌ if the commit is missing the signoff line

## Conclusion

This POC shows that enforcing Commit Signoff in CI is a simple yet powerful way to ensure:

- Legal and license compliance  
- Developer accountability  
- A secure, trusted codebase  

Automating this check in GitHub Actions (or other CI tools) helps maintain code quality and compliance before merging.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## Reference

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [Git Docs - Signoff Option](https://git-scm.com/docs/git-commit#Documentation/git-commit.txt--s) | Official Git documentation on using the `--signoff` flag.         |
| [GitHub Actions Docs](https://docs.github.com/en/actions)                                        | Guide for creating and managing GitHub Actions workflows.         |
