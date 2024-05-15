pipeline {
  agent any
  parameters {
    string(name: 'Docker_Username', defaultValue: 'kbindesh', description: 'DockerHub Account name')
  }
  environment {
    DOCKERHUB_CREDENTIALS = credentials('docker_creds')
  }

  stages {
    stage("Build image") {
      steps {
        sh 'docker build -t ${Docker_Username}/nodejsapp:$BUILD_NUMBER .'
        sh 'docker image list'
      }
    }
    stage('Test image'){
      steps {
            echo 'Empty'
      }
    }
    stage('Connecting to DockerHub') {
      steps{
          sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
      }
    }
    
    stage("Push Image"){
      steps {
        sh 'docker push ${Docker_Username}/nodejsapp:$BUILD_NUMBER'
      }
    }

    stage("Display EKS version"){
      steps{ 
           sh 'sh(aws eks --region us-east-1 update-kubeconfig --name labekscluster'
           sh 'kubectl get nodes'
          }
      }
    }  
}
