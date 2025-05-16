![githublab](https://github.com/user-attachments/assets/a59a79d8-7e56-456b-b493-1d4b7160d170)

# **Setup VCS**

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-14  | 2025-05-16   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Table of Contents

- [Introduction](#introduction)
- [Pre-requisites](#pre-requisites)
- [Step-by-Step Setup Guide](#step-by-step-setup-guide)
  - [A. SaaS (GitHub) Setup](#a-saas-github-setup)
  - [B. Local (GitLab CE) Setup – On-Prem](#b-local-gitlab-ce-setup--on-prem)
- [SaaS vs. On-Premise Version Control](#saas-vs-on-premise-version-control)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [Reference](#reference)


## Introduction
This document provides a step-by-step guide to setting up a Version Control System (VCS) based on the recommendation made in the last sprint: GitHub for SaaS and GitLab CE for Local (On-Prem) setup. It helps teams decide which setup suits their needs and walks through both installation and configuration processes.For more detailed about VCS and VCS Setup you can refer to this repository for [Document](https://github.com/Cloud-NInja-snaatak/Documentation/blob/tharik_scrum57/commonstack/vcs/Documentation.md) and [Setup](https://github.com/Cloud-NInja-snaatak/Documentation/blob/Tharik_SCRUM-76/vcs_design/features_of_vcs/vcs_setup/README.md)

## Pre-requisites
| Tool                       | Description                                          | Required For  |
| -------------------------- | ---------------------------------------------------- | ------------- |
| Git                        | Version Control CLI                                  | Both setups   |
| GitHub Account             | Free account creation                                | SaaS setup    |
| Ubuntu Server              | Virtual/Physical machine or VM (min 2 vCPU, 4GB RAM) | On-Prem setup |
| Docker & Docker Compose    | Required for GitLab CE containerized setup           | On-Prem setup |
| Root Access / Admin Rights | To install and configure tools                       | On-Prem setup |

## Step-by-Step Setup Guide
## A. SaaS (GitHub) Setup

### Step 1. Create a GitHub Account  
Visit [github.com/join](https://github.com/join), fill in username, email, password, and verify your email.
![github](https://github.com/user-attachments/assets/d80d76e1-7b0b-45a4-844a-2c6b41885db7)

### Step 2. Create a GitHub Organization  
   Go to [https://github.com/organizations/new](https://github.com/organizations/new)  
   Choose a plan (Free is fine for most teams). 
![github2](https://github.com/user-attachments/assets/fad7ca35-0cbf-421f-b9f2-3dfb7b143a7b)
   Enter:  
   - Organization name (e.g., `team-devops`)  
   - Email address for communication.  
   Complete setup and optionally invite team members.
![github3](https://github.com/user-attachments/assets/6767c230-25d2-47ef-8529-4620259b905c)

### Step 3. Manage Organization  
   Navigate to **Your Organizations** > Select your Organization.  
   - To add members: Go to **People** tab → **Invite Member**.  
   - To create teams: Go to **Teams** → **New Team** → Define name, access, and add members.  
   Teams help manage repository access more easily.
![github4](https://github.com/user-attachments/assets/3588c99b-51a7-46f8-8da8-e374866032b9)

### Step 4. Create a Repository in the Organization  
   Go to Organization → **Repositories** → Click **New**.  
   Enter repository name, choose visibility (public/private), initialize with README.
![github5](https://github.com/user-attachments/assets/7d8b48b2-8d0f-431d-89af-6823fed30511)

### Step 5. Clone Repository Locally
```bash
git clone https://github.com/<org-name>/<repo-name>.git
cd <repo-name>
```
### Step 6. Make Changes and Push
```bash
touch hello.txt
git add .
git commit -m "Initial commit"
git push origin main
```
### Step 7. Add Collaborators to a Repository (if not using organization teams)
- Go to the repository
- Click Settings → Collaborators
- Click Add People
- Enter the GitHub username → Set access level
![github6](https://github.com/user-attachments/assets/a7771421-e7ae-49dd-a5b5-74c5cbe660d4)

## B. Local (GitLab CE) Setup – On-Prem

### Step 1. Launch an Ubuntu Server (Cloud/VM/Bare Metal)
![gitlab](https://github.com/user-attachments/assets/32fc40a2-733e-4c63-a6c3-d5565be94618)

### Step 2. Install Docker & Docker Compose
```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl start docker
sudo systemctl enable docker
```
![gitlab2](https://github.com/user-attachments/assets/222b0cbf-4449-4468-9f53-64cba93bf69c)

### Step 3. Run GitLab CE with Docker
```bash
# docker-compose.yml
version: '3'
services:
  gitlab:
    image: gitlab/gitlab-ce:latest
    restart: always
    hostname: 'gitlab.local'
    environment:
      GITLAB_OMNIBUS_CONFIG: |
        external_url 'http://gitlab.local'
    ports:
      - '80:80'
      - '443:443'
      - '22:22'
    volumes:
      - ./config:/etc/gitlab
      - ./logs:/var/log/gitlab
      - ./data:/var/opt/gitlab
```
- Start and run your multi-container Docker application in detached mode
```bash
docker-compose up -d
```
![gitlab5](https://github.com/user-attachments/assets/194f9d02-f054-4bfa-89a6-4a2d5b7c4654)

### Step 4. Access GitLab
- Open browser: http://your-server-ip

![gitlab3](https://github.com/user-attachments/assets/3501eabc-5c94-4b4e-8f8f-b6f186c6d0c9)

- Set root password on first login

![gitlab4](https://github.com/user-attachments/assets/9abb0014-a34b-4ce1-9ed2-69f86848a02a)

- Create users and projects
![gitlab7](https://github.com/user-attachments/assets/a60ce8dd-407b-461d-91f6-1e12b8c0c06c)

![Screenshot-67](https://github.com/user-attachments/assets/533fb822-f6fb-4095-940b-4aa78e6114c9)

### Step 5. Clone GitLab Repo and Push Code
```bash
git clone http://<your-server-ip>/<username>/<repo>.git
cd <repo>
echo "Hello GitLab!" > file.txt
git add . && git commit -m "Initial commit" && git push origin main
```

![gitlab8](https://github.com/user-attachments/assets/e2aaf505-a023-47a4-b8ec-0fc9a2275122)

## SaaS vs. On-Premise Version Control
This table outlines the key differences between using a SaaS-based version control system like GitHub and a self-hosted (on-premise) solution like GitLab CE. It helps evaluate based on infrastructure, cost, customization, and control.

| **Aspect**          | **SaaS (GitHub)**                                                | **On-Prem (GitLab CE)**                                                           |
| ------------------- | ---------------------------------------------------------------- | --------------------------------------------------------------------------------- |
| **Infra Ownership** | Hosted and managed by the provider (e.g., GitHub).               | Fully managed by your team, including servers and networking.                     |
| **Setup Time**      | Fast — sign up and start using in minutes.                       | Slower — setup involves Docker, servers, and networking.                          |
| **Maintenance**     | GitHub handles updates, uptime, backups.                         | You’re responsible for all maintenance and patching.                              |
| **Cost**            | Free tiers available; paid plans for extra features.             | Requires hardware, power, and skilled personnel — higher total cost of ownership. |
| **Customization**   | Limited to what GitHub allows.                                   | Fully customizable based on your needs and integrations.                          |
| **Access**          | Accessible from anywhere with internet.                          | Restricted to internal networks or VPN unless configured for public access.       |
| **Scalability**     | Automatically scaled by GitHub’s infrastructure.                 | Needs manual intervention to scale as usage grows.                                |
| **Security**        | Shared — GitHub secures infra, you secure your data/repo access. | Full responsibility — you manage everything from infra to access control.         |
| **Examples**        | GitHub, GitLab SaaS, Bitbucket Cloud.                            | GitLab CE, Gitea, Bitbucket Server.                                               |

## Conclusion
Both SaaS (like GitHub) and On-Prem (like GitLab CE) setups serve version control needs well. GitHub is great for fast, scalable, and low-maintenance workflows, ideal for most agile teams. On the other hand, GitLab CE is a robust choice if you require full control, data compliance, and internal integrations. Choose based on your team's size, infrastructure, compliance needs, and operational maturity.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## Reference

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
  | [GitHub Docs](https://docs.github.com/en)                       | Official documentation for using and managing GitHub repositories and features. |
  | [GitLab Docs](https://docs.gitlab.com/ee/)                      | Comprehensive guide for installing, configuring, and managing GitLab CE/EE.     |
   | [Gitea Docs](https://docs.gitea.com/)                           | Lightweight, self-hosted Git service documentation.                             |
| [Docker Install Guide](https://docs.docker.com/engine/install/) | Step-by-step instructions to install Docker on various platforms.               |

