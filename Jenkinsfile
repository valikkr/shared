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
          sh 'docker build -t shakurit0/my-first-docker-repo .'
        }
      }
    }
    stage('Pushing Image') {
      steps{
        script {
          sh 'cat ~/my_password.txt | docker login --username foo --password-stdin'
           sh 'docker tag flask:latest shakurit0/my-first-docker-repo:${BUILD_NUMBER}'
            sh'docker push shakurit0/my-first-docker-repo:${BUILD_NUMBER}'
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
