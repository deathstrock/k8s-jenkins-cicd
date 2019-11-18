pipeline {

  environment {
    registry = "your-docker-hub/repo"
    dockerImage = ""
    registryCredential = "dockerhub"
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git 'https://github.com/deathstrock/k8s-jenkins-cicd.git'
      }
    }

    stage('Build image') {
      steps{
        script {
          dockerImage = docker.build registry + ":$BUILD_NUMBER"
        }
      }
    }

    stage('Push Image') {
      steps{
        script {
          docker.withRegistry( "", registryCredential ) {
            dockerImage.push()
          }
        }
      }
    }

    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "myweb.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
