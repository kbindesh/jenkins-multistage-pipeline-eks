# Project-03: Build a Docker image for Python flask App using Jenkins pipeline

# GitHub >> Jenkins >> Docker >> DockerHub >> Amazon EKS

## Project Overview

This project will demonstrate the end-to-end implementation (CD pipeline) of app development, packaging, shipping and deployment of an application (JS) using AWS, Jenkins, Docker and Amazon EKS Cluster.

## Prerequisites

- [AWS Account](https://aws.amazon.com/free/)
- IDE
  - [Visual Studio Code](https://code.visualstudio.com/download)
- Install VS Code Extensions (Terraform, terraform-lint)
- [Git](https://git-scm.com/downloads)
- [Docker Hub Account](https://hub.docker.com/signup)

## Step-01: Develop and Test Application Code (on local system)

### 1.1 Develop `Application code` + `Dockerfile` + `Jenkinsfile + Kubernetes manifests`

- This repo has a sample application code, Dockerfile, Jenkinsfile and K8s manifests which you may clone/fork it for learning purpose.

```
# To clone the code on your local system
git clone https://github.com/kbindesh/jenkins-multistage-pipeline-eks.git
```

### 1.2 Create a new GitHub repository

- Sign-in to your Github account (https://github.com/login).
- In the upper-right corner of your github account page, select + , then click New repository.
- Type a _name_ for your repository, and an optional _description_.
- Choose a repository visibility. For more information, see [About repositories](https://docs.github.com/en/repositories/creating-and-managing-repositories/about-repositories#about-repository-visibility).
- Click **Create repository**.

### 1.3 Clone the project repo (Github), add the app code & check-in the code

- Open the Visual Studio Code editor >> Open your project folder.
- Go to **View** menu >> **Terminal**

```
# Syntax: Clone a particular remote branch
git clone -b <branch_name> <remote_repo_url>

# Example: Clone a particular remote branch
git clone -b dev-branch https://github.com/kbindesh/jenkins-multistage-pipeline-eks.git

# Get inside the project folder
cd jenkins-multistage-pipeline-eks

# Create a featurebranch and switch to it
git checkout -b bin-featurebranch
```

- **Move the application code (Step#1.1) to the cloned repository and commit the changes.**

```
# To check all the new changes
git status

# Stage new changes
git add .

# Commit the changes locally
git commit -m "Nodejs App 1.0.0"
```

- **Check-in the bin-featurebranch to remote repo**

```
git push -u origin bin-featurebranch
```

## Step-02: Setup the Jenkins Server

### 2.1 Provision an EC2 Instance

- **AMI**: Amazon Linux 2 (you can have any other OS)
- **Instance Type**: t2.micro
- **VPC & Subnet**: Default
- **Public IP**: Enabled
- **Security Group**: Allow Ingress traffic on SSH(22) and 8080 (for jenkins)
- **Storage**: Root Volume(recommeded 20GB+)
- **Tags**
  - Name <=> Jenkins-Server

### 2.2 Install and Configure `Jenkins`

- Connect to Jenkins server (EC2 instance)
- **Configure Jenkins on Amazon Linux 2 EC2 Instance**

```
# Install Java
sudo yum install -y java-17-amazon-corretto.x86_64

# Download Jenkins repo
sudo wget -O /etc/yum.repos.d/jenkins.repo https://pkg.jenkins.io/redhat-stable/jenkins.repo
sudo rpm --import https://pkg.jenkins.io/redhat-stable/jenkins.io-2023.key
sudo yum -y upgrade

# Install Jenkins
sudo yum install -y jenkins

# Start and enable Jenkins service
sudo systemctl start jenkins
sudo systemctl enable jenkins
```

- **On other platforms**

- Refer to this link: https://www.jenkins.io/doc/book/installing/

### 2.3 Install & Configure `Docker` on Jenkins server

- Execute below commands to install docker:

```
sudo amazon-linux-extras install -y docker
sudo service docker start
sudo usermod -aG docker jenkins
sudo chkconfig docker on
```

### 2.4 Download and Install `Git`

- On Amazon Linux 2/RHEL/CentOS

```
sudo yum install -y git
```

- On other OS platforms, refer to this link: https://git-scm.com/downloads

### 2.5 Elevate the permissions of `jenkins` user

```
# Open the file /etc/sudoers in vi mode
sudo vi /etc/sudoers

# Add the following line at the end of the file
jenkins ALL=(ALL) NOPASSWD: ALL
```

- Now we can use jenkins jobs as root user; run the following command:

```
sudo su - jenkins
```

### 2.6 Install and Setup `AWS CLI`

### 2.7 Install and setup `eksctl` and `kubectl`

### 2.8 Add `Docker and Github credentials` on Jenkins server

### 2.9 Create a new Github Webhook for triggering Jenkins jobs

- Navigate to the Github repository you created on **Step# 1.2**
- Go to **Setting** tab >> **Webhooks** >> **Add Webhook**
- **Payload URL**: https://<your_jenkins_server_public_ip_or_dns>:8080/github-webhook/
- **Content type**: application/json
- **Which events would you like to trigger this webhook?** : Let me select individual events >> Select **Pull requests** and **Pushes** checkboxes
- Check the **Active** checkbox >> **Create Webhook**

## Step-03: Create an Amazon EKS Cluster (using eksctl)

- [Click me for detailed documentation](https://github.com/kbindesh/kubernetes-masterclass/tree/main/Module-03_Setting_up_K8s_Cluster/03-amazon-eks){:target="\_blank" rel="noopener"}

<a href="http://..." target="_blank">external link</a>

## Step-04: Build, Deploy and Test CI/CD pipeline

### 4.1 Create a Jenkins job (pipeline)

### 4.2 Trigger the Jenkins job

### 4.3 Verify the created Docker Image in Docker Hub

### 4.4 Verify the deployed application on EKS cluster over the web
