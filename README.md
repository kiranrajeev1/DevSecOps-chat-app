# Chatty - Real Time Chat Application
## DevSecOps
### Kubernetes | AWS EKS | Jenkins | Docker | OWASP | Trivy | SonarQube | Helm | Prometheus | Grafana | ArgoCD

## Project Overview

Chatty is a real-time chat application that showcases complete DevOps workflows — from containerization to deployment and monitoring and venturing into DevSecOps using security practices as well for a three tier application. Starting with a base application, the focus was on building a reliable, automated pipeline and cloud-native infrastructure, highlighting how DevSecOps practices can bring scalability, security, and efficiency to modern applications.

Key DevOps practices implemented:

- Containerization with Docker
- Automated CI/CD pipelines using Jenkins
- Security scanning with OWASP Dependency Check, Trivy, and SonarQube
- Kubernetes (EKS on AWS) for orchestration and scaling
- Helm charts for simplified deployment and versioning
- Monitoring & observability with Prometheus and Grafana
- AWS Cloud Infrastructure for hosting and scaling
- ArgoCD for kubernetes based GitOps
- SMTP service for email alerts

This project demonstrates the ability to take an existing application and make it cloud-ready and enterprise-grade using AWS EKS, CI/CD automation, security best practices, and monitoring. 

---

## Architecture

```mermaid
flowchart TD
    subgraph User
    A[User Browser]
    end

    subgraph App
    B[Frontend Service]
    C[Backend Service]
    end

    subgraph DevOps
    D[Docker]
    E[Jenkins CI/CD]
    F[Kubernetes on AWS EKS]
    G[Helm Charts]
    H[Prometheus]
    I[Grafana]
    J[Argo CD]
    end

    A --> B --> C
    C --> D --> F
    E --> F
    F --> H --> I
    F --> G
    J --> F
    J --> G
```
---

## Tech Stack

### Cloud & Orchestration
- **AWS Cloud** -> Hosting & infrastructure
- **Amazon EKS (Elastic Kubernetes Service)** -> Kubernetes orchestration and scaling

### Containerization & Deployment
- **Docker** -> Containerization of frontend and backend
- **Helm** -> Deployment templates and versioning
- **Kubernetes** -> Orchestration of services

### CI/CD & Automation
- **Jenkins** -> Continuous integration and delivery pipelines
- **Argo CD** -> GitOps-based continuous deployment to Kubernetes

### Security & Code Quality
- **OWASP Dependency Check** -> Dependency vulnerability scanning
- **Trivy** -> Container image security scanning
- **SonarQube** -> Code quality and static analysis

### Monitoring & Observability
- **Prometheus** -> Metrics collection and monitoring
- **Grafana** -> Dashboards and visualization

---

## Implementation

1. **AWS Configuration**

An AWS Instance was created with all nessecary configurations and limitations to serve as the master machiene for the project.

2. **Containerisation with docker**

The Chatty Chat application was containerized using Docker to ensure consistent behavior across all environments. Docker containers provide isolated, lightweight environments that simplify testing and deployment. Dockerfiles were created in the backend/ and frontend/ directories, images were built locally using docker build, and functionality was verified by running the containers locally. 

3. **CI/CD Integration - Jenkins**

A CI/CD pipeline was implemented using Jenkins to automate the building, testing, and deployment of the Chatty Chat application. Jenkins was configured to trigger the pipeline on commits to the main branch. The pipeline builds Docker images for both backend and frontend, runs automated tests, and pushes the images to a container registry. Deployment is automated by pulling these images to the target environment, ensuring that updates are delivered reliably and consistently. This setup enables rapid and repeatable deployments, reduces manual errors, and maintains synchronization between code changes and the running application.

Jenkins was installed and the Jenkins Master was configured to port 8080 of the master instance. All required plugins were installed into the Jenkins Master.

4. **EKS Cluster Setup**

After installing and configuring Jenkins, an EKS (Elastic Kubernetes Service) cluster was created to serve as the deployment environment for the Chatty Chat application. The cluster provides a scalable and managed Kubernetes infrastructure to run the containerized backend and frontend services. Jenkins was configured with credentials and access to the EKS cluster, enabling automated deployment of Docker images. Kubernetes manifests were applied to the cluster. This setup allows the application to run in isolated pods with proper networking, scaling, and load balancing, ensuring high availability and reliability. 

