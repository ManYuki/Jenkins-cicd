# Jenkins CI/CD Pipeline Configuration

## Project Overview
This repository contains the infrastructure-as-code and application logic for an automated Continuous Integration (CI) pipeline. The implementation transitions manual code integration into an automated workflow, validating commits in real-time to ensure software reliability. 

## Environment Setup
The continuous integration infrastructure is hosted natively on an Ubuntu Linux environment. The Jenkins automation server was provisioned via the APT package manager, configured with the OpenJDK 21 runtime, and is managed as a persistent `systemd` background service to ensure stable and performant pipeline execution.

## Pipeline Architecture
The workflow is defined declaratively using a `Jenkinsfile` and handles a Node.js application through the following automated stages:
1. **Fetch Code:** Pulls the latest source from the version control system.
2. **Build:** Installs application dependencies securely via `npm install`.
3. **Test:** Executes the automated test suite using Jest. The pipeline is configured to fail the build if any unit tests fail.
4. **Package / Artifact:** Archives the application code and dependencies into a deployable `.tar.gz` artifact.

## Technical Implementation Details
To ensure the pipeline had the correct runtime context, the Jenkins environment was extended using the NodeJS plugin. The runtime was mapped globally and invoked within the `tools` block of the declarative pipeline, allowing the Jenkins agent to natively execute `npm` commands without manual path configurations on the host server.
