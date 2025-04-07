# Make your AI-Powered Chatbot Secure and Scalable 🤖

“Explore how EIT seamlessly implemented a secure, scalable, and efficient AI-Powered Chatbot after launching the client’s first chatbot model.”

---

## The Problem:⚠️
As businesses increasingly rely on cloud-based solutions ☁️, the client recognized the need to enhance user engagement by integrating AI-powered Chatbots 🤖. Traditional deployment methods were insufficient, lacking automation and rigorous security checks 🛡️. This approach posed risks such as potential vulnerabilities, scalability issues 📉, and operational inefficiencies. Additionally, the client faced challenges with the AI-Powered Chatbot's response time and overall performance. These performance issues could frustrate users, leading to decreased satisfaction and engagement 😤. Inconsistent chatbot performance could also hinder the user experience, resulting in lost opportunities for customer interaction and support.

## Key Infrastructure Needs 🛠️:
With rising user expectations, delivering a personalized, always-on AI-Powered Chatbot experience was critical to scaling engagement. The client needed a robust infrastructure that could: 
- Handle large volumes of traffic and dynamically scale 🔄.
- Secure the deployment process to protect sensitive data 🔒.
- Streamline continuous deployment for faster updates and maintenance ⚙️.

---

## Technology Overview:
- **CI/CD ♾️**: Jenkins was used as the CI/CD tool for automating the deployment process, with pipelines created to handle continuous integration and deployment tasks efficiently.
  
- **Containerization 🐳**: Docker was employed to containerize the AI-Powered Chatbot application, providing a lightweight, portable environment that ensures consistent behavior across various stages of the deployment process. This simplified scaling and updating the application in a controlled manner.

- **Container Orchestration 🚢**: AWS EKS (Kubernetes) was chosen for container orchestration due to its powerful scaling and self-healing capabilities. This enabled automated management of the Docker containers, ensuring high availability for the AI-Powered Chatbot application.

- **Infrastructure as Code (IaC) 🏗️**: Terraform was employed for IaC, enabling automated provisioning and management of AWS resources, including the AWS EKS cluster.

- **Dependency Check 🛡️**: OWASP Dependency Check was used to scan dependencies for vulnerabilities, ensuring the application is safeguarded against external threats.

- **Code Analysis 🧐**: SonarQube was utilized for static code analysis, ensuring that the code remains free from bugs and security vulnerabilities.

- **Container Image Scanning 🔍**: Trivy was integrated to scan Docker images and file systems, adding an additional layer of security by identifying vulnerabilities prior to deployment.


## Phase 1: Initial Setup

### Step 1: Launch EC2 (Ubuntu 24.04)
1. **Provision an EC2 instance on AWS** with Ubuntu 24.04.
2. **Launch an Ubuntu T2 Large Instance.**
   - Create a new key pair or use an existing one.
   - Enable HTTP and HTTPS in the Security Group and open all ports (not ideal, but acceptable for learning purposes).
3. **Connect to the instance via SSH.**

### Create IAM Role
1. Go to **IAM** in AWS and create a role:
   - Select **AWS service** as the entity type, and **EC2** as the use case.
   - Choose **Administrator Access** for permissions (for learning purposes), then create the role.
2. **Attach the role** to the EC2 instance:
   - In the EC2 Dashboard, select the instance.
   - Go to **Actions → Security → Modify IAM Role**.
   - Select the newly created role and update the IAM role.
---
### Step 2: Clone the Code
1. **Fork the repository to your GitHub account**
2. **Update all packages**:
   ```bash
   # Update all installed packages to the latest version
   sudo apt-get update && sudo apt-get upgrade -y
   ```
2. **Clone your application repository**:
   ```bash
   # Replace the URL with your GitHub repository URL and clone the repository from GitHub
   git clone <Your-repo-url>
   
   # Change into the cloned repository directory
   cd chatbot-ui
   
   # Switch to the 'legacy' branch
   git switch legacy
   ```

## Phase 2:

