# **Setup Commit & PR (Pull Request) workflow**

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-14  | 2025-05-14   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Table of Contents
- [Introduction](#introduction)
- [Demonstration](#demonstration)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [Reference](#reference)

## Introduction 
This document outlines the setup of a Commit and Pull Request (PR) Workflow for a project using GitHub and Jenkins CI.For more detail about PR you can refer to this [PR Documnet](https://github.com/Cloud-NInja-snaatak/Documentation/blob/aditya_SCRUM-88/vcs_design/pr/pr_documantation/README.md) and [PR POC Document](https://github.com/Cloud-NInja-snaatak/Documentation/blob/Tharik_SCRUM-87/vcs_design/pr/pr_poc/README.md) 

## Demonstration
### Acceptance criteria for PR approvals
#### Step 1: PR should have source branch as feature-XXX and target branch should be main

![pr_poc4](https://github.com/user-attachments/assets/a14e83d6-7a7a-489f-b9f9-5e500da6f9aa)

#### Step 2: Two reviewers should give sign off

![pr_poc2](https://github.com/user-attachments/assets/752c2336-7071-4c88-8840-7fe937d88242)

#### Step 3: System(Jenkins) should give sign off

![pr_poc3](https://github.com/user-attachments/assets/fb4e6f94-7c48-4bca-a431-6570c1667850)
![pr_poc5](https://github.com/user-attachments/assets/bee911a2-6261-489b-8261-b471279b581e)

#### Step 4: Code merge should be validated

![pr_poc6](https://github.com/user-attachments/assets/75722b2b-630b-4231-8b8d-f1938e06c3d9)

#### Step 5: Lead should be able to merge

![pr_poc7](https://github.com/user-attachments/assets/2bd6cbc8-b36b-4c03-b378-ff7d1bc55eac)

## Conclusion
This setup enforces a secure and quality-driven PR process using GitHub and Jenkins. It prevents direct merges, ensures proper review, and automates validation—boosting code quality and team collaboration.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## Reference

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [GitHub Docs – About Rulesets](https://docs.github.com/en/repositories/configuring-branches-and-merges-in-your-repository/managing-rulesets/about-rulesets) | Official GitHub documentation explaining branch protection rulesets, how to configure them, and enforce code reviews, status checks, and restrictions. |
