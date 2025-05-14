<p align="center">
  <img src="https://github.com/user-attachments/assets/9ed09a96-a6f0-4b75-9ab9-25ac6b9d9e4b" alt="VCS Design" width="500"/>
</p>

# **Feature document of Buildpiper**

| Created        | Last updated      | Version         | author|  Internal Reviewer | L0 | L1 | L2|
|----------------|----------------|-----------------|-----------------|-----|------|----|----|
| 2025-05-12  | 2025-05-13   |     Version 1         |  Mohamed Tharik |Priyanshu|Khushi|Mukul Joshi |Piyush Upadhyay|

## Purpose
This document outlines the features, benefits, workflow, and best practices of BuildPiper, a powerful Continuous Integration (CI) orchestration platform designed for modern DevOps practices. It is intended for DevOps engineers, software developers, and infrastructure architects looking to streamline CI/CD workflows with scalable, secure, and automated tooling.

## Table of Contents

- [Introduction](#introduction)
- [What is BuildPiper?](#what-is-buildpiper)
- [Why We Use BuildPiper for CI Orchestration?](#why-we-use-buildpiper-for-ci-orchestration)
- [Workflow Diagram](#workflow-diagram)
- [Key Features & Advantages](#key-features--advantages)
- [Best Practices](#best-practices)
- [Conclusion](#conclusion)
- [Contact Information](#contact-information)
- [References](#references)

## Introduction 
BuildPiper is a microservices delivery platform built to help organizations implement end-to-end DevSecOps pipelines. It offers comprehensive features around CI orchestration, empowering teams to automate code integration, quality checks, vulnerability scans, and artifact management seamlessly.

## What is BuildPiper?
**BuildPiper** is a CI/CD orchestration platform that offers:

- **VCS Integration**: Works seamlessly with GitHub, GitLab, and Bitbucket
- **CI Automation**: Enables quick creation and execution of CI pipelines
- **Security Integration**: Supports SAST, DAST, and SBOM tools
- **Container Support**: Builds and publishes Docker images effortlessly
- **Notifications**: Sends alerts via Slack and Email
- **Deployment Ready**: Integrates with Kubernetes and Helm for smooth releases
- **Access Control**: Provides RBAC and detailed audit logs for governance

## Why We Use BuildPiper for CI Orchestration?
| **Key Reason**                  | **Description**                                                                 |
|-------------------------------|---------------------------------------------------------------------------------|
| Unified Pipeline Management   | Centralized platform for managing CI/CD with full visibility and traceability. |
| Microservices-first Architecture | Tailored for containerized, distributed applications and microservices.         |
| Security Integration          | Built-in support for tools like SonarQube, Trivy, Checkmarx, and Snyk.         |
| Scalability                   | Efficiently manages multiple projects and environments with low overhead.       |
| Plug & Play                   | Easily integrates with existing tools and repos—no need for major changes.     |

## Workflow Diagram
<p align="center">
<img src="https://github.com/user-attachments/assets/23de4ecc-0868-4a01-86d2-e9096a698c6c" alt="Image" width="500"/>
</p>

## Key Features & Advantages

| **Feature**                     | **Brief Description**                                                                 |
|--------------------------------|----------------------------------------------------------------------------------------|
| Centralized CI Management      | Manage pipelines via GUI or YAML without vendor lock-in.                             |
| Microservices Support          | Connect individual or grouped microservices to pipelines.                            |
| GitOps Compatible              | Enables Git-based triggers, approvals, and rollbacks.                                |
| Security & Compliance          | Supports SAST, DAST, and SBOM with compliance checks.                                 |
| Audit Trails & RBAC            | Tracks all actions and ensures secure, role-based access.                            |
| Tool Integrations              | Works with Jenkins, GitHub Actions, SonarQube, Docker, Kubernetes, JFrog, S3, etc.   |

## Best Practices
- **Use Modular Pipelines**: Break down large CI pipelines into reusable components.
- **Implement Quality Gates**: Use code coverage and SAST scans as blockers before release.
- **Leverage Notifications**: Enable Slack/email alerts for pipeline status to reduce MTTR.
- **Secure Secrets**: Use built-in or external secret managers to handle credentials.
- **Use Immutable Artifacts**: Version container images and store in a trusted registry.

## Conclusion 
BuildPiper is a powerful CI orchestration tool tailored for modern DevSecOps workflows. Its ability to tightly integrate CI, security, and artifact management makes it ideal for teams building secure, scalable, and cloud-native applications. With its intuitive interface, deep integrations, and microservice support, BuildPiper can significantly improve software delivery efficiency and security.

## Contact Information
| Name | Email address         |
|------|------------------------|
| Mohamed Tharik  | md.tharik.sanaatak@mygurukulam.co    |

## References

| Link                                                                                                         | Description                                      |
|--------------------------------------------------------------------------------------------------------------|--------------------------------------------------|
| [BuildPiper Documentation](https://www.buildpiper.io/)                                                      | Official BuildPiper Documentation                |
| [CI/CD Design Patterns](https://martinfowler.com/articles/continuousIntegration.html)                         | Martin Fowler's article on CI/CD design patterns for continuous integration. |