### Step 1: Install Docker
1. **Set up Docker**:
   ```bash
   # Install Docker
   sudo apt-get install docker.io -y
   
   # Add current user to the Docker group to manage Docker as a non-root user
   sudo usermod -aG docker $USER
   
   # Apply Docker group changes immediately
   newgrp docker
   
   # Adjust permissions on the Docker socket
   sudo chmod 777 /var/run/docker.sock
   ```
---
### Step 2: Install SonarQube and Trivy
#### SonarQube
1. **Run SonarQube in a container**:
   ```bash
   # Run SonarQube in a Docker container and expose it on port 9000
   docker run -d --name sonar -p 9000:9000 sonarqube:lts-community
   ```
2. **Access SonarQube**:
   - Open port **9000** in the EC2 Security Group.
   - Access via `publicIP:9000` (default credentials: `admin/admin`).

#### Trivy
1. **Install dependencies**:
   ```bash
   # Install required packages for downloading and verifying Trivy
   sudo apt-get install wget apt-transport-https gnupg lsb-release -y
   ```
2. **Install Trivy**:
   ```bash
   # Download and add Trivy's GPG key
   wget -qO - https://aquasecurity.github.io/trivy-repo/deb/public.key | gpg --dearmor | sudo tee /usr/share/keyrings/trivy.gpg > /dev/null
   
   # Add Trivy's repository to the sources list
   echo "deb [signed-by=/usr/share/keyrings/trivy.gpg] https://aquasecurity.github.io/trivy-repo/deb $(lsb_release -sc) main" | sudo tee -a /etc/apt/sources.list.d/trivy.list
   
   # Update package list and install Trivy
   sudo apt-get update && sudo apt-get install trivy -y
   ```
---
### Step 3: Install Jenkins for Automation
1. **Install Java**:
   ```bash
   # Update package list
   sudo apt update -y
   
   # Fetch and install Adoptium GPG key for Java
   wget -O - https://packages.adoptium.net/artifactory/api/gpg/key/public | sudo tee /etc/apt/keyrings/adoptium.asc
   
   # Add Adoptium's repository to the sources list
   echo "deb [signed-by=/etc/apt/keyrings/adoptium.asc] https://packages.adoptium.net/artifactory/deb $(awk -F= '/^VERSION_CODENAME/{print $2}' /etc/os-release) main" | sudo tee /etc/apt/sources.list.d/adoptium.list
   
   # Update package list again to include the new Java source
   sudo apt update -y
   
   # Install Temurin 17 JDK (Java Development Kit)
   sudo apt install temurin-17-jdk -y
   ```
2. **Install Jenkins**:
   ```bash
   # Fetch and install Jenkins' GPG key
   curl -fsSL https://pkg.jenkins.io/debian-stable/jenkins.io-2023.key | sudo tee /usr/share/keyrings/jenkins-keyring.asc > /dev/null
   
   # Add Jenkins repository to the sources list
   echo "deb [signed-by=/usr/share/keyrings/jenkins-keyring.asc] https://pkg.jenkins.io/debian-stable binary/" | sudo tee /etc/apt/sources.list.d/jenkins.list > /dev/null
   
   # Update package list to include Jenkins' repository
   sudo apt-get update -y
   
   # Install Jenkins
   sudo apt-get install jenkins -y
   
   # Start Jenkins service
   sudo systemctl start jenkins
   
   # Enable Jenkins to start at boot
   sudo systemctl enable jenkins
   ```
3. **Access Jenkins**:
   - Open port **8080** in the EC2 Security Group.
   - Access via `publicIP:8080`.
   - Retrieve the admin password:
     ```bash
     # Retrieve Jenkins' initial admin password
     sudo cat /var/lib/jenkins/secrets/initialAdminPassword
     ```
4. **Unlock Jenkins**:
   - Unlock Jenkins using the administrative password.
   - Install the suggested plugins.
   - Jenkins will automatically install the necessary libraries.
   - Create a user and click "Save and Continue."
   - You'll now be on the **Jenkins Getting Started** screen.
