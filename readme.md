# JFrog Home Assignment - Secure DevSecOps Pipeline

**Candidate:** Michael Salami 

## Overview

This project demonstrates a secure CI pipeline for building and releasing a Java application (Spring PetClinic) as a Docker image, using **GitHub Actions** and the **JFrog Platform** (Artifactory + Xray).

The pipeline achieves:
- Secure dependency resolution (via JFrog virtual repo)
- Shift-left scanning (Frogbot)
- Artifact build & push to Artifactory
- Automatic Xray security scan + quality gate enforcement
- Scan export. This was also carried out via the UI, (bonus)
- Traceability and auditability of the software supply chain

## Pipeline Overview

Triggered on **push** to `main` or **pull requests** to `main`.

1. **Checkout code**
2. **Setup JDK 17** + Maven cache
3. **Build & verify** (compile, test, package JAR)
4. **Frogbot** shift-left dependency scan
5. **Secure Maven resolution** — deps pulled from JFrog virtual repo (not needed, as pom.xml was changed to always use Artifactory for deps. Just left it for completeness)
6. **Build Docker image**
7. **Push to Artifactory** (triggers automatic Xray scan + quality gate)
8. **Explicit Xray scan** + JSON export (`xray-scan.json`)
9. **Simulate deploy** (echo kubectl command)

**Quality gates (bonus)**: Xray policy blocks high/critical vulnerabilities on push (set in JFrog UI → Policies/Watches on `local-docker` repo).


### Steps to run
1. Clone the repo:
   ```bash
   git clone https://github.com/MikeG1t/spring-petclinic.git
   cd spring-petclinic

  
**Docker:**

 1. **Load the .tar file you in the zip sent over**
    docker load < petclinic.tar

4. **Run the container (maps port 8080 on your Mac to 8080 in container)**
   docker run -d -p 8080:8080 --name petclinic trialsiq0nr.jfrog.io/local-docker/petclinic:16622337b758abafffb51d42321c1803c657479dbdb

3. **Check it's running**
   docker ps

4. **Open in browser**
   open http://localhost:8080

**Kubernetes**

1. **Apply the deployment + service YAML you created**
   kubectl apply -f deploy.yaml

2. **Verify resources**
   kubectl get deployments
   kubectl get pods
   kubectl get svc

3. **Wait for pod to be Running (check status)**
   kubectl get pods -w   # press Ctrl+C when Ready

4. **Access the app locally (port-forward the service)**
   kubectl port-forward svc/petclinic-service 8080:80

5. **Open in browser**
   open http://localhost:8080

6. **When done, delete**
   kubectl delete -f deploy.yaml

