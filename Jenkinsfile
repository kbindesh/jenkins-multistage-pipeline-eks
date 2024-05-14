pipeline {
  agent any
  parameters {
    string(name: 'Docker_Username', defaultValue: 'kbindesh', description: 'DockerHub Account name')
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker_creds')
  }

  stages {
    stage("Git Checkout"){
        git credentialsId: 'GIT_HUB_CREDENTIALS', url: 'https://github.com/kbindesh/jenkins-multistage-pipeline-eks.git', branch: 'main' 
    }

    stage("Build image") {

        sh 'docker build -t ${Docker_Username}/nodejsapp:$BUILD_NUMBER' .
        sh 'docker image list'

    }

    withCredentials([string(credentialsId: 'DOCKERHUB_CREDENTIALS', variable: 'PASSWORD')]) {
        sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
    }

    stage("Push Image"){
        sh 'docker push ${Docker_Username}/node-todo-test:$BUILD_NUMBER'
    }

    stage("Deploy app to EKS"){
        sh 'kubectl apply -f deployment.yml'
    }
  }
}