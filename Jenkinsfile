pipeline {
  environment {
    imagename = "shakurit0/my-first-docker-repo"
    registryCredential = 'dockerhub-cred'
    dockerImage = ''
  }
  agent any
  stages {
    stage('Cloning Git') {
      steps {
        checkout([$class: 'GitSCM', branches: [[name: '*/*']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/valikkr/shared.git']]])

      }
    }
    stage('Building image') {
      steps{
        script {
          dockerImage = docker.build imagename
        }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          docker.withRegistry( '', registryCredential ) {
            dockerImage.push("$BUILD_NUMBER")
             dockerImage.push('latest')

          }
        }
      }
    }
    stage('Remove Unused docker image') {
      steps{
        sh "docker rmi $imagename:$BUILD_NUMBER"
         sh "docker rmi $imagename:latest"

      }
    }
  }
}