Kubectl was installed, Eksctl installed, Associate IAM OIDC Provider was given and a nodegroup was created.

5. **OWASP, Trivy and SonarQube integration**

To ensure the security and quality of the Chatty Chat application, OWASP Trivy and SonarQube were integrated into the Jenkins pipeline. OWASP Trivy was used to scan Docker images for vulnerabilities, identifying any security risks in both backend and frontend containers before deployment. SonarQube was configured to analyze the source code for bugs, code smells, and maintainability issues, providing detailed reports on code quality. These tools were incorporated as pipeline stages in Jenkins, so that any security vulnerabilities or critical code quality issues automatically fail the build, preventing unsafe or low-quality code from being deployed. This integration enforces robust security practices and maintains high code standards throughout the development lifecycle.

The credentials were then updated into the Jenkins Master for complete integration. SonarQube was 

6. **ArgoCD Setup**

ArgoCD was integrated to implement a GitOps approach for managing the Chatty Chat application on the EKS cluster. All Kubernetes manifests for the backend and frontend, including Deployment, Service, and Ingress files, were stored in a dedicated Git repository. ArgoCD continuously monitors this repository and automatically synchronizes the cluster state with the Git configuration. Any changes pushed to the repository, such as updated container images or configuration updates, are detected by ArgoCD and applied to the cluster without manual intervention. This ensures a declarative, version-controlled, and reliable deployment process, maintaining consistency between the Git repository and the live application environment.


7. **SMTP Configuration**

SMTP was configured to enable the Chatty Chat application to send email notifications, such as account verification, password resets, and system alerts. This setup was also integrated into the Jenkins pipeline to send notifications for build status, deployment success, or failures, providing real-time visibility into the CI/CD process.

The app was built on ArgoCD and ensured that all kubernetes manifests were healthy. The app was then deployed on ports 31000 and 31100. 

8. **Pipeline Build**

All configurations were given to Jenkins and the pipelines were successfully built and running.

9. **Helm Installed and Prometheus-Grafana repositories added**

Helm was installed to simplify the deployment and management of applications on the EKS cluster. Using Helm, Prometheus and Grafana repositories were added to enable monitoring of the Chatty Chat application. Prometheus collects metrics from the cluster and application pods, while Grafana visualizes these metrics through customizable dashboards. This setup provides real-time insights into application performance, resource usage, and system health. Helm charts were used to manage the installation and configuration, ensuring consistent and reproducible deployments of the monitoring stack across the cluster.

10. **Prometheus-Grafana**

Prometheus and Grafana were deployed on the EKS cluster to provide comprehensive monitoring of the Chatty Chat application. Prometheus collects metrics from the application pods, Kubernetes nodes, and cluster resources, tracking CPU, memory, network usage, and application-specific metrics. Grafana consumes these metrics to create interactive dashboards, allowing visualization of system performance and health in real time. Alerts were configured in Prometheus for critical thresholds, enabling proactive response to performance issues or failures. This monitoring setup ensures operational reliability, observability, and quick troubleshooting across the application environment.

Prometheus was exposed on port 32487 and Grafana was exposed on port 30500.

---

## Steps

1. Creation of EC2 Master Instance in AWS Console with 2CPU, 8GB of RAM (t2.large) and 29 GB of storage.
2. Installation and configuration of Docker.
    ```bash
    sudo apt-get install docker.io -y
    sudo usermod -aG docker ubuntu && newgrp docker
    ```
3. Install and configure Jenkins on the instance.
    ```bash
        sudo apt update -y
    sudo apt install fontconfig openjdk-17-jre -y

    sudo wget -O /usr/share/keyrings/jenkins-keyring.asc \
    https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key
    
    echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc]" \
    https://pkg.jenkins.io/debian-stable binary/ | sudo tee \
    /etc/apt/sources.list.d/jenkins.list > /dev/null
    
    sudo apt-get update -y
    sudo apt-get install jenkins -y
    ```
4. Add port 8080 to the instance security group and access Jenkins Master on the browser on port 8080 and configure it.