---
### Step 4: Install Terraform
1. **Install Terraform**:
   ```bash
   # Install wget for downloading files
   sudo apt install wget -y
   
   # Download and add HashiCorp's GPG key for Terraform
   wget -O- https://apt.releases.hashicorp.com/gpg | sudo gpg --dearmor -o /usr/share/keyrings/hashicorp-archive-keyring.gpg
   
   # Add HashiCorp's repository to the sources list
   echo "deb [signed-by=/usr/share/keyrings/hashicorp-archive-keyring.gpg] https://apt.releases.hashicorp.com $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/hashicorp.list
   
   # Update package list and install Terraform
   sudo apt update && sudo apt install terraform -y
   ```
---
### Step 5: Install kubectl
1. **Install kubectl**:
   ```bash
   # Install curl for downloading files
   sudo apt install curl -y
   
   # Download the latest stable version of kubectl
   curl -LO https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl
   
   # Install kubectl
   sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
   
   # Check the installed version of kubectl
   kubectl version --client
   ```
---
### Step 6: Install AWS CLI
1. **Install AWS CLI**:
   ```bash
   # Download AWS CLI installer
   curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
   
   # Install unzip tool
   sudo apt-get install unzip -y
   
   # Unzip AWS CLI installer
   unzip awscliv2.zip
   
   # Install AWS CLI
   sudo ./aws/install
   ```
---
### Final Check: Ensure All Tools Are Installed
Run these commands to check the installation versions:
```bash

# Check the installed version of docker
docker --version

# Check the installed version of Trivy
trivy --version

# Check the installed version of Jenkins
jenkins --version

# Check the installed version of Terraform
terraform --version

# Check the installed version of AWS CLI
aws --version

# Check the installed version of kubectl (client version)
kubectl version --client
```

## Phase 3:

### Step 1: Install Necessary Plugins in Jenkins

1. **Navigate to the Plugins section**:
   - Go to **Manage Jenkins** → **Manage Plugins** → **Available Plugins**.

2. **Install the following plugins**:

   - **Eclipse Temurin Installer**
   - **SonarQube Scanner**
   - **NodeJs Plugin**
   - **Blue Ocean**
   - **Docker**
   - **Docker Commons**
   - **Docker Pipeline**
   - **Docker API**
   - **Docker Build Step**
   - **OWASP Dependency-Check**
   - **Terraform**
   - **Kubernetes**
   - **Kubernetes CLI**
   - **Kubernetes Client API**
   - **Kubernetes Pipeline DevOps Steps**
   - **Kubernetes Credentials**
   - **Kubernetes Credentials Provider**
---
### Step 2: Configure Global Tools in Jenkins

1. **Navigate to Global Tool Configuration**:
   - Go to **Manage Jenkins** → **Global Tool Configuration**.

2. **Configure Java (JDK 17)**:
   - **Click on Add JDK**.
   - **Name**: `jdk17`.
   - **Install automatically** → Install from adoptium.net.
   - **Version**: `jdk-17.0.10+7`.

3. **Configure NodeJS (NodeJS 19)**:
   - **Click on Add NodeJS**.
   - **Name**: `node19`.
   - **Version**: `NodeJS 19.0.0`.

4. **Configure Docker**:
   - **Click on Add Docker**.
   - **Name**: `docker`.
   - **Install automatically**.
   - **Download from docker.com**.
   - **Version**: `latest`.

5. **Configure OWASP Dependency-Check**:
   - **Click on Add Dependency-Check**.
   - **Name**: `DP-Check`.
   - **Install automatically**.
   - **Install from github.com**.
   - **Version**: `dependency-check 8.4.0`.

6. **Configure Terraform**:
   - **Click on Add Terraform**.
   - **Name**: `terraform`.
   - **Install directory**: `/usr/bin/`.
7. **Configure SonarQube Scanner**:
   - **Click on Add SonarQube Scanner**.
   - **Name**: `sonar-scanner`.
   - **Install automatically**.
   - **Install from Maven Central**.
   - **Version**: `SonarQube Scanner 5.0.1.3006`.


8. **Click on Apply and Save** to save all the tool configurations.
---

### Step 3: Configure SonarQube in Jenkins

