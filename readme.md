# JFrog Home Assignment - Secure DevSecOps Pipeline

**Candidate:** Michael Salami  
**JFrog Contact:** Tal Etinger (Field CTO, EMEA)  

## Overview

This project demonstrates a secure CI pipeline for building and releasing a Java application (Spring PetClinic) as a Docker image, using **GitHub Actions** and the **JFrog Platform** (Artifactory + Xray).

The pipeline achieves:
- Secure dependency resolution (via JFrog virtual repo)
- Shift-left scanning (Frogbot)
- Artifact build & push to Artifactory
- Automatic Xray security scan + quality gate enforcement
- SBOM export (bonus)
- Traceability and auditability of the software supply chain

## Pipeline Overview

Triggered on **push** to `main` or **pull requests** to `main`.

1. **Checkout code**
2. **Setup JDK 17** + Maven cache
3. **Build & verify** (compile, test, package JAR)
4. **Frogbot** shift-left dependency scan
5. **Secure Maven resolution** — deps pulled from JFrog virtual repo
6. **Build Docker image**
7. **Push to Artifactory** (triggers automatic Xray scan + quality gate)
8. **Explicit Xray scan** + SBOM export (`xray-scan.json`)
9. **Simulate deploy** (echo kubectl command)

**Quality gates (bonus)**: Xray policy blocks high/critical vulnerabilities on push (set in JFrog UI → Policies/Watches on `local-docker` repo).

## How to Run / Test Locally

### Prerequisites
- Docker Desktop
- JFrog CLI (optional for local test: `curl -fL https://getcli.jfrog.io | sh`)

### Steps
1. Clone the repo:
   ```bash
   git clone https://github.com/MikeG1t/spring-petclinic.git
   cd spring-petclinic
