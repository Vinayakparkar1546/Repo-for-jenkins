# CartForge Jenkins CI Project

## 1. Project Overview

This project demonstrates the implementation of a Jenkins Continuous Integration environment for the CartForge application. The project includes Jenkins Controller and Agent configuration, GitHub integration, automated pipeline execution, GitHub Webhook configuration, testing, packaging, and artifact delivery.

## 2. Jenkins Architecture

The project uses a Jenkins Controller and a separate Jenkins Agent running on Ubuntu EC2 instances.

```text
                    GitHub Repository
                          |
                    GitHub Webhook
                          |
                          v
                +-------------------+
                | Jenkins Controller |
                |     Ubuntu EC2     |
                +---------+---------+
                          |
                     SSH Connection
                          |
                          v
                +-------------------+
                | Jenkins Agent     |
                | jenkins-agent-1   |
                |     Ubuntu EC2     |
                +---------+---------+
                          |
                          v
             Jenkins Pipeline Execution
```

The Jenkins Controller manages the jobs and pipeline, while `jenkins-agent-1` executes the pipeline tasks.

## 3. Installation Steps

### Jenkins Controller

1. Launch an Ubuntu EC2 instance on AWS.
2. Connect to the instance using SSH.
3. Update the system packages.
4. Install Java.
5. Install Jenkins.
6. Start and enable the Jenkins service.
7. Access Jenkins using port `8080`.
8. Complete the initial Jenkins setup and create the administrator account.

Example commands:

```bash
sudo apt update
sudo apt install fontconfig openjdk-17-jre -y
sudo systemctl enable jenkins
sudo systemctl start jenkins
sudo systemctl status jenkins
```

### Jenkins Agent

1. Launch a second Ubuntu EC2 instance.
2. Install Java on the Agent.
3. Configure SSH authentication.
4. Create a Jenkins Node/Agent in Jenkins.
5. Configure the node with the label `jenkins-agent-1`.
6. Connect the Agent to the Jenkins Controller.
7. Verify that the Agent shows as online.

## 4. Jenkins Configuration

The Jenkins Controller was configured to manage the CI pipeline.

The Jenkins Agent was configured with:

```text
Node Name: jenkins-agent-1
Label: jenkins-agent-1
```

The pipeline was configured to execute on this Agent using:

```groovy
agent {
    label 'jenkins-agent-1'
}
```

GitHub was connected to Jenkins using the repository:

```text
https://github.com/Vinayakparkar1546/Repo-for-jenkins
```

The Jenkins job was configured with:

```text
Definition: Pipeline script from SCM
SCM: Git
Branch: */main
Script Path: Jenkinsfile
```

## 5. Pipeline Workflow

The Jenkins Pipeline contains six stages:

```text
GitHub
   |
   v
Clone Source Code
   |
   v
Install Dependencies
   |
   v
Build Application
   |
   v
Run Tests
   |
   v
Package Application
   |
   v
Deliver Artifact
   |
   v
cartforge-app.tar.gz
```

### Pipeline Stages

1. **Clone Source Code**
   Retrieves the source code from the GitHub repository.

2. **Install Dependencies**
   Verifies the required tools and environment for the static HTML application.

3. **Build Application**
   Validates `index.html` and prepares the build directory.

4. **Run Tests**
   Performs basic validation of the generated HTML file.

5. **Package Application**
   Creates the `cartforge-app.tar.gz` package.

6. **Deliver Artifact**
   Archives the packaged application in Jenkins.

## 6. GitHub Continuous Integration

A GitHub Webhook was configured to automatically notify Jenkins when code was pushed to the repository.

The CI workflow is:

```text
Code Change
     |
     v
Git Push to GitHub
     |
     v
GitHub Webhook
     |
     v
Jenkins
     |
     v
Automatic Pipeline Execution
     |
     v
jenkins-agent-1
     |
     v
Build → Test → Package
     |
     v
Artifact Generated
```

The automatic build was successfully tested by pushing a change to the GitHub repository.

## 7. Commands Used

### System Update

```bash
sudo apt update
```

### Install Java

```bash
sudo apt install fontconfig openjdk-17-jre -y
```

### Jenkins Service

```bash
sudo systemctl start jenkins
sudo systemctl enable jenkins
sudo systemctl status jenkins
```

### Check Git

```bash
git --version
```

### Check Jenkins Port

```bash
sudo ss -lntp | grep 8080
```

### Create Backup Directory

```bash
mkdir ~/jenkins-backup
```

### Create Application Package

```bash
tar -czf cartforge-app.tar.gz -C build index.html
```

## 8. Folder Structure

The GitHub repository contains:

```text
Repo-for-jenkins/
│
├── Jenkinsfile
└── index.html
```

The Jenkins Pipeline generates a build directory during execution:

```text
workspace/
│
├── index.html
├── Jenkinsfile
├── build/
│   └── index.html
│
└── cartforge-app.tar.gz
```

## 9. Challenges Faced

During the project, several configuration and troubleshooting issues were encountered:

* Configuring SSH authentication between the Jenkins Controller and Agent.
* Setting the correct permissions for the Jenkins user's SSH files.
* Connecting the Jenkins Pipeline with the GitHub repository.
* Configuring the Jenkins Agent and making sure it was online.
* Configuring the GitHub Webhook with the Jenkins public IP address.
* Troubleshooting the initial GitHub Webhook connection failure.
* Updating the Webhook when the Jenkins EC2 public IP changed.
* Verifying automatic pipeline execution after a GitHub code push.

These issues were resolved through configuration changes and troubleshooting.

## 10. Learning Outcomes

Through this project, I learned:

* Jenkins installation and administration.
* Jenkins Controller and Agent architecture.
* Jenkins Node/Agent configuration.
* SSH-based Agent connectivity.
* Jenkins Declarative Pipeline creation.
* GitHub and Jenkins integration.
* GitHub Webhook configuration.
* Continuous Integration implementation.
* Automated pipeline execution.
* Build and test automation.
* Artifact packaging and archiving.
* Pipeline monitoring using Console Output and Build History.
* Basic pipeline optimization concepts.

## 11. Project Status

**Project Completed Successfully**

```text
Jenkins Controller       ✅
Jenkins Agent            ✅
GitHub Integration       ✅
Jenkins Pipeline         ✅
GitHub Webhook           ✅
Automatic Build          ✅
Pipeline Testing         ✅
Artifact Generation      ✅
Documentation            ✅
```

## 12. Repository

GitHub Repository:

`https://github.com/Vinayakparkar1546/Repo-for-jenkins`