1. **Set up `Quality Gate` in SonarQube**:
   - Go to your **SonarQube Server**.
   - In the **SonarQube Dashboard**:
     - Navigate to **Administration** → **Configuration** → **Webhooks**.
     - **Click on Create** → Name: `Jenkins`.
     - **URL**: `http://jenkins-public-ip:8080/sonarqube-webhook/` (replace with your Jenkins public IP).
     - **Click on Create**.

2. **Generate and Configure SonarQube Token**:
   - In the **SonarQube Dashboard**:
     - Navigate to **Administration** → **Security** → **Users** → **Tokens**.
     - **Click on Update Token** → Name the token → **Click on Generate Token**.
     - **Copy the generated token**.

3. **Add SonarQube Token to Jenkins**:
   - Go to the **Jenkins Dashboard** → **Manage Jenkins** → **Credentials** → **System** → **Global credentials (unrestricted)**.
   - **Add Credentials** → **Kind**: `Secret Text`.
   - **Secret**: Paste the token copied from SonarQube.
   - **ID**: `Sonar-token`.
   - **Description**: `Sonar-token`.
   - **Click on Create**.

4. **Add DockerHub Credentials to Jenkins**:
   - In **Global Credentials**:
     - **Add Credentials** → **Kind**: `Username and Password`.
     - **Username**: Your DockerHub username.
     - **Password**: Your DockerHub password.
     - **ID**: `docker`.
     - **Description**: `docker`.
     - **Click on Create**.

---

### Step 4: Configure SonarQube in Jenkins

1. Go to **Dashboard** → **Manage Jenkins** → **System**.
2. Find **SonarQube Servers** → **SonarQube Installations**.
3. **Click on Add SonarQube**:
   - **Name**: `sonar-server`.
   - **Server URL**: `http://ipaddress:9000/` (replace with your SonarQube server IP).
   - **Server authentication token**: `Sonar-token`.
4. **Click on Apply and Save**.
---
### Fix Docker Login Failed Error: (Optional)

If you encounter a **Docker login failed** error during the pipeline execution, follow these steps:

```bash
# Add Jenkins user to the Docker group
sudo usermod -aG docker jenkins

# Restart the Jenkins service to apply changes
sudo systemctl restart jenkins
```

## Phase 4: Configure CI/CD Pipelines in Jenkins
### Step 1:
1. **Create a CI/CD pipeline in Jenkins** to automate your application deployment:
   - In the **Jenkins dashboard**, click on **“New Item”**.
   - Enter an **item name**: `Chatbot`.
   - Select an **item type**: `Pipeline`.
   - Click on **Create**.

2. **Configure the Pipeline**:
   - Click on **Pipeline** → paste the following pipeline script under the **Script** section.
3. **Click on `Apply` and `Save`**.
4. **Click on `Build Now` to initiate the pipeline.**.
5. **Pipeline Script Example**:

```groovy
pipeline {
    agent any
    tools {
        jdk 'jdk17'
        nodejs 'node19'
    }
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }
    stages {
        stage('Checkout from Git') {
            steps {
                // Clone the repository from GitHub
                git branch: 'legacy', url: 'your-repo-url'
            }
        }
        stage('Install Dependencies') {
            steps {
                // Install npm dependencies
                sh "npm install"
            }
        }
        stage("SonarQube Analysis") {
            steps {
                // Perform SonarQube analysis
                withSonarQubeEnv('sonar-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                    -Dsonar.projectKey=Chatbot '''
                }
            }
        }
        stage("Quality Gate") {
            steps {
                // Wait for SonarQube quality gate result
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                }
            }
        }
        stage('OWASP FS SCAN') {
            steps {
                // Run OWASP Dependency-Check scan
                dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
            }
        }
        stage('TRIVY FS SCAN') {
            steps {
                // Run Trivy filesystem scan
                sh "trivy fs . > trivyfs.json"
            }
        }
        stage("Docker Build & Push") {
            steps {
                // Build and push Docker image to DockerHub
                script {
                    withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                        sh "docker build -t chatbot ."
                        sh "docker tag chatbot your-dockerhub-username/chatbot:latest"
                        sh "docker push your-dockerhub-username/chatbot:latest"
                    }
                }
            }
        }
        stage("TRIVY Image Scan") {
            steps {
                // Run Trivy image scan
                sh "trivy image your-dockerhub-username/chatbot:latest > trivy.json"
            }
        }
        stage("Remove Container") {
            steps {
                // Remove existing container if any
                sh "docker stop chatbot || true"
                sh "docker rm chatbot || true"
            }
        }
        stage('Deploy to Container') {
            steps {
                // Deploy the new container
                sh 'docker run -d --name chatbot -p 3000:3000 your-dockerhub-username/chatbot:latest'
            }
        }
    }
}
```
Access Chatbot in a web browser using the public IP of your EC2 instance.
```
http://<public-ip>:3000
```
### Note:

