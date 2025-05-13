# **VCS Setup Notification for Branch** 

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-12  | 2025-05-13   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Overview
This document is to guide the setup of branch-specific notifications in a Version Control System (VCS) like GitHub, ensuring teams get real-time alerts for key events (e.g., push or pull request) on branches like `dev`, `main`, or `release`.

## Table of Contents
- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Step-by-Step Setup Guide](#step-by-step-setup-guide)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [Reference](#reference)

## Introduction 
VCS tools support branching workflows, but without targeted alerts, critical updates may be missed. This document outlines how to configure notifications for selected branches to improve collaboration and code visibility.

## Prerequisites

| Requirement            | Details                                        |
|------------------------|------------------------------------------------|
| VCS Admin Access       | Admin rights on GitHub, GitLab, or Bitbucket  |
| Communication Tool     | Slack, Teams, Email, etc.                     |
| Webhook/Integration    | Valid webhook URL or bot setup                |
| Target Branches        | Branches like `dev`, `main`, etc.             |
| Hook Permissions       | Access to add/configure webhooks              |

## Step-by-Step Setup Guide

### Step 1: Identify Target Branches
- For setting up Notification,I am taking the GitHub tool.
- Select the branches (e.g., `main`, `dev`, `release`) that need notifications.
- Note down what type of activity should trigger a notification (e.g., push, PR, tag).

### Step 2: Configure Webhook in GitHub
1. Go to **Repository Settings → Webhooks**
2. Click **"Add webhook"**
3. Set **Payload URL** to your Slack/Webhook URL
4. Choose **Content type:** `application/json`
5. Under **Events**, select:
   - `Push events`
   - `Pull request events`
6. (Optional) Use GitHub Actions for branch-specific filtering
7. Click **Add webhook**

### Step 3: Setup Slack Notification (Example)
1. In Slack, go to **Apps → Create App**
2. Select **From Scratch**, name it, and choose a workspace
3. Enable **Incoming Webhooks**
4. Create a webhook URL and assign it to a channel (e.g., `#dev-notifications`)
5. Copy the webhook URL and use it in your GitHub webhook config

> This sets up notifications **only for specified branches** when using branch filters in GitHub Actions or CI tools.

### Step 4: Filter Notifications by Branch (GitHub Actions)
- Use GitHub Actions for advanced filtering
- Create the file in `.github/workflows/branch_notify.yml` in your git repo 
```bash
name: Branch Notification

on:
  push:
    branches:
      - tharik
  pull_request:
    branches:
      - tharik

jobs:
  notify:
    runs-on: ubuntu-latest
    steps:
      - name: Send Slack Notification
        run: |
          curl -X POST -H 'Content-type: application/json' \
          --data '{"text":"🔔 Push event on *tharik* branch!"}' \
          ${{ secrets.SLACK_WEBHOOK }}
```
This ensures only `tharik` branch events send notifications.

### Step 5: Configure GitHub Secrets

To securely store your Slack Webhook URL:

1. Go to your GitHub repository → **Settings**.
2. Navigate to **Secrets and variables** → **Actions**.
3. Click **"New repository secret"**.
4. Enter the following details:
   - **Name**: `SLACK_WEBHOOK_URL` (must match what’s used in your workflow).
   - **Value**: Paste your **Slack Webhook URL**.
5. Click **"Add secret"**.

> Use `${{ secrets.YOUR_SECRET_NAME }}` to reference it in your GitHub Actions workflow.

### Step 6: Test & Demonstrate
- Push code to the configured branch
- Verify notification appears in the target channel/tool
- Validate the branch filter is respected

## Conclusion

Branch-specific notifications improve team responsiveness and visibility into the development process. This setup ensures that stakeholders are only alerted for relevant events, reducing noise and focusing attention on high-priority branches.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## Reference

| Link                                                                                                         | Description                                                       |
|--------------------------------------------------------------------------------------------------------------|-------------------------------------------------------------------|
| [GitHub Webhooks Documentation](https://docs.github.com/en/developers/webhooks-and-events/webhooks/about-webhooks) | Official documentation on setting up and managing GitHub webhooks. |
| [GitHub Actions - Branch Filters](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#onpushpull_requestbranchestags) | How to configure GitHub Actions to trigger on specific branches.  |
| [Slack Webhooks](https://api.slack.com/messaging/webhooks)                                                  | Guide to setting up and using Incoming Webhooks in Slack.         |
| [GitLab Webhook Filters](https://docs.gitlab.com/ee/user/project/integrations/webhooks.html#filtering-push-events-by-branch) | Instructions for filtering webhook events by branch in GitLab.   |

