# CI/CD Pipeline Infrastructure Guide

## Architecture Overview
This repository defines an automated Continuous Integration (CI) pipeline demonstrating modern DevOps practices. The infrastructure-as-code implementation transitions manual workflows into an event-driven system that builds, tests, and containerizes application logic in real-time.

## Infrastructure Setup
The Jenkins automation server is provisioned on a native Linux environment and managed as a persistent `systemd` service. The CI server is deeply integrated with the host's Docker daemon, allowing the pipeline to natively build and authenticate container images without relying on nested virtualization.

## Pipeline Lifecycle and Automation
The declarative `Jenkinsfile` orchestrates the following automated lifecycle:
1. **Automated Triggering:** The pipeline utilizes SCM polling (`pollSCM`) to detect repository changes, ensuring builds are triggered automatically upon code commits.
2. **Build & Test:** Dependencies are resolved securely via `npm`, followed by automated unit testing. Pipeline execution halts immediately upon test failure.
3. **Artifact Generation:** Source code is packaged into an archived `.tar.gz` artifact.
4. **Containerization & Credential Management:** The application is containerized using a multi-stage `Dockerfile`. Registry authentication is handled securely via Jenkins Credential Binding (`withCredentials`), ensuring sensitive authentication tokens are masked and never exposed in the host environment or build logs.

## Resource Management
To ensure long-term stability and prevent disk exhaustion, the pipeline implements strict retention policies:
- **Build Discarder:** Retains only the 5 most recent builds and artifacts.
- **Workspace Teardown:** A `post` execution stage forces a `cleanWs()` operation to purge ephemeral workspace files upon completion.