Replace the following placeholders:
- `your-username/your-repository.git` with your GitHub repository URL.
- `your-dockerhub-username` with your DockerHub username.

This pipeline performs the following tasks:
- Checkout code from GitHub.
- Install NodeJS dependencies.
- Perform a SonarQube analysis and enforce quality gates.
- Scan the application with OWASP Dependency-Check and Trivy.
- Build and push a Docker image to DockerHub.
- Run Trivy scan on the Docker image.
- Deploy the application inside a Docker container.

---

### Step 2: Create EKS Cluster through Terraform in Jenkins

1. **Go to Legacy Branch in GitHub Repository**:
   - Navigate to the legacy branch in your GitHub repository.
   - Locate the folder called `Eks-terraform`, where you can find the backend file.

2. **Update Configuration Files**:
   - In the `backend.tf` file, change your **S3 bucket name** or **region**.
   - In the `provider.tf` file, change your **region** as necessary.

3. **Update K8s file**:
   - Update the `chatbot-ui.yaml` file in the `k8s` directory.
   - In this file, update the image name to reflect your Docker Hub image name.

**Handle Errors During Pipeline Execution:** (optional)
   - If you encounter an error during the pipeline execution in the `init` stage, replace the command in the pipeline as follows:
   - Option 1: Migrate the State
        ```bash
        # Initializes Terraform with the migration option
        terraform init -migrate-state  
        ```
   - Option 2: Reconfigure Without Migrating the State
        ```bash
        # Initializes Terraform with the reconfiguration option
        terraform init -reconfigure  
        ```

5. **Create a New Pipeline**:
   - Name the new pipeline: `Terraform-Eks`.
   - In the general section, check **“This project is parameterized”** and add a parameter:
     - **Parameter Type**: Choice Parameter
     - **Name**: `action`
     - **Choices**:
       ```
       apply
       destroy
       ```
6. **Click on `Apply` and `Save`**.
7. **Click on `Build with Parameters` to initiate the pipeline. Select `apply` from the action dropdown and then click on `Build`.**
8. **Add the Pipeline Script**:

```groovy
pipeline {
    agent any
    stages {
        stage('Checkout from Git') {
            steps {
                // Clone the repository from GitHub
                git branch: 'legacy', url: 'Your-repo-url'
            }
        }
        stage('Terraform Version') {
            steps {
                // Check the installed Terraform version
                sh 'terraform --version'
            }
        }
        stage('Terraform Init') {
            steps {
                // Initialize Terraform
                dir('Eks-terraform') {
                    sh 'terraform init'
                }
            }
        }
        stage('Terraform Validate') {
            steps {
                // Validate the Terraform configuration
                dir('Eks-terraform') {
                    sh 'terraform validate'
                }
            }
        }
        stage('Terraform Plan') {
            steps {
                // Create a Terraform execution plan
                dir('Eks-terraform') {
                    sh 'terraform plan'
                }
            }
        }
        stage('Terraform Apply/Destroy') {
            steps {
                // Apply or destroy the Terraform infrastructure based on the parameter
                dir('Eks-terraform') {
                    sh 'terraform ${action} --auto-approve'
                }
            }
        }
    }
}
```
### Note:

Replace the following placeholders:
- `your-username/your-repository.git` with your GitHub repository URL.
- Stage view it will take max 10mins to provision.
- Check in Your Aws console whether it created EKS or not.

