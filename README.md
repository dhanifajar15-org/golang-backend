# Technical Test - CI/CD and Kubernetes

## Overview

In this documentation, I detail the setup and workflow of the CI/CD process that I have implemented.

## System Overview

- I have developed a robust CI/CD system to streamline our development workflow.
- The backend applications are written in the Go programming language.

## CI/CD Implementation

- For implementing the CI/CD pipeline, I have chosen to use GitHub Actions in combination with Standard GitHub-hosted runners.
- I have established separate CI/CD pipelines for various branches, such as: feature, develop, staging, master, and hotfix.

## Reusable Workflow

- To enhance efficiency and eliminate redundancy, I have adopted a reusable workflow methodology.
- The reusable workflow allows for the invocation of multiple jobs, utilizing inputs and securely stored secrets.
- I have incorporated multiple stages into the workflow, including testing, containerization (Dockerization), and deployment to Google Kubernetes Engine (GKE).

### Testing

- The testing phase involves using the `go test -v .test/...` command.

### Dockerization

- The application is Dockerized, utilizing DockerHub as the repository for docker images.

- A dynamic tag, consisting of the environment identifier and a 7-digit SHA hash, is assigned to the Docker image. An example of such a tag is: 

  ```
  dhanifajar15/golang-backend:${{ inputs.ENVIRONMENT }}-sha-${{ steps.tags.outputs.sha_short }}
  ```

### Deployment

- **All Deployment Job except Production**

  - Deploys the Dockerized application to GKE automatically.

  **Production Deployment Job**

  - Deploys the Dockerized application to GKE via tags.
  - Triggered by tagging a version (e.g., v1.0) on the `master` branch.

## Branching Strategy

- The code flow is governed by three main branches: develop, staging, and master (production).
- Additional temporary branches, such as hotfix and feature, are used for specific purposes.

## Docker Process

- The Dockerization process involves a multi-stage build approach.

## Custom CI/CD Setup per Branch

- A distinctive aspect of our CI/CD setup is the customization for each branch, particularly focusing on wildcard branches like hotfix and feature.
- Each of these branches is equipped with dedicated jobs for  testing, Dockerization, and deployment to Kubernetes.
- Domains are generated based on the branch name and commit number, with 'nip.io' serving as the wildcard domain provider. For instance, `feature-${{github.sha}}.34.142.207.149.nip.io`.