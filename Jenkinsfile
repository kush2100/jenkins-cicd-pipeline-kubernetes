pipeline {
  environment {
    registry = "kenp8036/jenkins"
    registryCredential = 'dockerhub'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[credentialsId: 'github', url: 'https://github.com/kush2100/jenkins-cicd-pipeline-kubernetes.git']]])
        }
      }
    stage('Building Docker image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }
    stage('Deploy Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $registry:$BUILD_NUMBER"
      }
    }    
    stage('Deploy App') {
      steps {
        script {
                kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "testkubeconfig") {
         }
       }  
     }
    }
  }  
}