5. Create EKS Cluster on AWS (Master machine), add IAM user, obtain access keys, and configure AWS CLI.
    ```bash
        curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
    sudo apt install unzip
    unzip awscliv2.zip
    sudo ./aws/install
    aws configure
    ```

6. Install Kubectl and Eksctl
    ```bash
        curl -o kubectl https://amazon-eks.s3.us-west-2.amazonaws.com/1.19.6/2021-01-05/bin/linux/amd64/kubectl
    chmod +x ./kubectl
    sudo mv ./kubectl /usr/local/bin
    kubectl version --short --client

    curl --silent --location "https://github.com/weaveworks/eksctl/releases/latest/download/eksctl_$(uname -s)_amd64.tar.gz" | tar xz -C /tmp
    sudo mv /tmp/eksctl /usr/local/bin
    eksctl version
    ```

7. Create EKS Cluster, provide OIDC and create Nodegroup.
    ```bash
    eksctl create cluster --name=chat-app \
                    --region=us-east-1 \
                    --version=1.30 \
                    --without-nodegroup

    eksctl utils associate-iam-oidc-provider \
                    --region us-east-1 \
                    --cluster chat-app \
                    --approve

    eksctl create nodegroup --cluster=chat-app \
                     --region=us-east-1 \
                     --name=chat-app \
                     --node-type=t2.large \
                     --nodes=2 \
                     --nodes-min=2 \
                     --nodes-max=2 \
                     --node-volume-size=29 \
                     --ssh-access \
                     --ssh-public-key=eks-nodegroup-key 

8. Install and Configure SonarQube on Master Instance, add to instance security group and expose on port 9000.

    ```bash 
    docker run -itd --name SonarQube-Server -p 9000:9000 sonarqube:lts-community
    ```

9. Install Trivy.
    ```bash
        sudo apt-get install wget apt-transport-https gnupg lsb-release -y
    wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | sudo apt-key add -
    echo deb https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main | sudo tee -a /etc/apt/sources.list.d/trivy.list
    sudo apt-get update -y
    sudo apt-get install trivy -y
    ```

10. Install and configure ArgoCD on the cluster.
    ```bash
    kubectl create namespace argocd
    kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
    watch kubectl get pods -n argocd
    sudo curl --silent --location -o /usr/local/bin/argocd https://github.com/argoproj/argo-cd/releases/download/v2.4.7/argocd-linux-amd64sudo chmod +x /usr/local/bin/argocd
    ```
    Patch ArgoCD Server as a NodePort and access it on the browser.
    ```bash
    kubectl patch svc argocd-server -n argocd -p '{"spec": {"type": "NodePort"}}'
    ```

11. Allow port 465 on master instance for SMTPS notifications. Configure it in corresponding google account and Jenkins Master. Test the SMTP connection.

12. Install Jenkins Plugins for all required services. Add sonarqube Scanner installations. Add credentials from the respective service UI to Jenkins master. Create a sonarqube webhook.

13. Update instance ID on the github repository and under Automations directory update the instance-id field on both the updatefrontendnew.sh updatebackendnew.sh.

14. Add credentials to DockerHub for the built images to be pushed.

15. Create the CI and CD pipelines.

16. Add the EKS Cluster to ArgoCD for application deployment.
    ```bash 
    argocd cluster add Chat-app@chat-app.us-east-1.eksctl.io --name chat-app-eks-cluster
    ```
    Open ArgoCD, add repositories and build the application.

17. Install Helm Chart and add helm repository for Prometheus and Grafana.
    ```bash
    curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3
    chmod 700 get_helm.sh
    ./get_helm.sh

    helm repo add stable https://charts.helm.sh/stable

    helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
    ```
    ```bash
    helm install stable prometheus-community/kube-prometheus-stack -n prometheus

    kubectl get svc -n prometheus
    ```

    ```bash
    Expose it as a NodePort and access on browser. 

18. Expose Grafana similiarly and access it on browser.

19. Gather metrics and monitor app data.

20. Clean up and delete Cluster.
    ```bash
    eksctl delete cluster --name=chat-app --region=us-east-1
    ```

---





    