## Phase 4: 
### **Now in the Jenkins Instance:**

1. Run the following command to update your kubeconfig file:
   ```bash
   aws eks update-kubeconfig --name <clustername> --region <region>
   ```
   This will generate a Kubernetes configuration file.

2. Navigate to the directory containing the config file:
   ```bash
   cd ~/.kube
   ```

3. Display the content of the config file:
   ```bash
   cat config
   ```

4. Copy the content of the config file and create a new file on your local system with any name and a `.yaml` extension. Paste the copied content into this file.

5. Go to **Manage Jenkins** → **Credentials** → **System** → Click on **Jenkins Global** → **Add Credentials**.

6. Select **Kind** as **Secret file** and choose the file you saved locally for Kubernetes configuration.

   - **ID and Description:** `k8s`
   
   - Click on **Create**.

7. Don’t forget to update the image in the `chatbot-ui.yaml` file.

8. Create another pipeline and add the below script to the pipeline:
   
   ```groovy
   pipeline {
       agent any
       tools {
           jdk 'jdk17'
           nodejs 'node19'
       }
       environment {
           SCANNER_HOME = tool 'sonar-scanner'
       }
       stages {
           stage('Checkout from Git') {
               steps {
                   git branch: 'legacy', url: 'Your-repo-url'
               }
           }
           stage('Install Dependencies') {
               steps {
                   sh "npm install"
               }
           }
           stage("Sonarqube Analysis") {
               steps {
                   withSonarQubeEnv('sonar-server') {
                       sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=Chatbot \
                       -Dsonar.projectKey=Chatbot '''
                   }
               }
           }
           stage("Quality Gate") {
               steps {
                   script {
                       waitForQualityGate abortPipeline: false, credentialsId: 'Sonar-token'
                   }
               }
           }
           stage('OWASP FS SCAN') {
               steps {
                   dependencyCheck additionalArguments: '--scan ./ --disableYarnAudit --disableNodeAudit', odcInstallation: 'DP-Check'
                   dependencyCheckPublisher pattern: '**/dependency-check-report.xml'
               }
           }
           stage('TRIVY FS SCAN') {
               steps {
                   sh "trivy fs . > trivyfs.json"
               }
           }
           stage("Docker Build & Push") {
               steps {
                   script {
                       withDockerRegistry(credentialsId: 'docker', toolName: 'docker') {
                           sh "docker build -t chatbot ."
                           sh "docker tag chatbot rizwanshaukat/chatbot:latest "
                           sh "docker push rizwanshaukat/chatbot:latest "
                       }
                   }
               }
           }
           stage("TRIVY") {
               steps {
                   sh "trivy image rizwanshaukat/chatbot:latest > trivy.json"
               }
           }
           stage("Remove Container") {
               steps {
                   sh "docker stop chatbot || true"
                   sh "docker rm chatbot || true"
               }
           }
           stage('Deploy to Kubernetes') {
               steps {
                   script {
                       withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                           sh 'kubectl apply -f k8s/chatbot-ui.yaml'
                       }
                   }
               }
           }
           stage('Scan Cluster') {
               steps {
                   script {
                       withKubeConfig(caCertificate: '', clusterName: '', contextName: '', credentialsId: 'k8s', namespace: '', restrictKubeConfigAccess: false, serverUrl: '') {
                           sh 'trivy k8s --include-namespaces default --report summary'
                       }
                   }
               }    
           }
       }
   }
   ```

9. In the Jenkins instance, run the following commands to check the resources:
   ```bash
   kubectl get all 
   kubectl get svc
   ```

10. Copy the **EXTERNAL IP** and access it in your browser:
    ```
    http://<external-ip>
    ```
---
**Examples:** (Optional)
- **Cluster scanning:**
  ```bash
  trivy k8s --report summary
  ```

- **Cluster scanning with specific namespace:**
  ```bash
  trivy k8s --include-namespaces default --report summary
  ```

- **Cluster with specific context:**
  ```bash
  trivy k8s kind-kind --report summary
  ```

